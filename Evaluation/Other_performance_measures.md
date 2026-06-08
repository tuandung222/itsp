(content:otherperformance)=
# Các tiêu chí hiệu suất khác

## Độ phức tạp tính toán

Ở cấp độ ứng dụng, các thuật toán xử lý tiếng nói thường được triển khai trên các thiết bị hạn chế tài nguyên như điện thoại di động. Các thiết bị di động có năng lực tính toán hạn chế, do đó việc thiết kế các thuật toán hiệu quả là rất quan trọng để tiết kiệm pin. Có nhiều cách khác nhau để phân tích độ phức tạp tính toán của một thuật toán, tùy thuộc vào giai đoạn thiết kế hoặc mục đích của ứng dụng thực tế:

### Ký hiệu O-lớn (Big-O)

Độ phức tạp của một thuật toán thường được hiểu là thước đo lượng thời gian cần thiết để thuật toán hoàn thành với đầu vào có kích thước $n$. Khi kích thước đầu vào tăng lên, thời gian tính toán vẫn cần phải nằm trong một giới hạn thực tế. Vì lý do này, độ phức tạp được đo dưới dạng tiệm cận khi $n$ tiến tới vô cùng. Cách biểu diễn phổ biến nhất của độ phức tạp thuật toán là ký hiệu O-lớn (Big-O notation). Ký hiệu O-lớn cung cấp một giới hạn trên cho tốc độ tăng trưởng thời gian tính toán của một thuật toán. Điều này đặc biệt hữu ích vì ký hiệu này cho phép chúng ta so sánh các thuật toán trong các tình huống xấu nhất (worst-case scenarios). Hình 1 minh họa tốc độ tăng trưởng của các ký hiệu O-lớn khác nhau đối với kích thước đầu vào.

Ví dụ, độ phức tạp $O(n)$, đọc là "độ phức tạp O của n", đại diện cho một thuật toán có thời gian tính toán tăng tuyến tính với kích thước đầu vào. Dưới đây là một số ví dụ cho từng loại độ phức tạp:

-   $O(1)$ - Thời gian tính toán không tăng theo kích thước đầu vào:
    -   Truy cập một phần tử trong mảng qua chỉ số (`a = array[4]`).
          
-   $O(\log n)$ - Thời gian tính toán tăng theo logarit của kích thước đầu vào:
    -   Thuật toán tìm kiếm nhị phân (Binary Search).
          
-   $O(n)$ - Thời gian tính toán tăng tuyến tính theo kích thước đầu vào:
    -   Duyệt qua một mảng.
    -   So sánh hai chuỗi ký tự.
          
-   $O(n^2)$ - Thời gian tính toán tăng theo bình phương của kích thước đầu vào:
    -   Các phép toán ma trận như duyệt qua mảng hai chiều hoặc nhân mảng một chiều với ma trận hai chiều.
    -   Biến đổi Fourier rời rạc thông thường (phép nhân ma trận).
          
-   $O(n \log n)$ - Thành phần "$\log n$" được thêm vào khi các thuật toán $O(n^2)$ được thực hiện bằng các kỹ thuật chia để trị (divide-and-conquer) nhằm tăng hiệu suất của chúng.
    -   Biến đổi Fourier nhanh (FFT).
          
-   $O(2^n)$ - Thời gian tính toán tăng gấp đôi sau mỗi lần thêm dữ liệu đầu vào, do đó tăng theo cấp số nhân (exponentially):
    -   Các thuật toán đệ quy $\rightarrow$ Để giải một bài toán kích thước $N$, cần phải giải hai bài toán kích thước $N - 1$.
          
-   $O(n!)$ - Tốc độ tăng trưởng giai thừa đại diện cho các thuật toán tăng thậm chí còn nhanh hơn cả các ví dụ hàm mũ:
    -   Tìm tất cả các hoán vị có thể có của một danh sách.

### Triệu phép tính có trọng số trên giây (WMOPS)
[ITU-T. Software tool library: User’s manual, 2009](https://www.itu.int/rec/T-REC-G.191-200911-S/en)

Ký hiệu O-lớn mang lại cho chúng ta khái niệm trực quan về độ phức tạp của các thuật toán cụ thể. Điều này giúp chúng ta so sánh và chọn lựa thuật toán hiệu quả nhất để sử dụng. Tuy nhiên, trong các ứng dụng như mã hóa tiếng nói (speech coding), việc biết chính xác số lượng phép tính mà hệ thống cần thực hiện để xử lý mỗi khung âm thanh (audio frame) là cực kỳ quan trọng.

Liên minh Viễn thông Quốc tế (ITU-T) cung cấp các hướng dẫn để đo lường số lượng phép tính trong một chương trình. Phép đo này tính đến thực tế là không phải tất cả các phép tính đều có tải trọng tính toán như nhau và thực hiện gán trọng số tương ứng cho các giá trị của chúng. Ví dụ, phép tính logarit nặng hơn rất nhiều so với phép cộng. Kết quả cuối cùng được biểu diễn dưới dạng Triệu phép tính có trọng số trên giây (Weighted Million Operations Per Second - WMOPS). Bảng 1 trình bày trọng số được sử dụng cho từng phép tính cụ thể.

![bigo](attachments/175510471.png)

**Hình 1:** Sự tiến triển của thời gian tính toán đối với các ký hiệu O-lớn khác nhau phụ thuộc vào kích thước đầu vào.

| Phép tính | Ví dụ | Trọng số |
|:----------------------:|:------------------------:|:---------------------:|
| Phép cộng | a = b + c | 1 |
| Phép nhân | a = b ∗ c | 1 |
| Nhân + Cộng | a+ = b ∗ c | 1 |
| Gán giá trị | a = b | 1 |
| Lưu vào mảng | a\[i\] = b\[i\] + c\[i\] | 1 |
| Phép toán logic | AND, OR, v.v. | 1 |
| Phép dịch bit | a = b \>\> c | 1 |
| Phép rẽ nhánh | if, if...else | 4 |
| Phép chia | a = b/c | 18 |
| Căn bậc hai | a = sqrt(b) | 10 |
| Hàm siêu việt | sine, arctan, v.v. | 25 |
| Gọi hàm | a = func(b, c, d) | 2 + số lượng đối số truyền vào và trả về |
| Khởi tạo vòng lặp | for(i=0;i... | 3 |
| Trỏ gián tiếp | a = b.c | 2 |
| Khởi tạo con trỏ | a\[i\] | 1 |
| Hàm mũ | pow, en | 25 |
| Logarit | log | 25 |
| Kiểm tra điều kiện | được sử dụng kết hợp với BRANCH | 2 |

**Bảng 1:** Các phép tính được công cụ WMOPS tính đến và trọng số tương đối của chúng.
