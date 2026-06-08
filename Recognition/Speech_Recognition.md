(content:asr)=
# Nhận dạng tiếng nói (Speech Recognition)

## Giới thiệu về nhận dạng tiếng nói tự động (ASR)

Một hệ thống nhận dạng tiếng nói tự động (Automatic Speech Recognition - ASR) tạo ra chuỗi từ có khả năng xảy ra cao nhất cho trước một tín hiệu tiếng nói đầu vào. Phương pháp tiếp cận thống kê cho nhận dạng tiếng nói đã thống trị nghiên cứu ASR trong vài thập kỷ qua và đạt được nhiều thành tựu quan trọng. Bài toán nhận dạng tiếng nói được định nghĩa là quá trình chuyển đổi các phát ngôn nói thành các câu văn bản bởi máy tính. Trong khung thống kê, quy tắc quyết định Bayes được sử dụng để tìm chuỗi từ có xác suất cao nhất, $\hat H$, cho trước chuỗi quan sát $O = (o_1, \dots, o_T)$:

$$ \hat H= \operatorname*{argmax}_H \;P(H|O) $$

Theo quy tắc Bayes, xác suất hậu nghiệm trong phương trình trên có thể được biểu diễn dưới dạng xác suất có điều kiện của chuỗi từ cho trước các quan sát âm học, $P(O|H)$, nhân với xác suất tiên nghiệm của chuỗi từ, $P(H)$, và chuẩn hóa bởi xác suất biên của các chuỗi quan sát, $P(O)$:

$$ \hat H= \operatorname*{argmax}_H \; \frac {P(O|H)\;P(H)} {P(O)} $$ 
$$ \hat H= \operatorname*{argmax}_H \; P(O|H)\;P(H) $$

Xác suất biên $P(O)$ được loại bỏ ở phương trình thứ hai vì nó là hằng số đối với việc xếp hạng các giả thuyết, và do đó không làm thay đổi việc tìm kiếm giả thuyết tốt nhất. $P(O|H)$ được tính toán bởi **mô hình âm học (acoustic model)** và $P(H)$ được mô hình hóa bởi **mô hình ngôn ngữ (language model)**.

## Các thành phần của hệ thống ASR

-   **Trích xuất đặc trưng (Feature Extraction):** Chuyển đổi tín hiệu tiếng nói thành một chuỗi các vectơ đặc trưng âm học. Các quan sát này cần nhỏ gọn và mang đầy đủ thông tin cho việc nhận dạng ở giai đoạn sau.
-   **Mô hình âm học (Acoustic Model):** Chứa biểu diễn thống kê của các âm thanh riêng biệt cấu thành nên mỗi từ trong Mô hình ngôn ngữ hoặc Ngữ pháp. Mỗi âm thanh riêng biệt tương ứng với một âm vị (phoneme).
-   **Mô hình ngôn ngữ (Language Model):** Chứa một danh sách rất lớn các từ và xác suất xuất hiện của chúng trong một chuỗi cho trước.
-   **Bộ giải mã (Decoder):** Là một chương trình phần mềm tiếp nhận các âm thanh do người dùng phát ra và tìm kiếm các âm thanh tương đương trong Mô hình âm học. Khi tìm thấy sự trùng khớp, bộ giải mã xác định âm vị tương ứng với âm thanh đó. Nó theo dõi các âm vị trùng khớp cho đến khi gặp một khoảng dừng trong tiếng nói của người dùng. Sau đó, nó tìm kiếm trong mô hình ngôn ngữ chuỗi âm vị tương đương. Nếu khớp, nó sẽ trả về văn bản của từ hoặc cụm từ tương ứng cho chương trình gọi hệ thống.

![ASR.png](attachments/165127140.png)
**Hình 1:** Kiến trúc của một hệ thống ASR

## Phân loại các hệ thống ASR

Các hệ thống nhận dạng tiếng nói có thể được phân loại dựa trên các ràng buộc trong quá trình phát triển và các ràng buộc mà chúng áp đặt lên người dùng. Các ràng buộc này bao gồm: sự phụ thuộc vào người nói, loại phát ngôn, kích thước từ vựng, các ràng buộc ngôn ngữ, loại tiếng nói và môi trường sử dụng. Chúng tôi mô tả từng ràng buộc như sau:

**Sự phụ thuộc vào người nói (Speaker Dependence):** Hệ thống nhận dạng tiếng nói phụ thuộc vào người nói (speaker dependent) yêu cầu người dùng tham gia vào quá trình phát triển (huấn luyện) hệ thống, trong khi hệ thống độc lập với người nói (speaker independent) thì không cần. Hệ thống độc lập với người nói có thể được sử dụng bởi bất kỳ ai. Các hệ thống phụ thuộc vào người nói thường hoạt động tốt hơn nhiều so với các hệ thống độc lập với người nói. Điều này là do các biến thiên âm học giữa những người nói khác nhau là rất khó mô tả và mô hình hóa. Có hai hướng tiếp cận để xây dựng hệ thống độc lập với người nói: hướng thứ nhất là sử dụng nhiều biểu diễn cho mỗi mẫu đối chứng để nắm bắt sự biến thiên của người nói, và hướng thứ hai là phương pháp thích ứng người nói (speaker adaptation).

**Loại phát ngôn (Type of Utterance):** Một bộ nhận dạng tiếng nói có thể nhận dạng từng từ một cách độc lập. Nó có thể yêu cầu người dùng nói từng từ trong một câu và ngăn cách chúng bằng các khoảng dừng nhân tạo, hoặc nó có thể cho phép người dùng nói một cách tự nhiên. Loại hệ thống thứ nhất được phân loại là hệ thống nhận dạng từ cô lập (isolated word recognition system). Đây là dạng đơn giản nhất của chiến lược nhận dạng. Nó có thể được phát triển bằng cách sử dụng các mô hình âm học cấp từ (word-based) mà không cần bất kỳ mô hình ngôn ngữ nào. Tuy nhiên, nếu từ vựng tăng lên và các câu bao gồm các từ cô lập cần được nhận dạng, việc sử dụng các mô hình âm học dưới cấp từ (sub-word) và mô hình ngôn ngữ trở nên quan trọng. Loại thứ hai là hệ thống nhận dạng tiếng nói liên tục (continuous speech recognition systems). Nó cho phép người dùng phát âm thông điệp một cách tương đối hoặc hoàn toàn tự nhiên. Các bộ nhận dạng như vậy phải có khả năng hoạt động tốt dưới sự hiện diện của tất cả các ảnh hưởng đồng cấu âm (co-articulatory effects). Do đó, phát triển hệ thống nhận dạng tiếng nói liên tục là tác vụ khó khăn nhất. Điều này là do các đặc tính sau của tiếng nói liên tục: ranh giới từ không rõ ràng trong tiếng nói liên tục; và các ảnh hưởng đồng cấu âm mạnh hơn nhiều trong tiếng nói liên tục.

**Kích thước từ vựng (Vocabulary Size):** Số lượng từ trong từ vựng là một ràng buộc phân loại hệ thống nhận dạng tiếng nói thành loại nhỏ, vừa hoặc lớn. Theo quy tắc kinh nghiệm, các hệ thống từ vựng nhỏ là những hệ thống có kích thước từ vựng trong khoảng 1-99 từ; vừa là 100-999 từ; và lớn là từ 1000 từ trở lên. Các hệ thống nhận dạng tiếng nói từ vựng lớn (LVCSR) hoạt động kém hơn nhiều so với các hệ thống từ vựng nhỏ do các yếu tố khác nhau, chẳng hạn như sự nhập nhằng từ (word confusion) tăng lên theo số lượng từ trong từ vựng. Đối với bộ nhận dạng từ vựng nhỏ, mỗi từ có thể được mô hình hóa riêng biệt. Tuy nhiên, không thể huấn luyện các mô hình âm học cho hàng nghìn từ riêng biệt vì chúng ta không thể có đủ dữ liệu tiếng nói huấn luyện và dung lượng lưu trữ cần thiết cho các tham số. Do đó, việc phát triển bộ nhận dạng từ vựng lớn đòi hỏi phải sử dụng các đơn vị dưới cấp từ (sub-word units). Mặt khác, việc sử dụng các đơn vị dưới cấp từ dẫn đến suy giảm hiệu suất vì chúng không thể nắm bắt các ảnh hưởng đồng cấu âm tốt như cấp từ. Quá trình tìm kiếm trong bộ nhận dạng từ vựng lớn cũng sử dụng kỹ thuật cắt tỉa (pruning) thay vì thực hiện tìm kiếm toàn bộ (complete search).

**Loại tiếng nói (Type of Speech):** Một bộ nhận dạng tiếng nói có thể được phát triển để chỉ nhận dạng tiếng nói đọc sẵn (read speech) hoặc cho phép người dùng nói một cách tự phát (spontaneous speech). Tiếng nói tự phát khó xây dựng hơn nhiều so với tiếng nói đọc sẵn do thực tế là tiếng nói tự phát đặc trưng bởi các lỗi bắt đầu sai (false starts), câu không hoàn chỉnh, từ vựng không giới hạn và chất lượng phát âm bị giảm. Sự khác biệt chính về tỷ lệ lỗi nhận dạng giữa tiếng nói đọc sẵn và tiếng nói tự phát là do các hiện tượng ngập ngừng (disfluencies) trong tiếng nói tự phát. Các hiện tượng ngập ngừng này có thể đặc trưng bởi các khoảng dừng dài và phát âm sai. Do đó, tiếng nói tự phát rất khó nhận dạng cả về mặt âm học và ngữ pháp.

**Môi trường (Environment):** Bộ nhận dạng tiếng nói có thể yêu cầu tiếng nói phải sạch, không có tiếng ồn môi trường, méo âm học, méo do microphone và kênh truyền dẫn, hoặc chúng có thể xử lý lý tưởng bất kỳ vấn đề nào trong số này. Trong khi các bộ nhận dạng tiếng nói hiện tại cho hiệu suất chấp nhận được trong các môi trường được kiểm soát cẩn thận, hiệu suất của chúng giảm nhanh chóng khi áp dụng trong môi trường có nhiễu. Nhiễu này có thể ở dạng tiếng nói từ những người nói khác; âm thanh thiết bị, máy điều hòa không khí hoặc các nguồn khác. Nhiễu cũng có thể do chính người nói tạo ra dưới dạng tặc lưỡi, ho hoặc hắt hơi.

## Các mô hình cho nhận dạng tiếng nói từ vựng lớn (LVCSR)

LVCSR có thể được chia thành hai danh mục chính: mô hình dựa trên HMM và mô hình đầu-cuối (end-to-end).

### Mô hình dựa trên HMM (HMM-Based Model)

Mô hình dựa trên HMM là mô hình LVCSR chủ đạo trong nhiều năm với độ chính xác nhận dạng tốt nhất. Một mô hình dựa trên HMM được chia thành ba phần: mô hình âm học, mô hình phát âm và mô hình ngôn ngữ. Trong mô hình dựa trên HMM, mỗi mô hình độc lập với nhau và đóng một vai trò khác nhau. Trong khi mô hình âm học mô hình hóa mối quan hệ ánh xạ giữa đầu vào tiếng nói và chuỗi đặc trưng, mô hình phát âm ánh xạ giữa các âm vị (hoặc phân đoạn âm vị) sang các grapheme (ký tự), và mô hình ngôn ngữ ánh xạ chuỗi ký tự sang bản dịch văn bản trôi chảy cuối cùng.

**Mô hình âm học (Acoustic Model):** Trong mô hình âm học, xác suất quan sát thường được biểu diễn bằng Mô hình hỗn hợp Gauss (GMM). Phân phối xác suất hậu nghiệm của trạng thái ẩn có thể được tính toán bằng phương pháp mạng nơ-ron sâu (DNN). Hai cách tính toán khác nhau này dẫn đến hai mô hình khác nhau là HMM-GMM và HMM-DNN. Mô hình HMM-GMM từng là cấu trúc phổ biến cho nhiều hệ thống nhận dạng tiếng nói. Tuy thế, với sự phát triển của công nghệ học sâu, DNN đã được đưa vào nhận dạng tiếng nói để mô hình hóa âm học. DNN được sử dụng để tính toán xác suất hậu nghiệm của trạng thái HMM thay thế cho xác suất quan sát GMM truyền thống. Do đó, mô hình HMM-GMM đã bị thay thế bởi HMM-DNN vì HMM-DNN cung cấp kết quả tốt hơn nhiều và trở thành mô hình ASR tiên tiến nhất (state-of-the-art). Trong mô hình dựa trên HMM, các mô-đun khác nhau sử dụng các công nghệ khác nhau và có vai trò khác nhau. Trong khi HMM chủ yếu được sử dụng để thực hiện co dãn thời gian động (dynamic time warping) ở cấp độ khung hình, GMM và DNN được sử dụng để tính toán xác suất phát xạ (emission probability) của các trạng thái ẩn HMM.

**Mô hình phát âm (Pronunciation Model):** Mục tiêu chính của nó là đạt được sự kết nối giữa chuỗi âm học và chuỗi ngôn ngữ. Từ điển phát âm bao gồm các cấp độ ánh xạ khác nhau, chẳng hạn như từ phát âm sang âm tố (phone), từ âm tố sang âm tố ba (tri-phone). Từ điển được sử dụng để đạt được ánh xạ cấu trúc và ánh xạ mối quan hệ tính toán xác suất.

**Mô hình ngôn ngữ (Language Model):** Nó chứa thông tin cú pháp cơ bản. Mục tiêu của nó là dự đoán khả năng xảy ra của các từ cụ thể xuất hiện liên tiếp nhau trong một ngôn ngữ nhất định. Các bộ nhận dạng điển hình sử dụng các mô hình ngôn ngữ n-gram. Một n-gram chứa xác suất tiên nghiệm của sự xuất hiện của một từ (unigram), hoặc của một chuỗi từ (bigram, trigram v.v.):

xác suất unigram $ P(w_i) $

xác suất bigram $ P(w_i|w_{i−1}) $

xác suất n-gram $ P(w_n|w_{n−1},w_{n−2}, \dots,w_1) $

**Hạn chế của mô hình HMM**

-   **Quy trình huấn luyện phức tạp và khó tối ưu hóa toàn cục:** Mô hình dựa trên HMM thường sử dụng các phương pháp huấn luyện và tập dữ liệu khác nhau để huấn luyện các mô-đun khác nhau. Mỗi mô-đun được tối ưu hóa độc lập với các hàm mục tiêu tối ưu hóa riêng, các hàm này thường khác với tiêu chí đánh giá hiệu suất LVCSR thực tế. Vì vậy, tính tối ưu của từng mô-đun không nhất thiết mang lại sự tối ưu hóa toàn cục cho toàn bộ hệ thống.
-   **Giả định độc lập có điều kiện:** Để đơn giản hóa việc xây dựng và huấn luyện mô hình, mô hình dựa trên HMM sử dụng các giả định độc lập có điều kiện bên trong HMM và giữa các mô-đun khác nhau. Điều này không phù hợp với tình huống thực tế của LVCSR.

### Mô hình đầu-cuối (End-to-End Model)

Do những khuyết điểm nêu trên của mô hình dựa trên HMM cùng với sự thúc đẩy của công nghệ học sâu, ngày càng có nhiều nghiên cứu tập trung vào LVCSR đầu-cuối (end-to-end). Mô hình đầu-cuối là một hệ thống trực tiếp ánh xạ chuỗi âm thanh đầu vào thành chuỗi các từ hoặc ký tự khác.

![untitled.png](attachments/165127650.png)
**Hình 2:** Cấu trúc chức năng của mô hình đầu-cuối

Hầu hết các mô hình nhận dạng tiếng nói đầu-cuối bao gồm các phần sau: bộ mã hóa (encoder) ánh xạ chuỗi tiếng nói đầu vào thành chuỗi đặc trưng; bộ căn chỉnh (aligner) thực hiện căn chỉnh giữa chuỗi đặc trưng và ngôn ngữ; bộ giải mã (decoder) giải mã kết quả nhận dạng cuối cùng. Lưu ý rằng sự phân chia này không phải lúc nào cũng tồn tại rõ ràng vì bản thân kiến trúc đầu-cuối là một cấu trúc hoàn chỉnh. Trái ngược với mô hình dựa trên HMM gồm nhiều mô-đun, mô hình đầu-cuối thay thế nhiều mô-đun bằng một mạng sâu duy nhất, thực hiện ánh xạ trực tiếp các tín hiệu âm học thành chuỗi nhãn mà không cần các trạng thái trung gian được thiết kế thủ công phức tạp. Thêm vào đó, không cần thực hiện hậu xử lý xác suất trên đầu ra.

So với mô hình dựa trên HMM, các đặc điểm chính của LVCSR đầu-cuối bao gồm:

-   **Nhiều mô-đun được gộp chung vào một mạng để huấn luyện đồng thời:** Lợi ích của việc gộp nhiều mô-đun là không cần thiết kế nhiều mô-đun phụ để thực hiện ánh xạ giữa các trạng thái trung gian khác nhau. Huấn luyện đồng thời cho phép mô hình đầu-cuối sử dụng một hàm mục tiêu liên quan chặt chẽ đến tiêu chí đánh giá cuối cùng làm mục tiêu tối ưu hóa toàn cục, từ đó tìm kiếm kết quả tối ưu toàn cục.
-   **Trực tiếp ánh xạ chuỗi đặc trưng âm học đầu vào thành chuỗi văn bản kết quả**, và không yêu cầu xử lý thêm để đạt được bản chuyển ngữ thực tế hoặc để cải thiện hiệu suất nhận dạng. Ngược lại, trong các mô hình dựa trên HMM, thường có một biểu diễn nội bộ cho cách phát âm của một chuỗi ký tự.

Những tính năng này của mô hình LVCSR đầu-cuối cho phép đơn giản hóa đáng kể việc xây dựng và huấn luyện các mô hình nhận dạng tiếng nói.

Mô hình đầu-cuối chủ yếu được chia thành ba danh mục khác nhau tùy thuộc vào cách triển khai căn chỉnh mềm (soft alignment) của chúng:

-   **Dựa trên CTC (CTC-based):** Trước tiên nó liệt kê tất cả các căn chỉnh cứng (hard alignments) khả thi. Sau đó, nó đạt được căn chỉnh mềm bằng cách tổng hợp các căn chỉnh cứng này. CTC giả định rằng các nhãn đầu ra độc lập với nhau khi liệt kê các căn chỉnh cứng.
-   **Bộ chuyển đổi RNN (RNN-transducer):** Nó cũng liệt kê tất cả các căn chỉnh cứng khả thi và sau đó tổng hợp chúng để căn chỉnh mềm. Nhưng khác với CTC, bộ chuyển đổi RNN không đưa ra giả định độc lập về các nhãn khi liệt kê các căn chỉnh cứng. Do đó, nó khác với CTC về mặt định nghĩa đường đi (path) và tính toán xác suất.
-   **Dựa trên cơ chế chú ý (Attention-based):** Phương pháp này không còn liệt kê tất cả các căn chỉnh cứng khả thi mà sử dụng cơ chế chú ý (attention mechanism) để tính toán trực tiếp thông tin căn chỉnh mềm giữa dữ liệu đầu vào và nhãn đầu ra.

**Mô hình đầu-cuối dựa trên CTC**

Mặc dù HMM-DNN vẫn cung cấp các kết quả tiên tiến nhất, vai trò của DNN vẫn bị giới hạn. Nó chủ yếu được sử dụng để mô hình hóa xác suất trạng thái hậu nghiệm của trạng thái ẩn HMM. Đặc trưng miền thời gian vẫn được mô hình hóa bởi HMM. Khi cố gắng mô hình hóa các đặc trưng miền thời gian bằng RNN hoặc CNN thay vì HMM, người ta đối mặt với bài toán căn chỉnh dữ liệu: cả hàm mất mát của RNN và CNN đều được định nghĩa tại mỗi điểm trong chuỗi, vì vậy để có thể thực hiện huấn luyện, cần phải biết mối quan hệ căn chỉnh giữa chuỗi đầu ra RNN và chuỗi mục tiêu.

CTC cho phép tận dụng DNN đầy đủ hơn trong nhận dạng tiếng nói và xây dựng các mô hình đầu-cuối, đây là một bước đột phá trong sự phát triển của phương pháp đầu-cuối. Về mặt bản chất, CTC là một hàm mất mát (loss function), nhưng nó giải quyết bài toán căn chỉnh cứng trong khi tính toán mất mát. CTC chủ yếu vượt qua hai khó khăn sau cho các mô hình LVCSR đầu-cuối:

-   **Bài toán căn chỉnh dữ liệu:** CTC không còn cần phân đoạn và căn chỉnh dữ liệu huấn luyện thủ công. Điều này giải quyết bài toán căn chỉnh để DNN có thể được sử dụng nhằm mô hình hóa các đặc trưng miền thời gian, giúp tăng cường đáng kể vai trò của DNN trong các tác vụ LVCSR.
-   **Trực tiếp xuất bản chuyển ngữ mục tiêu:** Các mô hình truyền thống thường xuất ra các âm vị hoặc các đơn vị nhỏ khác, và cần xử lý thêm để có được bản chuyển ngữ cuối cùng. CTC loại bỏ nhu cầu về các đơn vị nhỏ và xuất trực tiếp ở dạng mục tiêu cuối cùng, đơn giản hóa đáng kể việc xây dựng và huấn luyện mô hình đầu-cuối.

**Mô hình đầu-cuối bộ chuyển đổi RNN (RNN-Transducer)**

CTC có hai khuyết điểm chính làm cản trở tính hiệu quả của nó:

-   CTC không thể mô hình hóa các mối phụ thuộc lẫn nhau trong chuỗi đầu ra vì nó giả định các phần tử đầu ra độc lập với nhau. Do đó, CTC không thể tự học mô hình ngôn ngữ. Mạng nhận dạng tiếng nói được huấn luyện bởi CTC chỉ nên được coi là một mô hình âm học đơn thuần.
-   CTC chỉ có thể ánh xạ các chuỗi đầu vào thành các chuỗi đầu ra ngắn hơn nó. Do đó, nó không khả thi cho các kịch bản có chuỗi đầu ra dài hơn chuỗi đầu vào.

Đối với nhận dạng tiếng nói, điểm thứ nhất có tác động rất lớn. Bộ chuyển đổi RNN (RNN-transducer) được đề xuất để giải quyết các khuyết điểm nêu trên của CTC. Về mặt lý thuyết, nó có thể ánh xạ một đầu vào thành bất kỳ chuỗi đầu ra rời rạc hữu hạn nào. Các mối phụ thuộc lẫn nhau giữa đầu vào và đầu ra cũng như giữa các phần tử đầu ra cũng được mô hình hóa đồng thời.

Bộ chuyển đổi RNN có nhiều điểm tương đồng với CTC: mục tiêu chính của chúng là giải quyết bài toán căn chỉnh phân đoạn cưỡng bức trong nhận dạng tiếng nói; chúng đều đưa vào một nhãn "trống" (blank); chúng đều tính toán xác suất của tất cả các đường đi khả thi và tổng hợp chúng để có được chuỗi nhãn. Tuy nhiên, quy trình tạo đường đi và phương pháp tính xác suất đường đi của chúng hoàn toàn khác nhau. Điều này mang lại những ưu thế vượt trội cho bộ chuyển đổi RNN so với CTC.

## Các loại lỗi của bộ nhận dạng tiếng nói

Mặc dù nghiên cứu ASR đã có những bước tiến dài, các hệ thống ngày nay vẫn còn xa mới đạt đến độ hoàn hảo. Bộ nhận dạng tiếng nói dễ bị ảnh hưởng và mắc lỗi do nhiều nguyên nhân khác nhau. Hầu hết các lỗi của hệ thống ASR rơi vào một trong các danh mục sau:

-   **Lỗi ngoài từ vựng (Out-of-vocabulary - OOV):** Các bộ nhận dạng tiếng nói tiên tiến hiện nay có từ vựng đóng (closed vocabularies). Điều này có nghĩa là chúng không có khả năng nhận dạng các từ nằm ngoài từ vựng huấn luyện của chúng. Bên cạnh việc nhận dạng sai, sự xuất hiện của một từ ngoài từ vựng trong phát ngôn đầu vào sẽ khiến hệ thống nhận dạng nhầm thành một từ tương tự có trong từ vựng của nó. Các kỹ thuật đặc biệt để xử lý các từ OOV đã được phát triển cho các hệ thống HMM-GMM và hệ thống ASR nơ-ron (xem ví dụ {cite:p}`zhang2019strategies`).
-   **Thay thế từ đồng âm (Homophone substitution):** Các lỗi này có thể xảy ra nếu có nhiều hơn một từ trong từ điển có cùng cách phát âm (cùng chuỗi âm tố), tức là các từ đồng âm. Trong quá trình giải mã, các từ đồng âm có thể bị nhầm lẫn với nhau gây ra lỗi. Nói chung, một mô hình ngôn ngữ hoạt động tốt cần giải quyết sự nhập nhằng của từ đồng âm dựa trên ngữ cảnh.
-   **Thiên lệch mô hình ngôn ngữ (Language model bias):** Do sự thiên lệch quá mức đối với mô hình ngôn ngữ (gây ra bởi trọng số tương đối của mô hình ngôn ngữ quá cao), bộ giải mã có thể bị buộc phải từ chối giả thuyết đúng để chọn một ứng viên giả với xác suất mô hình ngôn ngữ cao hơn. Các lỗi này có thể xảy ra cùng với thiên lệch mô hình âm học tương tự.
-   **Nhiều vấn đề âm học kết hợp:** Đây là một danh mục lỗi rộng bao gồm các lỗi do các mục từ phát âm trong từ điển bị sai; hiện tượng nói ngập ngừng, phát âm sai bởi chính người nói, hoặc các lỗi do chính các mô hình âm học tạo ra (có thể do nhiễu âm học, sự không khớp dữ liệu giữa huấn luyện và sử dụng thực tế, v.v.).

## Các thách thức của nhận dạng tiếng nói tự động

Những tiến bộ gần đây trong ASR đã đưa độ chính xác của nhận dạng tiếng nói tự động tiệm cận với khả năng của con người trong nhiều tác vụ thực tế. Tuy nhiên, vẫn còn những thách thức lớn:

-   Các từ ngoài từ vựng (OOV) rất khó nhận dạng chính xác.
-   Tiếng ồn môi trường thay đổi làm giảm độ chính xác nhận dạng.
-   Tiếng nói bị chồng chéo (overlapping speech) là vấn đề nan giải đối với hệ thống ASR.
-   Nhận dạng tiếng nói của trẻ em và tiếng nói của những người bị khuyết tật phát âm vẫn chưa đạt mức tối ưu khi sử dụng dữ liệu huấn luyện thông thường.
-   Các mô hình dựa trên DNN thường đòi hỏi rất nhiều dữ liệu huấn luyện, lên đến hàng nghìn giờ. Các mô hình đầu-cuối có thể cần tới 100.000 giờ tiếng nói để đạt được hiệu suất cao.
-   Khả năng tự nhận biết độ không chắc chắn (uncertainty self-awareness) còn hạn chế: các hệ thống ASR điển hình luôn xuất ra chuỗi từ có khả năng xảy ra cao nhất thay vì báo cáo nếu một phần của đầu vào không thể hiểu được hoặc có độ không chắc chắn cao.

## Đánh giá hiệu suất

Hiệu suất của một hệ thống ASR được đo lường bằng cách so sánh các bản chuyển ngữ giả thuyết (transcriptions) đầu ra và bản chuyển ngữ tham chiếu (reference). Tỷ lệ lỗi từ (Word Error Rate - WER) là chỉ số được sử dụng rộng rãi nhất. Hai chuỗi từ trước tiên được căn chỉnh bằng cách sử dụng thuật toán căn chỉnh chuỗi dựa trên quy hoạch động (dynamic programming). Sau khi căn chỉnh, số lượng từ bị xóa (deletion - D), thay thế (substitution - S), và chèn thêm (insertion - I) được xác định. Tất cả các trường hợp xóa, thay thế và chèn thêm đều được coi là lỗi, và WER được tính bằng tỷ lệ giữa tổng số lỗi và tổng số từ (N) trong bản tham chiếu:

$$ WER = \frac{I + D + S}{N} * 100\% $$

Tỷ lệ lỗi câu (Sentence Error Rate - SER) đôi khi cũng được sử dụng để đánh giá hiệu suất của hệ thống ASR. SER tính toán tỷ lệ phần trăm các câu có ít nhất một lỗi.

### Tài liệu tham khảo

```{bibliography}
:filter: docname in docnames
```
