<details>
  <summary><strong> OOP </strong></summary>




<details>
  <summary><strong> Tính đóng gói </strong></summary>

**Encapsulation** là một nguyên lý quan trọng trong lập trình hướng đối tượng (OOP), có nghĩa là **gói gọn dữ liệu (property) và hành vi (method)** vào bên trong lớp và ẩn đi các chi tiết nội bộ để bảo vệ tính toàn vẹn của đối tượng.

Cụ thể:

+  **Ẩn dữ liệu**: Các thuộc tính nhạy cảm sẽ được khai báo là 'private' hoặc 'protected', không cho phép truy cập trực tiếp từ bên ngoài lớp.

+  **Cung cấp phương thức truy cập gián tiếp**: Dữ liệu được truy cập thông qua các phương thức getter/setter ở mức public.

Ví dụ ta có chương trình như sau:

```cpp
#include <iostream>
#include <string>
using namespace std;

class SinhVien{
    private:
        string name;
        int id;
   
    public:
        SinhVien(){
            static int ID = 1;
            id = ID;
            ID++;
        }

        void setName(string newName){   // setter method
            // kiểm tra điều kiện
            name = newName;
        }

        string getName(){   // getter method
            return name;
        }

        int getID(){
            return id;
        }

        void display(){
            cout << "Ten: " << getName() << endl;
            cout << "ID: " << getID() << endl;
        }
};

int main(int argc, char const *argv[])
{
    SinhVien sv1, sv2;

    sv1.setName("Trung");
    sv1.display();

    sv2.setName("Anh");
    sv2.display();
    return 0;
}
```

Ở đây ta có 2 thuộc tính là `name` và `id`, hai thông tin này ta không muốn bị truy cập một cách tùy tiện gây ảnh hưởng đến độ chính xác của thông tin. Do đó ta sẽ muốn giới hạn thông tin chỉ có truy cập thông qua các phương thức nhất định, ở đây là:

+  **Ghi dữ liệu có kiểm soát** (qua setter): Ở đây ta chỉ phép gán tên thông qua duy nhất phương thức `setName`, đối với `id` sẽ tự động được ghi vào khi ta khởi tạo một `class` mới (không thể tự khởi tạo id)

+  **Đọc dữ liệu an toàn** (qua getter): Tương tự ta chỉ có thể đọc dữ liệu thông qua các phương thức `getName`, `getID` và `display`.

Khi chạy chương trình thành công ta sẽ được:

```c
Ten: Trung
ID: 1
Ten: Anh
ID: 2
```

</details>




<details>
  <summary><strong> Tính trừu tượng </strong></summary>

Tính trừu tượng đề cập đến việc ẩn đi các chi tiết cụ thể của một đối tượng và chỉ hiển thị những gì cần thiết để sử dụng đối tượng đó. Và để làm được điều này, ta sẽ khai báo các method ở quyền truy cập private/protected.

Đối với chương trình đã viết ở trên, ta không muốn việc có thể tự do đọc giá trị tên thông qua các phương thức `getName` và `getID` thì ta cũng có thể đổi phạm vi truy cập của 2 phương thức trên sang `private` và ta chỉ có đọc dữ liệu thông qua phương thức `display` chứ không cần biến nó hoạt động thế nào, ta được tính triều tượng.

```c
#include <iostream>
#include <string>
using namespace std;

class SinhVien{
    private:
        string name;
        int id;

        string getName()    // getter method
        {   
            return name;
        }

        int getID()
        {
            return id;
        }

    public:
        SinhVien(){
            static int ID = 1;
            id = ID;
            ID++;
        }

        void setName(string newName){   // setter method
            // kiểm tra điều kiện
            name = newName;
        }

        void display(){
            cout << "Ten: " << getName() << endl;
            cout << "ID: " << getID() << endl;
        }
};

int main(int argc, char const *argv[])
{
    SinhVien sv1, sv2;

    sv1.setName("Trung");
    sv1.display();

    sv2.setName("Anh");
    sv2.display();
    return 0;
}
```
</details>






<details>
  <summary><strong> Tính kế thừa </strong></summary>

**Kế thừa** là một trong bốn tính chất quan trọng của lập trình hướng đối tượng (OOP), cho phép một lớp (class)** kế thừa lại** các **thuộc tính** (property) và **phương thức** (method) từ lớp khác — giúp tái sử dụng mã, giảm trùng lặp và mở rộng chức năng dễ dàng.

Trong đó:
+  Lớp cha (Base class): Là lớp được kế thừa.

+  Lớp con (Derived class): Là lớp kế thừa từ lớp cha.

Cú pháp kế thừa:

```c
class Base {
    // class cha
};

class Derived : public Base {
    // class con kế thừa class cha
};
```

Có 3 kiểu kế thừa tất cả:

<details>
  <summary><strong> Kế thừa public </strong></summary>

  Các đặc điểm
1.  Các member `public` của class cha vẫn sẽ là `public` trong class con nghĩa là có thể truy cập trực tiếp thông qua đối tượng của lớp con. VD:

```cpp
class Parent {
public:
    void sayHello() { cout << "Hello from parent\n"; }
};

class Child : public Parent { };

Child c;
c.sayHello(); // Gọi được vì sayHello vẫn là public
```

2.  Các member `protected` của class cha vẫn sẽ là `protected` trong class con nghĩa là tuy không thể truy cập từ bên ngoài, nhưng lớp con có thể truy cập. VD:

```cpp
class Parent {
protected:
    int value = 42;
};

class Child : public Parent {
public:
    void show() {
        cout << "Value: " << value << endl; //Truy cập được
    }
};

Child c;
c.show(); // OK
// cout << c.value; Lỗi: không thể truy cập từ bên ngoài
```

3.  Các member private của class cha không thể truy cập trực tiếp từ class con nhưng có thể được truy cập gián tiếp qua các phương thức `public` hoặc `protected` của class cha. VD:

```cpp
class Parent {
private:
    int secret = 123;

protected:
    int getSecret() { return secret; } // Truy cập gián tiếp

public:
    int readSecret() { return secret; } // Cũng được
};

class Child : public Parent {
public:
    void reveal() {
        cout << "Secret is: " << getSecret() << endl; //Truy cập gián tiếp qua protected
    }
};

Child c;
c.reveal(); // OK
// cout << c.secret; Không được vì secret là private
```
</details>



<details>
  <summary><strong> Kế thừa protected </strong></summary>

`public` → `protected`:

+  Bên ngoài lớp con không thể truy cập nữa.

+  Bên trong lớp con hoặc lớp kế tiếp vẫn có thể sử dụng.

`protected` → `protected`:

+  Không thay đổi. Lớp con vẫn có quyền truy cập nội bộ.

`private`:

+  Không được kế thừa trực tiếp.

+  Nhưng có thể truy cập gián tiếp thông qua các phương thức public hoặc protected của lớp cha (ví dụ như getter/setter).

Ta có chương trình mẫu như sau

```cpp
class Parent {
public:
    int a = 1;

protected:
    int b = 2;

private:
    int c = 3;

protected:
    int getC() { return c; }  // Gián tiếp cho phép lớp con truy cập 'c'
};

// Kế thừa theo kiểu protected
class Child : protected Parent {
public:
    void show() {
        cout << "a = " << a << endl;      // Được (a trở thành protected)
        cout << "b = " << b << endl;      // Được (b vẫn là protected)
        cout << "c = " << getC() << endl; // Truy cập gián tiếp c qua phương thức protected
    }
};

int main() {
    Child c;
    c.show();

    // cout << c.a;  Lỗi: a đã trở thành protected trong lớp con
}
```


</details>




<details>
  <summary><strong> Kế thừa private </strong></summary>

Khi một lớp kế thừa lớp cha bằng từ khóa `private`, thì:

+  `public` → `private`: Không thể truy cập từ bên ngoài lớp con nữa. Chỉ lớp con có thể dùng nội bộ.

+  `protected` → `private`: Cũng không thể truy cập từ bên ngoài, và lớp kế tiếp nữa (nếu có) cũng không thấy được.

+  `private` của lớp cha: Không được kế thừa trực tiếp. Nhưng có thể truy cập gián tiếp qua các method public hoặc protected của lớp cha.

Chương trình mẫu:
```cpp
class Parent {
public:
    int a = 10;

protected:
    int b = 20;

private:
    int c = 30;

protected:
    int getC() { return c; }
};

// Kế thừa theo kiểu private
class Child : private Parent {
public:
    void show() {
        cout << "a = " << a << endl;        //  Được (a đã trở thành private trong Child)
        cout << "b = " << b << endl;        //  Được (b cũng trở thành private trong Child)
        cout << "c = " << getC() << endl;   //  Truy cập gián tiếp c
    }
};

int main() {
    Child c;
    c.show();

    // cout << c.a;  Lỗi: a là private trong Child → không truy cập được
}
```
</details>





<details>
  <summary><strong> Đa kế thừa </strong></summary>

Khi nhiều lớp cha có các phương thức hoặc thuộc tính trùng tên, việc gọi chúng từ lớp con có thể gây ra sự nhầm lẫn.

Khi một lớp con kế thừa từ hai lớp cha, mà hai lớp cha này đều cùng kế thừa từ cùng một lớp khác. Tình huống này tạo ra cấu trúc hình thoi (diamond), do đó được gọi là vấn đề "Diamond".

```c
    A
   / \
  B   C
   \ /
    D
```

+  A là lớp gốc (base class).

+  B và C cùng kế thừa từ A.

+  D kế thừa từ cả B và C.


Nếu A có một thuộc tính hoặc phương thức, thì D sẽ **kế thừa hai bản sao** của A — một từ B, và một từ C. Điều này dẫn đến:

+  Nhân đôi dữ liệu từ A.

+  Không rõ ràng khi gọi phương thức/thành viên từ A: Gọi A::method() là từ nhánh B hay C?

Ta có một chương trình mẫu như sau:

```cpp
#include <iostream>

using namespace std;

class A{
    public:
        A(){ cout << "Constructor A\n"; }

        void hienThiA(){ cout << "Day la lop A\n"; }
};

class B : public A{
    public:
        B(){ cout << "Constructor B\n"; }

        void hienThiB(){ cout << "Day la lop B\n"; }
};

class C : public A {
    public:
        C(){ cout << "Constructor C\n"; }

        void hienThiC(){ cout << "Day la lop C\n"; }
};

class D : public B, public C{
    public:
        D(){ cout << "Constructor D\n"; }

        void hienThiD(){ cout << "Day la lop D\n"; }
};

int main() {
    cout << "Các constructor đã được thực hiện\n";

    D d;             // Contructor của A sẽ được nhân đôi do kế thừa từ cả B và C

    cout << "/////////////////////////////////\n";
    // d.hienThiA(); // wrong không thể thực hiện do không biết sẽ gọi từ B hay C

    // Gọi phương thức từ lớp A qua B và C
    cout << "hienThiA từ lớp A thông qua B\n";
    d.B::hienThiA(); // Gọi hàm hienThiA từ lớp A thông qua B
    cout << "hienThiA từ lớp A thông qua C\n";
    d.C::hienThiA(); // Gọi hàm hienThiA từ lớp A thông qua C

    cout << "/////////////////////////////////\n";

    cout << "Các hàm hiển thị từng lớp\n";
    d.hienThiB();
    d.hienThiC();
    d.hienThiD();

    return 0;
}
```

Khi chạy sẽ được:

```
Các constructor đã được thực hiện
Constructor A
Constructor B
Constructor A
Constructor C
Constructor D
/////////////////////////////////
hienThiA từ lớp A thông qua B
Day la lop A
hienThiA từ lớp A thông qua C
Day la lop A
/////////////////////////////////
Các hàm hiển thị từng lớp
Day la lop B
Day la lop C
Day la lop D
```

</details>






</details>










</details>
