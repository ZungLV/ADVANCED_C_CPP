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

~~~
va_list: là một kiểu dữ liệu để đại diện cho danh sách các đối số biến đổi. va_list là một typedef char* hay có thể hiểu là con trỏ kiểu char. Kiểu char chỉ có thể lưu trữ 1 kí tự trong khi đó con trỏ kiểu char có thể lưu trữ nhiều kí tự nói cách khác là 1 chuỗi. Sử dụng macro này sẽ khai báo biến mà ta mong muốn có dạng char* và dùng để lưu trữ số lượng tham số không xác định.
~~~

~~~
va_start: Bắt đầu một danh sách đối số biến đổi. Nó cần được gọi trước khi truy cập các đối số biến đổi đầu tiên.
~~~

~~~
