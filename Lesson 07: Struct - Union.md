<details>
  <summary><strong> Struct </strong></summary>
<details>
  <summary><strong> Giới thiệu về Struct </strong></summary>
Trong ngôn ngữ lập trình C/C++, **struct** là một cấu trúc dữ liệu cho phép lập trình viên **tự định nghĩa** một **kiểu dữ liệu mới** bằng cách nhóm các biến có các kiểu dữ liệu khác nhau lại với nhau, struct cho phép tạo ra một thực thể dữ liệu lớn hơn và có tổ chức hơn từ các thành viên (members) của nó.
</details>
<details>
  <summary><strong> Các cú pháp của Struct </strong></summary>
Có 2 cú pháp của `struct` như sau:

**Cách 1:** Khai báo `struct` với tên của kiểu **struct**
```c
struct name_struct 
{
    <data type 1> <member 1>;
    <data type 2> <member 2>;
    // ...
};
```
Khi muốn khai báo các biến khác dựa theo kiểu dữ liệu này ta sử dụng cú pháp
```c
struct name_struct var1, var2, var3;
```
**Cách 2:** Khai báo `typedef` cùng `struct` với tên của kiểu **struct**
```c
typedef struct name_struct
{
    <data type 1> <member 1>;
    <data type 2> <member 2>;
    // ...
} name_struct;
```
Hoặc
```c
typedef struct
{
    <data type 1> <member 1>;
    <data type 2> <member 2>;
    // ...
} name_struct;
```
Khi muốn khai báo các biến khác dựa theo kiểu dữ liệu này ta sử dụng cú pháp
```c
name_struct var1, var2, var3;
```
Đối với biến và biến con trỏ sẽ có cấu trúc truy cập biến thành viên khác nhau. Ví dụ ta có kiểu struct như sau:
```c
typedef struct
{
   char *name; // thành viên
   int stt;
} user;

user u1, *u2, u3; // u1 và u3 là biến thường còn u2 là biến con trỏ
```
Để truy xuất biến thành viên đối với biến thường ta sử dụng kí tự `.`
```c
u1.name = "name1";
u1.stt = 1;
```
Để truy xuất biến thành viên đối với biến con trỏ ta sử dụng kí tự `->`
```c
u2->name = "name2";
u2->stt = 2;
```
</details>
<details>
  <summary><strong> Tính kích thước Struct </strong></summary>
  
Đối với các kiểu dữ liệu thông thường thì sẽ có quy định về kích thước cố định, còn đối với kiểu **struct** thì kích của nó sẽ phụ thuộc vào các biến thành phần và canh hàng của trình biên dịch. Ví dụ ta có đoạn code sau:

```c
#include <stdio.h>
#include <stdint.h>

typedef struct
{
   uint8_t a;  // 1 byte
   uint8_t b;  // 1 byte
   uint16_t c; // 2 byte
} user;

int main()
{  

   printf("Size of struct: %ld\n", sizeof( user));
   
   return 0;
}
```
```
Size of struct: 4
```
Ở ví dụ trên thì kích thước của **struct** sẽ bằng tổng kích thước của ba biến thành viên

Ở một ví dụ khác như sau:

```c
#include <stdio.h>
#include <stdint.h>

typedef struct
{
   uint8_t a;     // 1 byte
   uint16_t b;    // 2 byte
   uint32_t c;    // 4 byte
} user;

int main()
{  

   printf("Size of struct: %ld\n", sizeof( user));
   
   return 0;
}
```
```
Size of struct: 8
```
Mặc dù tổng kích thước các biến thành viên là bằng 7 tuy nhiên kích thước của **struct** lại bằng 8 điều này là do có sự tác động của canh hàng dữ liệu (**data alignment**). Canh hàng (alignment) trong C là quy tắc xác định vị trí trong bộ nhớ mà các biến (hoặc thành viên trong struct) phải được lưu trữ — thường là tại địa chỉ chia hết cho kích thước của kiểu dữ liệu đó. Điều này giúp CPU truy xuất dữ liệu hiệu quả hơn.

Một biến có kiểu dữ liệu T (ví dụ: int, float) thường được lưu tại địa chỉ chia hết cho sizeof(T).
+ `char` → 1 byte → không cần canh hàng.
+ `short` → 2 byte → cần canh hàng tại địa chỉ chia hết cho 2.
+ `int` / `float` → 4 byte → canh hàng tại địa chỉ chia hết cho 4.
+ `double` → 8 byte → cần canh hàng tại địa chỉ chia hết cho 8.
Các biến thành viên trong biến **struct** sẽ có vùng địa chỉ nằm liền kề nhau
Đối với kiểu **struct** trên ví dụ
```c
typedef struct
{
   uint8_t a;     // 1 byte
   uint16_t b;    // 2 byte
   uint32_t c;    // 4 byte
} user;
```
Máy sẽ sắp xếp như hình sau

![image](https://github.com/user-attachments/assets/1f2791da-48f5-4873-a4c3-b1419b16631a)

Padding (byte đệm) là các byte **thêm vào giữa các thành viên** để đảm bảo alignment. Các biến thành viên sẽ được cấp phát địa chỉ theo thứ tự các biến được khai báo (vd: a->b->c). Các biến thành viên sau mỗi lần cấp phát sẽ giống nhau về số lượng bytes và lấy kích thước biến lớn nhất làm chuẩn. Sau đó dựa trên kích thước thực tế của các biến mà quyết định biến đó sử dụng bao nhiêu bytes, các bytes dư sẽ được sử dụng cho biến khác hoặc dùng cho padding để các biến sau có địa chỉ phù hợp. Ta có ví dụ khác:
```c
typedef struct
{
   uint8_t  a;    // 1 byte
   uint32_t b;    // 4 byte
   uint16_t c;    // 2 byte
} user;
```
```c
Size of struct: 12
```
Cấu trúc trên sẽ được sắp xếp như sau

![image](https://github.com/user-attachments/assets/d89f5b00-6c88-4366-bccf-3ddd84d93b39)

Mặc dù kiểu **struct** trên chiếm dụng 12 bytes nhưng thực tế nó chỉ sử dụng 7 bytes tất cả và dư 5 bytes không dữ liệu (5 padding). Như vậy kích thước của một **struct** bằng tổng kích thước của các biến thành viên và các padding phát sinh.
Ta có một ví dụ khác với kiểu mảng như sau:
```c
struct Example1 
{
    uint8_t  arr1[5];  // 5x1  
    uint16_t arr2[4];  // 4x2  
    uint32_t arr3[2];  // 2x4
};
```
Như vậy đối với kiểu struct này ta sẽ có kích thước mà chưa tính tới phát sinh padding là 5x1 + 4x2 + 2x4 = 21 bytes. Ta có hiểu kiểu struct trên khai báo như sau:
```c
struct Example1 
{
uint8_t  arr1[0];
uint8_t  arr1[1];
uint8_t  arr1[2];
uint8_t  arr1[3];
uint8_t  arr1[4];
uint16_t arr2[0];  
uint16_t arr2[1]; 
uint16_t arr2[2]; 
uint16_t arr2[3];  
uint32_t arr3[0];
uint32_t arr3[1];
};
```
Chúng ta sẽ phân tích từng phần một, đối với mảng `arr1` với kích thước từng thành phần chỉ là một byte nên sắp xếp thoải mái mà không cần căn hàng

![image](https://github.com/user-attachments/assets/55a5972a-07a5-4098-aac9-f6d3f5e8e2f6)

Đến mảng `arr2` lúc này các thành phần trong mảng có kích thước là 2 bytes nên cần được đặt ở vị trí chia hết cho 2, do ban đầu ở mảng arr1 đã được cấp phát 4 bytes (do arr3 có kích thước lớn nhất là 4 bytes) còn dư 3 bytes nên 2 bytes tiếp theo sẽ được cấp phát cho `arr2[0]` còn dư 1 byte padding

![image](https://github.com/user-attachments/assets/0bd89608-4009-4fdb-b97f-82d28d9e0ee2)

Cuối cùng sau khi cấp phát xong cả mảng `arr2` thì vẫn còn dư 2 bytes, tuy nhiên mảng `arr3` có kích thước là 4 bytes nên 2 bytes này không đủ nên chuyển thành padding để chọn vị trí chia hết cho 4 cho mảng `arr3`

![image](https://github.com/user-attachments/assets/8f93b73b-099f-4ec6-b9d0-0c2550e1fbbb)

Như vậy kích thước của struct này là bằng 21 bytes dữ liệu cộng với 3 bytes padding là bằng 24 bytes cùng là bội số của kích thước của biến lớn nhất là 4 bytes luôn

</details>
</details>
<details>
  <summary><strong> Bit field </strong></summary>

  Trong C, “**bit field**” (trường bit) là một thành phần đặc biệt của cấu trúc (struct) cho phép bạn chỉ định số lượng bit cụ thể dùng để lưu trữ một biến số nguyên. Thay vì sử dụng toàn bộ kích thước của một kiểu dữ liệu, bạn có thể “cắt nhỏ” bộ nhớ theo số bit cần thiết, giúp tiết kiệm không gian bộ nhớ và mô tả chính xác hơn ý nghĩa của dữ liệu (ví dụ: lưu trạng thái bật/tắt chỉ cần 1 bit).

Ta có cú pháp như sau:

```c
struct name_struct 
{
    <data type 1> <member 1> : <number of bits>;
    <data type 2> <member 2> : <number of bits>;
    // ...
};
```
Ví dụ mẫu

```c
struct Example 
{
    int32_t flag  : 1;	// chỉ sử dụng 1 trong 32 bit
    int64_t count : 4;	// chỉ sử dụng 4 trong 64 bit
};
```

Số bit mà ta chỉ định trực tiếp sẽ giới hạn phạm vi giá trị có thể lưu. Ví dụ: một bit field khai báo với : 3 có thể lưu các giá trị từ 0 đến 7 (đối với unsigned).

Do khi sử dụng máy sẽ lấy ngẫu nhiên vị trí trong các bit có thể sử dụng nên không thể sử dụng toán tử lấy địa chỉ (&) trên  các thành viên bit field do máy không thể xác định được vị trí cụ thể.

Bit field được sử dụng để giảm thiểu bộ nhớ mà ta cần phải sử dụng nhưng vẫn đạt được hiệu quả. Ví dụ ta có một kiểu **struct** như sau:

```c
typedef struct
{
    uint8_t  color  ;           /**< 2-bit cho màu sắc              */
    uint8_t  power  ;           /**< 2-bit cho công suất            */
    uint8_t engine ;           /**< 1-bit cho loại động cơ         */
    uint8_t additionalOptions ;  /**< 3-bit cho các tùy chọn bổ sung */
} CarOptions;
```
Như ở đây ta kiểu **CarOptions** sẽ có kích thước bằng tổng số bytes của các biến thành viên và bằng 4 bytes. Giờ ta áp dụng bit field cho kiểu trên.
```c
typedef struct
{
    CarColor  color  : 2;           /**< 2-bit cho màu sắc              */
    CarPower  power  : 2;           /**< 2-bit cho công suất            */
    CarEngine engine : 1;           /**< 1-bit cho loại động cơ         */
    uint8_t additionalOptions : 3;  /**< 3-bit cho các tùy chọn bổ sung */
} CarOptions;
```
Sau khi sử dụng bit field thì kích thước kiểu struct sẽ bằng tổng các thành viên bằng 8 bits hay bằng 1 bytes là giảm 4 lần so với ban đầu như vẫn thực hiện được yêu cầu đề ra.
</details>
