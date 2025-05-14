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
  
Text Segment là một trong những vùng nhớ quan trọng nhất của chương trình trong quá trình thực thi. Nó chứa **mã máy** – tức là tập hợp các lệnh nhị phân được CPU giải mã và thực hiện. Phần này thường được sinh ra từ mã nguồn sau khi chương trình được biên dịch và liên kết.

Nội dung chứa trong Text Segment:

Các lệnh thực thi (machine instructions) của chương trình.

Trong một số trình biên dịch, như Clang trên macOS, Text Segment còn có thể bao gồm:

- Biến hằng số toàn cục (e.g., const int MAX = 100;)

- Chuỗi hằng (string literals), ví dụ "Hello, world!"

Điều này là do các hằng số và chuỗi không thay đổi trong suốt vòng đời chương trình, nên trình biên dịch có thể tối ưu bằng cách đặt chúng vào vùng chỉ đọc.
Text Segment thường được đánh dấu **chỉ cho phép đọc và thực thi (Read & Execute)**.

Tất cả các dữ liệu trong Text Segment đều **không thể bị thay đổi** trong lúc chạy chương trình. Nếu có thao tác ghi vào vùng này, chương trình sẽ gặp lỗi vi phạm truy cập bộ nhớ (segmentation fault hoặc access violation).

</details>

<details>
  <summary><strong> Data Segment </strong></summary>

Chứa các biến toàn cục được khởi tạo với giá trị khác 0.

Chứa các biến static (global + local) được khởi tạo với giá trị khác 0.
Quyền truy cập là đọc và ghi, tức là có thể đọc và thay đổi giá trị của biến .

Với compiler GCC/G++ (Windows): Data Segment còn lưu trữ biến hằng số toàn cục (const) và chuỗi hằng (string literal) nhưng quyền truy cập là chỉ đọc.

Tất cả các biến sẽ được thu hồi sau khi chương trình kết thúc.

</details>

<details>
  <summary><strong> BSS Segment (Block Started by Symbol) </strong></summary>

</details>

<details>
  <summary><strong> Heap </strong></summary>

</details>

<details>
  <summary><strong> Stack </strong></summary>

</details>

</details>
