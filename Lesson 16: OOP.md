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

## **Kế thừa ảo**

+  **Kế thừa ảo** giúp tránh vấn đề **diamond problem** trong đa kế thừa.

+  Chỉ có một bản sao duy nhất của lớp cơ sở chung được kế thừa.

+  Kế thừa ảo giúp quản lý các lớp liên quan đến phần cứng và giao tiếp. Điều này giúp tránh trùng lặp tài nguyên và quản lý hiệu quả trong hệ thống nhúng.

```cpp
#include <iostream>
using namespace std;

class A {
    public:
        A(){ cout << "Constructor A\n"; }

        void hienThiA(){ cout << "Day la lop A\n"; }
};

class B : virtual public A{
    public:
        B(){ cout << "Constructor B\n"; }

        void hienThiB(){ cout << "Day la lop B\n"; }
};

class C : virtual public A {
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
    D d;

    d.hienThiA();

    return 0;
}
```

Kết quả chạy được 

```
Constructor A
Constructor B
Constructor C
Constructor D
Day la lop A
```

Như này khi khởi tạo đối tượng của lớp con `D`, constructor của class `D` không còn in ra 2 lần `Constructor A` nữa. Lúc này `D` chỉ còn kế thừa một hàm constructor `A()` duy nhất.

</details>






</details>






<details>
  <summary><strong> Tính đa hình </strong></summary>





<details>
  <summary><strong> Tổng quan tính đa hình </strong></summary>

Tính đa hình (Polymorphism) có nghĩa là "nhiều dạng" và nó xảy ra khi chúng ta có nhiều class có liên quan với nhau thông qua tính kế thừa. 

Tính đa hình cho phép một hành động hoặc phương thức có thể có nhiều cách thực thi khác nhau, tùy thuộc vào đối tượng thực hiện nó.

Tính đa hình có thể được chia thành hai loại chính:
+  Đa hình tại thời điểm biên dịch (Compile-time Polymorphism).
+  Đa hình tại thời điểm chạy (Run-time Polymorphism).

</details>





<details>
  <summary><strong> Đa hình tại thời điểm chạy </strong></summary>


<details>
  <summary><strong> Upcasting & Downcasting </strong></summary>

**Upcasting** là việc chuyển (ép kiểu) một con trỏ hoặc tham chiếu của class dẫn xuất (class con) sang class cơ sở (class cha). Đây là thao tác an toàn và được thực hiện tự động mà không cần ép kiểu tường minh.

**Downcasting** là việc chuyển một con trỏ hoặc tham chiếu của class cha về lại class con. Đôi khi đây là thao tác không an toàn và sẽ gây lỗi **undefined behavior**.


Ví dụ ta có chương trình sau:

```c
#include <iostream>
#include <string>
using namespace std;

class DoiTuong{
    protected:
        string ten;
        int id;

    public:
        DoiTuong(){  
            static int ID = 1;
            id = ID;
            ID++;
        }

        void setName(string _ten){
            // check chuỗi nhập vào
            ten = _ten;
        }

        void display(){
            cout << "ten: " << ten << endl;
            cout << "id: " << id << endl;
        }
};

class SinhVien : public DoiTuong{
    protected:
        string chuyenNganh;

    public:
        void setChuyenNganh(string _nganh){
            chuyenNganh = _nganh;
        }

        void display()  {
            cout << "ten: " << ten << endl;
            cout << "id: " << id << endl;
            cout << "chuyen nganh: " << chuyenNganh << endl;
        }
};

class HocSinh : public DoiTuong{
    protected:
        string lop;
   
    public:
        void setLop(string _lop){
            lop = _lop;
        }

        void display() {
            cout << "ten: " << ten << endl;
            cout << "id: " << id << endl;
            cout << "lop: " << lop << endl;
        }
};

int main()
{
    SinhVien sv1;
    sv1.setName("Trung");
    sv1.setChuyenNganh("TDH");

    HocSinh hs1;
    hs1.setName("Tuan");
    hs1.setLop("12A1");

    DoiTuong *dt;

    dt = &sv1;            // Downcasting từ class cha DoiTuong xuống class con SinhVien
    dt->display();

    dt = &hs1;
    dt->display();        // Downcasting từ class cha DoiTuong xuống class con HocSinh
    return 0;
}
```

Ở đây ta có:
+  Hàm `display` của class cha có 2 thông tin
+  Hàm `display` của class con có 3 thông tin
Sau khi chuyển một con trỏ của class cha về lại class con ta chạy chương trình được

```
ten: Trung
id: 1
ten: Tuan
id: 2
```

Mặc dù class con có 3 thông tin nhưng khi được class cha trỏ vào thì chỉ còn lại 2 thông tin, 1 thông tin mất đi (do kiểu con trỏ class cha chỉ có 2 thông tin). Để không mất đi thông tin ta có thể ép lại kiểu class con khi gọi hàm.

Sửa lại trong hàm `main`:

```c
int main()
{
    SinhVien sv1;
    sv1.setName("Trung");
    sv1.setChuyenNganh("TDH");

    HocSinh hs1;
    hs1.setName("Tuan");
    hs1.setLop("12A1");

    DoiTuong *dt;

    dt = &sv1;                        // Downcasting từ class cha DoiTuong xuống class con SinhVien
    ((SinhVien*)dt)->display();       // Ép lại kiểu SinhVien

    dt = &hs1;                        // Downcasting từ class cha DoiTuong xuống class con HocSinh
    ((HocSinh*)dt)->display();        // Ép lại kiểu HocSinh
    return 0;
}
```

Kết quả:

```
ten: Trung
id: 1
chuyen nganh: TDH
ten: Tuan
id: 2
lop: 12A1
```

Viết thêm vào hàm `main` như sau:

```cpp
int main()
{
    SinhVien sv1;
    sv1.setName("Trung");
    sv1.setChuyenNganh("TDH");

    HocSinh hs1;
    hs1.setName("Tuan");
    hs1.setLop("12A1");

    DoiTuong *dt;

    dt = &sv1;                        // Downcasting từ class cha DoiTuong xuống class con SinhVien
    ((SinhVien*)dt)->display();       // Ép lại kiểu SinhVien
    cout<<"############################################\n";

    dt = &hs1;                        // Downcasting từ class cha DoiTuong xuống class con HocSinh
    ((HocSinh*)dt)->display();        // Ép lại kiểu HocSinh
    cout<<"############################################\n";

    SinhVien *sv = &sv1;
    ((DoiTuong*)sv)->display();       // Upcasting từ class con SinhVien lên class cha DoiTuong

    return 0;
}
```

Ta có class con `SinhVien` được ép kiểu (upcasting) lên class cha `DoiTuong`. Khi này từ một class con có 3 thông tin đã bị giảm xuống còn 2 thông tin như class cha:

```
ten: Trung
id: 1
chuyen nganh: TDH
############################################
ten: Tuan
id: 2
lop: 12A1
############################################
ten: Trung
id: 1
```
</details>



<details>
  <summary><strong> Virtual & Pure Virtual </strong></summary>

## **Hàm ảo (Virtual Function)**

Hàm ảo là một hàm thành viên được khai báo trong **class cha** với từ khóa `virtual`.

Khi một hàm là `virtual`, nó có thể được ghi đè (**override**) trong class con để cung cấp cách triển khai riêng.

Khi gọi một hàm ảo thông qua một con trỏ hoặc tham chiếu đến lớp con, hàm sẽ được **quyết định dựa trên đối tượng thực tế** mà con trỏ hoặc tham chiếu đang trỏ tới chứ không dựa vào kiểu của con trỏ.

Cú pháp `virtual`:

```cpp
class Base
{
    public:
        virtual void display()
 		{
            cout << "Display from Base class" << endl;
    }
};
```

Ta có chương trình mẫu như sau:

```cpp
#include <iostream>
#include <string>
using namespace std;

class DoiTuong{
    protected:
        string ten;
        int id;

    public:
        DoiTuong(){  
            static int ID = 1;
            id = ID;
            ID++;
        }

        void setName(string _ten){
            // check chuỗi nhập vào
            ten = _ten;
        }

        virtual void display(){             // Tạo hàm ảo
            cout << "ten: " << ten << endl;
            cout << "id: " << id << endl;
        }
};

class SinhVien : public DoiTuong{
    protected:
        string chuyenNganh;

    public:
        void setChuyenNganh(string _nganh){
            chuyenNganh = _nganh;
        }

        void display()  {
            DoiTuong::display();
            cout << "chuyen nganh: " << chuyenNganh << endl;
        }
};

int main()
{
    SinhVien sv1;
    sv1.setName("Trung");
    sv1.setChuyenNganh("TDH");

    DoiTuong *dt = &sv1;
    dt->display();

    return 0;
}
```

Ở đây ta có:
+  `virtual void display()`: Tạo một hàm ảo, và hàm này sẽ được mở rộng ở hàm con
+  `DoiTuong::display()`: Kế thừa lại hàm cha, khi chạy `display` sẽ chạy luôn display ở hàm cha

Chạy chương trình:

```
ten: Trung
id: 1
chuyen nganh: TDH
```

Như vậy ta thấy rằng mặc dù kiểu con trỏ class cha có 2 thông tin, nhưng mặc dù không ép kiểu khi gọi `display` thì vẫn ra 3 thông tin như class con. Vì vậy có nghĩa là khi sử dụng từ khóa `virtual` hàm sẽ được quyết định dựa trên đối tượng thực tế mà con trỏ hoặc tham chiếu đang trỏ tới chứ không dựa vào kiểu của con trỏ.

## **Hàm thuần ảo (Pure Virtual Function)**

Hàm thuần ảo là một **hàm ảo không có phần định nghĩa** trong class cha, được khai báo với **cú pháp = 0** và khiến class cha trở thành **class trừu tượng (abstract class)**, nghĩa là không thể tạo đối tượng từ class này.

Ví dụ ta có chương trình sau:

```cpp
#include <iostream>
using namespace std;

class cha{
    public:
        virtual void display() = 0; // Hàm ảo thuần túy
};

class con : public cha{
    public:
        void display() override{   // Ghi đè hàm thuần ảo
            cout << "display from class con" << endl;
        }
};

int main(){
    // cha ptr; // wrong
    cha *ptr;
    con obj;

    ptr = &obj;
    ptr->display();

    return 0;
}
```

Ở đây ta có:
+  Không thể tạo đối tượng với class cha (`cha ptr;` không hợp lệ)
+  Có thể tạo đối tượng là con trỏ với class cha
```cpp
cha *ptr;
```
+  Cú pháp hàm thuần ảo
```cpp
  virtual void display() = 0; // Hàm ảo thuần túy
```
+  Khi class cha có hàm thuần ảo, class con khi kế thừa phải viết rõ hàm thuần ảo (override) ra nếu không sẽ không tạo đối tượng được

Kết quả:
```
display from class con
```


</details>






<details>
  <summary><strong> Override & Overload </strong></summary>

+ **Override**: Khi một hàm ảo được ghi đè, hành vi của nó sẽ phụ thuộc vào kiểu của đối tượng thực tế, chứ không phải kiểu của con trỏ hay tham chiếu.
Tính đa hình runtime xảy ra khi quyết định gọi hàm nào (phiên bản của class cha hay class con) được đưa ra tại thời điểm chạy, không phải lúc biên dịch, giúp mở rộng chức năng. Điều này giúp chương trình linh hoạt hơn, cho phép việc mở rộng chức năng mà không cần sửa đổi mã nguồn hiện tại.

```cpp
class DoiTuong{
    protected:
        string ten;
        int id;

    public:
        DoiTuong(){  
            static int ID = 1;
            id = ID;
            ID++;
        }

        void setName(string _ten){
            // check chuỗi nhập vào
            ten = _ten;
        }

        virtual void display(){             // Tạo hàm ảo
            cout << "ten: " << ten << endl;
            cout << "id: " << id << endl;
        }
};

class SinhVien : public DoiTuong{
    protected:
        string chuyenNganh;

    public:
        void setChuyenNganh(string _nganh){
            chuyenNganh = _nganh;
        }

        void display() override
        {                   // Override
            DoiTuong::display();
            cout << "chuyen nganh: " << chuyenNganh << endl;
        }
};
```

Trong đó hàm `display` trong class cha là **hàm ảo**, hàm `display` trong class con **ghi đè lại** hàm trong class cha, như vậy đây là **override**. Khi khai báo hàm con ghi đè từ một hàm ảo ở hàm cha ta có thể viết thêm từ khóa `override` để phân biệt.

```cpp
void display() override
```

+ **Overload**: Overload là khả năng cho phép nhiều class con sửa đổi hàm của class nhưng vẫn giữ chung một tên gọi. Khi sử dụng overload ta có thể thay đổi tham số hàm ở class con theo ý muốn, điều mà ta không thể làm được ở override. Không như override, overload không có từ khóa để phân biệt.

```cpp
#include <iostream>
#include <string>
using namespace std;

class DoiTuong{
    protected:
        string ten;
        int id;

    public:
        DoiTuong(){  
            static int ID = 1;
            id = ID;
            ID++;
        }

        void setName(string _ten){
            // check chuỗi nhập vào
            ten = _ten;
        }

        virtual void display(){             // Tạo hàm ảo
            cout << "ten: " << ten << endl;
            cout << "id: " << id << endl;
        }
};

class SinhVien : public DoiTuong{
    protected:
        string chuyenNganh;

    public:
        void setName(string _ten, int num){
            ten = _ten;
            cout << "Hàm class con overload thêm số: " << num << "\n";
        }

        void display() override
        {                   
            DoiTuong::display();
            cout << "chuyen nganh: " << ten << endl;
        }
};

int main()
{
    SinhVien sv1;
    sv1.setName("Trung",100);
    return 0;
}
```

Ta có hàm `void setName(string _ten, int num)` được bổ sung thêm tham số `num` so với hàm trong class cha và khi ta chạy chương trình:

```
Hàm class con overload thêm số: 100
ten: Trung
id: 1
chuyen nganh: Trung
```

</details>




<details>
  <summary><strong> vtable </strong></summary>

## **vtable**

**vtable (virtual table)** là một bảng tra cứu các con trỏ hàm mà trình biên dịch tạo ra để hỗ trợ tính đa hình động (dynamic polymorphism) của các hàm ảo (virtual function).

Mỗi class có **ít nhất một hàm ảo** hoặc **kế thừa từ class có hàm ảo** sẽ được trình biên dịch tạo một bảng vtable riêng tương ứng với class đó.

vtable giúp đảm bảo rằng hàm đúng của class con được gọi, kể cả khi dùng con trỏ/đối tượng của lớp cha.

## **vpointer**

Mỗi object của class có hàm ảo đều sẽ có một vpointer (vptr) để trỏ tới vtable tương ứng.

vpointer thường được trình biên dịch tự động thêm vào như một thành viên ẩn của object.

Khi gọi hàm ảo, chương trình sẽ lấy vtable thông qua vptr, sau đó tra địa chỉ hàm đúng (tùy theo object thực sự thuộc class nào).

## Hoạt động khi gọi hàm ảo:
1. Lấy `vptr` từ object.
2. Trỏ tới `vtable` của class thực tế của object.
3. Lấy đúng địa chỉ hàm `override`.
4. Gọi hàm

Hàm ảo `override` sẽ có địa chỉ khác với hàm ảo trong class cha. Nếu gọi hàm ảo `override` thì `vtable` sẽ trỏ đúng vào địa chỉ hàm ảo `override` của class con, còn không sẽ trỏ vào địa chỉ hàm ảo được kế thừa ở class cha.

</details>





<details>
  <summary><strong> Interface & Abstract Class </strong></summary>

**Interface** là một class **chỉ chứa các hàm thuần ảo (pure virtual)** và không có bất kỳ cài đặt nào.

```cpp
class IExample
{
    public:
        virtual void func1() = 0;
        virtual void func2() = 0;
        virtual ~IExample(){}
};
```

**Abstract Class** là class có **ít nhất một hàm thuần ảo**, nhưng có thể chứa cả hàm thường và dữ liệu thành viên (có thể có cài đặt).

```cpp
class Base
{
    public:
        virtual void foo() = 0; // hàm thuần ảo
        void commonFunc() {}    // hàm thường
};
```

</details>






</details>








<details>
  <summary><strong> Đa hình tại thời điểm biên dịch </strong></summary>


</details>





</details>







</details>
