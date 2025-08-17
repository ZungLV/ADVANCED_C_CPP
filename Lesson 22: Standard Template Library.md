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

Một số method của `list`:
+  `push_back()`: thêm node cuối list
+  `push_front()`: thêm node đầu list
+  `insert()`: thêm node vị trí bất kỳ
+  `pop_back()`: xóa node cuối list
+  `pop_front()`: xóa node đầu list
+  `erase()`: xóa node bất kỳ của list
+  `size()`: trả về kích thước của list
+  `begin()`: địa chỉ node đầu tiên
+  `end()`: sau địa chỉ node cuối cùng

Cú pháp khai báo của `list`:
```cpp
list<data_type> name;                // Tạo một list rỗng
list<data_type> name(1,2,3,4,5);     // Tạo list với các phần tử đã được chỉ định trước
```

Ví dụ minh họa:
```cpp
#include <iostream>
#include <list>
using namespace std;

void Display(list<int> lst)
{   
    // Sử iterator để duyệt qua từng node trong list
    // Duyệt xuôi
    list<int>::iterator it;
    // In ra list ban đầu
    int i = 0;
    for (it = lst.begin(); it != lst.end(); it++){
        cout << "node: " << i++ << ", value: " << *it << endl;
    }

    cout << "------------------\n";
}

void RDisplay(list<int> lst)
{   
    // Sử reverse iterator để duyệt qua từng node trong list
    // Duyệt ngược
    list<int>::reverse_iterator rit;
    // In ra list ban đầu
    int i = 0;
    for (rit = lst.rbegin(); rit != lst.rend(); rit++){
        cout << "node: " << i++ << ", value: " << *rit << endl;
    }

    cout << "------------------\n";
}

int main(int argc, char const *argv[])
{
    list<int> lst{10,10,100};   // Tạo list với 3 node đầu tiên
    Display(lst);

    // Thêm node ở cuối list
    lst.push_back(1);
    lst.push_back(3);
    Display(lst);

    // Thêm node ở đầu list
    lst.push_front(2);
    lst.push_front(4);

    // Duyệt xuôi từng phần tử từ đầu list đến cuối list
    cout << "Duyệt xuôi: " << endl;
    Display(lst);
    // Duyệt ngược từng phần tử từ cuối list đến đầu list
    cout << "Duyệt ngược: " << endl;
    RDisplay(lst);
    

    // Sử dụng size để xác định số lượng node
    cout << "Số lượng node trong list: " << lst.size() << endl;

    return 0;
}
```

```
node: 0, value: 10
node: 1, value: 10
node: 2, value: 100
------------------
node: 0, value: 10
node: 1, value: 10
node: 2, value: 100
node: 3, value: 1
node: 4, value: 3
------------------
Duyệt xuôi:
node: 0, value: 4
node: 1, value: 2
node: 2, value: 10
node: 3, value: 10
node: 4, value: 100
node: 5, value: 1
node: 6, value: 3
------------------
Duyệt ngược:
node: 0, value: 3
node: 1, value: 1
node: 2, value: 100
node: 3, value: 10
node: 4, value: 10
node: 5, value: 2
node: 6, value: 4
------------------
Số lượng node trong list: 7
```

`insert()` và `erase()` là các hàm thêm và xóa node vào vị trí mong muốn trong list, để có thể sử dụng `insert()` và `erase()` thì cần phải xác định được đúng vị trí mà cần thêm. Do đó `insert()` và `erase()` thường dùng chung với thao tác duyệt danh sách.
```cpp
#include <iostream>
#include <list>
using namespace std;

// Hàm thêm node vào vị trí bất kì trong list
void insert_lst(list<int>& lst, int node_num, int value)
{
    // Sử iterator để duyệt qua từng node trong list
    list<int>::iterator it;
    // Duyệt rồi thêm vào
    int i = 0;
    for (it = lst.begin(); it != lst.end(); it++){
        if(i == node_num)
        {
            lst.insert(it,value);   // insert vào node mà it trỏ vào với giá trị là value
            return;
        }
        i++;
    }
}

// Hàm xóa node ở vị trí bất kì trong list
void erase_lst(list<int>& lst, int node_num)
{
    // Sử iterator để duyệt qua từng node trong list
    list<int>::iterator it;
    // Duyệt rồi thêm vào
    int i = 0;
    for (it = lst.begin(); it != lst.end(); it++){
        if(i == node_num)
        {
            lst.erase(it);   // erase node mà it trỏ vào với giá trị là value
            return;
        }
        i++;
    }
}

void Display(list<int> lst)
{   
    // Sử iterator để duyệt qua từng node trong list
    // Duyệt xuôi
    list<int>::iterator it;
    // In ra list ban đầu
    int i = 0;
    for (it = lst.begin(); it != lst.end(); it++){
        cout << "node: " << i++ << ", value: " << *it << endl;
    }

    cout << "------------------\n";
}

void RDisplay(list<int> lst)
{   
    // Sử reverse iterator để duyệt qua từng node trong list
    // Duyệt ngược
    list<int>::reverse_iterator rit;
    // In ra list ban đầu
    int i = 0;
    for (rit = lst.rbegin(); rit != lst.rend(); rit++){
        cout << "node: " << i++ << ", value: " << *rit << endl;
    }

    cout << "------------------\n";
}

int main(int argc, char const *argv[])
{
    list<int> lst{10,10,100};   // Tạo list với 3 node đầu tiên
    Display(lst);

    // Thêm node vào vị trí thứ 2
    insert_lst(lst, 2, 12);
    Display(lst);
    // Thêm node vào vị trí thứ 3
    erase_lst(lst, 3);
    Display(lst);

    // Sử dụng size để xác định số lượng node
    cout << "Số lượng node trong list: " << lst.size() << endl;

    return 0;
}
```

```
node: 0, value: 10
node: 1, value: 10
node: 2, value: 100
------------------
node: 0, value: 10
node: 1, value: 10
node: 2, value: 12
node: 3, value: 100
------------------
node: 0, value: 10
node: 1, value: 10
node: 2, value: 12
------------------
```

</details> 


<details>
  <summary><strong> map </strong></summary>

Map là một container trong STL của C++, cung cấp một cấu trúc dữ liệu ánh xạ **key-value** (tương tự JSON).

Mỗi phần tử trong `std::map` là một `std::pair<const Key, T>`:
+  **Key** là hằng số (không thể thay đổi sau khi thêm vào **map**).
+  **T** là kiểu dữ liệu của giá trị (**value**).

Đặc điểm chính:
+  Các phần tử được tự động sắp xếp theo thứ tự tăng dần theo key
+  Mỗi key chỉ xuất hiện một lần duy nhất.
+  Key không thể thay đổi sau khi được thêm vào map

Các hàm phổ biến:
+  `map[key] = value`: Chèn hoặc cập nhập
+  `map.at(key)`:  Truy cập an toàn, ném exception nếu không có
+  `map.insert({key, value})`:  Chèn nếu chưa có key
+  `map.find(key)`:  Trả về iterator hoặc `map.end()` nếu không thấy
+  `map.erase(key)`:  Xóa phần tử theo key
+  `map.clear()`:  Xóa toàn bộ
+  `map.size()`:  Trả về số phần tử
+  `map.empty()`:  Kiểm tra rỗng

Các cách khai báo map:
+  Cách 1: Khai báo nhiều cặp key-value cùng lúc:
```cpp
map<int, string> m = 
{
    {1, "Chó"},
    {2, "Mèo"}
};
```
+  Cách 2: Khai báo từng cặp key-value:
```cpp
map<int, string> m;
m[3] = "Chuột";
```
Các cách để duyệt dữ liệu trong `map`:
+  Cách 1: Duyệt các phần tử bằng `auto` và `item`:
```cpp
for(const auto &item : m)
{
    cout << "key: "     <<  item.first
         << " value: "   <<  item.second << endl;
}   
```
+  Cách 2: Duyệt các phần tử bằng hai biến đại diện cho **key-value**:
```cpp
for(const auto &[k,v] : m)
{
    cout << "key: "     <<  k
        << " value: "   <<  v << endl;
}   
```

Code minh họa:
```cpp
#include <iostream>
#include <map>
using namespace std;


int main(int argc, char const *argv[])
{
    map<int, string> m = 
    {
        {10, "Chó"},
        {1, "Mèo"}
    };

    m[5] ="Chuột";

    // Cách 1
    for(const auto &item : m)
    {
        cout << "key: "     <<  item.first
             << " value: "   <<  item.second << endl;
    }   

    // Cách 2
    for(const auto &[k,v] : m)
    {
        cout << "key: "     <<  k
            << " value: "   <<  v << endl;
    }   

    return 0;
}
```
```
key: 1 value: Mèo
key: 5 value: Chuột
key: 10 value: Chó
key: 1 value: Mèo
key: 5 value: Chuột
key: 10 value: Chó
```
Mặc dù `key` được khai báo theo thứ tự `10`, `1`, `5` nhưng khi in ra sẽ được sắp xếp theo thứ tự từ bé đến lớn theo giá trị `key`

</details>





</details>








</details>
