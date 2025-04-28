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

</details>
