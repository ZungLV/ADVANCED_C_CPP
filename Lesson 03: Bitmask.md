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

    uint8_t left = a << 4;      // Dịch 4 bit về phía bên trái 
    printf("a << 4  = "); print_binary(left); printf(" (%d)\n", left);

    uint8_t right = a >> 2;     // Dịch 2 bit về phía bên phải 
    printf("a >> 2  = "); print_binary(right); printf(" (%d)\n", right);

    return 0;
}
```
```
a       = 00001111 (15)
a << 4  = 11110000 (240)
a >> 2  = 00000011 (3)
```

## Thao tác BIT
Thay vì phải chia ra làm nhiều biến khác nhau và chiếm nhiều bộ nhớ của chương trình, ta có thể quy định các các đặc điểm khác nhau nằm trên các bit nhất định, điều đó sẽ giúp giảm thiểu bộ nhớ phải sài và dễ quản lý hơn.

VD: Sử dụng `#define` để định nghĩa từng bit ứng với từng loại đồ uống

```C
#include <stdio.h>
#include <stdint.h>

#define TEA           1 << 0  // Bit 0: Trà (0 = Không, 1 = Có)
#define COFFEE        1 << 1  // Bit 1: Cà phê (0 = Không, 1 = Có)
#define MILK          1 << 2  // Bit 2: Sữa (0 = Không, 1 = Có)
#define BUBBLE TEA    1 << 3  // Bit 3: Trà sữa (0 = Không, 1 = Có)

// Tự thêm tính năng khác
#define ENERGY DRINK  1 << 4  // Bit 4: Tăng lực (0 = Không, 1 = Có)
#define ORANGE JUICE  1 << 5  // Bit 5: Nước cam (0 = Không, 1 = Có)
#define COCA COLA     1 << 6  // Bit 6: Coca Cola (0 = Không, 1 = Có)
#define BEER          1 << 7  // Bit 7: Bia (0 = Không, 1 = Có)
```

Tiếp đó tạo 1 hàm để order nước, sử dụng toán tử OR với món nước mình đã định nghĩa bằng `#define` ta sẽ được món đó lên `1` do OR với `1`. Trong khi tất cả các món còn lại giữ nguyên do OR với `0`.

```C
void AddOrder(uint8_t *drinks, uint8_t drink)
{
    *drinks |= drink;
}
```

Tiếp theo đó tạo 1 hàm để hủy order nước, sử dụng toán tử AND và NOT với món nước mình đã định nghĩa bằng `#define`. Ví dụ với món `COFFEE` dịch trái 1 sẽ có mã nhị phân là `0b00000010`. Sử dụng toán tử NOT sẽ thành `0b11111101` và khi sử dụng toán tử AND với `drink` ta sẽ được duy nhất bit thứ 2 trong `drinks` = `0` do AND với `0` còn các biến còn lại bằng `1` do AND với `1`.

```C
int isFeatureEnabled(uint8_t drinks, uint8_t drink)
{
    return (drinks & drink) != 0;
}
```

Sau cùng tạo một hàm kiểm tra các thức uống đã đặt bằng cách xem ở các BIT tương ứng đã lên `1` hay chưa.

```C
void listSelectedDrinks(uint8_t drinks)
{
    printf("Selected Drinks:\n");

    if (drinks & TEA) {
        printf("- TEA\n");
    }
    if (drinks & COFFEE) {
        printf("- COFFEE\n");
    }
    if (drinks & MILK) {
        printf("- MILK\n");
    }
    if (drinks & BUBBLE_TEA ) {
        printf("- BUBBLE TEA \n");
    }
    if (drinks & ENERGY_DRINK) {
        printf("- ENERGY DRINK\n");
    }
    if (drinks & ORANGE_JUICE) {
        printf("- ORANGE JUICE\n");
    }
    if (drinks & COCA_COLA) {
        printf("- COCA COLA\n");
    }
    if (drinks & BEER) {
        printf("- BEER\n");
    }
    for (int i = 0; i < 8; i++)
    {
        printf("drink selected: %d\n", (drinks >> i) & 1);
    }
   
}
```

Ta sẽ có một chương trình hoàn chỉnh như sau 
```C
#include <stdio.h>
#include <stdint.h>

#define TEA           1 << 0  // Bit 0: Trà (0 = Không, 1 = Có)
#define COFFEE        1 << 1  // Bit 1: Cà phê (0 = Không, 1 = Có)
#define MILK          1 << 2  // Bit 2: Sữa (0 = Không, 1 = Có)
#define BUBBLE_TEA    1 << 3  // Bit 3: Trà sữa (0 = Không, 1 = Có)

// Tự thêm tính năng khác
#define ENERGY_DRINK  1 << 4  // Bit 4: Tăng lực (0 = Không, 1 = Có)
#define ORANGE_JUICE  1 << 5  // Bit 5: Nước cam (0 = Không, 1 = Có)
#define COCA_COLA     1 << 6  // Bit 6: Coca Cola (0 = Không, 1 = Có)
#define BEER          1 << 7  // Bit 7: Bia (0 = Không, 1 = Có)

void AddOrder(uint8_t *drinks, uint8_t drink)
{
    *drinks |= drink;
}

void CancelOrder(uint8_t *drinks, uint8_t drink)
{
    *drinks &= ~drink;
}


void listSelectedDrinks(uint8_t drinks)
{
    printf("Selected Drinks:\n");

    if (drinks & TEA) {
        printf("- TEA\n");
    }
    if (drinks & COFFEE) {
        printf("- COFFEE\n");
    }
    if (drinks & MILK) {
        printf("- MILK\n");
    }
    if (drinks & BUBBLE_TEA ) {
        printf("- BUBBLE TEA \n");
    }
    if (drinks & ENERGY_DRINK) {
        printf("- ENERGY DRINK\n");
    }
    if (drinks & ORANGE_JUICE) {
        printf("- ORANGE JUICE\n");
    }
    if (drinks & COCA_COLA) {
        printf("- COCA COLA\n");
    }
    if (drinks & BEER) {
        printf("- BEER\n");
    }
    for (int i = 0; i < 8; i++)
    {
        printf("drink selected: %d\n", (drinks >> i) & 1);
    }
   
}

int main()
{
    uint8_t options = 0;

    // Thêm tính năng
    AddOrder(&options, BUBBLE_TEA | COCA_COLA | BEER | ORANGE_JUICE);

    // Xóa tính năng
    CancelOrder(&options, ORANGE_JUICE);

    // Liệt kê các tính năng đã chọn
    listSelectedDrinks(options);
   
    return 0;
}

```
Kết quả chạy được
```
Selected Drinks:
- BUBBLE TEA 
- COCA COLA
- BEER
drink selected: 0
drink selected: 0
drink selected: 0
drink selected: 1
drink selected: 0
drink selected: 0
drink selected: 1
drink selected: 1
```
