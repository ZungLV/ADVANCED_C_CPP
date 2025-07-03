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
  
`protected` là một mức độ truy cập trung gian giữa private và public:

+  Giống như private: Các thành viên protected không thể truy cập từ bên ngoài class (tức là không thể truy cập qua đối tượng).

+  Giống như public: Các thành viên protected có thể được truy cập từ các lớp dẫn xuất (lớp con).

`protected` thường được dùng khi không muốn người dùng bên ngoài lớp truy cập trực tiếp, nhưng muốn cho phép các lớp con kế thừa sử dụng hoặc sửa đổi thuộc tính hoặc phương thức đó.

Chương trình mẫu:

```c
#include <iostream>
using namespace std;

class HinhChuNhat
{
    protected:
        double chieuDai = 5;
        double chieuRong = 10;
   
    public:
        // Hàm tính diện tích
        double tinhDienTich()
        {
            return chieuDai * chieuRong;
        }
};


class Display : public HinhChuNhat
{
    public:
        void display()
        {
            cout << "Chieu dai: " << chieuDai << "\n";
            cout << "Chieu rong: " << chieuRong <<"\n";
        }

};


int main()
{
    Display hinh1;
    hinh1.display();
    //cout << "Chieu dai: " << hinh1.chieuDai << "\n";      //  Không thể chạy do đây là protected
    //cout << "Chieu rong: " << hinh1.chieuRong <<"\n";     //  Không thể chạy do đây là protected
    return 0;
}
```

Kết quả đạt được

```
Chieu dai: 5
Chieu rong: 10
```

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

Ta có constructor được viết như sau

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
Chieu dai: 10
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



<details>
  <summary><strong> Destructor </strong></summary>
  
Destructor trong C++ là một phương thức đặc biệt của lớp, được **tự động gọi khi đối tượng bị hủy** – tức là khi nó thoát khỏi phạm vi hoạt động  hoặc được giải phóng.

Đặc điểm của destructor:

+  Có tên trùng với tên lớp, nhưng có thêm dấu ~ ở đầu.

+  Không có tham số và không có kiểu trả về.

+  Mỗi lớp chỉ có duy nhất một destructor, không thể nạp chồng.

+  Thường được sử dụng để xóa tất cả dữ liệu

+  Tự động gọi trước khi đối tượng được thu hồi.

Ta có chương trình sau:

```c
#include <iostream>
using namespace std;

class HinhChuNhat
{
    public:
        double chieuDai;
        double chieuRong;

        HinhChuNhat()
        {
            cout << "Constructor " << '\n';      
            chieuDai = 10;
            chieuRong = 9;
            display();
        }

        ~HinhChuNhat()
        {
            cout << "#####################" << '\n';
            cout << "Destructor " << '\n';
            chieuDai = 0;
            chieuRong = 0;
            display();
        }

        // Hàm tính diện tích
        double tinhDienTich()
        {
            return chieuDai * chieuRong;
        }

        void display()
        {   
            cout << "Chieu dai: " << chieuDai << '\n';
            cout << "Chieu rong: " << chieuRong << '\n';
            cout << "Dien tich: " << tinhDienTich() << '\n';
        }
};

int main()
{
    HinhChuNhat hinh1;
    return 0;
}
```

Chạy chương trình ta được:

```
Constructor 
Chieu dai: 10
Chieu rong: 9
Dien tich: 90
#####################
Destructor 
Chieu dai: 0
Chieu rong: 0
Dien tich: 0
```

Có thể thấy ban đầu constructor sẽ được gọi đầu tiên để khởi tạo các thuộc tính trong `class`. Khi kết thúc hàm `main` đồng nghĩa với kết thúc hoạt động `class` thì destructor sẽ được gọi để xóa hết các thuộc tính.
</details>



<details>
  <summary><strong> Static </strong></summary>


<details>
  <summary><strong> Static property </strong></summary>
  
Khi một thuộc tính (property) trong lớp được khai báo với từ khóa `static`, thì thuộc tính đó không thuộc về bất kỳ đối tượng cụ thể nào, mà **thuộc về chính lớp đó**. Điều này có nghĩa là tất cả các đối tượng của lớp sẽ **dùng chung một bản sao duy nhất** của thuộc tính này – tức là **dùng chung địa chỉ** trong bộ nhớ.

Đặc điểm của static property:

+  Được chia sẻ bởi tất cả các object của lớp.

+  Có thể được truy cập mà không cần tạo đối tượng, thông qua cú pháp ClassName::property.

+  Thường được dùng cho các biến đếm số lượng đối tượng, giá trị cấu hình chung, hoặc các hằng số dùng chung.

+  Bắt buộc phải khởi tạo toàn cục.

Ví dụ ta có một chương trình như sau:

```cpp
#include <iostream>
using namespace std;

class HinhChuNhat
{
    public:
        double chieuDai;
        double chieuRong;
        static int var;
};
   
int HinhChuNhat::var;

int main()
{
    HinhChuNhat hinh1;
    HinhChuNhat hinh2;
    HinhChuNhat hinh3;

    cout << "address of chieu dai: " << &hinh1.chieuDai << '\n';
    cout << "address of chieu dai: " << &hinh2.chieuDai << '\n';
    cout << "address of chieu dai: " << &hinh3.chieuDai << '\n';

    cout << "address of var: " << &hinh1.var << '\n';
    cout << "address of var: " << &hinh2.var << '\n';
    cout << "address of var: " << &hinh3.var << '\n';
    return 0;
}
```

Ta có ba thuộc tính `chieuDai`, `chieuRong`, `var` trong đó thuộc tính `var` là static property. Ta muốn thử kiểm tra địa chỉ của thuộc tính thường và static property, khi chạy chương trình ta được.

```cpp
address of chieu dai: 0x4c9a7ffca0
address of chieu dai: 0x4c9a7ffc90
address of chieu dai: 0x4c9a7ffc80
address of var: 0x7ff78f877030
address of var: 0x7ff78f877030
address of var: 0x7ff78f877030
```

Vậy mặc dù được gọi ở ba đối tượng khác nhau nhưng thuộc tính `var` vẫn chỉ có một địa chỉ.

</details>


<details>
  <summary><strong> Static method </strong></summary>
  
Khi một phương thức (method) trong lớp được khai báo với từ khóa `static`, phương thức đó có các đặc điểm sau:

Đặc điểm của static method:

+  Độc lập với đối tượng (object): static method không hoạt động trên một đối tượng cụ thể, tức là không có con trỏ this. Vì vậy, không thể truy cập trực tiếp các thuộc tính hay phương thức không-static trong class từ một static method.
+  Không cần tạo object để gọi: có thể gọi static method ngay cả khi chưa có bất kỳ đối tượng nào của lớp được tạo ra.
+  Truy cập thông qua tên lớp và toán tử ::  `ClassName::methodName();`
+  Chỉ có thể truy cập các thành phần static khác: static method có thể gọi các static method khác hoặc truy cập các static property bên trong cùng lớp hoặc từ lớp khác.

Ta có chương trình minh họa:

```cpp
#include <iostream>
using namespace std;

class HinhChuNhat
{
    public:
        double chieuDai;
        double chieuRong;
        static int var;
        static void display()
        {
            cout << "Day la static method va ta co var: " << var << '\n';
        }
};
   
int HinhChuNhat::var = 100;

int main()
{
    HinhChuNhat::display();
    return 0;
}
```

Như vậy mặc dù không khởi tạo bất kì đối tượng nào ta vẫn có thể gọi trực tiếp static method (phương thức `display`) mà phương thức này chỉ có thể truy cập duy nhất một thuộc tính `var` (static property).

```
Day la static method va ta co var: 100
```

</details>


</details>





<details>
  <summary><strong> Khai báo Class trong file header </strong></summary>

Đối với file header chúng ta khai báo như bình thường trong C, các phương thức trong `class` chúng ta chỉ khai báo tên chứ không viết chi tiết ra

```cpp
#include <iostream>
#include <string>

using namespace std;

class SinhVien
{
    public:
        int ID;         // property
        string name;    // property
        string lop;     // property
        void display(); // method
};
```

Đối với file source thì chúng ta sẽ khai báo chi tiết các hàm trong `class` bằng cú pháp 

`kiểu_trả_về  tên_class :: tên_hàm`

```cpp
#include <iostream>
#include <string>
#include “main.hpp”

using namespace std;

void SinhVien::display()
{
    cout << "MSSV: " << ID << endl;
    cout << "TEN: " << name << endl;
    cout << "LOP: " << lop << endl;
}

int main(int argc, char const *argv[])
{
    SinhVien sv; // sv được gọi là object
    sv.ID = 2010117;
    sv.name = "Anh";
    sv.lop = "DD20TD1";
    sv.display();  
    return 0;
}
```

</details>





</details>
