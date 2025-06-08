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
<summary><strong> 000 </strong></summary>


</details>




</details>

</details>
