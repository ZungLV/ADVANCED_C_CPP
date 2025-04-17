# Bitmask
Bitmask là một kỹ thuật sử dụng các bit để lưu trữ và thao tác với các cờ (flags) hoặc trạng thái. Có thể sử dụng bitmask để đặt, xóa và kiểm tra trạng thái của các bit cụ thể.

Bitmask thường được sử dụng để tối ưu hóa bộ nhớ, thực hiện các phép toán logic trên một cụm bit, và quản lý các trạng thái, quyền truy cập, hoặc các thuộc tính khác của một đối tượng.
Các phép toán logic của Bitmask như sau

## NOT bitwise

"NOT bitwise" (hay còn gọi là bitwise NOT, hoặc toán tử NOT theo từng bit) là một toán tử trong lập trình dùng để lật giá trị của từng bit trong một số nguyên. Bitwise NOT sẽ biến:

Bit `0` thành `1`

Bit `1` thành `0`

Cú pháp của phép NOT là

```C
int result = ~num
```

EX:
```C
#include <stdio.h>
#include <stdint.h>

int main() {
    uint8_t x = 0;                          // 0 dưới dạng nhị phân là 0b0000 0000
    uint8_t y = ~x;                         // ~x => y = 0b1111 1111 sẽ có giá trị lớn nhất 2^8-1 = 255
    printf("~(%d) = (%d)\n", x, y);         // Kết quả: ~(0) = (255)
    return 0;
}
```
```
~(0) = (255)
```
![image](https://github.com/user-attachments/assets/dad1f5fc-359f-4efd-b983-255ea8b24c04)

## AND bitwise

Bitwise AND (phép "và" theo từng bit) là một phép toán nhị phân so sánh từng cặp bit của hai số nguyên. Nó chỉ trả về `1` nếu cả hai bit đều là `1`, ngược lại trả về `0`.

Cú pháp của phép AND là

```C
int result = num1 && num2
```

EX

```C
#include <stdio.h>
#include <stdint.h>

void print_binary(uint8_t n) {      // Sử dụng phép dịch bit sang bên phải và and với 1 để lấy chính nó
    for (int i = 7; i >= 0; i--) {
        printf("%d", (n >> i) & 1);
    }
}

int main() {
    uint8_t a = 0b11010100;                                     
    uint8_t b = 0b10101010;
    uint8_t result = a & b;

    printf("a      = "); print_binary(a); printf("\n");
    printf("b      = "); print_binary(b); printf("\n");
    printf("a & b  = "); print_binary(result); printf("\n");

    return 0;
}
```
```
a      = 11010100
b      = 10101010
a & b  = 10000000
```
![image](https://github.com/user-attachments/assets/fd16899a-d3bd-4621-abc8-5e99001593ea)

## OR bitwise

Bitwise OR là phép toán nhị phân so sánh từng cặp bit của hai số. Nó trả về 1 nếu ít nhất một trong hai bit là 1, chỉ trả về 0 khi cả hai bit đều là 0.

Cú pháp của phép OR là

```C
int result = num1 | num2
```

EX

```C
#include <stdio.h>
#include <stdint.h>

void print_binary(uint8_t n) {        // Sử dụng phép dịch bit sang bên phải và and với 1 để lấy chính nó
    for (int i = 7; i >= 0; i--) {
        printf("%d", (n >> i) & 1);
    }
}

int main() {
    uint8_t a = 0b10010100;                                     
    uint8_t b = 0b10101010;
    uint8_t result = a | b;

    printf("a      = "); print_binary(a); printf("\n");
    printf("b      = "); print_binary(b); printf("\n");
    printf("a | b  = "); print_binary(result); printf("\n");

    return 0;
}
```
```
a      = 10010100
b      = 10101010
a | b  = 10111110
```
![image](https://github.com/user-attachments/assets/cc73171e-cc90-4105-8568-970293243297)

## XOR bitwise

Bitwise XOR (viết tắt từ Exclusive OR) là phép toán nhị phân so sánh từng cặp bit của hai số. Nó trả về:

`1` nếu hai bit khác nhau

`0` nếu hai bit giống nhau

Cú pháp của phép XOR là

```C
int result = num1 ^ num2
```
EX

```C
#include <stdio.h>
#include <stdint.h>

void print_binary(uint8_t n) {        // Sử dụng phép dịch bit sang bên phải và and với 1 để lấy chính nó
    for (int i = 7; i >= 0; i--) {
        printf("%d", (n >> i) & 1);
    }
}

int main() {
    uint8_t a = 0b10010100;                                     
    uint8_t b = 0b10101010;
    uint8_t result = a ^ b;

    printf("a      = "); print_binary(a); printf("\n");
    printf("b      = "); print_binary(b); printf("\n");
    printf("a ^ b  = "); print_binary(result); printf("\n");

    return 0;
}
```
```
a      = 10010100
b      = 10101010
a ^ b  = 00111110
```
![image](https://github.com/user-attachments/assets/79ce7229-9329-43c5-84aa-7ec95177f5c9)

# Shift left - Shift right bitwise

+ Dùng để di chuyển bit sang trái hoặc sang phải.
+ Trong trường hợp `<<`, các bit ở bên phải sẽ được dịch sang trái, và các bit trái cùng sẽ được đặt giá trị `0`.
+ Trong trường hợp `>>`, các bit ở bên trái sẽ được dịch sang phải, và các bit phải cùng sẽ được đặt giá trị `0` hoặc `1` tùy thuộc vào giá trị của bit cao nhất (bit dấu).

Cú pháp của Shift left - Shift right bitwise là

```C
int resultLeftShift  = num << shiftAmount;
int resultRightShift = num >> shiftAmount;
```
EX

```C
#include <stdio.h>
#include <stdint.h>

void print_binary(uint8_t n) {                                  // Sử dụng phép dịch bit sang bên phải và and với 1 để lấy chính nó
    for (int i = 7; i >= 0; i--) {
        printf("%d", (n >> i) & 1);
    }
}

int main() {
    uint8_t a = 0b00001111;
    printf("a       = "); print_binary(a); printf(" (%d)\n", a);

    uint8_t left = a << 4;      // Dịch 5 bit về phía bên trái 
    printf("a << 4  = "); print_binary(left); printf(" (%d)\n", left);

    uint8_t right = a >> 2;     // Dịch 3 bit về phía bên phải 
    printf("a >> 2  = "); print_binary(right); printf(" (%d)\n", right);

    return 0;
}
```
```
a       = 00001111 (15)
a << 4  = 11110000 (240)
a >> 2  = 00000011 (3)
```
