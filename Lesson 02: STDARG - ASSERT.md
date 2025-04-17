# STDARG - ASSERT
## STDARG
Cung cấp các phương thức để làm việc với các hàm có số lượng input parameter không cố định.\
Các hàm như printf và scanf là ví dụ điển hình.\
Để có thể sử dụng thư viện stdarg ta cần phải khai báo thư viện
~~~
#include <stdarg.h>
~~~
### Những thành phần trong thư viện stdarg
 ... : đại diện cho số lượng tham số không xác định trong hàm
~~~
#include <stdio.h>
#include <stdarg.h>

int sum(int count, ...)   // count là số lượng tham số tham gia
~~~
va_list: là một kiểu dữ liệu để đại diện cho danh sách các đối số biến đổi. va_list là một typedef char* hay có thể hiểu là con trỏ kiểu char. Kiểu char chỉ có thể lưu trữ 1 kí tự trong khi đó con trỏ kiểu char có thể lưu trữ nhiều kí tự nói cách khác là 1 chuỗi. Sử dụng macro này sẽ khai báo biến mà ta mong muốn có dạng char* và dùng để lưu trữ số lượng tham số không xác định.
~~~
#include <stdio.h>
#include <stdarg.h>

int sum(int count, ...)   // count là số lượng tham số tham gia
{
    va_list args;         // typedef char* va_list
                          // char* args
                          // sum(4,1,2,3,4) => args = "4,1,2,3,4"
~~~
va_start: Bắt đầu một danh sách đối số biến đổi. Nó cần được gọi trước khi truy cập các đối số biến đổi đầu tiên. Có dạng va_start(v,l) với v là value là danh sách các đối số biến đổi mong muốn, l là label dùng để so sánh với các phần tử trong danh sách v. Hàm va_start sẽ dò từng phần tử trong v và khi tìm ra phần tử trùng khớp với l, mọi phần tử sau l sẽ được nhập vào v. Dựa trên trình biên dịch các phần tử sẽ được lưu dưới dạng chuỗi hoặc dạng mảng.
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
va_arg: Truy cập một đối số trong danh sách. Hàm này nhận một đối số của kiểu được xác định bởi tham số thứ hai. Mỗi lần gọi va_arg sẽ truy cập đối số đằng sau đối số của lần gọi trước.
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
va_end: Kết thúc việc sử dụng danh sách đối số biến đổi. Dùng khi để thu hồi con trỏ động đã cấp phát trước đó bằng va_list. Nó cần được gọi trước khi kết thúc hàm.
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
Mặc dù đoạn code trên đã giải quyết được vấn đề nhập số lượng cho các đối số không xác định. Tuy nhiên việc ép kiểu hàng loạt thành một loại do va_arg khiến cho việc xác định danh sách không chính xác do kí tự 'a' với các tham số là hai kiểu khác nhau. Để giải quyết vấn đề này ta sẽ áp dụng thành phần cuối cùng trong thư viện starg.\
va_copy: Sao chép dữ liệu giữa 2 biến cùng kiểu va_list.
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

    

    while ( va_arg(check, char*) != (char*)'a')          // dò tìm kí tự 'a' để kết thúc vòng lặp
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
d
~~~
