Cấu trúc dữ liệu là cách **tổ chức**, và **lưu trữ** dữ liệu để chúng có thể được truy cập và sử dụng một cách hiệu quả, đóng vai trò quan trọng trong việc giải quyết các bài toán và tối ưu hóa thuật toán, vì nó ảnh hưởng trực tiếp đến tốc độ thực thi và tính phức tạp của chương trình.

Cấu trúc dữ liệu được phân làm 2 loại chính:

+ Cấu trúc dữ liệu tuyến tính (Linear Data Structure): **mảng (Array), ngăn xếp (Stack), hàng đợi (Queue), danh sách liên kết (Linked List)**.
+ Cấu trúc dữ liệu phi tuyến tính (Nonlinear Data Structure): **đồ thị (Graphs), cây (Trees)**.

Cấu trúc dữ liệu tuyến tính sẽ sắp xếp dữ liệu theo thứ tự nhất định. Trong khi đó cấu trúc dữ liệu phi tuyến tính thì sắp xếp dữ liệu không theo thứ tự nào hết, dữ liệu có thể phân chia theo nhiều hướng khác nhau.
<details>
  <summary><strong> Stack </strong></summary>

**Stack** (ngăn xếp) là một cấu trúc dữ liệu tuân theo nguyên tắc **"Last In, First Out" (LIFO)**, nghĩa là phần tử **cuối cùng được thêm vào** stack sẽ là phần tử **đầu tiên được lấy ra**. 

Các thao tác cơ bản trên stack bao gồm:
+ **"push"** để **thêm** một phần tử vào **đỉnh** của stack
+ **"pop"** để **xóa** một phần tử ở **đỉnh** stack.
+ **"peek/top"** để **lấy giá trị** của phần tử ở **đỉnh** stack.
+ Kiểm tra Stack đầy: top = size - 1
+ Kiểm tra Stack rỗng: top = -1

Cách hoạt động của Stack có thể mô tả như sau:

Giả sử ban đầu ta có một tách rỗng có thể chứa được tối đa **5 phần tử dữ liệu**

![image](https://github.com/user-attachments/assets/f21477c8-a079-41f7-aabe-3dd03a4d937e)

Do Stack rỗng nên lúc này ta có top = -1, ta tiến hành thêm phần tử đầu tiên vào Stack bằng thao tác **push**

![image](https://github.com/user-attachments/assets/0dcc955a-5ef6-4a4f-93fd-2dbf47254b78)

Khi đã thêm thành công phần tử đầu tiên vào Stack thì lúc này top = -1, đây cũng sẽ là phần tử **cuối cùng được lấy ra**. Sau đó ta cũng tiến hành thêm lần lượt các phần tử tiếp theo bằng thao tác **push** cho đến khi Stack đầy.

![image](https://github.com/user-attachments/assets/da193e46-e914-4e5c-bf9f-ad90fa030221)
![image](https://github.com/user-attachments/assets/360435a6-c5f3-44f3-a418-8053080f8f17)
![image](https://github.com/user-attachments/assets/f0f6ec21-7fb5-48e2-bdfe-8c0aea0001db)
![image](https://github.com/user-attachments/assets/0655ab6f-ff5d-4ff4-8bf4-1d246e5d83f9)

Sau 5 lần push tương ứng với 5 phần tử, mỗi lần push lên thì top sẽ cộng thêm 1 vào giá trị. Khởi đầu bằng -1 khi rỗng thì khi đầy ta sẽ có top sẽ bằng kích thước của stack trừ đi cho 1. Ở đây kích thước của stack = 5 nên khi stack đầy ta có top = 5 -1 = 4. Và khi **stack đầy** rồi thì ta sẽ **không thể thêm phần tử dữ liệu** nào nữa, nếu ta muốn **thêm một phần tử dữ liệu khác** ta sẽ phải **xóa đi phần tử hiện tại ở đỉnh stack**. Để xóa bớt dữ liệu trong stack ta sử dụng thao tác **pop**.

![image](https://github.com/user-attachments/assets/5d682a26-11c1-4341-a0b6-ec5fb837ead7)
![image](https://github.com/user-attachments/assets/9bfb9560-e219-4bdc-aa68-5ba26c28e6f8)
![image](https://github.com/user-attachments/assets/eb2a2bd9-1673-4a82-8400-d0cae8d64522)
![image](https://github.com/user-attachments/assets/66c5eac7-85ef-4383-84a3-41617f23412c)
![image](https://github.com/user-attachments/assets/5592c4eb-d3cc-4374-9f65-fb64b34a9ed6)

Ngược lại với **push** mỗi lần **pop** thì top sẽ trừ đi 1 vào giá trị của top, và khi top = -1 thì stack sẽ rỗng. Như vậy khi **thêm vào** ta có **1 là phần tử dữ liệu đầu tiên** được thêm vào còn **5 là phần tử cuối cùng** được thêm vào. Khi **lấy ra** thì ta có **5 là phần tử dữ liệu đầu tiên** được lấy ra còn **1 là phần tử cuối cùng** được lấy ra. **Stack** có thể thêm vào dữ liệu chỉ khi Stack không đầy (top != size - 1).

</details>

<details>
  <summary><strong> Queue </strong></summary>

</details>
