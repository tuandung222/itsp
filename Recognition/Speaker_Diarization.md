# Phân đoạn và định danh người nói (Speaker Diarization)

## Giới thiệu về phân đoạn và định danh người nói

Phân đoạn và định danh người nói (speaker diarization) là quá trình phân đoạn và phân cụm một bản ghi tiếng nói thành các vùng đồng nhất (homogeneous regions) nhằm trả lời câu hỏi "ai nói lúc nào" mà không cần bất kỳ thông tin tiên nghiệm nào về những người nói. Một hệ thống phân đoạn định danh điển hình thực hiện ba tác vụ cơ bản. Thứ nhất, nó phân biệt các phân đoạn tiếng nói với các phân đoạn phi tiếng nói. Thứ hai, nó phát hiện các điểm thay đổi người nói để phân đoạn dữ liệu âm thanh. Cuối cùng, nó nhóm các vùng đã phân đoạn này vào các cụm đồng nhất của từng người nói.

An overview of a speaker diarization system.
*(Sơ đồ tổng quan về hệ thống phân đoạn và định danh người nói)*

Mặc dù có nhiều hướng tiếp cận khác nhau để thực hiện phân đoạn định danh người nói, hầu hết các hệ thống đều tuân theo quy trình sau:

-   **Trích xuất đặc trưng (Feature extraction):** Trích xuất thông tin cụ thể từ tín hiệu âm thanh để phục vụ cho việc mô hình hóa và phân loại người nói ở các giai đoạn sau. Các đặc trưng được trích xuất lý tưởng nhất là tối đa hóa sự biến thiên giữa các người nói (inter-speaker variability), tối thiểu hóa sự biến thiên trong cùng một người nói (intra-speaker variability) và biểu diễn được các thông tin liên quan.
-   **Phân đoạn người nói (Speaker segmentation):** Chia tách dữ liệu âm thanh thành các phân đoạn đồng nhất về mặt âm học theo danh tính người nói. Nó phát hiện tất cả các vị trí ranh giới trong mỗi vùng tiếng nói tương ứng với các điểm thay đổi người nói, các điểm này sau đó được sử dụng cho việc phân cụm người nói.
-   **Phân cụm người nói (Speaker clustering):** Nhóm các phân đoạn tiếng nói thuộc về một người nói cụ thể lại với nhau. Nó có hai danh mục chính dựa trên yêu cầu xử lý: phân cụm trực tuyến (online) và phân cụm ngoại tuyến (offline). Trong phân cụm trực tuyến, các phân đoạn tiếng nói được hợp nhất hoặc chia tách trong các vòng lặp liên tiếp cho đến khi thu được số lượng người nói tối ưu. Vì toàn bộ tệp tiếng nói đều có sẵn trước khi đưa ra quyết định trong phân cụm ngoại tuyến, phương pháp này cung cấp kết quả tốt hơn nhiều so với phân cụm trực tuyến. Kỹ thuật phân cụm người nói phổ biến và được sử dụng rộng rãi nhất là Phân cụm phân cấp tích lũy (Agglomerative Hierarchical Clustering - AHC). AHC xây dựng một cấu trúc phân cấp các cụm, thể hiện mối quan hệ giữa các phân đoạn tiếng nói, và hợp nhất các phân đoạn tiếng nói dựa trên độ tương đồng. Các hướng tiếp cận AHC có thể được phân loại thành phân cụm dưới-lên (bottom-up) và trên-xuống (top-down).

Hai yếu tố cần được định nghĩa trong cả phân cụm dưới-lên và trên-xuống là:

1.  **Khoảng cách giữa các phân đoạn tiếng nói** để xác định độ tương đồng âm học. Chỉ số khoảng cách được sử dụng để quyết định xem hai cụm có phải hợp nhất (phân cụm dưới-lên) hoặc chia tách (phân cụm trên-xuống) hay không.
2.  **Tiêu chí dừng (stopping criterion)** để xác định khi nào đạt được số lượng cụm (người nói) tối ưu.

**Dưới-lên (Bottom-up / Agglomerative):** Bắt đầu từ một số lượng lớn các phân đoạn tiếng nói ban đầu và hợp nhất các phân đoạn tiếng nói gần nhau nhất một cách lặp đi lặp lại cho đến khi đáp ứng tiêu chí dừng. Kỹ thuật này được sử dụng rộng rãi nhất trong phân đoạn định danh người nói vì nó được áp dụng trực tiếp trên đầu ra của các phân đoạn tiếng nói từ bước phân đoạn người nói. Một ma trận khoảng cách giữa mọi cặp cụm khả thi được tính toán và cặp cụm có giá trị BIC cao nhất sẽ được hợp nhất. Sau đó, các cụm đã hợp nhất được loại bỏ khỏi ma trận khoảng cách. Cuối cùng, bảng ma trận khoảng cách được cập nhật bằng cách sử dụng khoảng cách giữa cụm mới hợp nhất và tất cả các cụm còn lại. Quá trình này được thực hiện lặp đi lặp lại cho đến khi đáp ứng tiêu chí dừng hoặc tất cả các cặp có giá trị BIC nhỏ hơn 0.

**Trên-xuống (Top-down):** Các phương pháp phân cụm phân cấp trên-xuống bắt đầu từ một số lượng nhỏ các cụm, thông thường là một cụm duy nhất chứa tất cả các phân đoạn tiếng nói, và các cụm ban đầu này được chia tách lặp đi lặp lại cho đến khi đáp ứng tiêu chí dừng. Phương pháp này không được sử dụng rộng rãi như phân cụm dưới-lên.

Bottom-Up and Top-down approaches to clustering
*(Các hướng tiếp cận phân cụm Dưới-lên và Trên-xuống)*

## Các hướng tiếp cận phân đoạn và định danh người nói

Phần này mô tả một số hệ thống phân đoạn định danh người nói tiên tiến nhất hiện nay.

**Hệ thống phân đoạn định danh dựa trên HMM/GMM:** Mỗi người nói được biểu diễn bằng một trạng thái của một mô hình HMM và các xác suất phát xạ trạng thái (state emission probabilities) được mô hình hóa bằng các mô hình GMM. Việc phân cụm ban đầu được thực hiện bằng cách phân chia tín hiệu âm thanh thành các phần bằng nhau để tạo ra một tập hợp các phân đoạn $\{s_i\}$. Gọi $c_i$ đại diện cho cụm người nói thứ $i$, $b_i$ đại diện cho xác suất phát xạ của cụm $c_i$ và $f_t$ biểu thị một vectơ đặc trưng tại thời điểm $t$. Khi đó, log-likelihood $\log b_i(s_t)$ của đặc trưng $f_t$ đối với cụm $c_i$ được tính toán như sau:

$$ \log b_i(s_t)=\log \sum_{(r)} {w}^r_i N (f_t,{\mu}^r_i,\Sigma_{(i)}^r) $$

ở đây $N()$ là hàm mật độ xác suất Gauss và $w^r_i, \mu^r_i, \Sigma_{(i)}^r$ tương ứng là trọng số, kỳ vọng và ma trận hiệp phương sai của thành phần hỗn hợp Gauss thứ $r$ của cụm $c_i$.

Phân cụm phân cấp tích lũy bắt đầu bằng việc ước lượng quá mức (overestimating) số lượng cụm ban đầu. Tại mỗi vòng lặp, các cụm tương đồng nhất được hợp nhất dựa trên khoảng cách BIC. Phép đo khoảng cách dựa trên tiêu chí thông tin Bayes delta sửa đổi (modified delta Bayesian Information Criterion) [Ajmera and Wooters, 2003]. Khoảng cách BIC sửa đổi không tính đến thành phần phạt (penalty term) tương ứng với số lượng các tham số tự do của phân phối Gauss đa biến và được biểu diễn dưới dạng:

$$ \Delta BIC (c_i,c_j)= \log \sum_{f_t \in ( {c_i \; \cup \; c_j})} b_{ij}(f_t) - \log \sum_{f_t \in c_i} b_{i}(f_t) - \log \sum_{f_t \in c_j} b_{j}(f_t) $$

ở đây $b_{ij}$ là phân phối xác suất của các cụm kết hợp $c_i$ và $c_j$.

Các cụm tạo ra điểm số $\Delta BIC$ cao nhất được hợp nhất tại mỗi vòng lặp. Thời lượng tối thiểu của các phân đoạn tiếng nói thông thường bị ràng buộc cho mỗi lớp để ngăn chặn việc giải mã các phân đoạn quá ngắn. Số lượng cụm giảm dần sau mỗi vòng lặp. Khi khoảng cách $\Delta BIC$ lớn nhất giữa các cụm này nhỏ hơn giá trị ngưỡng 0, hệ thống phân đoạn định danh người nói sẽ dừng và đưa ra giả thuyết kết quả cuối cùng.

**Các kỹ thuật phân tích nhân tố (Factor analysis):** Các kỹ thuật phân tích nhân tố, vốn là công nghệ tiên tiến nhất trong nhận dạng người nói, gần đây đã được áp dụng thành công trong phân đoạn định danh người nói. Các cụm tiếng nói trước tiên được biểu diễn bằng các vectơ i-vector và các giai đoạn phân cụm tiếp theo được thực hiện dựa trên mô hình hóa i-vector. Việc sử dụng kỹ thuật phân tích nhân tố để mô hình hóa các phân đoạn tiếng nói giúp giảm số chiều của vectơ đặc trưng bằng cách giữ lại hầu hết các thông tin liên quan. Một khi các cụm tiếng nói được biểu diễn bằng các i-vector, kỹ thuật tính điểm khoảng cách cosine và PLDA có thể được áp dụng để quyết định xem hai cụm thuộc về cùng một người nói hay các người nói khác nhau.

**Hướng tiếp cận học sâu (Deep learning):** Phân đoạn định danh người nói đóng vai trò quan trọng cho nhiều công nghệ tiếng nói khi có sự xuất hiện của nhiều người nói, nhưng hầu hết các phương pháp hiện tại sử dụng phân cụm i-vector cho các phân đoạn tiếng nói ngắn có khả năng quá cồng kềnh và tốn kém để đóng vai trò tiền xử lý (frontend). Vì vậy, Daniel Povey đã đề xuất một hướng tiếp cận thay thế để học các biểu diễn thông qua mạng nơ-ron sâu nhằm loại bỏ hoàn toàn quy trình trích xuất i-vector khỏi hệ thống. Kiến trúc được đề xuất đồng thời học một vectơ nhúng (embedding) có số chiều cố định cho các phân đoạn âm học có độ dài biến thiên và một hàm tính điểm để đo khả năng các phân đoạn này bắt nguồn từ cùng một người nói hay những người nói khác nhau. Hệ thống dựa trên mạng nơ-ron được đề xuất này đạt hiệu suất tương đương hoặc vượt trội so với các hệ thống baseline tiên tiến nhất.

## Các tiêu chí đánh giá hiệu suất

Tỷ lệ lỗi phân đoạn định danh (Diarization Error Rate - DER) là chỉ số được sử dụng để đo lường hiệu suất của các hệ thống phân đoạn định danh người nói. Nó được tính bằng tỷ lệ thời gian không được quy cho chính xác cho một người nói hoặc phi tiếng nói trên tổng thời gian.

DER được cấu thành từ ba loại lỗi sau:

-   **Lỗi người nói (Speaker Error):** Là tỷ lệ phần trăm thời gian tính điểm (scored time) mà ID người nói bị gán sai cho một người nói khác. Lỗi người nói chủ yếu là lỗi của hệ thống phân đoạn định danh (tức là nó không liên quan đến lỗi phát hiện tiếng nói/phi tiếng nói). Nó cũng không tính đến các phần tiếng nói chồng chéo (overlap speech) không được phát hiện.
-   **Báo động giả (False Alarm):** Là tỷ lệ phần trăm thời gian tính điểm mà một người nói giả thuyết được dán nhãn là phi tiếng nói trong bản tham chiếu. Lỗi báo động giả xảy ra chủ yếu do lỗi phát hiện tiếng nói/phi tiếng nói (tức là bộ phát hiện tiếng nói coi một phân đoạn phi tiếng nói là một phân đoạn tiếng nói). Do đó, lỗi báo động giả không liên quan đến lỗi phân đoạn và phân cụm.
-   **Bỏ sót tiếng nói (Missed Speech):** Là tỷ lệ phần trăm thời gian tính điểm mà một phân đoạn phi tiếng nói giả thuyết thực tế tương ứng với một phân đoạn tiếng nói của người nói trong bản tham chiếu. Lỗi bỏ sót tiếng nói xảy ra chủ yếu do lỗi phát hiện tiếng nói (tức là phân đoạn tiếng nói bị coi là phân đoạn phi tiếng nói). Do đó, lỗi bỏ sót tiếng nói không liên quan đến lỗi phân đoạn và phân cụm.

$$ DER = \text{Speaker Error} + \text{False Alarm} + \text{Missed Speech} $$
