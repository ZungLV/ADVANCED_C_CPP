<details>
  <summary><strong> Extern </strong></summary>
Extern trong ngôn ngữ lập trình C/C++ là từ khóa được sử dụng để thông báo rằng một biến hoặc hàm đã được khai báo ở một nơi khác trong chương trình hoặc trong một file nguồn khác.
  
Điều này giúp chương trình hiểu rằng biến hoặc hàm đã được định nghĩa và sẽ được sử dụng từ một vị trí khác, giúp quản lý sự liên kết giữa các phần khác nhau của chương trình hoặc giữa các file nguồn.

Giả sử ta có một file `test.c` như sau:
```c
#include <stdio.h>

int var_global = 50; // 0x01

void display()
{
    printf("%d\n",var_global);
}
```
Mà ta lại muốn sử dụng biến `var_global` và hàm `display` của file này cho một file khác thì ta sẽ dùng extern trước hàm hay biến đó. Ví dụ để sử dụng lại ở `main.c` ta viết như sau:
```c
#include <stdio.h>

extern int var_global;

extern void display();

int main(int argc, char const *argv[])
{
    display();
    return 0;
}

```
Khi liên kết `main.c` và `test.c` thành một file thực thi, chương trình sẽ tìm các hàm và biến được khai báo lại bằng `extern` ở file `main.c` ở file `test.c`. Extern chỉ cho phép ta **khai báo lại chứ không cho phép ta định nghĩa lại**.
Như ở ví dụ trên ta chỉ được phép khai báo `extern int var_global;` chứ không được định nghĩa `extern int var_global = 10;`. Sau khi được khai báo lại biến và hàm vẫn sẽ được giữ nguyên **giá trị** cũng như **địa chỉ** được cấp phát của mình như đã được khai báo ở file gốc.

Một trong những cách sử dụng `extern` phổ biến là sử dụng chung với file header. Ví dụ đối với file `test.c` ở trên ta có một file `test.h` như sau:
```c
#ifndef TEST_H
#define TEST_H

extern int var_global;

void display();

#endif
```
Khi ta thêm `include` file header vào `main.c` thì sau tiền xử lý nó sẽ sao chép toàn bộ file header vào file `main.c`. Vậy ta có file `main.c` như sau:

```c
#include <stdio.h>
#include "test.h"

int main(int argc, char const *argv[])
{
    display();
    return 0;
}
```
Với việc đã khai báo các hàm và biến ở file header rồi, ta không cần phải khai báo ở file `main.c` nữa do các hàm và biến sẽ được sao chép và khai báo lại. Ở đây ta thấy hàm `display` trong file `test.h` không khai báo `extern` do các hàm trong C không cần thiết phải khai báo `extern` để có thể sử dụng lại. Tuy nhiên các biến nếu muốn được sử dụng ở nhiều hàm khác nhau thì phải sử dụng `extern`


</details>
<details>
  <summary><strong> Static </strong></summary>
<details>
  <summary><strong> Static local </strong></summary>
"Static local" là biến cục bộ tĩnh, một biến local (cục bộ) thông thường chỉ tồn tại trong phạm vi hàm mà nó được khai báo và sẽ bị hủy sau khi hàm kết thúc. Tuy nhiên, nếu được khai báo với từ khóa `static`, thì:
  
+ Biến vẫn có phạm vi cục bộ, chỉ dùng được trong hàm đó.
+ Nhưng nó không bị xóa sau khi hàm kết thúc, mà giữ nguyên giá trị cho các lần gọi tiếp theo.
+ Nó được khởi tạo một lần duy nhất trong suốt chương trình.
Vd: Ở một hàm bình thường như sau
```c
#include <stdio.h>

void print_adr()
{
    int a = 1;
    printf("a = %d\n", a++);
}

int main(int argc, char const *argv[])
{
    print_adr();
    print_adr();
    print_adr();
    return 0;
}
```
```
a = 1
a = 1
a = 1
```
Dù với mục đích là in ra kết quả của a++ thì nó vẫn chỉ ra kết quả đã được khởi là bằng 1 bất kể gọi hàm bao nhiêu lần

Ta có cú pháp sử dụng static local:
```c
static int a = 1;
```
Thay lại vào hàm trên:
```c
#include <stdio.h>

void print_adr()
{
    static int a = 1;
    printf("a = %d\n", a++);
}

int main(int argc, char const *argv[])
{
    print_adr();
    print_adr();
    print_adr();
    return 0;
}
```
```c
a = 1
a = 2
a = 3
```
Biến vẫn được giữ nguyên giá trị và chỉ duy nhất 1 lần khai báo `a = 1`

Để có thể sử dụng biến `**static** trong hàm ở bên ngoài, ta có dựa vào đặc điểm giữ nguyên địa chỉ để áp dụng con trỏ trong việc thay đổi dữ liệu từ bên ngoài

```c
#include <stdio.h>

int *ptr = NULL;    //ptr là con trỏ toàn cục có thể sử dụng bất kì đâu trong code

void print_adr()
{
    static int a = 1;
    ptr = &a;      
    printf("a = %d\n", ++a);
}

int main(int argc, char const *argv[])
{
    print_adr();
    print_adr();
    *ptr = 49;
    print_adr();
    return 0;
}
```
```
a = 2
a = 3
a = 50
```
Do đã được can thiệp bởi con trỏ `ptr` nên ở lần gọi cuối cùng, giá trị của biến `a` đã hoàn toàn thay đổi
</details>
<details>
  <summary><strong> Static global </strong></summary>
Static global (biến toàn cục tĩnh) là biến được khai báo với từ khóa static ở ngoài mọi hàm, tức là có phạm vi toàn cục trong file, nhưng không thể truy cập từ các file khác.

Static global thường được dùng để:

Ẩn thông tin nội bộ của module (giấu biến không cho file khác dùng). Ví dụ ta có đoạn mã sau:

```c
// Nhập hệ số
void input_coefficients(Equation *eq);

// Tính delta
static double calculate_delta(double a, double b, double c);

// Giải phương trình
static void solve(Equation *eq);

// Hiển thị kết quả
void display_result(Equation eq);
```
Cả bốn hàm trên đều là hàm toàn cục, ta chỉ muốn người dùng chỉ có thể nhập thông số đầu vào để tính toán và lấy kết quả đầu ra chứ không muốn người dùng can thiệp vào các phép tính toán. Vì thế hai hàm `calculate_delta` và `solve` là static global để tránh người dùng lấy ra và can thiệp vào kết qủa và công thức tính toán.

Ngoài ra static global còn được dùng để giúp tránh xung đột tên biến khi làm việc với nhiều file. Thường sẽ được áp dụng khi có nhiều file tính toán sử dụng các biến giống nhau, việc sử dụng static global sẽ phù hợp nhất nếu các biến này chỉ được sử dụng trong các file đấy chứ không được lấy ra sử dụng ở file khác.
</details>
</details>
