<details>
  <summary><strong> Namespace </strong></summary>

  

<details>
  <summary><strong> Khái niệm </strong></summary>

Namespace là cách nhóm các đối tượng như biến, hàm, class, struct,... vào một không gian tách biệt.

Namespace được sử dụng với mục đích là để **tránh xung đột tên** khi có các định danh giống nhau được khai báo trong các phần của chương trình hoặc các thư viện khác nhau.

Ví dụ:
```cpp
#include <iostream>
using namespace std;

namespace A{
    char *name = (char*)"Anh 20";

    void display(){
        cout << "Name: " << name << endl;
    }
}
namespace B{
    char *name = (char*)"Anh 21";

    void display(){
        cout << "Name: " << name << endl;
    }
}

int main(){
    cout << "Name: " << A::name << endl;
    cout << "Name: " << B::name << endl;
    A::display();
    B::display();
    return 0;
}
```

```
Name: Anh 20
Name: Anh 21
Name: Anh 20
Name: Anh 21
```

Với cùng hàm và biến cùng tên có thể dễ dàng gọi ra dựa trên tên khác nhau.

</details>




<details>
  <summary><strong> Đặc điểm </strong></summary>

Đặc điểm của Namespace:

+  C++ cho phép tạo các namespace **lồng nhau (nested namespace)**, nghĩa là một namespace có thể chứa một namespace khác bên trong nó.

+  Namespace có thể được **mở rộng** bằng cách khai báo nhiều lần cùng một tên namespace trong các phần khác nhau của chương trình. Các khai báo này sẽ được trình biên dịch ghép lại thành một namespace duy nhất.



<details>
  <summary><strong> Namespace lồng nhau </strong></summary>

```cpp
#include <iostream>
using namespace std;

namespace A{
    char *name = (char*)"Trung 20";
    namespace C{
        char *str = (char*)"Nguyen Hoang";
    }
}

int main()
{
    cout << "Name: " << A::C::str << endl;
    //cout << "Name: " << C::str << endl;      // Không thể thực hiện
    return 0;
}
```

```
Name: Nguyen Hoang
```

Ta có `namespace C` được lồng trong `namespace A` nên để gọi được tên trong `C` sẽ cần cú pháp gọi thông qua `A` chứ không thể gọi trực tiếp được. 

</details>





<details>
  <summary><strong> Namespace mở rộng </strong></summary>

```cpp
#include <iostream>
using namespace std;

namespace A{
    char *name = (char*)"Trung 20";
    namespace C{
        char *str = (char*)"Nguyen Hoang";
    }
}

namespace A{

    int x = 100;
    int y = 200;

}

int main()
{
    cout << "Name: " << A::name << endl;
    cout << "X mở rộng: " << A::x << endl;
    cout << "Y mở rộng: " << A::y << endl;

    //cout << "Name: " << C::str << endl;      // Không thể thực hiện
    return 0;
}
```

```
Name: Trung 20
X mở rộng: 100
Y mở rộng: 200
```

Vậy khi ta khai báo `namespace` một lần nữa thì dữ liệu `namespace` sau sẽ tự động được thêm vào (mở rộng) `namespace` đó. Việc mở rộng `namespace` thường được ứng dụng trong việc mở rộng từ các file header giúp việc mở rộng code hiệu quả hơn, ví dụ:

<img width="928" height="583" alt="image" src="https://github.com/user-attachments/assets/eb30174a-57d0-406a-826d-ef9d73b2b4f3" />

</details>



</details>




<details>
  <summary><strong> Name Mangling </strong></summary>

**Biến đổi tên** (Name Mangling) là một cơ chế của trình biên dịch g++ nhằm **mã hóa tên** hàm, biến, class, namespace,... thành tên duy nhất, để **tránh xung độ**t trong quá trình biên dịch (giai đoạn compiler).

Trong C++, ta có thể dùng:

+  Class chứa hàm thành viên
+  Nạp chồng hàm (hàm trùng tên, khác tham số)
+  Template
+  Namespace

Gỉa sử ta có hai `namespace` như sau:

```cpp
namespace A {
    void foo() {
        cout << "A::foo()" << endl;
    }
}

namespace B {
    void foo() {
        cout << "B::foo()" << endl;
    }
}
```

Khi này ta có hai `namespace` trên sẽ được mã hóa như sau:
+  A::foo() → _ZN1A3fooEv
+  B::foo() → _ZN1B3fooEv

Việc mã hóa như vậy sẽ có ý nghĩa như sau:
+  `_z`: Bắt đầu name mangling
+  `N…E`: Tên nằm trong namespace hoặc class
+  `1A`: Namespace "A" (1 ký tự)
+  `3foo`: Tên hàm "foo" (3 ký tự)
+  `v`: Không có tham số hay kiểu `void`




</details>






</details>
