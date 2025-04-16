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
    for (int i = 0; i < count; i++)
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
    for (int i = 0; i < count; i++)
    {
        result += va_arg(args, int);    // gọi và cộng dồn giá trị cho result
    }
    va_end(args);
    return result;
}
~~~
~~~
Sum: 10
~~~
Tuy nhiên đoạn code trên bị phụ thuộc vào biến count để hoạt động, không còn đúng với yêu cầu sử lý số lượng tham số không xác định. Để giải quyết vấn đề này ta có thể áp dụng macro variadic.
