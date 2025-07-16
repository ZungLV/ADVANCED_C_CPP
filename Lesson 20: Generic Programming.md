<details>
  <summary><strong> Generic Programming </strong></summary>




<details>
  <summary><strong> Giới thiệu chung </strong></summary>

**Generic Programming** (Lập trình tổng quát) là một phương pháp lập trình sử dụng **các tham số kiểu dữ liệu** (type parameter) để viết mã có thể tái sử dụng và hoạt động với nhiều kiểu dữ liệu khác nhau. Kỹ thuật này giúp loại bỏ sự trùng lặp code và tăng tính linh hoạt trong thiết kế phần mềm.

Lập trình tổng quát thường được áp dụng trong các ngôn ngữ hỗ trợ Generics (như Java, Rust) hoặc Templates (C++).

C++ sử dụng Templates để triển khai Generic Programming. Templates có hai loại:
+  Function Templates (Hàm tổng quát)
+  Class Templates (Lớp tổng quát)

</details>




<details>
  <summary><strong> Function Template </strong></summary>

Trong C++, Templates giúp viết hàm có thể làm việc với **nhiều kiểu dữ liệu** mà **không cần overload** nhiều lần.

Template chỉ áp dụng cho một định nghĩa cụ thể của hàm, không áp dụng cho tất cả các hàm.

Cú pháp:
```cpp
template <typename T>
T func(T a, T b){}

template <typename T1, typename T2, typename T3>
T1 func(T1 a, T2 b, T3 c){}
```

Trong cú pháp trên:
+  `T`: là kiểu dữ liệu chung cho cả hàm, nếu tham số truyền vào là (int, double, char...) thì toàn bộ các tham số và kiểu dữ liệu trả về cũng là kiểu tương ứng.
+  `T1`, `T2`, `T3`: là các kiểu dữ liệu khác nhau, kiểu dữ liệu trả về của hàm sẽ phụ thuộc vào kiểu của tham số đầu tiên truyền vào `T1`. 

Ví dụ:

```cpp
#include <iostream>
using namespace std;

template <typename T>
T maxValue(T a, T b)
{
    return (a > b) ? a : b;
}

int main()
{
    cout << "Max(5, 10) = " << maxValue(5, 10) << endl;
    cout << "Max(3.5, 2.8) = " << maxValue(3.5, 2.8) << endl;
    cout << "Max('A', 'B') = " << maxValue('A', 'B') << endl;
    return 0;
}
```
```
Max(5, 10) = 10
Max(3.5, 2.8) = 3.5
Max('A', 'B') = B
```

Như vậy các tham số kiểu `int` truyền vào thì hàm sẽ trả về kiểu `int`, tương tự đối với kiểu `double` và `char`. Giờ thay vì truyền vào 2 tham số cũng kiểu ta thử truyền vào 2 tham số khác kiểu:

```cpp
int main()
{
    cout << "Max(5, 7.5) = " << maxValue(5, 7.5) << endl;
    return 0;
}
```

```
error: no matching function for call to 'maxValue(int, double)'
```

Do `template` được tạo là 2 tham số cùng kiểu dữ liệu nên khi truyền vào hàm hai tham số khác nhau khiến chương trình không thể tìm thấy hàm phù hợp với yêu cầu, dẫn đến lỗi.

Một ví dụ khác:

```cpp

```

</details>



<details>
  <summary><strong> Class Template </strong></summary>

Class templates trong C++ là một khái niệm tương tự như function templates, nhưng được áp dụng cho class thay vì function. Class templates cho phép tạo các class có thể làm việc với nhiều kiểu dữ liệu mà không cần viết lại code.

Template chỉ áp dụng cho một định nghĩa cụ thể của class, không áp dụng cho tất cả các class.

Cú pháp:

```cpp
template <typename T>
class <name_of_class>
{
    private:
        T var;
}
```
Ví dụ minh họa:

```cpp
#include <iostream>
using namespace std;

template <typename T>
class Sensor{
    private:
        T value;
    public:
        Sensor(T init): value(init){}
        void display(){ cout << "Gia tri cam bien: " << value << endl; }
};

int main(int argc, char const *argv[]){
    Sensor tempSensor(36.5);
    tempSensor.display();

    Sensor lightSensor(512);
    lightSensor.display();

    Sensor stateSensor("OFF");
    stateSensor.display();
    return 0;
}
```

```
Gia tri cam bien: 36.5
Gia tri cam bien: 512
Gia tri cam bien: OFF
```

Kiểu dữ liệu của `value` sẽ phụ thuộc vào kiểu dữ liệu của tham số truyền vào constructor `Sensor`

Trong một số trường hợp ta còn có thể ép kiểu cho class để có được kiểu dữ liệu mong muốn bằng cách thêm `<kiểu dữ liệu>` trước tên class

Ví dụ trong hàm main:
```cpp
int main(int argc, char const *argv[]){
    Sensor<int> tempSensor(36.5);
    tempSensor.display();

    return 0;
```
```
Gia tri cam bien: 36
```
Mặc dù truyền vào constructor là kiểu `double` nhưng khi nhận về `value` là kiểu `int` đã được ép kiểu

</details>




</details>

