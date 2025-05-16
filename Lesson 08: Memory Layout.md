<details>
  <summary><strong> Memory Layout </strong></summary>

Chương trình main.exe (trên window), main.hex (nạp vào vi điều khiển) được lưu ở bộ nhớ **SSD** hoặc **FLASH**. 
Khi nhấn run chương trình trên window (cấp nguồn cho vi điều khiển) thì những chương trình này sẽ được copy vào bộ nhớ RAM để thực thi.

Và ở trong **RAM**, bộ nhớ được chia thành 5 vùng để phục vụ các mục đích khác nhau, 5 vùng nhớ chính trong **RAM** là:

+ Stack

+ Heap

+ BSS Segment (Block Started by Symbol)

+ Data Segment

+ Text Segment (Code Segment)

![image](https://github.com/user-attachments/assets/70ea9e17-6047-4677-85da-b4b2184043dc)

  <details>
    <summary><strong> Text Segment (Code Segment) </strong></summary>
    
  Text Segment là một trong những vùng nhớ quan trọng nhất của chương trình trong quá trình thực thi. Nó chứa **mã máy** – tức là tập hợp các lệnh nhị phân được CPU giải mã      và thực hiện. Phần này thường được sinh ra từ mã nguồn sau khi chương trình được biên dịch và liên kết.
    
  Nội dung chứa trong Text Segment:
  
  Các lệnh thực thi (machine instructions) của chương trình.
  
  Trong một số trình biên dịch, như Clang trên macOS, Text Segment còn có thể bao gồm:
  
  - Biến hằng số toàn cục (e.g., const int MAX = 100;)
  
  - Chuỗi hằng (string literals), ví dụ "Hello, world!"
  
  Điều này là do các hằng số và chuỗi không thay đổi trong suốt vòng đời chương trình, nên trình biên dịch có thể tối ưu bằng cách đặt chúng vào vùng chỉ đọc.
  Text Segment thường được đánh dấu **chỉ cho phép đọc và thực thi (Read & Execute)**.
  
  Tất cả các dữ liệu trong Text Segment đều **không thể bị thay đổi** trong lúc chạy chương trình. Nếu có thao tác ghi vào vùng này, chương trình sẽ gặp lỗi vi phạm truy cập     bộ nhớ (segmentation fault hoặc access violation).

  </details>

  <details>
    <summary><strong> Data Segment </strong></summary>
  
  Data Segment là một vùng nhớ trong RAM dùng để lưu trữ **các biến toàn cục và static đã được khởi tạo với giá trị cụ thể (khác 0)**. Đây là một phần quan trọng của bộ nhớ      khi chương trình đang thực thi.
  
  Các biến trong Data Segment có thể được **đọc và ghi**, tức là có thể thay đổi giá trị của chúng trong quá trình thực thi. Đây là điểm khác biệt với vùng Text Segment – vốn    chỉ có quyền đọc.
  
  Trên một số hệ thống (như Windows với GCC hoặc G++), các biến **`const` toàn cục** và **chuỗi hằng (string literal)** có thể được đặt vào Data Segment, nhưng:
  
  +  Chúng sẽ được đánh dấu là **read-only**.
  
  +  Trình liên kết và hệ điều hành sẽ áp dụng bảo vệ chỉ đọc để **ngăn việc thay đổi các giá trị hằng**.
  
  Các biến trong Data Segment tồn tại trong suốt **vòng đời của chương trình**. Khi chương trình kết thúc, toàn bộ vùng nhớ này sẽ được **thu hồi** (giải phóng bởi hệ điều       hành hoặc kết thúc vòng sống của firmware).
  </details>
  
  <details>
    <summary><strong> BSS Segment (Block Started by Symbol) </strong></summary>
    
  BSS Segment là một vùng nhớ trong RAM, chuyên dùng để lưu trữ **các biến toàn cục hoặc static chưa được khởi tạo**, hoặc được **khởi tạo với giá trị bằng 0**.
  
  Giống như **Data Segment**, vùng BSS cũng có **quyền đọc và ghi**. Các biến trong đây có thể **thay đổi giá trị trong suốt quá trình thực thi của chương trình**.
  
  Các biến trong BSS Segment tồn tại **xuyên suốt chương trình**. Khi chương trình kết thúc, vùng nhớ BSS sẽ được **giải phóng hoàn toàn**.
  
  </details>
  
  <details>
    <summary><strong> Stack </strong></summary>
    
  **Stack Segment** là vùng nhớ trong RAM được sử dụng để lưu trữ **biến cục bộ (trừ static cục bộ), tham số hàm, và thông tin điều khiển quá trình gọi hàm** (như địa chỉ trả     về). Stack là một phần quan trọng của chương trình khi thực thi, đặc biệt trong lập trình hàm và đệ quy.
  
  Chứa **hằng số cục bộ**, có thể thay đổi thông qua con trỏ.
  
  Stack cho phép **đọc và ghi**, các biến có thể được cập nhật trong suốt thời gian hàm còn hoạt động. Sau khi hàm kết thúc, các biến sẽ bị **thu hồi tự động**. 
  
  Theo nguyên tắc **LIFO (Last In First Out)**:
  
  + Hàm được gọi **cuối cùng** thì được xử lý **trước tiên** khi thoát ra.
  
  + Còn gọi là **FILO** (First In Last Out) – tương đương về mặt logic.
  </details>
  
  <details>
    <summary><strong> Heap </strong></summary>
    
  Heap là một vùng nhớ trong RAM được sử dụng để **cấp phát bộ nhớ** động khi chương trình đang chạy. Không giống như Stack – vốn được cấp phát tự động theo từng khung hàm –     vùng Heap cho phép lập trình viên **tự quản lý việc cấp phát và giải phóng bộ nhớ**.
  
  Heap rất hữu ích khi:
  
  +  Không biết trước **kích thước dữ liệu** tại thời điểm biên dịch.
  
  +  Cần lưu trữ dữ liệu **lớn, linh hoạt và tồn tại lâu hơn phạm vi hàm**.
  
  +  Dữ liệu cần tồn tại sau khi hàm kết thúc, hoặc chia sẻ giữa các phần khác nhau của chương trình.
  
  <details>
    <summary><strong> stdlib.h </strong></summary>
    
  Trong ngôn ngữ C/C++ (thư viện `<stdlib.h>`):
  
  +  `malloc()`:	Cấp phát bộ nhớ chưa khởi tạo
  +  `calloc()`:	Cấp phát bộ nhớ và tự động khởi tạo bằng 0
  +  `realloc()`:	Thay đổi kích thước vùng nhớ đã cấp phát
  +  `free()`:	Giải phóng vùng nhớ đã cấp phát
  
  </details>
  
  Tất cả các hàm này trả về **con trỏ đến vùng nhớ cấp phát**. Nếu không giải phóng thủ công bằng free(), chương trình sẽ bị **rò rỉ bộ nhớ (memory leak)**.

  Đặc điểm hàm  `malloc()`:
  
  `void *malloc(size_t _Size)`
  
  + Tham số truyền vào: kích thước mong muốn ( byte)
  + Giá trị trả về: con trỏ void

  </details>
  
  <details>
    <summary><strong> Memory Leak </strong></summary>
    
  Stack: bởi vì bộ nhớ Stack cố định nên nếu chương trình bạn sử dụng quá nhiều bộ nhớ vượt quá khả năng lưu trữ của Stack chắc chắn sẽ xảy ra tình trạng tràn bộ nhớ Stack       (Stack overflow), các trường hợp xảy ra như bạn khởi tạo quá nhiều biến cục bộ, hàm đệ quy vô hạn,...

  Heap: Nếu bạn liên tục cấp phát vùng nhớ mà không giải phóng thì sẽ bị lỗi tràn vùng nhớ Heap. Nếu bạn khởi tạo một vùng nhớ quá lớn mà vùng nhớ Heap không thể lưu trữ một     lần được sẽ bị lỗi khởi tạo vùng nhớ Heap thất bại.
  
  </details>

</details>
