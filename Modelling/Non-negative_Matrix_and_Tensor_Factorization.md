# Phân tích ma trận và tensor không âm

## Giới thiệu

Nhiều đặc trưng mô tả nhất của tiếng nói được biểu diễn qua năng lượng; ví dụ, các formant là các đỉnh và tần số cơ bản hiện diện dưới dạng cấu trúc lược trong phổ công suất. Một tính chất cơ bản của các đặc trưng này là chúng có giá trị dương. Giá trị âm trong năng lượng không khả thi về mặt vật lý. Tuy nhiên, hầu hết các phương pháp xử lý tín hiệu chỉ áp dụng được cho biến giá trị thực và việc đưa vào ràng buộc không âm khá phức tạp.

*Phân tích ma trận không âm* (NMF hoặc NNMF) và các phương pháp tương ứng cho tensor là một họ các phương pháp giả định rõ ràng rằng các biến đầu vào là *không âm*, nghĩa là chúng mặc nhiên áp dụng được cho tín hiệu năng lượng. Theo một nghĩa nào đó, các phương pháp NMF là mở rộng của [phân tích thành phần chính (PCA)](https://en.wikipedia.org/wiki/Principal_component_analysis) và các [phương pháp không gian con](Sub-space_models.md) khác sang tín hiệu giá trị dương.


## Định nghĩa mô hình

Cụ thể, giả sử phổ công suất (hoặc biên độ) của một cửa sổ tín hiệu tiếng nói được biểu diễn dưới dạng vector $Nx1$ $v_k$, và hơn nữa ta sắp xếp $K$ cửa sổ thành ma trận $NxK$ $V$. Mô hình tín hiệu ta sử dụng là

$$ V \approx WH, $$

trong đó $W$ là ma trận trọng số $N\times M$, $H$ là ma trận mô hình $M\times K$ và vô hướng $M$ là bậc mô hình.

Ý tưởng là $H$ là một ma trận cố định tương ứng với mô hình tín hiệu của ta, tức mô hình nguồn. Nó mô tả các loại đặc trưng điển hình của dữ liệu. Với các trọng số $W$, ta nội suy giữa các cột của $H$. Theo một nghĩa nào đó, đây là sự tổng quát hóa của một bảng mã (codebook) (xem [lượng tử hóa vector](content:vq)), nhưng sao cho ta nội suy giữa các vector mã. Ngoài ra, ta yêu cầu tất cả các phần tử của $W$ và $H$ đều không âm, để đảm bảo rằng $V$ cũng không âm.

Vì bậc mô hình $K$ được chọn nhỏ hơn cả $N$ hoặc $K$, phép ánh xạ này nói chung là một phép xấp xỉ. Mô hình do đó cố gắng nắm bắt *các đặc trưng liên quan của tín hiệu đầu vào với số lượng tham số thấp*.

Mô hình thường được tối ưu hóa bằng

$$ \min_{W,H} \| V - WH \|_F\qquad\text{với điều kiện}\qquad
W,H\geq 0. $$

Ở đây chuẩn đề cập đến [chuẩn Frobenius](https://en.wikipedia.org/wiki/Matrix_norm#Frobenius_norm), được định nghĩa là căn bậc hai của tổng bình phương các phần tử. Ta không có nghiệm giải tích cho bài toán tối ưu trên, nhưng có thể giải bằng các phương pháp số, vốn được tích hợp trong các thư viện phần mềm thông dụng.


## Ứng dụng

Một ứng dụng điển hình của các thuật toán kiểu NMF là tách nguồn, trong đó ta tìm nghiệm của bài toán tối ưu trên và sau đó xác định các chiều của $H$ tương ứng với các nguồn khác nhau. Bằng cách chỉ giữ lại các chiều của $W$ tương ứng với nguồn mong muốn, ta có thể trích xuất tín hiệu nguồn mong muốn từ hỗn hợp với các nguồn gây nhiễu khác. Ví dụ, ta có thể muốn trích xuất tín hiệu tiếng nói bị ảnh hưởng bởi nhiễu bằng cách trích xuất các chiều tương ứng với tiếng nói và loại bỏ các chiều tương ứng với nhiễu.

Tuy nhiên, cần lưu ý rằng các phương pháp kiểu NMF chỉ trích xuất phổ công suất (hoặc biên độ) của tín hiệu mong muốn. Ngược lại, thông thường tín hiệu đầu vào là một biểu diễn thời gian-tần số cũng có thành phần pha. Sau khi áp dụng ước lượng NMF, ta do đó cũng cần một ước lượng thành phần pha của tín hiệu. Các phương pháp như vậy sẽ được thảo luận trong chương [nâng cao chất lượng tiếng nói](../Speech_enhancement.md) của tài liệu này.


Để biết thêm thông tin, xem bài viết Wikipedia: [Phân tích ma trận không âm](https://en.wikipedia.org/wiki/Non-negative_matrix_factorization).

