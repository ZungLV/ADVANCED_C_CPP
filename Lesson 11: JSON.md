<details>
  <summary><strong> JSON </strong></summary>
  
JSON là viết tắt của "JavaScript Object Notation" (Ghi chú về Đối tượng JavaScript). Đây là **một định dạng truyền tải dữ liệu** phổ biến trong lập trình và giao tiếp giữa các máy chủ và trình duyệt web, cũng như giữa các hệ thống khác nhau.
  
JSON được thiết kế để dễ đọc và dễ viết cho con người, cũng như dễ dàng để phân tích và tạo ra cho máy tính. Nó sử dụng một cú pháp nhẹ dựa trên cặp key - value, tương tự như các đối tượng và mảng trong JavaScript. 
  
Mỗi **đối tượng** JSON bao gồm một tập hợp các cặp **"key" và "value"**, trong khi mỗi mảng **JSON** là một **tập hợp các giá trị**.

Ví dụ một object (đối tượng)
```c
{ 
  "name": "John Doe",
  "age": 30,
  "city": "New York",
  "isStudent": false,
  "grades": [85, 90, 78]
}
```

<details>
<summary><strong> Các thao tác của JSON </strong></summary>
  
<details>
<summary><strong> Các kiểu dữ liệu của JSON </strong></summary>

```c
typedef enum {
JSON_NULL,
JSON_BOOLEAN,
JSON_NUMBER,
JSON_STRING,
JSON_ARRAY,
JSON_OBJECT
} JsonType;
```
    
  Trong JSON có thể chứa nhiều kiểu dữ liệu như **null, boolean, number, string, array, object** nên ta sử dụng enum để **tạo danh sách các kiểu dữ liệu nhằm phân loại và xử lý dữ liệu đúng cách** trong quá trình phân tích cú pháp (parse) hoặc truy xuất giá trị.

```c
typedef struct JsonValue {
    JsonType type;
    union {
        int boolean;
        double number;
        char *string;
        struct {
            struct JsonValue *values;
            size_t count;
        } array;
        struct {
            char **keys;
            struct JsonValue *values;
            size_t count;
        } object;
    } value;
} JsonValue;
```

Cấu trúc `JsonValue` sử dụng **struct** kết hợp với **union** để lưu trữ các **kiểu dữ liệu khác nhau trong cùng một vùng nhớ**, giúp tiết kiệm bộ nhớ. Biến `type` có kiểu `JsonType` dùng để xác định kiểu dữ liệu mà `value` đang lưu trữ.

Đối với **kiểu mảng** (`JSON_ARRAY`), các phần tử được lưu trong một mảng các `JsonValue`, cho phép mỗi phần tử có thể mang **kiểu dữ liệu khác nhau** (không giống như mảng thông thường chỉ chứa một kiểu dữ liệu cố định). Biến `count` lưu số lượng phần tử trong mảng.

Đối với **kiểu đối tượng** (`JSON_OBJECT`), dữ liệu được lưu dưới dạng các cặp key–value:

 +   `keys` là **mảng các chuỗi ký tự** (`char **`), luôn là kiểu chuỗi (string),

 +   `values` là mảng các `JsonValue`, cho phép lưu trữ các kiểu dữ liệu khác nhau cho từng key,

 +   `count` là số lượng cặp key–value trong đối tượng.

</details>






<details>
<summary><strong> Hàm bỏ qua khoảng trắng </strong></summary>

```c
static void skip_whitespace(const char **json) {
    while (isspace(**json)) {
        (*json)++;
    }
}
```

Hàm `skip_whitespace` dùng để bỏ qua các ký tự **khoảng trắng (whitespace)** trong chuỗi JSON đầu vào.
+ Tham số `const char **json` là con trỏ đến chuỗi JSON cần phân tích, cho phép hàm thay đổi vị trí con trỏ gốc từ hàm gọi.
+ Trong vòng lặp `while`, hàm `isspace(**json)` kiểm tra ký tự hiện tại có phải là ký tự trắng như `' '`, `'\t'`, `'\n'`, `'\r'` hay không.
+ Nếu có, con trỏ chuỗi sẽ được tăng lên `(*json)++`, tức là tiến sang ký tự kế tiếp.
+ Vòng lặp tiếp tục cho đến khi gặp ký tự không phải khoảng trắng, giúp loại bỏ tất cả ký tự trắng ở đầu chuỗi.
  
</details>


  



<details>
<summary><strong> Hàm phân tích NULL </strong></summary>

```c
JsonValue *parse_null(const char **json) {
    skip_whitespace(json);
    if (strncmp(*json, "null", 4) == 0) {
        JsonValue *value = (JsonValue *) malloc(sizeof(JsonValue));
        value->type = JSON_NULL;
        *json += 4;
        return value;
    }
    return NULL;
}
```

Hàm `parse_null` dùng để phân tích và xử lý giá trị null trong chuỗi JSON.

1.  `skip_whitespace(json)`: Gọi hàm phụ để bỏ qua các ký tự khoảng trắng ở đầu chuỗi, đảm bảo con trỏ trỏ đúng vào phần dữ liệu có ý nghĩa.
2.  `strncmp(*json, "null", 4) == 0`: So sánh 4 ký tự đầu tiên của chuỗi hiện tại với chuỗi "null". Nếu khớp, tức là gặp giá trị null trong JSON.
+  Khi đó khởi tạo đối tượng `JsonValue`, cấp phát bộ nhớ cho một `JsonValue` mới
+  Gán kiểu dữ liệu là `JSON_NULL`.
+  Sau đó di chuyển con trỏ chuỗi `*json` tiến 4 ký tự (bỏ qua chữ `"null"`).
+  Trả về đối tượng `JsonValue` đã khởi tạo.
3.  Nếu không khớp `"null"`, hàm trả về **NULL**, tức là không phải giá trị `null` hợp lệ tại vị trí đó.

</details>



<details>
<summary><strong> Hàm phân tích boolean </strong></summary>

```c
JsonValue *parse_boolean(const char **json) {
    skip_whitespace(json);
    JsonValue *value = (JsonValue *) malloc(sizeof(JsonValue));
    if (strncmp(*json, "true", 4) == 0) {
        value->type = JSON_BOOLEAN;
        value->value.boolean = true;
        *json += 4;
    } else if (strncmp(*json, "false", 5) == 0) {
        value->type = JSON_BOOLEAN;
        value->value.boolean = false;
        *json += 5;
    } else {
        free(value);
        return NULL;
    }
    return value;
}
```

Hàm `parse_boolean` dùng để phân tích và xử lý giá trị `boolean` (`true` hoặc `false`) trong chuỗi JSON.

1.  `skip_whitespace(json)`: Gọi hàm phụ để bỏ qua các ký tự khoảng trắng ở đầu chuỗi, đảm bảo con trỏ trỏ đúng vào phần dữ liệu có ý nghĩa.
2.   Cấp phát bộ nhớ cho `JsonValue` mới để lưu trữ giá trị boolean.
3.  So sánh với `"true"`, nếu chuỗi bắt đầu bằng `"true"`, gán type là `JSON_BOOLEAN` và `value.boolean = true`. Dịch con trỏ `*json` thêm 4 ký tự để vượt qua `"true"`.
4.  So sánh với `"false"`, nếu chuỗi bắt đầu bằng `"false"`, gán type là `JSON_BOOLEAN` và `value.boolean = false`. Dịch con trỏ JSON thêm 5 ký tự để vượt qua "false".
5.  Nếu không phải `"true"` hay `"false"` thì giải phóng bộ nhớ đã cấp phát và trả về `NULL` để báo lỗi phân tích cú pháp.
6.  Trả về con trỏ `JsonValue` đã được phân tích thành công.

</details>



<details>
<summary><strong> Hàm phân tích số liệu </strong></summary>

```c
JsonValue *parse_number(const char **json) {
    skip_whitespace(json);
    char *end;


    double num = strtod(*json, &end);
    if (end != *json) {
        JsonValue *value = (JsonValue *) malloc(sizeof(JsonValue));
        value->type = JSON_NUMBER;
        value->value.number = num;
        *json = end;
        return value;
    }
    return NULL;
}
```

Hàm `parse_number` dùng để **phân tích và xử lý số** trong chuỗi JSON, bao gồm số nguyên, số thực, và số mũ
1.  `skip_whitespace(json)`: Gọi hàm phụ để bỏ qua các ký tự khoảng trắng ở đầu chuỗi, đảm bảo con trỏ trỏ đúng vào phần dữ liệu có ý nghĩa
2.  `strtod` chuyển đổi chuỗi thành số thực (`double`) và lưu địa chỉ ký tự kế tiếp không phải là phần của số vào `end`
3.  `if (end != *json)`: Kiểm tra xem có số nào được phân tích không. Nếu `end == *json` có nghĩa là **không có ký tự nào trong chuỗi đầu vào** được `strtod` **chuyển đổi thành số thực hợp lệ**, khi đấy trả về `NULL`. Trong trường hợp đã chuyển đổi thành số thành công
+  Cấp phát động biến `value` để chứa thông tin dữ liệu được phân tích
+  Phân dữ liệu thành `JSON_NUMBER`
+  Lưu lại số thực đã được phân tích vào `value->value.number`
+  `*json = end` đưa con trỏ `json` về vị chỉ ngay sau số hợp lệ trong chuỗi, sẵn sàng cho việc phân tích sau này
+  Trả về `value` lưa trữ toàn bộ thông tin và giá trị được phân tích
 
</details>



<details>
<summary><strong> Hàm phân tích chuỗi </strong></summary>

```c
JsonValue *parse_string(const char **json) {
    skip_whitespace(json);


    if (**json == '\"') {
        (*json)++;
        const char *start = *json;
        while (**json != '\"' && **json != '\0') {
            (*json)++;
        }
        if (**json == '\"') {
            size_t length = *json - start; // 3
            char *str = (char *) malloc((length + 1) * sizeof(char));
            strncpy(str, start, length);
            str[length] = '\0';


            JsonValue *value = (JsonValue *) malloc(sizeof(JsonValue));
            value->type = JSON_STRING;
            value->value.string = str;
            (*json)++;
            return value;
        }
    }
    return NULL;
}
```

Hàm `parse_string` dùng để **phân tích chuỗi JSON** (kiểu "string")
1.  `skip_whitespace(json)`: Gọi hàm phụ để bỏ qua các ký tự khoảng trắng ở đầu chuỗi, đảm bảo con trỏ trỏ đúng vào phần dữ liệu có ý nghĩa
2.  `if (**json == '\"')`: Kiểm tra chuỗi có bắt đầu bằng dấu `"` hay không, nếu không thì trả về `NULL`, nếu có thì thực hiện các bước tiếp theo
3.  `const char *start = *json`: Ghi nhớ vị trí bắt đầu của chuỗi
4.  Thực hiện vòng lặp để lấy toàn bộ chuỗi có nghĩa và chỉ dừng khi trỏ đến dấu `"` hay đến cuối chuỗi `\0`
5.  Nếu như đã đến cuối chuỗi hợp lệ `if (**json == '\"')`
+  Tính độ dài chuỗi hợp lệ
+  Cấp phát bộ nhớ cho chuỗi đã được phân tích
+  Copy nội dung trong chuỗi hợp lệ vào bộ nhớ mới được tạo và viết kết thúc chuỗi `\0` vào cuối chuỗi
+  Cấp phát cho kiểu `JsonValue`, gán chuỗi đã được phân tích và kiểu dữ liệu
+  Cập nhập con trỏ để bỏ qua dấu `"` kết thúc và trả về biến giá trị và kiểu đã tạo



</details>





<details>
<summary><strong> Hàm phân tích mảng </strong></summary>

```c
JsonValue *parse_array(const char **json) {
    skip_whitespace(json);
    if (**json == '[') {
        (*json)++;
        skip_whitespace(json);

        JsonValue *array_value = (JsonValue *)malloc(sizeof(JsonValue));
        array_value->type = JSON_ARRAY;
        array_value->value.array.count = 0;
        array_value->value.array.values = NULL;

        /*
        double arr[2] = {};
        arr[0] = 30;
        arr[1] = 70;
        */

        while (**json != ']' && **json != '\0') {
            JsonValue *element = parse_json(json); // 70
            if (element) {
                array_value->value.array.count++;
                array_value->value.array.values = (JsonValue *)realloc(array_value->value.array.values, array_value->value.array.count * sizeof(JsonValue));
                array_value->value.array.values[array_value->value.array.count - 1] = *element;
                free(element);
            } else {
                break;
            }
            skip_whitespace(json);
            if (**json == ',') {
                (*json)++;
            }
        }
        if (**json == ']') {
            (*json)++;
            return array_value;
        } else {
            free_json_value(array_value);
            return NULL;
        }
    }
    return NULL;
}
```

Hàm `parse_array` dùng để **phân tích một mảng JSON**
1. `skip_whitespace(json)`: Gọi hàm phụ để bỏ qua các ký tự khoảng trắng ở đầu chuỗi, đảm bảo con trỏ trỏ đúng vào phần dữ liệu có ý nghĩa
2.  `if (**json == '[')`: Kiểm tra nếu bắt đầu bằng dấu `[` — tức là một mảng JSON, nếu không trả NULL. Nếu phát hiện bắt đầu mảng, dịch con trỏ khỏi `[` rồi sài hàm `skip_whitespace(json)`
3.  Cấp phát động cho mảng, gán kiểu dữ liệu `JSON_ARRAY`, do mảng trống nên `array_value->value.array.count = 0;` và `array_value->value.array.values = NULL;`
4.  Lặp qua từng phần tử trong mảng. Gọi đệ quy parse_json(json) để phân tích từng phần tử (có thể là số, chuỗi, object, mảng, v.v.).
5.
```
array_value->value.array.count++;
array_value->value.array.values = realloc(...);
```
Tăng số lượng phần tử trong mảng. Dùng `realloc` để mở rộng mảng giá trị (`values`).

6.  
```
array_value->value.array.values[count - 1] = *element;
free(element);
```
Sao chép nội dung `JsonValue` từ con trỏ tạm `element`. Sau đó giải phóng con trỏ `element`.

7.  Kết thúc mảng kiểm tra xem có dấu `]` kết thúc mảng không, nếu có trả về mảng được phân tích, nếu không giải phóng bộ nhớ vừa được cấp phát `array_value`


</details>






<details>
<summary><strong> Hàm phân tích đối tượng </strong></summary>

```c
JsonValue *parse_object(const char **json) {
    skip_whitespace(json);
    if (**json == '{') {
        (*json)++;
        skip_whitespace(json);

        JsonValue *object_value = (JsonValue *)malloc(sizeof(JsonValue));
        object_value->type = JSON_OBJECT;
        object_value->value.object.count = 0;
        object_value->value.object.keys = NULL;
        object_value->value.object.values = NULL;



        while (**json != '}' && **json != '\0') {
            JsonValue *key = parse_string(json);
            if (key) {
                skip_whitespace(json);
                if (**json == ':') {
                    (*json)++;
                    JsonValue *value = parse_json(json);
                    if (value) {
                        object_value->value.object.count++;
                        object_value->value.object.keys = (char **)realloc(object_value->value.object.keys, object_value->value.object.count * sizeof(char *));
                        object_value->value.object.keys[object_value->value.object.count - 1] = key->value.string;

                        object_value->value.object.values = (JsonValue *)realloc(object_value->value.object.values, object_value->value.object.count * sizeof(JsonValue));
                        object_value->value.object.values[object_value->value.object.count - 1] = *value;
                        free(value);
                    } else {
                        free_json_value(key);
                        break;
                    }
                } else {
                    free_json_value(key);
                    break;
                }
            } else {
                break;
            }
            skip_whitespace(json);
            if (**json == ',') {
                (*json)++;
            }
        }
        if (**json == '}') {
            (*json)++;
            return object_value;
        } else {
            free_json_value(object_value);
            return NULL;
        }
    }
    return NULL;
}
```

Hàm `parse_object` dùng để **phân tích một đối tượng JSON**
1. `skip_whitespace(json)`: Gọi hàm phụ để bỏ qua các ký tự khoảng trắng ở đầu chuỗi, đảm bảo con trỏ trỏ đúng vào phần dữ liệu có ý nghĩa
2.  `if (**json == '{')`: Kiểm tra nếu bắt đầu bằng dấu `{` — tức là một đối tượng JSON, nếu không trả NULL. Nếu phát hiện bắt đầu đối tượng, dịch con trỏ khỏi `{` rồi sài hàm `skip_whitespace(json)`
3.   Tạo đối tượng JSON kiểu `JSON_OBJECT`, và khởi tạo rỗng.
```
JsonValue *object_value = (JsonValue *)malloc(sizeof(JsonValue));
object_value->type = JSON_OBJECT;
object_value->value.object.count = 0;
object_value->value.object.keys = NULL;
object_value->value.object.values = NULL;
```
4.  Vòng lặp xử lý từng cặp key-value, chỉ thoát vòng lặp khi gặp `}` hay `\0` kết thúc chuỗi
+  `JsonValue *key = parse_string(json)`: JSON object chỉ chấp nhận key là chuỗi, nên sử dụng `parse_string`
+  `JsonValue *value = parse_json(json)`: value (ứng với key và ngăn cách với key bởi dấu `:`) có thể là bất kỳ kiểu nào (số, chuỗi, object, array,...), nên sử dụng `parse_json` để phân tích chung
5.  
```
object_value->value.object.count++;
object_value->value.object.keys = (char **)realloc(object_value->value.object.keys, object_value->value.object.count * sizeof(char *));
object_value->value.object.keys[object_value->value.object.count - 1] = key->value.string;
```
Tăng các cặp key-value thêm 1 (`object_value->value.object.count++`). Cấp phát động lại cho key rồi gán chuỗi phân tích được vào vị trí key tương ứng

6.
```
object_value->value.object.values = (JsonValue *)realloc(object_value->value.object.values, object_value->value.object.count * sizeof(JsonValue));
object_value->value.object.values[count - 1] = *value;
free(value);
```
Cấp phát động lại cho value rồi gán giá trị phân tích được vào vị trí value tương ứng rồi giải phóng biến `value` tạm

7. Trong trường hợp nếu có lỗi sảy ra như key không hợp lệ hay không có dấu kết thúc đối tượng `}` thì tùy trường hợp sẽ giải phóng key và toàn bộ đối tượng được cấp phát

 
</details>





<details>
<summary><strong> 000 </strong></summary>

</details>







</details>

</details>
