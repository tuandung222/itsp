(content:sid)=
# Nhận dạng và Xác thực người nói (Speaker Recognition and Verification)

## Giới thiệu về nhận dạng người nói

Nhận dạng người nói (speaker recognition) là tác vụ xác định danh tính của một người nói bằng cách sử dụng giọng nói của họ. Nhận dạng người nói được chia thành hai bài toán: định danh người nói (speaker identification) và xác thực người nói (speaker verification). Trong khi định danh người nói là quá trình xác định xem giọng nói nào trong một nhóm các giọng nói đã biết khớp nhất với giọng của người nói hiện tại, thì xác thực người nói là tác vụ chấp nhận hoặc từ chối tuyên bố danh tính của một người nói bằng cách phân tích các mẫu âm học của họ. Các hệ thống xác thực người nói có độ phức tạp tính toán thấp hơn so với các hệ thống định danh người nói vì chúng chỉ yêu cầu so sánh giữa một hoặc hai mô hình, trong khi định danh người nói yêu cầu so sánh một mô hình đầu vào với $N$ mô hình người nói trong cơ sở dữ liệu.

Các phương pháp xác thực người nói được chia thành các phương pháp phụ thuộc văn bản (text-dependent) và độc lập văn bản (text-independent). Trong các phương pháp phụ thuộc văn bản, hệ thống xác thực người nói đã biết trước văn bản sẽ được nói và người dùng được yêu cầu phát âm chính xác văn bản này. Ngược lại, trong hệ thống độc lập văn bản, hệ thống không có kiến thức trước về văn bản sẽ được nói và người dùng không cần phải hợp tác phát âm theo một kịch bản định sẵn. Các hệ thống phụ thuộc văn bản đạt được hiệu suất xác thực người nói cao với các phát ngôn tương đối ngắn, trong khi các hệ thống độc lập văn bản yêu cầu các phát ngôn dài để huấn luyện các mô hình đáng tin cậy và đạt được hiệu suất tốt.

![](attachments/SV_Architecture.png)
**Hình 1:** Sơ đồ khối của một hệ thống xác thực người nói cơ bản

Như được thể hiện trong sơ đồ khối ở trên, một hệ thống xác thực người nói bao gồm hai giai đoạn chính: giai đoạn đăng ký (training/enrolment phase) trong đó người nói mục tiêu được đăng ký thông tin giọng nói vào hệ thống, và giai đoạn kiểm thử (testing phase) trong đó quyết định về danh tính của người nói được đưa ra. Từ góc độ huấn luyện, các mô hình người nói có thể được phân loại thành mô hình sinh (generative) và mô hình phân biệt (discriminative). Các mô hình sinh như Mô hình hỗn hợp Gauss (GMM) ước lượng phân phối đặc trưng của từng người nói. Ngược lại, các mô hình phân biệt như Máy vectơ hỗ trợ (SVM) và Mạng nơ-ron sâu (DNN) mô hình hóa ranh giới quyết định giữa các người nói.

Hiệu suất của các hệ thống xác thực người nói bị suy giảm bởi sự biến thiên của kênh truyền và phiên ghi âm (channel and session variability) giữa tín hiệu tiếng nói lúc đăng ký và lúc xác thực. Các yếu tố ảnh hưởng đến sự biến thiên của kênh/phiên ghi âm bao gồm:

1.  Sự không khớp kênh (channel mismatch) giữa tín hiệu tiếng nói đăng ký và xác thực, chẳng hạn như sử dụng các microphone khác nhau trong hai giai đoạn này.
2.  Tiếng ồn môi trường và điều kiện vang âm (reverberation).
3.  Sự thay đổi trong giọng nói của chính người nói như sự già hóa, tình trạng sức khỏe, phong cách nói và trạng thái cảm xúc.
4.  Kênh truyền dẫn như điện thoại cố định, điện thoại di động, microphone và giao thức truyền giọng nói qua Internet (VoIP).

## Xử lý tiền tuyến (Front-end Processing)

Nhiều kỹ thuật xử lý tiền tuyến (front-end processing) được áp dụng để xử lý tín hiệu tiếng nói và trích xuất các đặc trưng sử dụng trong hệ thống xác thực người nói. Quá trình xử lý tiền tuyến này chủ yếu bao gồm phát hiện hoạt động tiếng nói (VAD), trích xuất đặc trưng và các kỹ thuật bù kênh:

1.  [**Phát hiện hoạt động tiếng nói (VAD)**](Voice_activity_detection.ipynb): Mục tiêu chính của phát hiện hoạt động tiếng nói là xác định phân đoạn nào của tín hiệu là tiếng nói và phân đoạn nào là phi tiếng nói. Một thuật toán VAD mạnh mẽ có thể cải thiện hiệu suất của hệ thống xác thực người nói bằng cách đảm bảo rằng danh tính người nói chỉ được tính toán từ các vùng có tiếng nói. Do đó, việc xem xét thuật toán VAD là cần thiết để khắc phục các vấn đề trong thiết kế một hệ thống xác thực người nói mạnh mẽ. Ba kỹ thuật được sử dụng rộng rãi cho VAD là: dựa trên năng lượng, dựa trên mô hình và các phương pháp lai (hybrid).
2.  **Kỹ thuật trích xuất đặc trưng** được sử dụng để chuyển đổi tín hiệu tiếng nói thành các vectơ đặc trưng âm học. Các đặc trưng âm học được trích xuất cần mang các đặc tính thiết yếu của tín hiệu tiếng nói giúp nhận diện danh tính người nói qua giọng nói của họ. Mục tiêu của trích xuất đặc trưng là giảm số chiều của các vectơ đặc trưng âm học bằng cách loại bỏ thông tin không mong muốn và nhấn mạnh thông tin đặc trưng của người nói. Các hệ số MFCC thường được sử dụng làm kỹ thuật trích xuất đặc trưng cho các hệ thống xác thực người nói hiện đại.
3.  **Kỹ thuật bù kênh (channel compensation)** được sử dụng để giảm thiểu ảnh hưởng của sự không khớp kênh và nhiễu môi trường. Bù kênh có thể được áp dụng ở các giai đoạn khác nhau của hệ thống xác thực người nói, chẳng hạn như miền đặc trưng và miền mô hình. Các kỹ thuật bù kênh khác nhau như phép trừ kỳ vọng cepstrum (Cepstral Mean Subtraction - CMS), uốn cong đặc trưng (feature warping), chuẩn hóa phương sai kỳ vọng cepstrum (Cepstral Mean Variance Normalization - CMVN) và xử lý phổ tương đối (RASTA) đã được sử dụng để giảm ảnh hưởng của không khớp kênh trong giai đoạn trích xuất đặc trưng. Trong miền mô hình, Phân tích nhân tố đồng thời (Joint Factor Analysis - JFA) và các vectơ i-vector được sử dụng để chống lại sự không khớp giữa lúc đăng ký và lúc xác thực.

## Các kỹ thuật mô hình hóa người nói

Một trong những vấn đề cốt lõi trong phân đoạn và định danh người nói (speaker diarization) là các kỹ thuật được sử dụng để mô hình hóa người nói. Một số kỹ thuật mô hình hóa đã được sử dụng trong các tác vụ nhận dạng và phân đoạn định danh người nói. Các kỹ thuật mô hình hóa người nói tiên tiến nhất bao gồm:

### Mô hình hỗn hợp Gauss (GMM) - Phương pháp mô hình nền phổ quát (UBM)

Mô hình hỗn hợp Gauss (GMM) là một hàm mật độ xác suất tham số được biểu diễn dưới dạng tổng có trọng số của các mật độ thành phần Gauss. GMM đã được sử dụng thành công để mô hình hóa các đặc trưng tiếng nói trong các ứng dụng xử lý tiếng nói khác nhau. Một mô hình hỗn hợp Gauss là tổng có trọng số của $M$ mật độ thành phần Gauss. Mỗi thành phần là một hàm Gauss đa biến. Một GMM được đại diện bởi các vectơ kỳ vọng (mean vectors), ma trận hiệp phương sai (covariance matrices) và các trọng số hỗn hợp (mixture weights):

$$ \lambda = \{w_i, \mu_i, \Sigma_i\}, \quad i= 1, \dots, C $$

Các ma trận hiệp phương sai của một GMM, $\Sigma_i$, có thể là ma trận đầy đủ (full rank) hoặc bị ràng buộc là ma trận chéo (diagonal). Các tham số của một GMM cũng có thể được chia sẻ hoặc liên kết (tied) giữa các thành phần Gauss. Số lượng thành phần GMM và loại ma trận hiệp phương sai thường được xác định dựa trên lượng dữ liệu sẵn có để ước lượng các tham số GMM.

Trong nhận dạng người nói, một người nói có thể được mô hình hóa bằng một GMM từ dữ liệu huấn luyện trực tiếp hoặc sử dụng phương pháp thích ứng cực đại hậu nghiệm (Maximum A Posteriori - MAP). Trong khi mô hình người nói được xây dựng bằng cách sử dụng các phát ngôn huấn luyện của một người nói cụ thể trong quá trình huấn luyện GMM thông thường, mô hình này cũng thường được thích ứng từ một mô hình chung của số lượng lớn người nói, được gọi là Mô hình nền phổ quát (Universal Background Model - UBM) trong phương pháp thích ứng MAP.

Cho trước một tập hợp các vectơ huấn luyện và một cấu hình GMM, có một số kỹ thuật sẵn có để ước lượng các tham số của GMM. Phương pháp phổ biến và được sử dụng nhiều nhất là ước lượng khả tích cực đại (Maximum Likelihood - ML).

Ước lượng ML tìm kiếm các tham số mô hình nhằm tối đa hóa khả tích của GMM cho trước một tập hợp dữ liệu. Giả định tính độc lập giữa các vectơ huấn luyện $X = \{x_i, \dots, x_N\}$, khả tích GMM thường được mô tả là:

$$ p(X|\lambda) = \prod_{t=1}^{N} p(x_t|\lambda) \quad (1) $$

Vì việc tối đa hóa trực tiếp là không thể thực hiện được trên phương trình 1, các tham số ML được thu thập một cách lặp đi lặp lại bằng cách sử dụng thuật toán cực đại hóa kỳ vọng (Expectation-Maximization - EM). Thuật toán EM ước lượng lặp để tìm các tham số mô hình mới $\bar{\lambda}$ dựa trên một mô hình $\lambda$ cho trước sao cho $p(X|\bar{\lambda}) \ge p(X|\lambda)$.

![image2020-1-20_15-34-38.png](attachments/165126354.png)
**Hình 2:** Ví dụ về thích ứng mô hình người nói

Bên cạnh thuật toán EM, các tham số của một GMM cũng có thể được ước lượng bằng phương pháp ước lượng cực đại hậu nghiệm (MAP). Kỹ thuật ước lượng MAP xây dựng một mô hình người nói bằng cách thích ứng từ một mô hình nền phổ quát (UBM). Bước "Kỳ vọng" (Expectation) của EM và MAP là giống nhau. MAP thích ứng các số liệu thống kê đầy đủ (sufficient statistics) mới bằng cách kết hợp chúng với các số liệu thống kê cũ từ các tham số hỗn hợp tiên nghiệm.

Cho trước một mô hình tiên nghiệm và các vectơ huấn luyện từ lớp mong muốn, $X = \{x_1, \dots, x_T\}$, trước tiên chúng ta xác định sự căn chỉnh xác suất của các vectơ huấn luyện vào các thành phần hỗn hợp tiên nghiệm. Đối với thành phần hỗn hợp $i$ trong mô hình tiên nghiệm, xác suất hậu nghiệm $Pr(i|x_t,\lambda_{UBM})$ được tính bằng tỷ lệ của thành phần hỗn hợp $i$ trên tổng khả tích:

$$ Pr(i|x_t,\lambda_{UBM})= \frac {w_i\,g(x_t|\mu_i,\Sigma_i)} {\sum_{i=1}^{M} w_i\,g(x_t|\mu_i,\Sigma_i)} $$

Sau đó, các số liệu thống kê đầy đủ cho các tham số trọng số, kỳ vọng và phương sai được tính toán như sau:

$$ n_i=\sum_{t=1}^{T}Pr(i|x_t,\lambda_{prior}) \quad \text{(trọng số)} $$
$$ E_i(x)=\frac{1}{n_i}\sum_{t=1}^{T}Pr(i|x_t,\lambda_{prior})x_t \quad \text{(kỳ vọng)} $$ 
$$ E_i(x^2)=\frac{1}{n_i}\sum_{t=1}^{T}Pr(i|x_t,\lambda_{prior})x_t^2 \quad \text{(phương sai)} $$

Cuối cùng, các số liệu thống kê đầy đủ mới từ dữ liệu huấn luyện được sử dụng để cập nhật các số liệu thống kê đầy đủ tiên nghiệm cho thành phần hỗn hợp $i$ nhằm tạo ra trọng số, kỳ vọng và phương sai thích ứng cho thành phần $i$ như sau:

$$ w_i=[\alpha^w_in_i/T + (1-\alpha^w_i)w_i]\gamma $$ 
$$ \mu_i=\alpha^m_iE_i(x) + (1-\alpha^m_i)\mu_i $$ 
$$ \sigma^2_i=\alpha^v_iE_i(x^2) + (1-\alpha^v_i)(\sigma^2_i+\mu^2_i)-\mu^2_i $$

Các hệ số thích ứng kiểm soát sự cân bằng giữa các ước lượng cũ và mới là $\{\alpha^w_i, \alpha^m_i, \alpha^v_i\}$ tương ứng cho trọng số, kỳ vọng và phương sai. Hệ số tỷ lệ $\gamma$ được tính trên tất cả các trọng số hỗn hợp đã thích ứng để đảm bảo tổng của chúng bằng 1.

### Các vectơ i-Vector (i-Vectors)

Nhiều hướng tiếp cận khác nhau đã được phát triển gần đây để cải thiện hiệu suất của các hệ thống nhận dạng người nói. Các phương pháp phổ biến nhất dựa trên GMM-UBM. Phương pháp Phân tích nhân tố đồng thời (Joint Factor Analysis - JFA) được xây dựng dựa trên sự thành công của phương pháp GMM-UBM. Mô hình hóa JFA định nghĩa hai không gian riêng biệt: không gian người nói được định nghĩa bởi ma trận eigenvoice và không gian kênh được biểu diễn bởi ma trận eigenchannel. Các nhân tố kênh được ước lượng bằng JFA, vốn được cho là chỉ mô hình hóa các ảnh hưởng của kênh, thực tế vẫn chứa thông tin về người nói. Một hệ thống xác thực người nói mới đã được đề xuất sử dụng phân tích nhân tố như một bộ trích xuất đặc trưng định nghĩa chỉ một không gian duy nhất, thay vì hai không gian riêng biệt. Trong không gian mới này, một bản ghi tiếng nói cho trước được biểu diễn bằng một vectơ mới, gọi là nhân tố tổng thể (total factors) vì nó chứa đồng thời cả sự biến thiên của người nói và của kênh truyền. Nhận dạng người nói dựa trên khung i-vector hiện đang là một kỹ thuật nền tảng quan trọng trong lĩnh vực này.

Cho trước một phát ngôn, siêu vectơ GMM phụ thuộc vào người nói và kênh truyền được định nghĩa như sau:

$$ M=m+Tw $$

ở đây $m$ là siêu vectơ độc lập với người nói và kênh truyền, $T$ là một ma trận chữ nhật có hạng thấp (low rank) và $w$ là một vectơ ngẫu nhiên có phân phối chuẩn tiêu chuẩn $N(0,I)$. Các thành phần của vectơ $w$ là các nhân tố tổng thể. Các vectơ mới này được gọi là i-vector. Siêu vectơ $M$ được giả định là có phân phối chuẩn với vectơ kỳ vọng $m$ và ma trận hiệp phương sai $TT^t$.

Nhân tố tổng thể là một biến ẩn, có thể được định nghĩa bằng phân phối hậu nghiệm của nó có điều kiện theo các số liệu thống kê Baum-Welch cho một phát ngôn cho trước. Phân phối hậu nghiệm này là một phân phối Gauss và kỳ vọng của phân phối này tương ứng chính xác với i-vector. Các số liệu thống kê Baum-Welch được trích xuất bằng cách sử dụng UBM.

Cho trước một chuỗi gồm $L$ khung hình $\{y_1, y_2, \dots, y_L\}$ và một UBM $\Omega$ bao gồm $C$ thành phần hỗn hợp được định nghĩa trong không gian đặc trưng có số chiều $F$, các số liệu thống kê Baum-Welch cần thiết để ước lượng hỗn hợp i-vector cho một phát ngôn tiếng nói $u$ cho trước là:

$$ N_c=\sum_{t=1}^{L}P(c|y_t,\Omega) $$ 
$$ F_c=\sum_{t=1}^{L}P(c|y_t,\Omega)y_t $$

ở đây $m_c$ là kỳ vọng của thành phần hỗn hợp UBM $c$. i-vector cho một phát ngôn cho trước có thể thu được bằng phương trình sau:

$$ w=(I + T^t \Sigma^{-1} N(u)T)^{-1} T^t\Sigma^{-1}\hat{F}(u) $$

ở đây $N(u)$ là một ma trận chéo có số chiều $CF \times CF$ có các khối đường chéo là $N_cI$ (với $c=1,\dots, C$). Siêu vectơ thu được bằng cách liên kết tất cả các số liệu thống kê Baum-Welch cấp một $F_c$ cho một phát ngôn $u$ cho trước được biểu diễn bởi $\hat{F}(u)$ có kích thước $CF \times 1$. Ma trận hiệp phương sai chéo, $\Sigma$, với số chiều $CF \times CF$ được ước lượng trong quá trình huấn luyện phân tích nhân tố để mô hình hóa sự biến thiên dư thừa không được nắm bắt bởi ma trận biến thiên tổng thể $T$.

![image2020-1-20_20-26-53.png](attachments/165126497.png)
**Hình 3:** Quy trình trích xuất i-Vector

Một trong những kỹ thuật chuẩn hóa đặc trưng được sử dụng rộng rãi nhất cho i-vector là chuẩn hóa độ dài (length normalization). Chuẩn hóa độ dài đảm bảo rằng phân phối của các i-vector khớp với phân phối chuẩn Gauss và làm cho phân phối của các i-vector tương đồng hơn. Thực hiện làm trắng dữ liệu (whitening) trước khi chuẩn hóa độ dài giúp cải thiện hiệu suất của hệ thống xác thực người nói. Chuẩn hóa i-vector cải thiện tính Gauss của các i-vector và giảm khoảng cách giữa các giả định của dữ liệu và phân phối thực tế. Nó cũng giảm sự dịch chuyển tập dữ liệu (dataset shift) giữa các i-vector huấn luyện và kiểm thử.

$$ w\leftarrow \frac{\Sigma^{-\frac{1}{2}}(w-\mu)}{\|\Sigma^{-\frac{1}{2}}(w-\mu)\|} $$

ở đây $\mu$ và $\Sigma$ tương ứng là kỳ vọng và ma trận hiệp phương sai của một tập dữ liệu huấn luyện. Dữ liệu được chuẩn hóa theo ma trận hiệp phương sai $\Sigma$ và chuẩn hóa độ dài (tức là các i-vector được giới hạn trong một mặt cầu đơn vị).

Hai kỹ thuật bù trừ phiên ghi âm (intersession compensation) phổ biến nhất cho i-vector là Chuẩn hóa hiệp phương sai trong lớp (Within-Class Covariance Normalization - WCCN) và Phân tích định vị tuyến tính (Linear Discriminant Analysis - LDA). WCCN sử dụng ma trận hiệp phương sai trong lớp để chuẩn hóa các hàm nhân cosine nhằm bù trừ cho biến thiên giữa các phiên ghi âm. LDA cố gắng xác định các trục đặc biệt được thu gọn nhằm tối thiểu hóa sự biến thiên trong cùng một người nói gây ra bởi ảnh hưởng của kênh truyền, và tối đa hóa sự biến thiên giữa các người nói khác nhau.

#### Khoảng cách Cosine (Cosine Distance)

Một khi các i-vector được trích xuất từ các cụm tiếng nói đầu ra, phép tính điểm khoảng cách cosine sẽ kiểm tra giả thuyết xem hai i-vector thuộc về cùng một người nói hay hai người nói khác nhau. Cho trước hai i-vector, khoảng cách cosine giữa chúng được tính toán như sau:

$$ cos(w_i,w_j)=\frac{w_i\cdot w_j}{\|w_i\|\cdot\|w_j\|}\gtreqless \theta $$

ở đây $\theta$ là giá trị ngưỡng, và $cos(w_i,w_j)$ là điểm khoảng cách cosine giữa cụm $i$ và $j$. Các i-vector tương ứng được trích xuất cho cụm $i$ và $j$ được đại diện tương ứng bởi $w_i$ và $w_j$.

Tính điểm khoảng cách cosine chỉ xem xét góc giữa hai i-vector chứ không xem xét độ lớn của chúng. Vì thông tin phi người nói như sự biến thiên của phiên ghi âm và kênh truyền ảnh hưởng đến độ lớn của i-vector, việc loại bỏ độ lớn có thể tăng tính mạnh mẽ của các hệ thống i-vector.

### Phân tích định vị tuyến tính xác suất (Probabilistic Linear Discriminant Analysis - PLDA)

Biểu diễn i-vector kết hợp với kỹ thuật mô hình hóa phân tích định vị tuyến tính xác suất (PLDA) là công nghệ tiên tiến nhất trong các hệ thống xác thực người nói. PLDA đã được áp dụng thành công trong các thử nghiệm nhận dạng người nói. Nó cũng được áp dụng để xử lý biến thiên người nói và phiên ghi âm trong tác vụ xác thực người nói. Ngoài ra, nó cũng được áp dụng thành công trong phân cụm người nói vì nó có thể phân tách các phần đặc trưng cho người nói và đặc trưng cho nhiễu của một tín hiệu âm thanh, đây là điều cần thiết cho phân đoạn và định danh người nói (speaker diarization).

![image2020-1-20_21-46-46.png](attachments/165126539.png)
**Hình 4:** Ví dụ về mô hình PLDA

Trong PLDA, giả định rằng dữ liệu huấn luyện bao gồm $J$ i-vector, trong đó mỗi i-vector này thuộc về người nói $I$, i-vector thứ $j$ của người nói thứ $I$ được ký hiệu bởi:

$$ w_{ij}=\mu + Fh_i + Gy_{ij} + \Sigma_{ij} $$

ở đây $\mu$ là kỳ vọng chung độc lập với người nói và phân đoạn của các i-vector trong tập dữ liệu huấn luyện, các cột của ma trận F xác định sự biến thiên giữa các người nói (between-speaker variability) và các cột của ma trận G xác định cơ sở cho không gian con biến thiên trong cùng một người nói (within-speaker variability subspace). $\Sigma_{ij}$ biểu thị bất kỳ biến thiên dữ liệu nào chưa được giải thích. Các thành phần của vectơ $h_i$ là các hệ số nhân tố eigenvoice và các thành phần của vectơ $y_{ij}$ là các hệ số nhân tố eigenchannel. Thành phần $Fh_i$ chỉ phụ thuộc vào danh tính của người nói, không phụ thuộc vào phân đoạn cụ thể.

Mặc dù mô hình PLDA giả định hành vi Gauss, có bằng chứng thực nghiệm cho thấy ảnh hưởng của kênh truyền và người nói dẫn đến các i-vector có phân phối phi Gauss. Người ta đã báo cáo rằng việc sử dụng phân phối t-Student trên mô hình PLDA Gauss giả định giúp cải thiện hiệu suất. Vì kỹ thuật chuẩn hóa này phức tạp, một phép biến đổi phi tuyến của các i-vector gọi là Gauss hóa hướng tâm (radial Gaussianization) đã được đề xuất. Nó làm trắng các i-vector và thực hiện chuẩn hóa độ dài. Điều này khôi phục các giả định Gauss của mô hình PLDA.

Một biến thể của mô hình PLDA được gọi là PLDA Gauss (GPLDA) được chứng minh là cung cấp kết quả tốt hơn. Do yêu cầu tính toán thấp và hiệu suất tốt, nó là mô hình PLDA được sử dụng rộng rãi nhất. Trong mô hình GPLDA, sự biến thiên trong cùng một người nói được mô hình hóa bằng một số dư hiệp phương sai đầy đủ (full covariance residual term), cho phép chúng ta bỏ qua không gian con của kênh. Mô hình sinh PLDA cho i-vector được biểu diễn bởi:

$$ w_{ij}=\mu + Fh_i + \Sigma_{ij} $$

Số dư $\Sigma$ biểu thị sự biến thiên trong cùng một người nói được giả định là có phân phối chuẩn với ma trận hiệp phương sai đầy đủ $\Sigma$.

Cho trước hai i-vector $w_1$ and $w_2$, PLDA tính toán tỷ số khả tích của hai i-vector như sau:

$$ Score(w_1,w_2)= \frac{p(w_1,w_2|H_1)}{p(w_1|H_2) p(w_2|H_2)} $$

ở đây giả thuyết $H_1$ chỉ ra rằng cả hai i-vector đều thuộc về cùng một người nói và giả thuyết $H_2$ chỉ ra rằng chúng thuộc về hai người nói khác nhau.

### Học sâu (Deep Learning - DL)

Những tiến bộ gần đây trong phần cứng tính toán, các kiến trúc DL mới và phương pháp huấn luyện, cùng khả năng tiếp cận lượng dữ liệu huấn luyện lớn đã truyền cảm hứng cho cộng đồng nghiên cứu áp dụng công nghệ DL vào các hệ thống nhận dạng người nói. Các kỹ thuật DL có thể được sử dụng ở phần frontend và/hoặc backend của một hệ thống nhận dạng người nói. Toàn bộ quy trình nhận dạng đầu-cuối thậm chí có thể được thực hiện bởi một kiến trúc DL duy nhất.

**Deep Learning Frontends:** Hướng tiếp cận i-vector truyền thống chủ yếu bao gồm ba giai đoạn: thu thập số liệu thống kê Baum-Welch, trích xuất i-vector và phân loại backend PLDA. Gần đây, người ta đã chỉ ra rằng nếu các số liệu thống kê Baum-Welch được tính toán dựa trên một mạng DNN thay vì GMM, hoặc nếu các đặc trưng nghẽn (bottleneck features) được sử dụng cùng với các đặc trưng phổ truyền thống, hiệu suất hệ thống sẽ được cải thiện đáng kể. Một ứng dụng khả thi khác của DL ở phần frontend là biểu diễn các đặc trưng người nói của một tín hiệu tiếng nói bằng một vectơ số chiều thấp duy nhất sử dụng một kiến trúc DL, thay vì thuật toán i-vector truyền thống. Các vectơ này thường được gọi là các vectơ nhúng người nói (speaker embeddings). Thông thường, đầu vào của mạng nơ-ron là một chuỗi các vectơ đặc trưng và đầu ra là các lớp người nói.

**Deep Learning Backends:** Một trong những kỹ thuật backend hiệu quả nhất cho i-vector là PLDA, thực hiện tính điểm cùng với việc bù trừ biến thiên phiên ghi âm. Thông thường, một số lượng lớn các người nói khác nhau với nhiều mẫu tiếng nói cho mỗi người là cần thiết để PLDA hoạt động hiệu quả. Việc tiếp cận dữ liệu được gán nhãn người nói là tốn kém và trong một số trường hợp là không thể. Hơn nữa, mức độ tăng hiệu suất (về độ chính xác) đối với các phát ngôn ngắn không nhiều như đối với các phát ngôn dài. Những thực tế này đã thúc đẩy cộng đồng nghiên cứu tìm kiếm các giải pháp backend thay thế dựa trên DL. Một vài kỹ thuật đã được đề xuất. Hầu hết các phương pháp này sử dụng nhãn người nói của dữ liệu nền để huấn luyện giống như trong PLDA, và phần lớn không mang lại hiệu suất vượt trội đáng kể so với PLDA.

**Deep Learning End-to-Ends:** Việc huấn luyện một hệ thống nhận dạng đầu-cuối có khả năng thực hiện nhiều giai đoạn xử lý tín hiệu với một kiến trúc DL thống nhất cũng rất được quan tâm. Mạng nơ-ron sẽ chịu trách nhiệm cho toàn bộ quy trình từ trích xuất đặc trưng đến các điểm số tương đồng cuối cùng. Tuy nhiên, việc xử lý trực tiếp trên các tín hiệu âm thanh trong miền thời gian vẫn còn quá tốn kém về mặt tính toán, và do đó, các hệ thống DL đầu-cuối hiện nay chủ yếu nhận đầu vào là các vectơ đặc trưng được thiết kế thủ công, ví dụ như MFCC. Gần đây, đã có nhiều nỗ lực xây dựng hệ thống nhận dạng người nói đầu-cuối sử dụng DL, mặc dù hầu hết chúng tập trung vào nhận dạng người nói phụ thuộc văn bản.

## Các ứng dụng của nhận dạng người nói

-   **Xác thực giao dịch (Transaction authentication):** Ngăn ngừa gian lận cước viễn thông, mua hàng bằng thẻ tín dụng qua điện thoại, môi giới qua điện thoại (ví dụ: giao dịch chứng khoán).
-   **Kiểm soát truy cập (Access control):** Các cơ sở vật lý, máy tính và mạng dữ liệu.
-   **Giám sát (Monitoring):** Điểm danh và ghi nhận thời gian làm việc từ xa, xác minh cải tạo tại nhà, giám sát sử dụng điện thoại trong tù.
-   **Truy xuất thông tin (Information retrieval):** Thông tin khách hàng cho các tổng đài chăm sóc khách hàng, lập chỉ mục âm thanh (thiết bị lướt nhanh tiếng nói), phân đoạn và định danh người nói.
-   **Pháp y (Forensics):** So sánh và khớp mẫu giọng nói nghi vấn với nghi phạm.

## Đánh giá hiệu suất

Hiệu suất của hệ thống xác thực người nói được đo lường dựa trên các lỗi (sai số). Các loại lỗi và tiêu chí đánh giá thường được sử dụng trong các hệ thống xác thực người nói bao gồm:

## Các loại lỗi

**Chấp nhận sai (False acceptance):** Một lỗi chấp nhận sai xảy ra khi hệ thống chấp nhận sai một tín hiệu từ một người nói giả mạo (imposter) là người nói mục tiêu (target).

$$ \text{Tỷ lệ Chấp nhận sai (FAR)} = \frac{\text{Tổng số lỗi chấp nhận sai}} {\text{Tổng số lượt cố gắng truy cập của người giả mạo}} $$

**Từ chối sai (False rejection):** Một lỗi từ chối sai xảy ra khi người nói mục tiêu bị hệ thống xác thực từ chối.

$$ \text{Tỷ lệ Từ chối sai (FRR)} = \frac{\text{Tổng số lỗi từ chối sai}} {\text{Tổng số lượt cố gắng truy cập của người nói mục tiêu đã đăng ký}} $$

## Các chỉ số hiệu suất

Các chỉ số hiệu suất của hệ thống xác thực người nói có thể được đo lường bằng tỷ lệ lỗi bằng nhau (Equal Error Rate - EER) và hàm chi phí quyết định tối thiểu (minimum Decision Cost Function - mDCF). Các thước đo này đại diện cho các đặc tính hiệu suất khác nhau của hệ thống, mặc dù độ chính xác của các phép đo phụ thuộc vào số lượng thử nghiệm được đánh giá để tính toán các số liệu thống kê liên quan một cách đáng tin cậy. Hiệu suất xác thực người nói cũng có thể được biểu diễn dưới dạng đồ thị bằng cách sử dụng đồ thị trao đổi lỗi phát hiện (Detection Error Trade-off - DET).

EER đạt được khi tỷ lệ chấp nhận sai và tỷ lệ từ chối sai có giá trị bằng nhau. Hiệu suất của hệ thống được cải thiện khi giá trị EER thấp hơn vì tổng lỗi của chấp nhận sai và từ chối sai tại điểm EER giảm đi.

Hàm chi phí quyết định (Decision Cost Function - DCF) được định nghĩa bằng cách gán một chi phí cho mỗi loại lỗi và tính đến xác suất tiên nghiệm của các thử nghiệm mục tiêu và giả mạo. Hàm chi phí quyết định được định nghĩa như sau:

$$ DCF = C_{miss}P_{miss}P_{target} + C_{fa}P_{fa}P_{impostor} $$

ở đây $C_{miss}$ và $C_{fa}$ tương ứng là các hàm chi phí của việc bỏ sót phát hiện (missed detection) và báo động giả (false alarm). Xác suất tiên nghiệm của thử nghiệm mục tiêu và giả mạo được cho tương ứng bởi $P_{target}$ và $P_{impostor}$. Tỷ lệ phần trăm các thử nghiệm mục tiêu bị bỏ sót và thử nghiệm giả mạo bị chấp nhận sai được biểu diễn tương ứng bởi $P_{miss}$ và $P_{fa}$.

mDCF được sử dụng để đánh giá xác thực người nói bằng cách chọn giá trị nhỏ nhất của DCF được ước lượng bằng cách thay đổi giá trị ngưỡng.

$$ mDCF = min[C_{miss}P_{miss}P_{target} + C_{fa}P_{fa}P_{impostor}] $$

ở đây $P_{miss}$ và $P_{fa}$ là tỷ lệ bỏ sót và tỷ lệ báo động giả ghi nhận được từ các thử nghiệm, và các tham số khác được điều chỉnh để phù hợp với việc đánh giá các yêu cầu cụ thể của ứng dụng.

## Xem thêm
- [](forensic-analysis)