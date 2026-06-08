# Mô hình không gian con


Trong nhiều trường hợp, ta có thể giả định rằng tín hiệu có chiều thấp theo nghĩa một quan sát chiều cao $ y\in{\mathbb
R}^{N\times 1} $ có thể được giải thích hoàn toàn bởi một biểu diễn chiều thấp $ x\in{\mathbb R}^{M\times 1} $ sao cho với ma trận $ A\in{\mathbb R}^{N\times M} $ ta có $ y = Ax $ với $N\>M$. Tín hiệu này do đó chỉ trải trên một *không gian con* $M$ chiều của toàn bộ không gian $N$ chiều.


## Ứng dụng với không gian con đã biết

Biểu diễn này trở nên hữu ích khi, ví dụ, ta giả định rằng chỉ có một quan sát nhiễu của $y$. Tín hiệu mong muốn nằm trong một không gian con, nên tất cả các chiều khác chỉ chứa nhiễu và ta có thể loại bỏ chúng. Ta do đó chỉ cần một ánh xạ từ toàn bộ không gian sang không gian con. Hóa ra, một ánh xạ như vậy là phép chiếu lên không gian con được trải bởi ma trận $A$. Thực tế, nghiệm sai số bình phương trung bình tối thiểu (xem [Hồi quy tuyến tính](Linear_regression)) chính là [giả nghịch Moore-Penrose](https://en.wikipedia.org/wiki/Moore%E2%80%93Penrose_inverse). Tuy nhiên, nhược điểm của phương pháp này là ở đây ma trận A cần được biết trước để có thể tạo giả nghịch.



## Ước lượng không gian con chưa biết

Trong các trường hợp thực tế, khá hiếm khi ta có quyền truy cập vào ma trận $A$, mà thay vào đó, nó phải được ước lượng từ dữ liệu có sẵn. Một phương pháp điển hình dựa trên [phân tích giá trị suy biến (SVD)](https://en.wikipedia.org/wiki/Singular_value_decomposition) hoặc [phân tích giá trị riêng](https://en.wikipedia.org/wiki/Eigendecomposition_of_a_matrix). Tóm lại, ta trước tiên ước lượng ma trận hiệp phương sai của tín hiệu và sau đó phân tích nó thành các thành phần không tương quan bằng phân tích giá trị suy biến. Thông thường, một tập nhỏ các giá trị suy biến chiếm phần lớn năng lượng của toàn bộ tín hiệu. Do đó, nếu ta loại bỏ các giá trị suy biến nhỏ nhất, ta không mất nhiều năng lượng nhưng có một tín hiệu có chiều thấp hơn nhiều. Phân tích giá trị suy biến do đó đóng vai trò của ma trận ánh xạ không gian con $A$, và ta có thể áp dụng mô hình như đã mô tả ở trên.



## Thảo luận

Các mô hình không gian con là những mô hình lý thuyết hấp dẫn, vì việc phân tích chúng rất trực tiếp. Về tín hiệu tiếng nói, khó khăn nằm ở việc tìm một biểu diễn thực sự có hạng thấp. Nói cách khác, không rõ ràng ngay trong miền nào ta có thể áp dụng phân tích sao cho tín hiệu tiếng nói có thể được mô hình hóa hiệu quả bằng các mô hình hạng thấp.

