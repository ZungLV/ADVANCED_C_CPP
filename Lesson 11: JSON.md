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

 +   `keys` là mảng các chuỗi ký tự (`char **`), luôn là kiểu chuỗi (string),

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
1.  `skip_whitespace(json)`: Gọi hàm phụ để bỏ qua các ký tự khoảng trắng ở đầu chuỗi, đảm bảo con trỏ trỏ đúng vào phần dữ liệu có ý nghĩa.
2.  `strtod` chuyển đổi chuỗi thành số thực (`double`) và lưu địa chỉ ký tự kế tiếp không phải là phần của số vào `end`.


</details>



<details>
<summary><strong> 000 </strong></summary>


</details>





</details>

</details>
