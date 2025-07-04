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



</details>






</details>










</details>
