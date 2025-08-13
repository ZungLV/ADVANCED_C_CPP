<details>
  <summary><strong> Standard Template Library </strong></summary>




<details>
  <summary><strong> Giới thiệu </strong></summary>

Standard Template Library (STL) là một tập hợp các thư viện thiết kế để hỗ trợ lập trình tổng quát (generic programming).

STL C++ cung cấp một tập hợp các **template classes** và **functions** để thực hiện nhiều loại **cấu trúc dữ liệu** và các **thuật toán phổ biến**.

STL đã trở thành một phần quan trọng của ngôn ngữ C++ và làm cho việc lập trình trở nên mạnh mẽ, linh hoạt và hiệu quả.

Một số thành phần chính của STL:
+  Containers (Cấu trúc dữ liệu)
+  Iterators (Bộ lặp)
+  Algorithms (Thuật toán)
+  Functors & Lambda

</details>



<details>
  <summary><strong> Container </strong></summary>

Một container là một cấu trúc dữ liệu chứa nhiều phần tử theo một cách cụ thể.

STL cung cấp một số container tiêu biểu giúp lưu trữ và quản lý dữ liệu như:

+  vector
+  list
+  map
+  array
+  stack
+  queue
+  deque


<details>
  <summary><strong> vector </strong></summary>

`std::vector` là một mảng động (dynamic array) trong C++. Nó tự động quản lý bộ nhớ, có thể tăng kích thước khi thêm phần tử mới, và cho phép truy cập ngẫu nhiên như mảng thông thường.

Một số method của vector:
+  `at()`: truy cập để đọc hoặc thay đổi giá trị phần tử của vector.
+  `size()`: trả về kích thước của vector.
+  `resize()`: thay đổi kích thước của vector.
+  `begin()`: trả về một iterator trỏ đến địa chỉ phần tử đầu tiên của vector.
+  `end()`: trả về một iterator trỏ đến địa chỉ sau phần tử cuối cùng của vector.
+  `push_back()`: thêm phần tử vào vị trí cuối của vector.
+  `pop_back()`: xóa phần tử ở vị trí cuối của vector.
+  `insert()`: thêm phần tử vào vị trí bất kỳ.
+  `erase()`: xóa phần tử ở vị trí bất kỳ hoặc xóa các phần tử trong phạm vi được chỉ định.
+  `clear()`: xóa toàn bộ phần tử của vector.

Cú pháp khai báo:
```cpp
vector<data_type> name;  // vector rỗng

vector<data_type> name(size);  // size là số lượng phần tử khởi tạo và giá trị khởi tạo mặc định là 0

vector<data_type> name(size, value);  // value: giá trị khởi tạo cho các phần tử các phần tử sẽ đều có giá trị là value

vector<data_type> name = {1, 2, 3, 4, 5};  // gán sẵn cho các phần tử giá trị riêng tương ứng
```

Chúng ta còn có cách duyệt các phần tử trong vector như sau:
+ Cách 1: Duyệt phần tử bằng cách lặp theo số lượng các phần tử của vector.
```cpp
    for (int i = 0; i < v.size(); ++i)
        cout << v[i] << " ";
```
+ Cách 2: Đối với **C++11** trở đi có thể sử dụng **range-based for loop** tức là cho phép duyệt qua từng phần tử của container (như `vector`, `array`, `map`, …) một cách gọn gàng hơn, không cần viết chỉ số dài dòng nữa.
```cpp
    for (int x : v)   // range-based for loop - C++11
        cout << x << " ";
``` 
+ Cách 3: Duyệt bằng con trỏ bắt đầu bằng trỏ từ phần tử đầu tiên và duyệt đến khi trỏ ngay sau phần tử cuối cùng của `vector`.
```cpp
    for (auto it = v.begin(); it != v.end(); ++it)
        cout << *it << " ";
```

Code kết hợp để xem cách hoạt động:
```cpp
#include <iostream>
#include <vector>
using namespace std;


int main()
{  

vector<int> v1;  // vector rỗng


vector<int> v2(5);  // size là số lượng phần tử khởi tạo và giá trị khởi tạo mặc định là 0
for (int x : v2)   // range-based for loop - C++11
    cout << x << " ";
cout << endl;


vector<int> v3(5, 17);  // value: giá trị khởi tạo cho các phần tử
for (auto it = v3.begin(); it != v3.end(); ++it)
    cout << *it << " ";
cout << endl;


vector<int> v4 = {1, 2, 3, 4, 5};
for (int i = 0; i < v4.size(); ++i)
    cout << v4[i] << " ";
cout << endl;
}
```
Kết quả:
```
0 0 0 0 0 
17 17 17 17 17
1 2 3 4 5
```

</details> 




<details>
  <summary><strong> list </strong></summary>

List là một container trong STL của C++, triển khai dưới dạng danh sách liên kết hai chiều.

Một số đặc điểm quan trọng của list:

+  Truy cập tuần tự: Truy cập các phần tử của list chỉ có thể thực hiện tuần tự, không hỗ trợ truy cập ngẫu nhiên.
+  Hiệu suất chèn và xóa: Chèn và xóa ở bất kỳ vị trí nào trong danh sách có hiệu suất tốt hơn so với vector. Điều này đặc biệt đúng khi thêm/xóa ở giữa danh sách.

<img width="739" height="176" alt="image" src="https://github.com/user-attachments/assets/1363f9ae-eec8-4ec0-a4de-e51759883133" />

Single Linked List: **duyệt 1 chiều** (từ node đầu → node cuối)

Doubly Linked List: 
+  duyệt xuôi: từ node đầu → node cuối: con trỏ **next**
+  duyệt ngược: từ node cuối → node đầu: con trỏ **prev**

Một số method của list:
+  `push_back()`: thêm node cuối list
+  `push_front()`: thêm node đầu list
+  `insert()`: thêm node vị trí bất kỳ
+  `pop_back()`: xóa node cuối list
+  `pop_front()`: xóa node đầu list
+  `erase()`: xóa node bất kỳ của list
+  `size()`: trả về kích thước của list
+  `begin()`: địa chỉ node đầu tiên
+  `end()`: sau địa chỉ node cuối cùng

</details> 








</details>








</details>
