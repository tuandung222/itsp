(content:gmm)=
# Mô hình hỗn hợp Gaussian (GMM)

## Động lực

Trong khi các phương pháp như [hồi quy tuyến tính](Linear_regression) và [mô hình không gian con](Sub-space_models) dựa trên việc giảm chiều của tín hiệu để nắm bắt thông tin thiết yếu, trong nhiều trường hợp ta muốn mô hình hóa toàn bộ dải tín hiệu có thể. Vì mục đích đó, ta có thể thiết kế các mô hình về *phân phối thống kê* của tín hiệu. Ví dụ, có thể mô hình hóa một tín hiệu như một [quá trình Gaussian](https://en.wikipedia.org/wiki/Gaussian_process), trong đó mỗi quan sát có phân phối Gaussian (đa biến) (chuẩn).

Tuy nhiên, tín hiệu tiếng nói có cấu trúc phong phú hơn nhiều so với các quá trình Gaussian đơn giản. Ví dụ, tín hiệu hữu thanh khác biệt rõ rệt so với tín hiệu vô thanh, và trong cả tín hiệu hữu thanh lẫn vô thanh, ta có vô số nhóm phát ngôn riêng biệt với các đặc trưng thống kê khác biệt. Việc mô hình hóa tất cả chúng bằng một quá trình Gaussian sẽ bỏ qua các cấu trúc như vậy và mô hình do đó sẽ kém hiệu quả.

*[Mô hình hỗn hợp](https://en.wikipedia.org/wiki/Mixture_model)* là một loại mô hình, trong đó ta giả định rằng tín hiệu đang nghiên cứu bao gồm nhiều lớp riêng biệt, mỗi lớp có mô hình thống kê riêng. Nghĩa là, ví dụ, thống kê của âm hữu thanh khác biệt rõ ràng so với âm vô thanh. Ta mô hình hóa mỗi lớp bằng phân phối riêng và phân phối chung của chúng là tổng có trọng số của các phân phối lớp. Trọng số của mỗi phân phối tương ứng với tần suất xuất hiện của chúng trong tín hiệu. Vì vậy, nếu tín hiệu vô thanh chiếm 30% tất cả các âm tiếng nói trong một ngôn ngữ giả định nào đó, thì trọng số của lớp vô thanh sẽ là 0,3.

Cấu trúc mô hình hỗn hợp điển hình nhất sử dụng phân phối Gaussian (chuẩn) cho mỗi lớp, sao cho toàn bộ mô hình được gọi là *mô hình hỗn hợp Gaussian* (GMM). Tùy theo ứng dụng, các phân phối lớp có thể mang hình thức khác ngoài Gaussian, ví dụ có thể sử dụng mô hình hỗn hợp Beta nếu các lớp riêng lẻ tuân theo phân phối Beta. Trong tài liệu này, ta tập trung vào mô hình hỗn hợp Gaussian vì nó phổ biến nhất trong các mô hình hỗn hợp và minh họa ứng dụng một cách dễ tiếp cận.


## Định nghĩa mô hình

[Phân phối chuẩn đa biến](https://en.wikipedia.org/wiki/Multivariate_normal_distribution) cho biến $x$ được định nghĩa là

$$ f\left(x;\Sigma,\mu\right) =
\frac{1}{\sqrt{\left(2\pi\right)^N \|\Sigma\|}}
\exp\left[-\frac12 (x-\mu)^T \Sigma^{-1}(x-\mu)\right], $$

trong đó $ \Sigma $ và $ \mu $ lần lượt là hiệp phương sai và trung bình của quá trình, với $N$ chiều. Nói cách khác, đây chính là quá trình Gaussian quen thuộc cho vector $x$.

Giả sử ta có $K$ lớp trong tín hiệu, mỗi lớp có hiệp phương sai và trung bình riêng $ \Sigma_k $ và $ \mu_k. $ *Mô hình hỗn hợp Gaussian* khi đó được định nghĩa là

$$ \boxed{f\left(x\right) = \sum_{k=1}^K \alpha_k f\left(x;
\Sigma_k,\mu_k\right),} $$

trong đó các trọng số $ \alpha_k $ cộng lại bằng 1: $ \sum_{k=1}^K
\alpha_k=1. $


## Ứng dụng

-   Trong các ứng dụng nhận dạng/phân loại, ta có thể, ví dụ, mô hình hóa một hệ thống có hai trạng thái riêng biệt (như tiếng nói và nhiễu) và huấn luyện một GMM với các thành phần hỗn hợp phù hợp với các trạng thái đó. Khi nhận được tín hiệu microphone, ta có thể xác định khả năng (likelihood) của mỗi thành phần hỗn hợp và từ đó suy ra khả năng tín hiệu là tiếng nói hay nhiễu.
-   Trong [các ứng dụng truyền dẫn](content:telecom), mục tiêu của ta là mô hình hóa tín hiệu sao cho có thể truyền các tín hiệu có khả năng cao với số bit nhỏ và các tín hiệu có khả năng thấp với số bit lớn. Nếu ta huấn luyện một GMM trên cơ sở dữ liệu tiếng nói, ta có thể xác định tín hiệu nào giống tiếng nói, để những tín hiệu đó có thể được truyền với số bit thấp.

