# Bitmask
Bitmask là một kỹ thuật sử dụng các bit để lưu trữ và thao tác với các cờ (flags) hoặc trạng thái. Có thể sử dụng bitmask để đặt, xóa và kiểm tra trạng thái của các bit cụ thể.

Bitmask thường được sử dụng để tối ưu hóa bộ nhớ, thực hiện các phép toán logic trên một cụm bit, và quản lý các trạng thái, quyền truy cập, hoặc các thuộc tính khác của một đối tượng.
Các phép toán logic của Bitmask như sau

## NOT bitwise

"NOT bitwise" (hay còn gọi là bitwise NOT, hoặc toán tử NOT theo từng bit) là một toán tử trong lập trình dùng để lật giá trị của từng bit trong một số nguyên. Bitwise NOT sẽ biến:

Bit `0` thành `1`

Bit `1` thành `0`

EX:
```C
#include <stdio.h>
#include <stdint.h>

int main() {
    uint8_t x = 0;                          // 0 dưới dạng nhị phân là 0b0000 0000
    uint8_t y = ~x;                         // ~x => y = 0b1111 1111 sẽ có giá trị lớn nhất 2^8-1 = 255
    printf("~(%d) = (%d)\n", x, y);     // Kết quả: ~(0) = (255)
    return 0;
}
```
```
~(0) = (255)
```
![image](https://github.com/user-attachments/assets/dad1f5fc-359f-4efd-b983-255ea8b24c04)

## AND bitwise

Bitwise AND (phép "và" theo từng bit) là một phép toán nhị phân so sánh từng cặp bit của hai số nguyên. Nó chỉ trả về `1` nếu cả hai bit đều là `1`, ngược lại trả về `0`.


