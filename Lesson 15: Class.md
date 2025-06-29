<details>
  <summary><strong> CLASS </strong></summary>



  

<details>
  <summary><strong> Giới thiệu chung </strong></summary>
Trong C++, từ khóa class được sử dụng để định nghĩa một lớp, là một cấu trúc dữ liệu tự định nghĩa có thể chứa dữ liệu và các hàm thành viên liên quan. Một lớp đại diện cho một mô hình hoặc đối tượng trong thế giới thực, giúp lập trình viên tổ chức và quản lý mã hiệu quả hơn thông qua nguyên lý hướng đối tượng.

Lớp là nền tảng của lập trình hướng đối tượng (OOP) trong C++. Thông qua lớp, các đặc điểm chính của OOP như đóng gói (**encapsulation**), kế thừa (**inheritance**), đa hình (**polymorphism**) và trừu tượng hóa (**abstraction**) được hiện thực hóa một cách rõ ràng.

![image](https://github.com/user-attachments/assets/ed249376-9971-48bf-9ec2-4f5a625c5053)

</details>




<details>
  <summary><strong> Phạm vi truy cập </strong></summary>

Phạm vi truy cập trong class là cách quy định mức độ truy cập của các thành viên (biến và hàm) trong một lớp, nhằm kiểm soát việc sử dụng và bảo vệ dữ liệu bên trong lớp khỏi truy cập không mong muốn. C++ cung cấp ba phạm vi truy cập chính:
+ public  (công khai)
+ private  (riêng tư)
+ protected  (được bảo vệ)

<details>
  <summary><strong> Phạm vi truy cập - public </strong></summary>
Các thành viên được khai báo public có thể được truy cập từ bất kỳ đâu trong chương trình, bao gồm cả bên ngoài lớp.

Thường được sử dụng cho các phương thức (hàm thành viên) mà người dùng lớp cần gọi.

Ta có chương trình như sau

```cpp
#include <iostream>
using namespace std;

class HinhChuNhat
{
    double chieuDai;    // property
    double chieuRong;   // property
};

int main()
{
    HinhChuNhat hinh1;
    hinh1.chieuDai = 10.0;
    hinh1.chieuRong = 5.0;
    cout << "Chieu dai: " << hinh1.chieuDai<< endl;
    return 0;
}
```

Nếu không có khai báo `public` thì khi ta cố truy cập các thuộc tính (biến) `chieuDai` và `chieuRong` thì chương trình sẽ báo lỗi

```
member "HinhChuNhat::chieuDai" (declared at line 6) is inaccessibleC/C++(265)
member "HinhChuNhat::chieuRong" (declared at line 7) is inaccessibleC/C++(265)
```

Điều đó có nghĩa là hai thuộc tính này không thể truy cập bên ngoài class được, đối với các thuộc tính (biến) và phương thức (hàm) mà ta muốn truy cập ở bên ngoài class ta sẽ cần sử dụng `public`

```cpp
#include <iostream>
using namespace std;

class HinhChuNhat
{
    public:
        double chieuDai;    // property
        double chieuRong;   // property
};

int main()
{
    HinhChuNhat hinh1;
    hinh1.chieuDai = 10.0;
    hinh1.chieuRong = 5.0;
    cout << "Chieu dai: " << hinh1.chieuDai<< endl;
    return 0;
}

```

```
Chieu dai: 10
```
</details>


<details>
  <summary><strong> Phạm vi truy cập - private </strong></summary>
Đây là mức truy cập mặc định nếu không khai báo rõ ràng.

Các thành viên private chỉ có thể được truy cập từ bên trong chính lớp đó.

Dữ liệu nhạy cảm hoặc nội bộ thường được khai báo là private để bảo vệ và chỉ được truy cập thông qua các phương thức công khai.

```cpp
#include <iostream>
using namespace std;

class HinhChuNhat
{
    private:
        double chieuDai = 5;
        double chieuRong = 10;
   
    public:

        // Hàm tính diện tích
        double tinhDienTich()
        {
            return chieuDai * chieuRong;
        }
};

int main()
{
    HinhChuNhat hinh1;
    cout << "Dien tich: " << hinh1.tinhDienTich() << '\n';
    return 0;
}

```

Hai thuộc tính trong class `HinhChuNhat` là `chieuDai` và `chieuRong` có phạm vi truy cập là `private`, điều đó có nghĩa là hai thuộc tính này không thể truy cập bên ngoài `class` được. Do đó ta sẽ truy cập chúng thông qua một phương thức có phạm vi truy cập là `public` ở đây là `tinhDienTich` và ta sẽ được kết quả như sau.

```
Dien tich: 50
```
</details>


<details>
  <summary><strong> Phạm vi truy cập - protected </strong></summary>

</details>



</details>



<details>
  <summary><strong> Constructor </strong></summary>

Constructor trong C++ là một phương thức đặc biệt của lớp, được tự động gọi khi một đối tượng (object) của lớp đó được tạo ra. Mục đích chính của constructor là khởi tạo giá trị ban đầu cho các thành viên dữ liệu của lớp.

Đặc điểm của constructor:
+ Có tên trùng với tên của lớp.

+ Không có kiểu trả về, kể cả `void`.

Ta có chương trình sau:

```cpp
#include <iostream>
using namespace std;

class HinhChuNhat
{
    public:
        double chieuDai;
        double chieuRong;
   
        HinhChuNhat(int _chieuDai = 10, int _chieuRong = 5)
        {
            chieuDai  = _chieuDai;
            chieuRong = _chieuRong;
        }

        // Hàm tính diện tích
        double tinhDienTich()
        {
            return chieuDai * chieuRong;
        }
};

int main()
{
    HinhChuNhat hinh1;
    cout << "Chieu dai: " << hinh1.chieuDai << '\n';
    cout << "Chieu rong: " << hinh1.chieuRong << '\n';
    cout << "Dien tich: " << hinh1.tinhDienTich() << '\n';
    return 0;
}
```

Ta con constructor được viết như sau

```cpp
        HinhChuNhat(int _chieuDai = 10, int _chieuRong = 5)
        {
            chieuDai  = _chieuDai;
            chieuRong = _chieuRong;
        }
```

Constructor được dùng để gán giá trị các thuộc tính và bản thân constructor cũng có tham số. Nếu ta gọi class mà không thay đổi tham số thì các thuộc tính sẽ mặc định sẽ được gán theo khai báo trong constructor 
`int _chieuDai = 10, int _chieuRong = 5`

Khi chạy chương trình:

```
hieu dai: 10
Chieu rong: 5
Dien tich: 50
```

Giờ ta thử thay đổi các tham số khi gọi class:

```cpp
#include <iostream>
using namespace std;

class HinhChuNhat
{
    public:
        double chieuDai;
        double chieuRong;
   
        HinhChuNhat(int _chieuDai = 10, int _chieuRong = 5)
        {
            chieuDai  = _chieuDai;
            chieuRong = _chieuRong;
        }

        // Hàm tính diện tích
        double tinhDienTich()
        {
            return chieuDai * chieuRong;
        }
};

int main()
{
    HinhChuNhat hinh1(2,8);
    cout << "Chieu dai: " << hinh1.chieuDai << '\n';
    cout << "Chieu rong: " << hinh1.chieuRong << '\n';
    cout << "Dien tich: " << hinh1.tinhDienTich() << '\n';
    return 0;
}
```

Ta sẽ được kết quả tương ứng với những gì ta viết khi gọi class

```
Chieu dai: 2
Chieu rong: 8
Dien tich: 16
```

</details>




</details>
