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
  <summary><strong> Hàm thêm node vào cuối danh sách </strong></summary>
   
  ```c
    // Thêm 1 node vào cuối list
  void push_back(Node **head, int data)
  {   
      // Tạo node mới
      Node *new_node =  createNode(data);
  
      
      // *head trỏ vào node đầu tiên
      if (empty(*head))
      {
          *head = new_node;   // vừa là node đầu vừa là node cuối
          return;
      }
  
  
      // Tạo node tạm
      Node *temp = *head;
  
      while((temp)->next != NULL) (temp) = (temp)->next;
  
      temp->next = new_node;
  
  }
  ```
  Đối với các hàm có tác dụng thay đổi và can thiệp các node sẽ sử dụng con trỏ bậc 2 để trỏ vào các node là con trỏ bậc 1. Hàm này sẽ tạo một node mới với hàm `createNode(data)`:
  + Nếu danh sách rỗng, node mới sẽ là node đầu tiên `*head = new_node`
  + Nếu danh sách không rỗng, tạo một con trỏ tạm `temp` để tránh con trỏ head lạc mất node đầu tiên dẫn đến sai lệch cho những lần gọi hàm của chương trình sau này.
  + Lặp cho đến khi nào node tiếp theo của con trỏ tạm là NULL thì liên kết node của của con trỏ hiện tại với node mới

 Chương trình minh họa trong hàm main:
 ```c
  int main()
  {
      Node *head = NULL;    // Tạo node đầu tiên
      
      push_back(&head,15);
      push_back(&head,25);
      push_back(&head,30);
      push_back(&head,40);
  
      display(head);
      printf("Size list: %d\n", size(head));
      printf("Front list data: %d\n", front(head));
      printf("Back list data: %d\n", back(head));
      printf("Data ở vị trí: %d là %d\n", 2,get(head, 2));
  
      clear(&head);
  }
 ```
 Sau khi chạy
 ```
Node 0: 15
Node 1: 25
Node 2: 30
Node 3: 40
Size list: 4
Front list data: 15
Back list data: 40
Data ở vị trí: 2 là 30
Đã xóa hoàn toàn tất cả các nodes
 ```

  </details>

  <details>
  <summary><strong> Hàm thêm node vào đầu danh sách </strong></summary>

  ```c
  // Thêm 1 node vào đầu list
  void push_front(Node **head, int data)
  {
      // Tạo node mới
      Node *new_node =  createNode(data);
  
      
      // *head trỏ vào node đầu tiên
      if (empty(*head))
      {
          *head = new_node;   // vừa là node đầu vừa là node cuối
          return;
      }
      //
      new_node->next = *head;
      *head = new_node;
  
  }
  ```
  Hàm này sẽ tạo một node mới với hàm `createNode(data)`:
  + Nếu danh sách rỗng, node mới sẽ là node đầu tiên `*head = new_node`
  + Nếu danh sách không rỗng, gán con trỏ tiếp theo của con trỏ mới tạo vào địa chỉ của node đầu tiên hiện tại. Sau đó gán con trỏ `*head` (con trỏ đầu danh sách) là node mới tạo

  Chương trình minh họa trong hàm main:
  ```c
  int main()
{
    Node *head = NULL;    // Tạo node đầu tiên
    
    push_front(&head,15);
    display(head);
    push_front(&head,25);
    display(head);
    push_front(&head,30);
    display(head);
    push_front(&head,40);
    display(head);


    clear(&head);
}
  ```
  Sau khi chạy
  ```c
Node 0: 15
Node 0: 25
Node 1: 15
Node 0: 30
Node 1: 25
Node 2: 15
Node 0: 40
Node 1: 30
Node 2: 25
Node 3: 15
Đã xóa hoàn toàn tất cả các nodes
  ```

  </details>

  <details>
  <summary><strong> Hàm thêm node vào vị trí bất kì </strong></summary>

  ```c
  void insert(Node **head, int value, int position)
  {
      Node *new_node = createNode(value);
  
      if (empty(*head))
      {
          *head = new_node;
      }
      else
      {
          if(position < 0 || position > size(*head)) 
          {
          printf("Lỗi vị trí âm hay vượt quá thứ tự node có thể tạo, vị trí tối đa có thẻ tạo là: %d tối thiểu là 0\n", size(*head));
          }
  
          else if(position == 0) push_front(head,value);                // Thêm vào node 0 (head)
  
          else if(position == size(*head)) push_back(head,value);  // Thêm vào node cuối cùng
  
          else
          {
          Node *p = *head;
          uint8_t index = 0;
  
          while(p != NULL && (index != position - 1))
          {
              index++;
              p = p->next;
          }
  
          new_node->next = p->next;
          p->next = new_node;
          }
      }
  }
  ```
 Hàm này sẽ tạo một node mới với hàm `createNode(data)`:
  + Nếu danh sách rỗng, node mới sẽ là node đầu tiên `*head = new_node`
  + Nếu `position` nằm ngoài phạm vi cho phép sẽ báo lỗi
  + Nếu `position == 0` dùng hàm push_front()
  + Nếu `position == size(*head)` dùng hàm push_back()
  + Nếu `position` thuộc phạm vi hợp lệ, tạo một con trỏ tạm, lặp cho đến khi con trỏ đến node nằm trước vị trí yêu cầu. Liên kết node mới với node nằm ở vị trí yêu cầu sau đó lại liên kết node đang được con trỏ tạm thời vào node mới ta sẽ được node mới nằm hoàn toàn ở vị trí yêu cầu

  Chương trình minh họa trong hàm main:
  ```c
    int main()
  {
      Node *head = NULL;    // Tạo node đầu tiên
      
      insert(&head,15,4);
      display(head);
      insert(&head,25,3);
      insert(&head,25,1);
      display(head);
      insert(&head,30,3);
      insert(&head,30,2);
      insert(&head,30,3);
      display(head);
  
  
      clear(&head);
  }
  ```
  Sau khi chạy
  ```
  Node 0: 15
  Lỗi vị trí âm hay vượt quá thứ tự node có thể tạo, vị trí tối đa có thẻ tạo là: 1 tối thiểu là 0
  Node 0: 15
  Node 1: 25
  Lỗi vị trí âm hay vượt quá thứ tự node có thể tạo, vị trí tối đa có thẻ tạo là: 2 tối thiểu là 0
  Node 0: 15
  Node 1: 25
  Node 2: 30
  Node 3: 30
  Đã xóa hoàn toàn tất cả các nodes
  ```

  </details>

  <details>
  <summary><strong> Hàm xóa một node cuối danh sách </strong></summary>

  ```c
  // Xóa 1 node cuối list
  void pop_back(Node **head)
  {
      if (empty(*head))
      {
          printf("Không có Node để xóa!\n");
          return;
      }
  
      // Nếu chỉ có 1 node
      if ((*head)->next == NULL)
      {
          free(*head);
          *head = NULL;
          return;
      }
  
      // Tạo node tạm
      Node *temp = *head;
      while(temp->next->next != NULL)
      {
          temp = temp->next;
      }
      free(temp->next);
      temp->next = NULL;
  
  }
  ```
  Hàm này sẽ xóa một node theo các trường hợp sau:
  + Nếu danh sách rỗng, báo lên màn hình
  + Nếu chỉ có duy nhất một node, giải phóng node đấy và đưa node đầu tiên thành node rỗng
  + Nếu không thuộc các trường hợp trên, tạo một node tạm. Lặp cho đến khi con trỏ nằm ở node kế cuối `temp->next->next = NULL`. Giải phóng node cuối cùng và biến con trỏ tiếp theo của node cuối cùng hiện tại thành NULL

  Chương trình chạy trong main.c
  ```c
  int main()
  {
      Node *head = NULL;    // Tạo node đầu tiên
      
      push_back(&head,10);
      push_back(&head,20);
      push_back(&head,30);
      push_back(&head,40);
  
      display(head);
  
      pop_back(&head);
      display(head);
      pop_back(&head);
      display(head);
      pop_back(&head);
      display(head);
      pop_back(&head);
      display(head);
      pop_back(&head);
  
  
      clear(&head);
  }
  ```
  Sau khi chạy
  ```
  Node 0: 10
  Node 1: 20
  Node 2: 30
  Node 3: 40
  Node 0: 10
  Node 1: 20
  Node 2: 30
  Node 0: 10
  Node 1: 20
  Node 0: 10
  Không có Node!
  Không có Node để xóa!
  Đã xóa hoàn toàn tất cả các nodes
  ```

  </details>

  <details>
  <summary><strong> Hàm xóa một node đầu danh sách </strong></summary>

  ```c
  // Xóa 1 node đầu list
  void pop_front(Node **head)
  {
      if (empty(*head))
      {
          printf("Không có Node để xóa!\n");
          return;
      }
  
      // Nếu chỉ có 1 node
      if ((*head)->next == NULL)
      {
          free(*head);
          *head = NULL;
          return;
      }
  
      Node *new_head = (*head)->next;
      free(*head);
      *head = new_head;
  
  }
  ```
  Hàm này sẽ xóa một node theo các trường hợp sau:
  + Nếu danh sách rỗng, báo lên màn hình
  + Nếu chỉ có duy nhất một node, giải phóng node đấy và đưa node đầu tiên thành node rỗng
  + Nếu không thuộc các trường hợp trên, gán node tạm vào node thứ 2 trong danh sách `Node *new_head = (*head)->next`, giải phóng node đầu tiên vào biến node thứ 2 ban đầu thành node đầu tiên
  Chương trình trong main:
  ```c
  int main()
  {
      Node *head = NULL;    // Tạo node đầu tiên
      
      push_back(&head,10);
      push_back(&head,20);
      push_back(&head,30);
      push_back(&head,40);
  
      display(head);
  
      pop_front(&head);
      display(head);
      pop_front(&head);
      display(head);
      pop_front(&head);
      display(head);
      pop_front(&head);
      display(head);
      pop_front(&head);
  
  
      clear(&head);
  }
  ```
  Sau khi chạy
  ```
  Node 0: 10
  Node 1: 20
  Node 2: 30
  Node 3: 40
  Node 0: 20
  Node 1: 30
  Node 2: 40
  Node 0: 30
  Node 1: 40
  Node 0: 40
  Không có Node!
  Không có Node để xóa!
  Đã xóa hoàn toàn tất cả các nodes
  ```
  
  </details>

  <details>
  <summary><strong> Hàm xóa một node ở vị trí bất kì </strong></summary>

  ```c
  void erase(Node **head, int position)
  {
      if (empty(*head))
      {
          printf("Không có Node để xóa!\n");
      }
          else
      {
          if(position < 0 || position > size(*head)) 
          {
          printf("Lỗi vị trí âm hay vượt quá vị trí node có thể xóa, vị trí tối đa có thẻ xóa là: %d tối thiểu là 0\n", size(*head));
          }
  
          else if(position == 0) pop_front(head);                // Xóa đi node 0 (head)
  
          else if(position == size(*head)) pop_back(head);  // Xóa đi node cuối cùng
  
          else
          {
          Node *temp = *head;
          uint8_t index = 0;
  
          while(temp != NULL && (index != position - 1))
          {
              index++;
              temp = temp->next;
          }
          Node *alt = temp->next->next;
          free(temp->next);
          temp->next = alt;
          }
      }
  }
  ```
  Hàm này sẽ xóa một node theo các trường hợp sau:
  + Nếu danh sách rỗng, báo lên màn hình
  + Nếu `position` không trong phạm vi hợp lệ, thông báo lên màn hình
  + Nếu `position == 0` tức vị trí đầu tiên, sử dụng hàm `pop_front()`
  + Nếu `position == size(*head)` tức vị trí bằng kích thước danh sách (vị trí cuối cùng), sử dụng hàm `pop_back()`
  + Nếu không thuộc các trường hợp trên, tạo con trỏ node tạm, lặp đến khi trỏ đến vị trí tiếp trước vị trí yêu cầu. Tạo một con trỏ tạm khác giữa địa chỉ của node tiếp sau vị trí yêu cầu. Giải phóng node ở vị trí yêu cầu rồi liên kết hai node trước sau lại với nhau

  Chương trình trong main:
  ```c
  int main()
  {
      Node *head = NULL;    // Tạo node đầu tiên
      
      push_back(&head,10);
      push_back(&head,20);
      push_back(&head,30);
      push_back(&head,40);
  
      display(head);
  
      erase(&head,9);
      erase(&head,2);
      display(head);
      erase(&head,1);
      display(head);
      erase(&head,1);
      display(head);
      erase(&head,0);
      display(head);
  
      clear(&head);
  }
  ```
  Sau khi chạy
  ```
  Node 0: 10
  Node 1: 20
  Node 2: 30
  Node 3: 40
  Lỗi vị trí âm hay vượt quá vị trí node có thể xóa, vị trí tối đa có thẻ xóa là: 4 tối thiểu là 0
  Node 0: 10
  Node 1: 20
  Node 2: 40
  Node 0: 10
  Node 1: 40
  Node 0: 10
  Không có Node!
  Đã xóa hoàn toàn tất cả các nodes
  ```
  </details>

  <details>
  <summary><strong> Hàm xóa tất cả các node </strong></summary>

  ```c
  // Xóa toàn bộ node
  void clear(Node **head)
  {
      while (!empty(*head))
      {
          pop_front(head);
      }
      printf("Đã xóa hoàn toàn tất cả các nodes\n");
  }
  ```
  Hàm này xóa hết tất cả các node bằng cách lặp hàm `pop_front()` cho đến khi nào list rỗng thì thôi

  Chương trình chạy trong main.c
  ```c
  int main()
  {
      Node *head = NULL;    // Tạo node đầu tiên
      
      push_back(&head,10);
      push_back(&head,20);
      push_back(&head,30);
      push_back(&head,40);
  
      display(head);
  
      clear(&head);
  }
  ```
  Sau khi chạy
  ```
  Node 0: 10
  Node 1: 20
  Node 2: 30
  Node 3: 40
  Đã xóa hoàn toàn tất cả các nodes
  ```
  </details>
  
</details>

</details>
