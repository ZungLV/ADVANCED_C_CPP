<details>
  <summary><strong>👆 Pointer </strong></summary>

<details>
  <summary><strong>📝 Giới thiệu về pointer </strong></summary>
  

Trong ngôn ngữ lập trình C, con trỏ (pointer) là một biến chứa địa chỉ bộ nhớ của một đối tượng khác (biến, mảng, hàm). 

Việc sử dụng con trỏ giúp chúng ta thực hiện các thao tác trên bộ nhớ một cách linh hoạt hơn. 

</details>

<details>
  <summary><strong>⚙️ Các thao tác với pointer  </strong></summary>

### Cách khai báo pointer

```C
int *int_ptr;        // con trỏ kiểu int
char *char_ptr;      // con trỏ kiểu char
double *double_ptr;  // con trỏ kiểu double
```

Cách nhận biết đơn giản nhất giữa kiểu con trỏ và một kiểu thường là dấu `*` trước tên biến. Dấu `*` là cách mà máy có thể nhận biết đâu là kiểu con trỏ.

### Lấy địa chỉ của một biến 

```C
int x = 10;
int *int_ptr = &x;
```
Chỉ có thể lấy địa chỉ của một biến đã tồn tại, khai báo từ trước. Để lấy địa chỉ ta đơn giản viết `&` đi cùng với biến mong muốn (trong ví dụ trên là biến `x`) và để con trỏ của ta `*int_ptr` trỏ vào đó. Biến `int_ptr` khi này đã thành công lưu trữ địa chỉ của biến `x` sẵn sàng cho việc thao tác sau này.

### Truy cập giá trị (giải tham chiếu - dereference)

```C
int x = 10;
int *int_ptr = &x;
int y = *int_ptr;
```
Cuối cùng sau khi đã khai báo và lưu địa chỉ thành công, ta tiến hành truy cập giá trị. Giả sử biến `x` có địa chỉ là 0x01 thì:

+ `int_ptr`: chứa địa chỉ của `x`, `int_ptr = 0x01`

+ `*int_ptr`: trỏ vào giá trị của `x`, `*int_ptr = *(0x01) = 10`

+ `y`: bằng với giá trị của `x`, `y = 10`

### Thay đổi giá trị gián tiếp

Thay vì trực tiếp thay đổi giá trị của `x` ta có thể dùng con trỏ `*int_ptr` để làm điều đó:

```c
#include <stdio.h>

int x = 10;
int *int_ptr = &x;


int main() {

    int y = *int_ptr;

    printf("Giá trị của x: %d\n", x);
    printf("Địa chỉ của x: %p\n", &x);
    printf("Giá trị của int_ptr (địa chỉ của x): %p\n", int_ptr);
    printf("Giá trị tại địa chỉ mà int_ptr trỏ tới: %d\n", *int_ptr);
    printf("Giá trị của y: %d\n", y);

    *int_ptr = 100;  // Thay đổi giá trị của x thông qua con trỏ
    printf("Giá trị mới của x: %d\n", x);

    return 0;
}
```

Chạy chương trình trên ta được kết quả là:

```c
Giá trị của x: 10
Địa chỉ của x: 0x581030a92010
Giá trị của int_ptr (địa chỉ của x): 0x581030a92010
Giá trị tại địa chỉ mà int_ptr trỏ tới: 10
Giá trị của y: 10
Giá trị mới của x: 100
```

Như vậy ta có thể thấy rằng:

+ Giá trị của `int_ptr` bằng đúng với địa chỉ của `x` là 0x581030a92010

+ Giá trị của `y` truy cập thông qua con trỏ `*int_ptr` bằng đúng với giá trị của `x` là 10

+ Giá trị của `x` mới bằng đúng với giá trị sau khi thay đổi con trỏ `*int_ptr` là 100

</details>

<details>
  <summary><strong>💾 Kích thước pointer </strong></summary>

Kích thước của một biến con trỏ (pointer) không phụ thuộc vào kiểu dữ liệu mà nó trỏ tới, mà phụ thuộc vào kiến trúc hệ thống và trình biên dịch.

+ Đối với kiến trúc máy tình, con trỏ có thể có kich thước 64 bit (8 bite), hay 32 bit (4 bite)

+ Đối với kiến trúc vi sử lý, như stm8, stm32, esp32, ta có thể nhận thấy dễ dàng thông qua tên của chúng, con trỏ của chúng sẽ có kích thước lần lượt là 8 bit, 32 bit, 32 bit

```C
#include <stdio.h>

int main()
{
    printf("Size of char: %ld bytes and sizeof pointer: %ld bytes\n", sizeof(char), sizeof(char*));
    printf("Size of int: %ld bytes and sizeof pointer: %ld bytes\n", sizeof(int), sizeof(int*));
    printf("Size of double: %ld bytes and sizeof pointer: %ld bytes\n", sizeof(double), sizeof(double*));
    return 0;
}
```
```c
Size of char: 1 bytes and sizeof pointer: 8 bytes
Size of int: 4 bytes and sizeof pointer: 8 bytes
Size of double: 8 bytes and sizeof pointer: 8 bytes
```
Như ta đã thấy dù con trỏ có là kiểu int, char, double đi chăng nữa thì kích thước của nó vẫn luôn bằng 8 bytes do kiến trúc máy tính là x64 (8 bytes x 8 = 64 bits)

</details>

<details>
  <summary><strong>📌 Địa chỉ của pointer </strong></summary>


    Hiệu suất phần cứng: Một số kiến trúc máy tính như x86 (Intel) sử dụng Little Endian vì thuận lợi khi làm việc với các số nhỏ (byte ít quan trọng nhất là phần đầu tiên, xử lý dễ dàng hơn).

    Phát triển hệ thống: Hệ thống Big Endian đôi khi được sử dụng trong các hệ thống như Motorola 68000 hay PowerPC. Một số giao thức mạng như TCP/IP cũng sử dụng Big Endian.

Thực chất khi một biến không được cấp phát 1 địa chỉ mà các địa chỉ 8 bits liên tiếp do máy chỉ lưu trữ được địa chỉ 8 bits.

Và giá trị của `x` là 10 => giá trị mà máy đọc được theo kiểu `int` là 0x00000000 00000000 00000000 00001010 hay 0x00 0x00 0x00 0x0A

Ở máy tính thường sẽ lưu trữ các bit dưới dạng Little Endian. Little Endian là cách lưu trữ dữ liệu trong bộ nhớ nơi mà byte ít quan trọng nhất (Least Significant Byte - LSB) được lưu trữ ở địa chỉ bộ nhớ thấp nhất. Còn byte quan trọng nhất (Most Significant Byte - MSB) sẽ được lưu ở địa chỉ bộ nhớ cao hơn.

Hiệu suất phần cứng: Một số kiến trúc máy tính như x86 (Intel) sử dụng Little Endian vì thuận lợi khi làm việc với các số nhỏ (byte ít quan trọng nhất là phần đầu tiên, xử lý dễ dàng hơn).
Phát triển hệ thống: Hệ thống Big Endian đôi khi được sử dụng trong các hệ thống như Motorola 68000 hay PowerPC. Một số giao thức mạng như TCP/IP cũng sử dụng Big Endian.

Như vậy ta có giá trị sẽ được lưu như sau:
0x01: 0x0A
0x02: 0x00
0x03: 0x00
0x04: 0x00

Tuy nhiên con trỏ có 8 bytes địa chỉ nên trong ví dụ trên ta có 4 bytes chứa giá trị còn 4 bytes còn lại sẽ chứa giá trị địa chỉ rác. Mặc dù kiểu dữ liệu không ảnh hưởng đến kích thước con trỏ nhưng nó lại quyết định sẽ có bao nhiêu bytes đọc được khi truy xuất dữ liệu. Vd: con trỏ kiểu `char` đọc 1 bytes thấp nhât, kiểu `int` đọc 4 bytes còn `double` đọc 8 bytes. Chính vì vậy mà ta cần phải khai báo kiểu dữ liệu của con trỏ cùng kích thước với biến mà con trỏ trỏ đến để tránh việc bị sai dữ liệu khi trích xuất.

```c
#include <stdio.h>

int x = 10;
double *int_ptr = &x;


int main() {

    printf("Giá trị của x: %d\n", x);
    printf("Giá trị tại địa chỉ mà int_ptr trỏ tới: %f\n", *int_ptr);

    return 0;
}
```
```
Giá trị của x: 10
Giá trị tại địa chỉ mà int_ptr trỏ tới: 0.000000
```

Ta có thể thấy do kiểu `double` đọc 8 bytes gồm 4 bytes dữ liệu của kiểu `int` và 4 byte rác dẫn đến trích suất dữ liệu sai.

</details>

<details>
  <summary><strong>📌 Truyền con trỏ vào hàm </strong></summary>
Ta không truyền con trỏ vào hàm khi hàm chỉ dùng để so sánh (truyền tham trị). Khi đó hàm sẽ sao chép biến và tạo ra biến có địa chỉ khác hoàn toàn.

Ta truyền con trỏ vào hàm khi muốn tác động và thay đổi giá trị của biến ấy thông qua tác động địa chỉ.

```c
#include <stdio.h>

void swap(int *a, int *b)
{
    int tmp = *a;
    *a = *b;
    *b = tmp;
}

int main()
{
   int a = 10, b = 20;
   swap(&a, &b);
   printf("value a is: %d\n", a);
   printf("value b is: %d\n", b);
   return 0;
}
```
```c
value a is: 20
value b is: 10
```
Sau khi sử dụng hàm thì hai biến `a` và `b` đã hoán đổi giá trị cho nhau.
</details>

</details>
