# Compiler - Macro
## 1. Compiler
Để 2 người từ hai nước khác nhau có thể hiểu tiếng nhau thì sẽ cần phải thông qua một quá trình học hỏi hay nhờ phiên dịch viên. Cũng tương tự như vậy, để máy tính có thể hiểu được mệnh lệnh của con người, ta cần phải đi qua các bước phức tạp để có thể phiên dịch từ ngôn ngữ bậc cao (như là C/C++) sang ngôn ngữ bậc thấp (ngôn ngữ máy).\
Các bước đó có quy trình như sau: Preprocessor -> Compiler -> Asembler -> Linker\
![Screenshot 2025-04-14 204059](https://github.com/user-attachments/assets/46d61cc2-3908-401f-be4c-629c5dc3c5f6)
### 1.1 Preprocessor
Quá trình Preprocessor (tiền xử lý) là bước đầu tiên trong quá trình biên dịch mã nguồn, đặc biệt trong các ngôn ngữ như C/C++. Trong quá trình này sẽ sử lý các chỉ thị bắt đầu bằng dấu # (gọi là preprocessor directives) trước khi mã được biên dịch thực sự. Qúa trình tiền xử lý sẽ thực hiện các công việc như sau.
+ Nhận mã nguồn: Nhận các file .c (như main.c) và các file .h mà đã được chèn nội dung
  ~~~
  #include <stdio.h>
  #include "a.h"
  #include "b.h"
  ~~~
  Trình tiền xử lý sẽ thay #include <stdio.h>, include "a.h" và include "b.h" bằng toàn bộ nội dung file stdio.h, a.h và b.h
+ Xóa bỏ tất cả chú thích, comments của chương trình:
  ~~~
  // Mọi comments được đặt sau dấu "//" sau quá trình tiền xử lý sẽ bị xóa sạch
  ~~~
+ Chỉ thị tiền xử lý (bắt đầu bằng #) cũng được xử lý
  ~~~
  #define PI 3.14
  // --> Sau tiền xử lý: mọi PI sẽ thành 3.14
  ~~~
  Để chạy giai đoạn tiền xử lý (preprocessing) trong quá trình biên dịch với GCC của VSCODE ta sử dụng lệnh như sau
  ~~~
  gcc -E main.c -o main.i
  ~~~
  
  Trong đó
  + gcc: Gọi trình biên dịch GCC

  + -E: Chỉ thực hiện bước tiền xử lý, không biên dịch, không liên kết

  + main.c: Tệp mã nguồn C đầu vào

  + -o main.i: Ghi kết quả đầu ra vào tệp main.i

  Các file .c sau quá trình tiền xử lý sẽ tạo thành các file .i chứa mã nguồn đã được xử lý.\
Vd: main.c và test.c sẽ tạo thành main.i và test.i
  ![Screenshot 2025-04-14 211343](https://github.com/user-attachments/assets/5d93f4ee-42bb-41b4-a399-b46ffebba92f)

### 1.2 Compiler

Ở quá trình compiler, file .i được tạo ra ở quá trình preprocessor sẽ được sử dụng để tạo file .s (file này chứa ngôn ngữ assembly một ngôn ngữ bậc thấp). Assembly (hợp ngữ) giúp thao tác trên RAM mượt mà hiệu quả hơn.\

Thực hiện bước biên dịch (compilation) và xuất ra mã hợp ngữ (assembly), sài lệnh cụ thể là:
~~~
gcc -S main.i -o main.s
~~~

Trong đó
+ gcc:	Gọi trình biên dịch GCC
  
+ -S:	Viết tắt của "Assembly only" – chỉ dịch mã nguồn thành mã hợp ngữ
  
+ main.i:	File đầu vào – mã C đã qua tiền xử lý
  
+ -o main.s:	Tên file đầu ra chứa mã hợp ngữ

Vd: main.i và test.i sẽ tạo thành main.s và test.s

![Screenshot 2025-04-15 220543](https://github.com/user-attachments/assets/af7f6cd7-702a-4f4e-9412-f7e85cd22370)

### 1.3 Asembler

Quá trình này sẽ tổng hợp file .s thành file .o (file này chứa mã nhị phân là ngôn ngữ máy).\

Để biên dịch mã hợp ngữ (assembly) thành mã máy (object code), ta sài lệnh cụ thể như sau

~~~
gcc -c main.s -o main.o
~~~

Trong đó
+ gcc:	Gọi trình biên dịch GCC

+ -c:	Compile only – chỉ biên dịch, không liên kết

+ main.s:	File mã hợp ngữ đầu vào (được tạo từ gcc -S)

+ -o main.o:	Xuất kết quả ra file object (.o)

Vd: main.s và test.s sẽ tạo thành main.o và test.o
![Screenshot 2025-04-15 220630](https://github.com/user-attachments/assets/8ce5f730-9e9c-4045-8ebf-7fc035943c47)

### 1.4 Linker

Linker là quá trình cuối cùng, tất cả các file .o sẽ được liên kết để tạo ra một file .exe cuối cùng và đây cũng là file thực thi code.\

Thực hiện giai đoạn liên kết (linking), cụ thể là lệnh
~~~
gcc main.o test.o -o main
~~~

Trong đó
+ gcc:	Gọi trình biên dịch GCC

+ main.o test.o:	Các file object (.o) – đã được biên dịch

+ -o main:	Đặt tên cho file thực thi đầu ra là main

Vd: main.o liên kết với test.o tạo thành main.exe mà ta có thể chạy với lệnh
~~~
./main
~~~
![Screenshot 2025-04-15 220655](https://github.com/user-attachments/assets/90484a32-c616-4d94-8085-2cb1126a3765)

## 2. Macro
Macros là từ hay chỉ thị dùng để chỉ những thông tin được xử lý ở quá trình tiền xử lý.\
Macros chia làm 3 nhóm chính:\
+Chỉ thị bao hàm tệp (#include)\
+Chỉ thị định nghĩa, hủy định nghĩa (#define, #undef)\
+Chỉ thị biên dịch có điều kiện (#if, #elif, #else, #ifdef, #ifndef, #endif)\
### 2.1 Include
#include trong C/C++ là một chỉ thị của preprocessor dùng để sao chép toàn bộ các file source code vào file .i\
  ~~~
  #include <stdio.h>
  #include <math.h>
  #include "a.h"
  #include "b.h"
  ~~~
Ta có thể thấy include có hai dạng <> và ""\
Đối với #include <> sẽ được áp dụng với các thư viện chuẩn (như stdio.h, math.h), máy sẽ tìm những thư viện này trong thư viện hệ thống trước hay ở trong các thư mục cài đặt. Còn đối với #include "" áp dụng với các code tự viết (như a.h, b.h) máy sẽ tìm trong thư mục hiện tại trước.	
### 2.2 Define
+ Macro là một khái niệm dùng để định nghĩa một tập hợp các hướng dẫn tiền xử lý
+ Dùng để thay thế một chuỗi mã nguồn bằng một chuỗi khác trước khi chương trình biên dịch.
+ Giúp giảm lặp lại mã, dễ bảo trì chương trình.
+ Macro được định nghĩa bằng cách sử dụng chỉ thị tiền xử lý #define
### Sử dụng define để định nghĩa biến
  ~~~
  #define PI 3.14
  ~~~
Mọi biến PI trong chương trình sẽ được đổi thành 3.14
### Sử dụng define để tạo một macro tính toán
~~~
#include <stdio.h>

// Macro để tính gấp đôi của một số
#define DOUBLE(x) ((x) * 2)

int main()
{
    // Sử dụng macro để tính gấp đôi một biến
    int result = DOUBLE(5);
    printf("Kết quả ta được là: %d\n", result);
    return 0;
}
~~~
Kết quả ta được
~~~
Kết quả ta được là: 10
~~~
### Sử dụng define để định nghĩa một hàm bất kì
~~~
#include <stdio.h>

#define DISPLAY_MULTI(a,b)                    \
printf("Đây là macro nhân 2 biến bất kì\n");  \
printf("Kết quả: %d", a*b);

int main()
{
    DISPLAY_MULTI(5,4)
    return 0;
}
~~~
Ta sử dụng các dấu "\\" để liên kết giữa các hàng\
Kết quả ta được
~~~
Đây là macro nhân 2 biến bất kì
Kết quả: 20
~~~
### Sử dụng undef để hủy định nghĩa
Để hủy định nghĩa đã được lập sẵn, ta có thể sử dụng #undef
~~~
// Hủy định nghĩa YEAR và định nghĩa lại YEAR
#define YEAR 2024
#undef YEAR
#define YEAR 2025
~~~
### Chỉ thị biên dịch có điều kiện (#if, #elif, #else)
+ #if sử dụng để bắt đầu một điều kiện tiền xử lý.
+ Nếu điều kiện trong #if là đúng, các dòng mã nguồn sau #if sẽ được biên dịch
+ Nếu sai, các dòng mã nguồn sẽ bị bỏ qua đến khi gặp #endif
+ #elif dùng để thêm một điều kiện mới khi điều kiện trước đó trong #if hoặc #elif là sai
+ #else dùng khi không có điều kiện nào ở trên đúng.
~~~
#define TRAFFIC_LIGHT GREEN

// Ứng với mỗi 1 màu của đèn giao thông mà ta sẽ có hiệu lệnh riêng

#if TRAFFIC_LIGHT == GREEN
    printf("Được chạy\n");
#elif TRAFFIC_LIGHT == RED
    printf("Dừng lại\n");
#elif TRAFFIC_LIGHT == YELLOW
    printf("Chạy chậm\n");
#else 
    printf("Đèn không hợp lệ\n");
#endif
~~~
### Chỉ thị biên dịch có điều kiện (#ifdef, #ifndef)
+ #ifdef dùng để kiểm tra một macro đã được định nghĩa hay chưa, nếu macro đã được định nghĩa thì mã nguồn sau #ifdef sẽ được biên dịch.
+ #ifndef dùng để kiểm tra một macro đã được định nghĩa hay chưa, nếu macro chưa được định nghĩa thì mã nguồn sau #ifndef sẽ được biên dịch.
+ Khi sử dụng chỉ thị biên dịch có điều kiện, các file header phải viết hoa hết toàn bộ. Vd: File test.h sẽ được viết thành __TEST_H
~~~
#ifndef __TEST_H
#define __TEST_H

#endif
~~~
Cú pháp trên giúp định nghĩa lại macro nếu chưa được định nghĩa, nhờ vậy mà tránh lỗi do khai báo thư viện nhiều lần.
### 2. Toán tử trong macro
### Toán tử ## để nối các chuỗi
~~~
#include <stdio.h>
#define DECLARE_VARIABLE(prefix, number) int prefix##number
int main()
{
    // Sử dụng macro để khai báo các biến động
    DECLARE_VARIABLE(var, 1); // int var1;
    DECLARE_VARIABLE(var, 2); // int var2;
    return 0;
}
~~~
Sau khi preprocessor ta sẽ được hai biến mới là var1 và var2 được nối với nhau bằng toán tử "##"

### Toán tử # chuẩn hóa lên chuỗi

~~~
#include <stdio.h>
#define CREATE_STRING(txt) print(#txt) // hàm in ra bất cứ văn bản nào được đặt trong hàm
int main()
{
    CREATE_STRING(ADVANCED_C) // hàm này sẽ trả về print("ADVANCED_C)
    return 0;
}
~~~
Nhờ có toán tử # mà đoạn văn bản ADVANCED_C trở thành chuỗi, khi đó ta có kết quả in được
~~~
ADVANCED_C
~~~

### Macro Variadic 

+ Là một dạng macro cho phép nhận một số lượng biến tham số có thể thay đổi.
+ Giúp định nghĩa các macro có thể xử lý một lượng biến đầu vào khác nhau

~~~
#define MACRO_NAME(...) __VA_ARGS__  // __VA_ARGS__ sẽ tự hiểu là những biến trong ...
~~~

Ví dụ về một hàm tính tổng sử dụng Macro Variadic 

~~~
#include <stdio.h>

#define DISPLAY_SUM(...)                      \
int arr[] = {__VA_ARGS__, 'n'}                \
int sum = 0;                                  \
int i = 0;                                    \
while (arr[i] != 'n'){                        \
    sum += arr[i];                            \
    i++;                                      \
}                                             \
printf("Kết quả: %d", sum);

int main()
{
    DISPLAY_SUM(1,2,3,4,5);
    return 0;
}
~~~

__VA_ARGS__ sẽ nhận về cho mảng các số đã được nhập vào hàm DISPLAY_SUM() khi đó ta có mảng int arr[] = {__VA_ARGS__, 'n'} sẽ thành int arr[] = {1,2,3,4,5, 'n'} để lại 'n' là phần tử cuối cùng trong mảng. Nhờ vậy mảng arr[] sẽ quét qua tất cả các số trước khi tới 'n' (thoát khỏi vòng while) và ta được một hàm tính tổng từ 1 tới 5.

~~~
Kết quả: 15
~~~




  

