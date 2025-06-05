<details>
  <summary><strong> Linked List </strong></summary>

<details>
  <summary><strong> Đặt vấn đề </strong></summary>
  
Trong cấu trúc dữ liệu **mảng (array)**, các phần tử được lưu trữ tại các **vị trí bộ nhớ liên tiếp**, nghĩa là chúng có mối liên kết chặt chẽ về mặt địa chỉ. Sự liên tiếp này cho phép truy xuất phần tử theo chỉ số (index) diễn ra với **độ phức tạp thời gian hằng số** (thời gian xử lý cố định bất kể kích thước dữ liệu)

Tuy nhiên, tính chất liên kết liên tiếp về bộ nhớ của mảng cũng dẫn đến một hệ quả: khi thực hiện các thao tác **chèn** hoặc **xóa** phần tử tại một vị trí bất kỳ (không phải cuối mảng), toàn bộ các phần tử phía sau vị trí thao tác phải được **dịch chuyển (shift)** để duy trì tính liên tục của mảng. Cụ thể:

+ Khi chèn một phần tử vào giữa mảng, các phần tử từ vị trí chèn trở về sau phải được dịch chuyển một vị trí sang phải để tạo không gian cho phần tử mới.

+ Khi xóa một phần tử, các phần tử phía sau vị trí bị xóa phải được dịch chuyển một vị trí sang trái để lấp đầy khoảng trống.

Các thao tác này có độ phức tạp thời gian là O(n), trong đó n là số lượng phần tử bị dịch chuyển. Điều này khiến mảng trở nên kém hiệu quả trong các ứng dụng yêu cầu cập nhật thường xuyên về mặt cấu trúc dữ liệu.

Trong các trường hợp cần thao tác chèn/xóa thường xuyên tại các vị trí không cố định, **danh sách liên kết (linked list)** thường được ưu tiên hơn do khả năng quản lý bộ nhớ linh hoạt thông qua con trỏ mà không cần dịch chuyển dữ liệu.
**
</details>


<details>
  <summary><strong> Khái niệm Link List </strong></summary>

Danh sách liên kết (linked list) là một cấu trúc dữ liệu tuyến tính được sử dụng phổ biến trong lập trình máy tính để tổ chức và lưu trữ dữ liệu một cách linh hoạt. Khác với mảng – nơi các phần tử được lưu trữ tại các vị trí bộ nhớ liên tiếp – danh sách liên kết bao gồm một tập hợp các phần tử riêng lẻ gọi là **nút (node)**, được liên kết với nhau thông qua các **con trỏ (pointers)**.

Mỗi nút trong danh sách liên kết thường bao gồm hai thành phần chính:

+ Dữ liệu (data): chứa giá trị cần lưu trữ.

+ Con trỏ (pointer): lưu địa chỉ của nút kế tiếp trong danh sách.

Trong danh sách liên kết đơn (singly linked list), mỗi nút chỉ chứa một con trỏ trỏ đến nút kế tiếp. **Nút cuối cùng** trong danh sách có con trỏ trỏ đến **NULL**, biểu thị điểm kết thúc của danh sách.

Do cấu trúc phân tán trong bộ nhớ và khả năng liên kết động giữa các nút, danh sách liên kết cho phép chèn và xóa phần tử tại bất kỳ vị trí nào mà không cần dịch chuyển các phần tử khác, giúp tối ưu hóa thời gian thực hiện so với mảng trong một số trường hợp.

</details>

<details>
  <summary><strong> Các thao tác của Link List </strong></summary>

  <details>
  <summary><strong> Cấu trúc </strong></summary>

  ```c
  // Tạo kiểu dữ liệu chung cho các Node
  typedef struct Node
  {
      int data;
      struct Node *next;  // "next" chứa địa chỉ của node tiếp theo
  }Node;
  ```
  Các node sẽ như là một kiểu struct có biến để chứa dữ liệu và một con trỏ vào node kế tiếp
  </details>

  <details>
  <summary><strong> Hàm tạo node </strong></summary>

  ```c
  // Hàm tạo các node có kiểu trả về là con trỏ kiểu node sẽ trả về địa chỉ
  Node* createNode(int data)
  {
      Node *node = (Node*)malloc(sizeof(Node));
      node->data = data;
      node->next = NULL;
      return node;
  }
  ```
  Hàm này tạo các node có kiểu trả về là con trỏ kiểu **Node**, gán data, trỏ tiếp theo set thành NULL, và trả về địa chỉ của node mới tạo
  </details>

  <details>
  <summary><strong> Hàm kiểm tra list rỗng </strong></summary>

  ```c
  // Kiểm tra list rỗng
  bool empty(Node *array)
  {
      if(array == NULL) return true;
      else return false;
  }                       
  ```
  Là một hàm kiểu bool trả về true nếu list rỗng và false nếu danh sách vẫn còn node
  </details>
  
  <details>
  <summary><strong> Hàm đếm số lượng node </strong></summary>

  ```c
  // Đếm số lượng node hiện tại
  int size(Node *head)
  {
      int count = 0;
      if(empty(head)) return 0;
  
      while(!empty(head))
      {
          count++;
          head = head->next;
      }
      return count;
  }
  ```
  Hàm trả về 0 nếu hàm **empty(head)** trả về true hoặc thực hiện vòng lặp miễn là **head** không có địa chỉ là **NULL**. Trả kích thước là **count** sau khi kết thúc vòng lặp.
  </details>

 <details>
  <summary><strong> Hàm đọc giá trị node đầu tiên </strong></summary>
   
  ```c
  // Đọc giá trị node đầu tiên
  int front(Node *head)
  {
      if(empty(head))
      {
          printf("Không có Node!\n");
          return 0;
      }
  
      else return head->data;
  }
  ```
  Hàm trả về **head->data** (giá trị của node đầu tiên) miễn là danh sách vẫn còn node
  </details>

 <details>
  <summary><strong> Hàm đọc giá trị node cuối cùng </strong></summary>
   
  ```c
  // Đọc giá trị node cuối cùng
  int back(Node *head)
  {
      if(empty(head))
      {
          printf("Không có Node!\n");
          return 0;
      }
      else
      {
      while(!empty(head->next))
      {
          head = head->next;
      }
      return head->data;
      }
  }
  ```
  Sử dụng vòng lặp để trỏ vào các node liên tiếp và dừng khi node tiếp theo của node đang được trỏ vào bằng NULL (**head->next == NULL**), trả về **head->data** (giá trị của node cuối cùng sau khi được trỏ tới)
  </details>

   <details>
  <summary><strong> Hàm đọc giá trị node bất kì </strong></summary>
   
  ```c
  // Đọc giá trị node bất kỳ
  int get(Node *head, int position)
  {
      if(empty(head))
      {
          printf("Không có Node!\n");
          return 0;
      }
      else
      {
          if(position < 0 || position > size(head)-1) 
          {
          printf("Lỗi vị trí âm hay vượt quá phạm vi node, vị trí tối đa có thẻ đọc là: %d tối thiểu là 0\n", size(head)-1);
          return 0;
          }
  
          else if(position == 0) front(head);                // Lấy giá trị node 0 (head)
  
          else if(position == size(head)) back(head);        // Lấy giá trị cuối cùng
  
          else
          {
          uint8_t index = 0;
  
          while( !empty(head) && (index != position))
          {
              index++;
              head = head->next;
          }
          return head->data;
          }
      }
  }
  ```
  Sử dụng hai tham số là node đầu tiên được tạo `*head` và vị trí node mong muốn `position`.

  + Khi list rỗng in thông báo ra màn hình
  + Khi position không thuộc phạm vi cho phép sẽ in thông báo báo lỗi
  + Khi position = 0 tức là node 0 thì sài hàm fron()
  + Khi position bằng với kích thước danh sách thì sài hàm back()
  + Sử dụng vòng lặp cho đến khi trỏ đúng vị trí rồi trả về giá trị tại node được trỏ

  </details>

  <details>
  <summary><strong> Hàm hiển thị danh sách </strong></summary>
   
  ```c
  // Hàm hiển thị ra màn hình
  void display(Node *head)
  {
      int i = 0;
      
      if(empty(head))
      {
          printf("Không có Node!\n");
          return;
      }
  
      while(!empty(head))
      {
          printf("Node %d: %d\n", i, head->data);
          head = head->next;
          i++;
      }
  
  }
  ```
  Sử dụng vòng lặp khi danh sách không rỗng và in ra từng node một
  
  </details>

    <details>
  <summary><strong>  </strong></summary>
   
  ```c

  ```

  </details>

</details>

</details>
