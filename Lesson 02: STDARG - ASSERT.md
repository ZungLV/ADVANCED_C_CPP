# STDARG - ASSERT
## STDARG
Cung cấp các phương thức để làm việc với các hàm có số lượng input parameter không cố định.\
Các hàm như `printf` và `scanf` là ví dụ điển hình.\
Để có thể sử dụng thư viện `stdarg` ta cần phải khai báo thư viện
~~~
#include <stdarg.h>
~~~
### Những thành phần trong thư viện stdarg
 `...` : đại diện cho số lượng tham số không xác định trong hàm
~~~
#include <stdio.h>
#include <stdarg.h>

int sum(int count, ...)   // count là số lượng tham số tham gia
~~~
`va_list`: là một kiểu dữ liệu để đại diện cho danh sách các đối số biến đổi. `va_list` là một `typedef char*` hay có thể hiểu là con trỏ kiểu `char`. Kiểu `char` chỉ có thể lưu trữ 1 kí tự trong khi đó con trỏ kiểu `char` có thể lưu trữ nhiều kí tự nói cách khác là 1 chuỗi. Sử dụng macro này sẽ khai báo biến mà ta mong muốn có dạng `char*` và dùng để lưu trữ số lượng tham số không xác định.
~~~
#include <stdio.h>
#include <stdarg.h>

int sum(int count, ...)   // count là số lượng tham số tham gia
{
    va_list args;         // typedef char* va_list
                          // char* args
                          // sum(4,1,2,3,4) => args = "4,1,2,3,4"
~~~
`va_start`: Bắt đầu một danh sách đối số biến đổi. Nó cần được gọi trước khi truy cập các đối số biến đổi đầu tiên. Có dạng `va_start(v,l)` với `v` là value là danh sách các đối số biến đổi mong muốn, `l` là label dùng để so sánh với các phần tử trong danh sách `v`. Hàm `va_start` sẽ dò từng phần tử trong `v` và khi tìm ra phần tử trùng khớp với `l`, mọi phần tử sau `l` sẽ được nhập vào `v`. Dựa trên trình biên dịch các phần tử sẽ được lưu dưới dạng chuỗi hoặc dạng mảng.
~~~
#include <stdio.h>
#include <stdarg.h>

int sum(int count, ...)     // count là số lượng tham số tham gia
{
    va_list args;           // typedef char* va_list
                            // char* args
                            // sum(4,1,2,3,4) => args = "4,1,2,3,4"
    va_start(args, count);  // arg = "1,2,3,4" or arg = {"\001","\002","\003","\004"} 
~~~
`va_arg`: Truy cập một đối số trong danh sách. Hàm này nhận một đối số của kiểu được xác định bởi tham số thứ hai. Mỗi lần gọi `va_arg` sẽ truy cập đối số đằng sau đối số của lần gọi trước.
~~~
#include <stdio.h>
#include <stdarg.h>

int sum(int count, ...)                 // count là số lượng tham số tham gia
{
    va_list args;                       // typedef char* va_list
                                        // char* args
                                        // sum(4,1,2,3,4) => args = "4,1,2,3,4"
    va_start(args, count);              // arg = "1,2,3,4" or arg = {"\001","\002","\003","\004"}
    int result = 0;         
    for (int i = 0; i < count; i++)     // lặp số lần bằng count
    {
        result += va_arg(args, int);    // gọi và cộng dồn giá trị cho result
    }
~~~
`va_end`: Kết thúc việc sử dụng danh sách đối số biến đổi. Dùng khi để thu hồi con trỏ động đã cấp phát trước đó bằng `va_list`. Nó cần được gọi trước khi kết thúc hàm.
~~~
#include <stdio.h>
#include <stdarg.h>

int sum(int count, ...)                 // count là số lượng tham số tham gia
{
    va_list args;                       // typedef char* va_list
                                        // char* args
                                        // sum(4,1,2,3,4) => args = "4,1,2,3,4"
    va_start(args, count);              // arg = "1,2,3,4" or arg = {"\001","\002","\003","\004"}
    int result = 0;         
    for (int i = 0; i < count; i++)     // lặp số lần bằng count
    {
        result += va_arg(args, int);    // gọi và cộng dồn giá trị cho result
    }
    va_end(args);
    return result;
}

int main()
{
    printf("Sum: %d\n", sum(4, 1, 2, 3, 4));
    return 0;
}
~~~
~~~
Sum: 10
~~~
Tuy nhiên đoạn code trên bị phụ thuộc vào biến count để hoạt động, không còn đúng với yêu cầu sử lý số lượng tham số không xác định. Để giải quyết vấn đề này ta có thể áp dụng macro variadic.
~~~
#include <stdio.h>
#include <stdarg.h>

#define sum_fn(...) sum(__VA_ARGS__,'a')          // sum_fn( 1, 2, 3, 4, 5) => sum( 1, 2, 3, 4, 5,'a')

int sum(int begin, ...)                           // begin là phần tử đầu tiên tham gia
{
    va_list args;                                 // typedef char* va_list
                                                  // char* args
                                                  // sum(1,2,3,4,5,a) => args = "2,3,4,5,a"
    va_start(args, begin);                        // arg = "2,3,4,5,a" or arg = {"\002","\003","\004","\005", "\015"}

    int result = begin;                           // để cộng dồn sau khi danh sách mất đi số đầu tiên

    int value;                                    // sử dụng biến value làm biến trung gian để tránh lỗi sót số do gọi va_arg

    while((value = va_arg(args, int)) !='a')      // dò tìm kí tự 'a' để kết thúc vòng lặp
    {
       result += value;
    }
    va_end(args);
    return result;
}

int main()
{
    printf("Sum: %d\n", sum_fn( 1, 2, 3, 4, 5));
    return 0;
}
~~~
Mặc dù đoạn code trên đã giải quyết được vấn đề nhập số lượng cho các đối số không xác định. Tuy nhiên việc ép kiểu hàng loạt thành một loại do `va_arg` khiến cho việc xác định danh sách không chính xác do kí tự 'a' với các tham số là hai kiểu khác nhau. Để giải quyết vấn đề này ta sẽ áp dụng thành phần cuối cùng trong thư viện starg.\
`va_copy`: Sao chép dữ liệu giữa 2 biến cùng kiểu `va_list`.
~~~
#include <stdio.h>
#include <stdarg.h>

#define sum_fn(...) sum(__VA_ARGS__,'a')                // sum_fn( 1, 2, 3, 4, 5) => sum( 1, 2, 3, 4, 5,'a')

int sum(int begin, ...)                                 // begin là phần tử đầu tiên tham gia
{
    va_list args;                                       

    va_list check;

    va_start(args, begin);                              // arg = "2,3,4,5,a" or arg = {"\002","\003","\004","\005", "\015"}
    va_copy(check, args);                               // check là 1 bản sao của args dùng để so sánh trong khi args dùng để tính tổng

    int result = begin;                                 // để cộng dồn sau khi danh sách mất đi số đầu tiên

    

    while ( va_arg(check, char*) != (char*)'a')          // do kí tự 'a' trong va_arg có thể là mảng "\015" nên cần ép kiểu char* để chứa
    {
       result += va_arg(args, int);
    }

    va_end(args);                                       // khi thu hồi args thì cũng thu hồi check
    return result;
}

int main()
{
    printf("Sum: %d\n", sum_fn( 1, 2, 3, 4, 5));
    return 0;
}
~~~
~~~
Sum: 15
~~~
Như vậy ta không phải xác định rõ số lượng đối số tham gia mới có thể sự dụng hàm, giúp code linh hoạt hơn trong việc sử dụng.
## ASSERT
Thư viện `assert.h` trong C cung cấp các macro để kiểm tra các điều kiện giả định trong chương trình. Nếu điều kiện không thỏa mãn, chương trình sẽ dừng lại và thông báo lỗi, giúp ta phát hiện lỗi trong quá trình phát triển.
+ Kiểm tra giả định: Giúp coder kiểm tra các giả định hoặc điều kiện mà chương trình phải thỏa mãn trong quá trình phát triển.

+ Giúp phát hiện lỗi sớm: Khi điều kiện giả định không đúng, chương trình sẽ dừng lại và thông báo lỗi, giúp bạn phát hiện lỗi nhanh chóng trong quá trình phát triển.

+ Hỗ trợ debug: assert giúp tìm ra các lỗi logic và bảo vệ chương trình khỏi các tình huống không lường trước được.
  
  Để sử dụng assert, cần bao gồm thư viện `assert.h` trong chương trình:
 ~~~
#include <assert.h>

 int main() {
     int x = 5;
     assert(x == 5);  // Nếu điều này sai, chương trình sẽ dừng lại
     assert(x != 5);  // Nếu điều này sai, chương trình sẽ dừng lại
     return 0;
 }
 ~~~
 Do khai báo `x=5` khi chạy sẽ báo lỗi ở line 6 của chương trình main.c.
 ~~~
 main: main.c:6: main: Assertion `x != 5' failed.
 ~~~
 Còn khi khai báo số khác 5:
 ~~~
 #include <assert.h>

 int main() {
     int x = 10;
     assert(x == 5);  // Nếu điều này sai, chương trình sẽ dừng lại
     assert(x != 5);  // Nếu điều này sai, chương trình sẽ dừng lại
     return 0;
 }
 ~~~
 Khi chạy sẽ báo lỗi ở line 5 của chương trình main.c.
 ~~~
 main: main.c:5: main: Assertion `x == 5' failed.
 ~~~
Ngoài ra ta còn có thể áp dụng macro để debug, giúp ta có gợi ý để hiểu rõ lỗi gặp phải là gì và thuận tiện hơn trong việc quản lý code.
 ~~~
 #include <assert.h>

 // Macro dùng để debug
 #define LOG(condition, cmd) assert(condition && #cmd)
 
 int main() {
     int x = 10;
     LOG(x == 5, x phải bằng 5);  // Nếu điều này sai, chương trình sẽ dừng lại
     LOG(x != 5, x phải khác 5);  // Nếu điều này sai, chương trình sẽ dừng lại
     return 0;
 }
 ~~~
Sử dụng toán tử && assert có 2 điều kiện để thực thi. Vì `#cmd` đã được chuẩn hóa thành chuỗi nên bất cứ điều kiện nào của `cmd` cũng đúng do là chuỗi, nên assert sẽ chỉ so sánh điều kiện đầu. Khi chạy chương trình ta sẽ có kết quả như sau.
 ~~~
 main: main.c:8: main: Assertion `x == 5 && "x phải bằng 5"' failed.
 ~~~
