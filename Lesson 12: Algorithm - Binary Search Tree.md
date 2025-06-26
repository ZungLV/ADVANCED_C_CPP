<details>
  <summary><strong> Bubble Sort </strong></summary>

Thuật toán sắp xếp nổi bọt (**Bubble Sort**) hoạt động dựa trên nguyên tắc **hoán đổi** các phần tử liền kề để đưa phần tử **lớn hơn về cuối dãy** (hoặc phần tử nhỏ hơn về đầu dãy).
Thuật toán gồm các bước sau:
1. Duyệt qua danh sách từ đầu đến cuối.
2. So sánh hai phần tử liền kề, nếu phần tử trước lớn hơn phần tử sau, thì hoán đổi vị trí.
3. Lặp lại quá trình cho đến khi không còn sự hoán đổi nào xảy ra (mảng đã được sắp xếp).

Chương trình thuật toán Bubble Sort

```c
void bubbleSort(int arr[], int n)
{
    for (int i=0; i<=n-2; i++)
    {
        for (int j=0; j<=n-i-2; j++)
        {
            if (arr[j] > arr[j+1])
            {
                int temp = arr[j];
                arr[j]   = arr[j+1];
                arr[j+1] = temp;
            }
        }
    }  
}
```
Chương trình hoạt động như sau:

1. `n` là số phần tử tham gia sắp xếp. Vòng lặp đầu tiên `for (int i=0; i<=n-2; i++)` sẽ thực hiện **các lượt hoán đổi n-1 lần** (`i<=n-2` là do `i` bắt đầu từ 0 nên trừ thêm 1). Sau khi vòng lặp này được thực hiện xong thì n-1 phần tử tham gia đã được đưa về cuối dãy để sắp xếp đúng thứ tự từ lớn đến bé, khi đó phần tử cuối cùng sẽ hiển nhiên nằm ở đúng vị trí.
2. Vòng lặp thứ hai `for (int j=0; j<=n-i-2; j++)`, vòng lặp con của vòng lặp đầu thực hiện hoán đổi hai phần tử liền kề nhau. Số lần hoán đổi sẽ bị ảnh hưởng bởi số lượng phần tử và số lần vòng lặp đầu đã thực hiện. `j<=n-i-2` có thể hiểu `j<=n-1-1-i` do trừ đi cho việc `j` bắt đầu từ 0 và chỉ cần `n-1` lần để hoán đổi hết n phần tử. Cứ mỗi lần một lặp mẹ được thực hiện sẽ có `i` phần tử nằm đúng vị trí, do đó ứng với mỗi vòng lặp đầu ta sẽ không cần phải hoán đổi `i` phần tử.    

</details>

<details>
  <summary><strong> Linear Search </strong></summary>

Thuật toán tìm kiếm tuyến tính (Linear Search) là phương pháp đơn giản nhất để tìm kiếm một phần tử trong mảng.

Nguyên tắc hoạt động:

1. Duyệt từng phần tử trong mảng từ trái sang phải.
2. Nếu phần tử đang xét trùng với giá trị cần tìm, trả về vị trí của nó.
3. Nếu duyệt hết mảng mà không tìm thấy, trả về kết quả không tồn tại.

</details>


<details>
  <summary><strong> Binary Search Tree </strong></summary>
  
Thuật toán tìm kiếm nhị phân (**Binary Search**) hoạt động bằng cách chia đôi mảng để tìm kiếm, thay vì duyệt tuần tự như Linear Search.
  
Nguyên tắc hoạt động:

1. Sắp xếp mảng (tăng dần hoặc giảm dần).

2. So sánh phần tử ở giữa mảng với giá trị cần tìm:

- Nếu trùng 	→ Trả về vị trí

- Nếu nhỏ hơn 	→ Tiếp tục tìm trong nửa phải

- Nếu lớn hơn 	→ Tiếp tục tìm trong nửa trái

3. Lặp lại bước 2 cho đến khi tìm thấy phần tử hoặc không còn phần tử nào để tìm.

![image](https://github.com/user-attachments/assets/eca49621-b5c7-4072-b87d-9182609557b6)
![image](https://github.com/user-attachments/assets/d9ac50d6-b873-44ea-aa4b-9aea904d17e3)
![image](https://github.com/user-attachments/assets/692ab55c-6761-4cec-bf69-194bb9221193)
![image](https://github.com/user-attachments/assets/65ada61c-7a7f-4c30-b26f-aa37e8209912)
![image](https://github.com/user-attachments/assets/77bfa517-d7e4-4ccb-ac7e-932e3be63d5a)
![image](https://github.com/user-attachments/assets/5a7a1f19-1e8a-4f64-8b5a-6d6a91060ef7)
![image](https://github.com/user-attachments/assets/5fd4eb30-06fa-4a96-a3da-c7d60dd6d692)



Cấu trúc dữ liệu phân cấp (Tree) là một cấu trúc dữ liệu phi tuyến tính, trong đó các phần tử (được gọi là nút, hay node) được tổ chức theo một thứ bậc phân cấp. Cây là một trong những cấu trúc dữ liệu quan trọng, được sử dụng rộng rãi trong khoa học máy tính để biểu diễn các quan hệ phân cấp, tìm kiếm, sắp xếp, và lưu trữ.

![image](https://github.com/user-attachments/assets/ea70e71f-fe90-4714-bc63-26389ac96581)

Cây Tìm Kiếm Nhị Phân (BST - Binary Search Tree) là một cấu trúc dữ liệu dạng cây, trong đó:

Mỗi nút có tối đa 2 con (gọi là cây con trái và cây con phải).

Dữ liệu trong cây tuân theo quy tắc:

-  Nút con trái chứa giá trị nhỏ hơn nút gốc.
-  Nút con phải chứa giá trị lớn hơn nút gốc.
-  Quy tắc này áp dụng đệ quy cho toàn bộ cây.

</details>


