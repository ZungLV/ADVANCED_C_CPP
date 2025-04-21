<details>
  <summary><strong>👆 Pointer </strong></summary>

<details>
  <summary><strong>🎯 Tổng quan về pointer </strong></summary>

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
Chỉ có thể lấy địa chỉ của một biến đã tồn tại, khai báo từ trước. Để lấy địa chỉ ta đơn giản viết `&` đi cùng với biến mong muốn (trong ví dụ trên là biến `x`) và để con trỏ của ta `int_ptr` trỏ vào đó. Biến `int_ptr` khi này đã thành công lưu trữ địa chỉ của biến `x` sẵn sàng cho việc thao tác sau này.

### Truy cập giá trị (giải tham chiếu - dereference)

```C
int x = 10;
int *int_ptr = &x;
int y = *int_ptr;
```
Cuối cùng sau khi đã khai báo và lưu địa chỉ thành công, ta tiến hành truy cập giá trị. Giả sử biến `x` có địa chỉ là 0x01 thì:

+ `int_ptr`: chứa địa chỉ của `x`, `int_ptr = 0x01`

+ `*int_ptr`: lấy giá trị tại địa chỉ int_ptr trỏ vào, `*int_ptr = *(0x01) = 10`

+ `y`: bằng với giá trị của `x`, `y = 10`

### Thay đổi giá trị gián tiếp

Thay vì trực tiếp thay đổi giá trị của `x` ta có thể dùng con trỏ `int_ptr` để làm điều đó:

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

+ Giá trị của `x` mới bằng đúng với giá trị sau khi thay đổi con trỏ `int_ptr` là 100

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
dvalue a is: 20
value b is: 10
```
Sau khi sử dụng hàm thì hai biến `a` và `b` đã hoán đổi giá trị cho nhau.
</details>

</details>


<details>
  <summary><strong>👥 Các loại con trỏ </strong></summary>

<details>
  <summary><strong> Void pointer </strong></summary>
Void pointer là một con trỏ có thể trỏ vô mọi kiểu dữ liệu mà không cần biết kiểu dữ liệu đó là gì.
  
Cú pháp: `void *ptr;`

Vì có thể truy cập mọi kiểu dữ liệu nên về bản chất con trỏ `void` không thuộc bất kì kiểu dữ liệu nào cả. Như ta đã biết, để có thể trích xuất dữ liệu từ một biến được trỏ vào, ta dựa vào kiểu dữ liệu của con trỏ để có thể xác định sẽ có bao nhiêu bytes dữ liệu được đọc. Do đó, nhược điểm của kiểu `void` là việc không thể truy xuất dữ liệu trực tiếp. Để có thể truy xuất dữ liệu thông qua con trỏ `void` ta cần phải ép kiểu thích hợp.

```c
#include <stdio.h>

int main()
{
    int value = 5;

    void *ptr = &value;
    printf("value is: %d\n", *(int*)(ptr));

    return 0;
}
```
Ta có cú pháp sau ` printf("value is: %d\n", *(int*)(ptr));`:

+ `*`: Lấy giá trỉ mà được `ptr` trỏ tới

+ `(int*)(ptr)`: Ép kiểu con trỏ thành kiểu `int`

Như vậy đoạn code trên sẽ giúp lấy ra giá trị mà `ptr` đã được ép kiểu `int` trỏ tới
```
value is: 5
```
Tuy nhiêu ưu điểm lớn nhất mà `void` lại được sử dụng chính là việc nó có thể truy cập nhiều kiểu dữ liệu khác nhau

```c
#include <stdio.h>

int main()
{
    int value = 5;
    char letter = 'x';
    double number = 10.5;

    void *ptr = &value;
    printf("Size of sizeof pointer: %ld bytes\n", sizeof(ptr));
    printf("value is: %d\n", *(int*)(ptr));
    printf("address is: %p\n", ptr); // %p là cú pháp lấy địa chỉ

    ptr = &letter;
    printf("value is: %c\n", *(char*)(ptr));
    printf("address is: %p\n", ptr); 

    ptr = &number;
    printf("value is: %f\n", *(double*)(ptr));
    printf("address is: %p\n", ptr);

    return 0;
}
```
```
Size of sizeof pointer: 8 bytes
value is: 5
address is: 0x7ffeccdca154
value is: x
address is: 0x7ffeccdca153
value is: 10.500000
address is: 0x7ffeccdca158
```
Qua đoạn code trên ta có thể thấy ptr đã trỏ qua từng kiểu `int`, `char`, `double` lấy được địa chỉ và giá trị của chúng. Thay vì phải tạo ra từng kiểu con trỏ một và tốn 8 bytes bộ nhớ cho mỗi con trỏ, việc chỉ sử dụng mỗi một con trỏ kiểu `void` giúp ta tiết kiệm đáng kể bộ nhớ phải sài.

### Mảng void pointer

```c
#include <stdio.h>

int main()
{
    char arr_txt[] = "ADVANCED_C_CPP";

    void *ptr = &arr_txt;

    printf("Value is: %c and adress is %p\n", *(char*)(ptr), ptr);

    int arr_num[] = {15,52,36,47,85};

    ptr = &arr_num;

    printf("Value is: %d and adress is %p\n", *(char*)(ptr), ptr);

    return 0;
}
```
```
Value is: A and adress is 0x7ffcbbd66019
Value is: 15 and adress is 0x7ffcbbd66000
```

Đối với kiểu mảng, thì con trỏ sẽ mặc định lấy địa chỉ và giá trị của biến đầu tiên. Vậy để lấy biến tiếp theo ta đơn giản chỉ cần cộng `ptr` với số thứ tự nhân kích thước kiểu dữ liệu.

```c
#include <stdio.h>

int main()
{
    char arr_txt[] = "ADVANCED_C_CPP";

    void *ptr = &arr_txt;
    printf("In kí tự đầu tiên và thứ 2\n");
    printf("Value is: %c and adress is %p\n", *(char*)(ptr), ptr);
    printf("Value is: %c and adress is %p\n", *(char*)(ptr+1), ptr+1);

    int arr_num[] = {15,52,36,47,85};

    ptr = &arr_num;

    printf("In số đầu tiên và thứ 2\n");
    printf("Value is: %d and adress is %p\n", *(char*)(ptr), ptr);
    printf("Value is: %d and adress is %p\n", *(char*)(ptr+4), ptr+4);

    return 0;
}
```
```
In kí tự đầu tiên và thứ 2
Value is: A and adress is 0x7ffff32d9dd9
Value is: D and adress is 0x7ffff32d9dda
In số đầu tiên và thứ 2
Value is: 15 and adress is 0x7ffff32d9dc0
Value is: 52 and adress is 0x7ffff32d9dc4
```
Đối với kiểu `char` địa chỉ của 2 kí tự cách nhau 1 bytes do kích thước của kiểu `char`, đường tương tự về kiểu `int`. Dựa vào đặc tính ấy ta có thể viết một chương trình để lấy toán bộ phần tử trong mảng.

```c
#include <stdio.h>

int main()
{
    char arr_txt[] = "ADVANCED";

    void *ptr = &arr_txt;
    printf("In mảng kí tự\n");  
    for(int i = 0; i<(sizeof(arr_txt)/sizeof(arr_txt[0])); i++)
    {
        printf("Value is: %c and adress is %p\n", *(char*)(ptr+i), ptr+i);
    }

    int arr_num[] = {15,52,36,47,85};

    ptr = &arr_num;

    printf("In mảng số \n");
    for(int i = 0; i<(sizeof(arr_num)/sizeof(arr_num[0])); i++)
    {
        printf("Value is: %d and adress is %p\n", *(int*)(ptr+(i*4)), ptr+(i*4));
    }

    return 0;
}
```
```
In mảng kí tự
Value is: A and adress is 0x7fff49a3ee0f
Value is: D and adress is 0x7fff49a3ee10
Value is: V and adress is 0x7fff49a3ee11
Value is: A and adress is 0x7fff49a3ee12
Value is: N and adress is 0x7fff49a3ee13
Value is: C and adress is 0x7fff49a3ee14
Value is: E and adress is 0x7fff49a3ee15
Value is: D and adress is 0x7fff49a3ee16
Value is:
In mảng số 
Value is: 15 and adress is 0x7fff49a3edf0
Value is: 52 and adress is 0x7fff49a3edf4
Value is: 36 and adress is 0x7fff49a3edf8
Value is: 47 and adress is 0x7fff49a3edfc
Value is: 85 and adress is 0x7fff49a3ee00
```
Ta thể thấy đối với mảng chuỗi ở địa chỉ cuối là một kí tự kiểu null. Đây là cách để chuỗi có thể phát hiện điểm kết thúc của chuỗi

Ngoài ra ta còn có thể tạo ra một mảng `void` để lưu trữ nhiều kiểu dữ liệu khác nhau

```c
#include <stdio.h>

int main()
{
    int num = 55;
    double value = 12.8;
    char text = 'x';
    void *ptr[] = {&num, &value, &text};
    printf("Value of int is: %d and adress is %p\n", *(int*) ptr[0], ptr[0]);
    printf("Value of double is: %f and adress is %p\n", *(double*) ptr[1], ptr[1]);
    printf("Value of char is: %c and adress is %p\n", *(char*) ptr[2], ptr[2]);
    return 0;
}
```
Đối với kiểu mảng phần tử đầu tiên luôn bắt đầu từ 0 do đó  `ptr[0]` sẽ trỏ vào phần tử đầu tiên trong mảng là `num` 
```
Value of int is: 55 and adress is 0x7fff12166e34
Value of double is: 12.800000 and adress is 0x7fff12166e38
Value of char is: x and adress is 0x7fff12166e33
```
Như vậy cứ tương ứng với mỗi phần tử trong mảng sẽ là địa chỉ và giá trị của biến tương ứng tại vị trí đó
</details>

<details>
  <summary><strong> Function pointer </strong></summary>
Khác với con trỏ khác con trỏ hàm là một biến mà giữ địa chỉ của một hàm. Điều đó nghĩa là, nó trỏ đến vùng nhớ trong bộ nhớ chứa mã máy của hàm được định nghĩa trong chương trình.
  
Trong ngôn ngữ lập trình C, con trỏ hàm cho phép bạn truyền một hàm như là một đối số cho một hàm khác, lưu trữ địa chỉ của hàm trong một cấu trúc dữ liệu, hoặc thậm chí truyền hàm như một giá trị trả về từ một hàm khác.
```c
<return_type> (*func_pointer)(<data_type_1>, <data_type_2>)
```
Tuy nhiên khác với con trỏ `void` con trỏ hàm buộc phải:
+ Phải giống với kiểu trả về của hàm được trỏ
+ Phải cùng có chung số lượng các tham số
+ Các tham số phải có cùng kiểu dữ liệu tương ứng

```c
#include <stdio.h>

// Hàm int
int print_int(){ printf("int function!\n"); }

// Hàm mẫu 2
double print_double(){ printf("double function!\n"); }

int main()
{
    // Khai báo con trỏ hàm
    int (*ptr_int)();
    double (*ptr_double)();
    // Gán địa chỉ của hàm cho con trỏ hàm
    ptr_int = print_int;
    ptr_double = print_double;
    // Gọi hàm thông qua con trỏ hàm
    ptr_int();  
    ptr_double();
    return 0;
}
```
```
int function!
double function!
```
Nếu đáp ứng đủ các yêu cầu trên thì một con trỏ có thể trỏ đến các hàm có cùng dữ liệu trả về và giống nhau về số lượng cũng như đặc điểm của các tham số.
```c
#include <stdio.h>

// Các hàm mẫu
void morning(){ printf("Good morning\n"); }
void afternoon(){ printf("Good afternoon\n"); }
void evening(){ printf("Good evening\n"); }

int main()
{
    // Khai báo con trỏ hàm
    void (*ptr)();
    // Gán địa chỉ của hàm cho con trỏ hàm và gọi hàm
    ptr = &morning;
    ptr();
    // Gán địa chỉ của hàm cho con trỏ hàm và gọi hàm
    ptr = afternoon;
    (*ptr)();
    // Gán địa chỉ của hàm cho con trỏ hàm và gọi hàm
    ptr = &evening;
    (*ptr)();
    return 0;
}
```
Ngoài ra khi dùng con trỏ lấy địa chỉ ta không cần đặt dấu `&` trước hàm cần lấy. Đồng thời khi gọi hàm con trỏ ta còn có thể sử dụng cú pháp `(*func_pointer)(<data_type_1>, <data_type_2>)`
```
Good morning
Good afternoon
Good evening
```
### Ứng dụng function pointer để tạo một hàm dùng để sử dụng các hàm khác
Như ta đã biết thì ta có thể truyền con trỏ vào hàm với mong muốn thay đổi giá trị thông qua các tính toán trong hàm. Và ta hoàn toàn có thể truyền function pointer vào một hàm bất kì luôn. Ví dụ ở một đoạn khai báo hàm như sau.
```c
#include <stdio.h>

void sum(int a, int b)
{
    printf("Sum of %d and %d is: %d\n",a,b, a+b);
}

void subtract(int a, int b)
{
    printf("Subtract of %d by %d is: %d \n",a,b, a-b);
}

void multiple(int a, int b)
{
    printf("Multiple of %d and %d is: %d \n",a,b, a*b );
}

void divide(int a, int b)
{
    printf("%d divided by %d is: %f \n",a,b, (double)a/b);
}

void calculator(void (*ptr)(int, int), int a, int b)
{
    printf("Program calculate: \n");
    ptr(a,b);
}
```
Ta có thể thấy các hàm `sum`,`subtract`,`multiple`,`divide` đều có cùng đặc diểm. Vì vậy hàm `void calculator` được tạo ra để tận dụng điều đó. Với con trỏ hàm `ptr` sẽ được sử dụng để trỏ vào bất cứ hàm nào trong 4 hàm trên miễn là hàm đó được truyền vào hàm `calculator` khi gọi. Sau đó hàm `calculator` sẽ thực hiện đúng chức năng của hàm được truyền vào sử dùng con trỏ hàm `ptr` và các tham số `a` và `b`
Đoạn code hoàn chỉnh
```c
#include <stdio.h>

void sum(int a, int b)
{
    printf("Sum of %d and %d is: %d\n",a,b, a+b);
}

void subtract(int a, int b)
{
    printf("Subtract of %d by %d is: %d \n",a,b, a-b);
}

void multiple(int a, int b)
{
    printf("Multiple of %d and %d is: %d \n",a,b, a*b );
}

void divide(int a, int b)
{
    printf("%d divided by %d is: %f \n",a,b, (double)a/b);
}

void calculator(void (*ptr)(int, int), int a, int b)
{
    printf("Program calculate: \n");
    ptr(a,b);
}

int main()
{
    calculator(sum,5,10);
    calculator(subtract,5,10);
    calculator(multiple,5,10);
    calculator(divide,5,10);
    return 0;
}

```
```
Program calculate: 
Sum of 5 and 10 is: 15
Program calculate: 
Subtract of 5 by 10 is: -5 
Program calculate: 
Multiple of 5 and 10 is: 50 
Program calculate: 
5 divided by 10 is: 0.500000 
```
Ngoài ra ta hoàn toàn có thể khai báo một con trỏ hàm kiểu mảng và mỗi lần gọi một phần tử trong hàm là ta được một hàm tương ứng

```c
int main()
{
    void (*calculator[])(int, int) = {sum,subtract,multiple,divide} ;   
    calculator[0](5,10);
    calculator[1](5,10);
    calculator[2](5,10);
    calculator[3](5,10);
    return 0;
}
```
```
Sum of 5 and 10 is: 15
Subtract of 5 by 10 is: -5 
Multiple of 5 and 10 is: 50 
5 divided by 10 is: 0.500000 
```
</details>
<details>
  <summary><strong> Pointer to Constant </strong></summary>
Là cách định nghĩa một con trỏ không thể thay đổi giá trị tại địa chỉ mà nó trỏ đến thông qua dereference nhưng giá trị tại địa chỉ đó có thể thay đổi. Con trỏ này hoàn toàn có thể trỏ vào con trỏ khác có cùng kiểu dữ liệu.
Cú pháp:

  
```c
<data_type> const *ptr_const;
const <data_type> *ptr_const;
```

Con trỏ này có ứng dụng trong việc bảo vệ dữ liệu không bị thay đổi trong quá trình thực thi. Sử dụng khi muốn trỏ vào những vùng thông tin mà không làm thay đổi chúng.
</details>
<details>
  <summary><strong> Constant Pointer </strong></summary>
Ngược lại với con trỏ hằng là hằng con trỏ. Khác với con trỏ hằng, constant pointer có thể thay đổi giá trị tại địa chỉ mà nó trỏ tới. Tuy nhiên con trỏ này chỉ có thể trỏ đến một địa chỉ duy nhất.
Cú pháp 
  
```c
int *const const_ptr = &value;
```
Được ứng dụng để tránh việc thay đổi địa chỉ của một biến hay một thanh ghi có sẵn

Vậy nếu muốn vừa báo vệ giá trị lẫn địa chỉ không bị thay đổi khi trỏ vào, ta sử dụng kết hợp cả hai loại con trỏ trên. Cú pháp như sau:

```c
const int *const ptr;
```
</details>
</details>
