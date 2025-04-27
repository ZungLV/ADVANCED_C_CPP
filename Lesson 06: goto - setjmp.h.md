<details>
  <summary><strong> goto </strong></summary>

Trong ngôn ngữ lập trình C, `goto` là một lệnh điều khiển luồng chương trình bằng cách nhảy trực tiếp tới một vị trí (label) đã định nghĩa trong cùng một hàm. Cú pháp cơ bản:

```c
goto label_name;
/* ... */
label_name:
    // đoạn mã thực thi khi nhảy tới đây
```
### Ưu điểm của goto

Không tốn tài nguyên của chương trình, làm code gọn hơn

Giảm độ sâu của lồng ghép, nếu dùng `goto`, vào nhiều chương trình cần lồng nhiều lớp `if`, `else`, `while` để có thể .

```c
#include <stdio.h>

int main()
{
   int i = 0;

   // Đặt nhãn
   start:
      if (i >= 5)
      {
         goto end;  // Chuyển control đến nhãn "end"
      }

      printf("%d ", i);
      i++;

      goto start;  // Chuyển control đến nhãn "start"

   // Nhãn "end"
   end:
      printf("\n");
   return 0;
}
```
Ở đoạn code trên, cho đến khi thỏa điều kiện i>=5 thì chương trình sẽ liên tục nhảy về label 'start'. Chỉ khi thỏa điều kiện thì chương trình mới nhảy quay label `start` về label `end` và kết thúc chương trình.

```c
0 1 2 3 4 
```
Ở một ví dụ khác với chương trình lồng nhiều lớp vòng lặp

```c
#include <stdio.h>
int i;
int j;

int main()
{
    while(1)
    {
        for( i = 0; i<5; i++)
        {
            for( j = 0; j<5; j++)
            {
                if(i==3 && j==3) goto exit;
            }
        }
    }

    exit:
        printf("Stop at i = %d and j = %d\n", i,j);

   return 0;
}
```
Ở chương trình trên mặc dù được lồng bởi hai vòng lặp `for` và một vòng lặp `while` ta có thể thoát khỏi cả ba vòng lặp một lúc mà không cần đợi cả ba vòng lặp chạy hết.

```
Stop at i = 3 and j = 3
```
### Nhược điểm của goto
`goto` chỉ có thể sử dụng trong cùng một hàm, một phạm vi với label

Khi chương trình nhảy lung tung giữa các vị trí, người đọc phải dò tìm nhãn (label) để hiểu dòng chảy, làm tăng thời gian đọc và hiểu mã
</details>
<details>
  <summary><strong> setjmp.h </strong></summary>
  
`setjmp.h` là một thư viện trong ngôn ngữ lập trình C, cung cấp hai hàm chính là `setjmp` và `longjmp`. `setjmp` và `longjmp` dùng để lưu và khôi phục trạng thái thực thi (program state), giúp chương trình "nhảy" về một điểm đã lưu trước đó. Đây là một cách xử lý nhảy xa (non-local jump), giống như goto, nhưng mạnh hơn vì nó nhảy giữa các hàm khác nhau.

`setjmp(jmp_buf env)`: Lưu trạng thái chương trình vào biến `env` để có thể quay lại bằng longjmp.
+ Trả về 0 khi được gọi lần đầu.
+ Trả về một giá trị khác 0 khi quay lại từ `longjmp`.

`longjmp(jmp_buf env, int value)`: Khôi phục trạng thái đã lưu, và làm cho `setjmp` trả về `value`.

```c
#include <stdio.h>
#include <setjmp.h>

jmp_buf buf;
```
Khai báo biến `buf` kiểu `jmp_buf` đây sẽ là biến dùng để lưu vị trí và trạng thái của chương trình. Biến này bắt buộc phải khai báo toàn cục do các hàm của setjmp.h có thể sài ở các hàm khác nhau

```c
#include <stdio.h>
#include <setjmp.h>

// Biến lưu trạng thái
jmp_buf buf;

// Biến ngoại lệ dùng cho setjmp
int exception = 0;

// Gọi func2 sẽ longjmp về  ban đầu và trả exception = 2
void func2()
{
    printf("This is function 2\n");
    longjmp(buf, 2);
}

// Gọi func3 sẽ longjmp về  ban đầu và trả exception = 3
void func3()
{
    printf("This is function 3\n");
    longjmp(buf, 3);
}

// Gọi func1 để xử lý các exception được trả về
void func1()
{
    exception = setjmp(buf);
    if (exception == 0)
    {
        printf("This is function 1\n");
        printf("exception = %d\n", exception);
        func2();
    }
    else if (exception == 2)
    {
        printf("exception = %d\n", exception);
        func3();
    }
    else if (exception == 3)    // Hàm cuối cùng được thực thi trước khi kết thúc chương trình
    {
        printf("exception = %d\n", exception);
    }
}

int main(int argc, char const *argv[])
{
    func1();
    return 0;
}
```
```
This is function 1
exception = 0
This is function 2
exception = 2
This is function 3
exception = 3
```
Chỉ với các hàm trong `setjmp.h` ta có thể nhảy liên tiếp 3 hàm khác nhau
</details>

<details>
  <summary><strong> Exception Handling </strong></summary>
Xử lý ngoại lệ (Exception Handling) là một cơ chế trong lập trình giúp phát hiện và xử lý các lỗi hoặc tình huống bất thường xảy ra trong quá trình thực thi chương trình, giúp chương trình hoạt động ổn định và không bị dừng đột ngột.

Ngoại lệ (Exception) là những lỗi hoặc sự kiện không mong muốn xảy ra trong quá trình thực thi chương trình, chẳng hạn như:
+ Chia một số cho 0 (division by zero).
+ Truy cập mảng ngoài phạm vi (out of bounds array access).
+ Truy xuất con trỏ null (null pointer dereference).
+ Lỗi khi mở hoặc đọc tập tin (file not found).
+ Lỗi cấp phát bộ nhớ (bad allocation).

Cơ chế xử lý ngoại lệ giúp chương trình phản ứng kịp thời với các lỗi mà không làm gián đoạn toàn bộ chương trình.

Hầu hết các ngôn ngữ lập trình hiện đại như C++, Java, Python, C# đều hỗ trợ xử lý ngoại lệ thông qua các từ khóa chính như:
+ try: Định nghĩa một khối lệnh có thể phát sinh lỗi.
+ catch: Xử lý ngoại lệ nếu có lỗi xảy ra.
+ throw: Ném ra một ngoại lệ khi xảy ra lỗi.

Cú pháp chung:

```c
try
{
    // Khối lệnh có thể phát sinh lỗi
}
catch (loại_ngoại_lệ_1)
{
    // Xử lý ngoại lệ loại 1
}
catch (loại_ngoại_lệ_2)
{
    // Xử lý ngoại lệ loại 2
}
catch (...)
{
    // Xử lý tất cả các ngoại lệ khác
}
```

Tuy nhiên trong C không có sẵn xử lý ngoại lệ, để có được hỗ trợ xử lý ngoại lệ ta có thể áp dụng `setjmp.h` với các câu lệnh `if`, `else`

Cú pháp khai báo đơn giản để mô phỏng các hàm TRY-CATCH-THROW trong C

```c
#define TRY if ((exception_code = setjmp(buf)) == 0)
#define CATCH(x) else if (exception_code == x)
#define THROW(x) longjmp(buf, x)
```

+ `TRY{]` sẽ được dùng để chứa các câu lệnh mà ta dự đoán có khả năng phát sinh lỗi, đây cũng là `macro` sẽ dùng để đặt điểm nhảy và dùng biến `exception_code` để đại diện cho mã các ngoại lệ dùng để phân biệt các lỗi
+  `CATCH{}` dùng để chứa các biện pháp sử lý ngoại lệ nhất định được quy định bằng mã ngoại lệ `exception_code`
+  `THROW{}` dùng để nhảy vị trí ở `TRY` được lưu ở biến `buf` và trả về mã ngoại lệ ứng với ngoại lệ đó

```c
#include <stdio.h>
#include <setjmp.h>

jmp_buf buf;

int exception_code;

typedef enum
{
    NO_ERROR,
    NO_EXIT,
    DIVIDE_BY_0
} ErrorCodes;  

#define TRY if ((exception_code = setjmp(buf)) == 0)
#define CATCH(x) else if (exception_code == x)
#define THROW(x) longjmp(buf, x)

//Hàm chia biến a và b
double divide(int a, int b)
{
    //Khối lệnh trả về ngoại lệ
    if (a == 0 && b == 0)
    {
        THROW(NO_EXIT);      //longjmp(buf, NO_EXIT), trả về exception_code = NO_EXIT
    }
    else if (b == 0)
    {
        THROW(DIVIDE_BY_0);  //longjmp(buf, DIVIDE_BY_0), trả về exception_code = DIVIDE_BY_0
    }

    return (double)a/b;
}

int main(int argc, char const *argv[])
{
    // Gán exception_code = 0
    exception_code = NO_ERROR;  

    // Khối lệnh kiểm tra ngoại lệ
    TRY                 //if ((exception_code = setjmp(buf)) == 0)
    {
        printf("Ket qua: %0.3f\n", divide(0,0));  //Gọi hàm divide() cũng là hàm có khả năng có lỗi
    }
    CATCH(NO_EXIT)      //else if (exception_code == NO_EXIT)
    {
        printf("ERROR! Không tồn tại\n");
    }
    CATCH(DIVIDE_BY_0)  //else if (exception_code == DIVIDE_BY_0)
    {
        printf("ERROR! Chia cho 0\n");
    }

    // thêm code ở đây
    printf("Hello world\n");
    return 0;
}
```
```
```
### Sự khác biệt giữa TRY - CATCH - THROW và ASSERT
Khi hàm `assert` tìm ra được lỗi, nó sẽ dừng hẳn toàn bộ chương trình kể cả các chương trình sau `assert` có lỗi hay không. Còn đối với xử lý ngoại lệ thì kể cả khi phát hiện ra lỗi vẫn sẽ tiếp tục thực thi các chương trình không liên quan tới lỗi.
</details>
