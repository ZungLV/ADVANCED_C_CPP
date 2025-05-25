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

**Queue** là một cấu trúc dữ liệu tuân theo nguyên tắc "First In, First Out" (FIFO), nghĩa là phần tử đầu tiên được thêm vào hàng đợi sẽ là phần tử đầu tiên được lấy ra. 

Các thao tác cơ bản trên hàng đợi bao gồm:

+ **enqueue**: thêm phần tử vào cuối hàng đợi
+ **dequeue**: xóa phần tử từ đầu hàng đợi. 
+ **front**: đọc giá trị của phần tử đứng đầu hàng đợi.
+ **rear**: đọc giá trị của phần tử đứng cuối hàng đợi.
+ Kiểm tra hàng đợi đầy/rỗng.

Ở đây ta sẽ nói về hai cấu trúc Queue là **Linear Queue** và **Circular Queue**

<details>
  <summary><strong> Linear Queue </strong></summary>

<details>
  <summary><strong> Đặc điểm của Linear Queue </strong></summary>
  
**Linear Queue** (Hàng đợi tuyến tính) là một cấu trúc dữ liệu tuyến tính có thể mô tả cách hoạt động như sau:

![image](https://github.com/user-attachments/assets/0d7cd111-2cf6-401c-bac6-48a92ef018bd)

Ban đầu khởi tạo một hàng chờ chứa **5 phần tử dữ liệu**. Vì ban đầu hàng chờ rỗng (chưa lưu bất kì dữ liệu nào) nên ta sẽ có 

`front = rear = -1`

![image](https://github.com/user-attachments/assets/19a22c0a-b4b0-4199-8afc-4b3328183942)

Thực hiện thao tác **enqueue** để thêm dữ liệu đầu tiên, khi này hàng chờ không còn rỗng nữa nên lúc này ta sẽ có 

`front = rear = 0`

![image](https://github.com/user-attachments/assets/21bf15f4-4701-429e-a26c-982135724e3e)
![Screenshot from 2025-05-25 20-39-43](https://github.com/user-attachments/assets/ac6f71e4-b67d-4c01-b75a-c4b43ab0d46b)
![Screenshot from 2025-05-25 20-39-47](https://github.com/user-attachments/assets/95e2bebe-8a3b-4900-999e-058c911acfb6)
![image](https://github.com/user-attachments/assets/0d118741-b177-4d53-835f-82c031650efc)

Thêm dữ liệu cho đến khi hàng chờ đầy, Linear Queue có các đặc điểm như sau:
+ Khi **thêm** dữ liệu bằng thao tác **enqueue** sẽ chỉ ảnh hưởng đến giá trị **rear** chứ không thay đổi gì **front**. Mỗi lần thêm vào một thành phần dữ liệu thì **rear sẽ tăng thêm 1 đơn vị**.
+ Khi hàng chờ đã **đầy** thì ta có `rear = size - 1`. Ở đây ta có **size = 5** vậy thì khi đầy thì **rear = 4**.

Đối với Linear Queue ta chỉ có thể thêm dữ liệu chỉ khi **hàng chờ rỗng** tức là nếu giờ ta muốn thêm dữ liệu gì vào hàng chờ hiện tại thì ta phải lấy hết các thành phần dữ liệu ra. Để lấy dữ liệu ra khỏi hàng chờ ta sẽ thực hiện thao tác **dequeue**.

![image](https://github.com/user-attachments/assets/625ee8e0-90a0-4aec-97b7-dcbf8906264c)
![image](https://github.com/user-attachments/assets/0e82d813-9b7e-447f-abd3-f525fd6a7547)
![image](https://github.com/user-attachments/assets/27cb6e77-00ae-4608-b293-025b49b77521)
![image](https://github.com/user-attachments/assets/0ffe620e-1a77-451b-b131-cf31d36782d8)
![image](https://github.com/user-attachments/assets/d2ca9d85-dee2-47ab-89b4-5522d8bd015d)

Xóa dữ liệu cho đến khi hàng chờ rỗng, Linear Queue có các đặc điểm như sau:

+ Khi **xóa** dữ liệu bằng thao tác **dequeue** sẽ chỉ ảnh hưởng đến giá trị **front** chứ không thay đổi gì **rear**. Mỗi lần thêm vào một thành phần dữ liệu thì **front sẽ tăng thêm 1 đơn vị**.
+ Khi hàng chờ đã **rỗng** thì ta có **front > rear**. Ở đây ta có **front = 5** và **rear = 4** khi hàng chờ đã xóa hết dữ liệu.

Trong Linear Queue, nếu **'rear' đã đạt tới max**, thì queue sẽ được coi là đầy và **không thể thêm phần tử mới**, ngay cả khi phía trước còn khoảng trống do các phần tử đã bị xóa.

Chỉ có thể thêm phần tử mới khi đã dequeue toàn bộ các phần tử hiện có (tức là queue rỗng hoàn toàn và front được reset về vị trí ban đầu).

</details>

<details>
  <summary><strong> Code mô phỏng </strong></summary>

Đối với cấu trúc Linear Queue ta có chương trình mô phỏng như sau:

+ Mô phỏng hàng đợi bằng **struct**

```c
typedef struct
{
    int *item;  // mảng lưu trữ giá trị các phần tử
    int size;   // số lượng phần tử tối đa có thể đưa vào
    int front;  // chỉ số của phần tử đầu hàng đợi
    int rear;   // chỉ số của phần tử cuối hàng đợi
} Queue;
```

+ Hàm khởi tạo để **cấp phát bộ nhớ** theo kích thước đặt ra cho queue

```c
void initialize(Queue *queue, int size)
{
    queue->size  = size;
    queue->item  = (int*)malloc(size * sizeof(int));
    queue->front = queue->rear = -1;
}
```

+ Các hàm kiểm tra hàng chờ đầy hoặc rỗng dựa trên đặc điểm của queue. Hàm chờ đầy khi **rear = size - 1** còn hàm chờ rỗng khi **front = rear = -1** hoặc khi **front > rear**

```c
// kiểm tra hàng đợi đầy
bool isFull(Queue queue)
{
    return (queue.rear == queue.size - 1);
}

// kiểm tra hàng đợi rỗng
bool isEmpty(Queue queue)
{
    return (queue.front == -1 || queue.front > queue.rear);
}
```

+ Hàm **enqueue** để thêm phần tử dữ liệu vào hàng chờ. Hàm này chỉ thêm vào dữ liệu chỉ khi **hàng chờ rỗng**. Nếu khi sử dụng hàm này lần đầu khi hàng chờ rỗng thì hàm sẽ gán **rear = front = 0**, còn đối với các lần sau thì với mỗi lần gọi hàm để thêm dữ liệu thì **rear sẽ cộng thêm 1**

```c
void enqueue(Queue *queue, int data)
{
    if (isFull(*queue))
    {
        printf("Hàng đợi đầy!\n");
        return;
    }
    else
    {
        if (queue->front == -1) queue->front = queue->rear = 0;
        else queue->rear++;
        queue->item[queue->rear] = data;
        printf("Enqueue data %d\n", data);
    }
}
```

+  Hàm **dequeue** để xóa phần tử dữ liệu khỏi hàng chờ. Khi gọi hàm này nếu hàm chờ không rỗng thì dữ liệu nằm ở vị trí **front** tức đầu hàng đợi sẽ bị xóa (gán bằng 0). Với mỗi lần gọi hàm thì **front sẽ cộng thêm 1**, trong trường hợp khi xóa mà hàm rỗng thì thay vì cộng thêm 1 để front > rear thì reset lại cả front và rear luôn (front = rear = -1)

```c
// xóa phần tử đầu hàng đợi
int dequeue(Queue *queue)
{
    if (isEmpty(*queue))
    {
        printf("Hàng đợi rỗng!\n");
        return QUEUE_EMPTY;
    }
    else
    {
        int dequeue_value = queue->item[queue->front];

        queue->item[queue->front] = 0;

        if (queue->front == queue->rear && queue->rear == queue->size - 1)
        {
            queue->front = queue->rear = -1;
        }
        else
        {
            queue->front++;
        }
        return dequeue_value;
    }
}
```

+  Hàm hiện thị hàng chờ ra màn hình, in lần lượt từ front đến rear

```c
void display(Queue queue)
{
    if (isEmpty(queue))
    {
        printf("Hàng đợi rỗng!\n");
    }
    else
    {
        printf("Queue: ");

        for (int i=queue.front; i<=queue.rear; i++)
        {
            printf("%d ", queue.item[i]);
        }
        printf("\n");
    }
}
```

+  Các hàm trả về giá trị tại vị trí front và rear

```c
int front(Queue queue)
{
    if (isEmpty(queue))
    {
        printf("Hàng đợi rỗng!\n");
        return QUEUE_EMPTY;
    }
    else
    {
        return queue.item[queue.front];
    }
}

int rear(Queue queue)
{
    if (isEmpty(queue))
    {
        printf("Hàng đợi rỗng!\n");
        return QUEUE_EMPTY;
    }
    else
    {
        return queue.item[queue.rear];
    }
}
```

Sau khi đã khai báo đủ các hàm cần thiết ta sẽ thử ứng dụng hàm trong main.

```c
Queue liQueue;

initialize(&liQueue, 5);

enqueue(&liQueue, 1);
enqueue(&liQueue, 2);
enqueue(&liQueue, 3);
enqueue(&liQueue, 4);
enqueue(&liQueue, 5);
```

Tạo một biến `liQueue` kiểu **Queue** để làm hàng đợi, khởi tạo `liQueue` có kích thước là 5 và lần lượt thêm vào hàng đợi cho tới khi hàng đợi đầy. Khi này hàng đợi đã đầy ta sẽ thử thêm một phần tử nữa và cho in kết quả ra màn hình

```c
enqueue(&liQueue, 6);

printf("Front: %d\n", front(liQueue));
printf("Rear: %d\n", rear(liQueue));
```

Kết quả là:

```
Enqueue data 1
Enqueue data 2
Enqueue data 3
Enqueue data 4
Enqueue data 5
Hàng đợi đầy!
Front: 1
Rear: 5
Queue: 1 2 3 4 5 
```

Khi ta cố thêm dữ liệu vào hàng đợi đầy thì chương trình sẽ báo lỗi. Sau đó ta sẽ thử xóa đi 2 dữ liệu và thêm vào đó 2 dữ liệu rồi xóa hết toàn bộ và hiện thị hàng đợi lên màn hình:

```c
printf("Dequeue %d\n", dequeue(&liQueue));
printf("Dequeue %d\n", dequeue(&liQueue));
enqueue(&liQueue, 10);
enqueue(&liQueue, 20);
printf("Dequeue %d\n", dequeue(&liQueue));
printf("Dequeue %d\n", dequeue(&liQueue));
printf("Dequeue %d\n", dequeue(&liQueue));

display(liQueue);
```

Kết quả là:

```
Dequeue 1
Dequeue 2
Hàng đợi đầy!
Hàng đợi đầy!
Dequeue 3
Dequeue 4
Dequeue 5
Hàng đợi rỗng!
```

Mặc dù có sẵn hai chỗ trống như ta vẫn không thể thêm dữ liệu vào hàng chờ được. Lúc này hàng chờ đã rỗng ta thử thêm dữ liệu vào hàng chờ:

```c
enqueue(&liQueue, 10);
enqueue(&liQueue, 30);

display(liQueue);
```

Kết quả là:

```
Enqueue data 10
Enqueue data 30
Queue: 10 30 
```

Toàn bộ chương trình trong main:
```c
int main(int argc, char const *argv[])
{
    Queue liQueue;

    initialize(&liQueue, 5);

    enqueue(&liQueue, 1);
    enqueue(&liQueue, 2);
    enqueue(&liQueue, 3);
    enqueue(&liQueue, 4);
    enqueue(&liQueue, 5);
    enqueue(&liQueue, 6);

    printf("Front: %d\n", front(liQueue));
    printf("Rear: %d\n", rear(liQueue));

    display(liQueue);

    printf("Dequeue %d\n", dequeue(&liQueue));
    printf("Dequeue %d\n", dequeue(&liQueue));
    enqueue(&liQueue, 10);
    enqueue(&liQueue, 20);
    printf("Dequeue %d\n", dequeue(&liQueue));
    printf("Dequeue %d\n", dequeue(&liQueue));
    printf("Dequeue %d\n", dequeue(&liQueue));

    display(liQueue);

    enqueue(&liQueue, 10);
    enqueue(&liQueue, 30);

    display(liQueue);
    return 0;
}
```
</details>

</details>

<details>
  <summary><strong> Circular Queue </strong></summary>

</details>
</details>
