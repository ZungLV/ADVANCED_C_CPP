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

Ngoài ra nếu ta không muốn việc có thể tự do đọc giá trị tên thông qua các phương thức `getName` và `getID` thì ta cũng có thể đổi phạm vi truy cập của 2 phương thức trên sang `private`

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

</details>



</details>
