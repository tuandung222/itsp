# Tổng hợp tiếng nói tham số thống kê (Statistical parametric speech synthesis)
  

Mặc dù phương pháp tổng hợp ghép nối có thể tạo ra tiếng nói tổng hợp có độ tự nhiên cao, hướng tiếp cận này vốn dĩ bị giới hạn bởi các đặc tính của kho ngữ liệu tiếng nói dùng cho quá trình chọn đơn vị. Các hệ thống ghép nối chỉ có thể tạo ra các từ ngữ mà các đoạn cấu thành của chúng (ví dụ: các diphone) đã được ghi âm trước. Để âm thanh tổng hợp nghe tự nhiên, chúng ta cần phải thu thập một lượng lớn dữ liệu tiếng nói từ một người nói duy nhất. Điều này làm hạn chế tính linh hoạt của các hệ thống ghép nối trong việc tạo ra các giọng nói khác nhau, các phong cách nói, biểu cảm cảm xúc khác nhau hoặc các biến đổi âm thanh khác vốn rất phổ biến trong giao tiếp hàng ngày của con người. 

Như một giải pháp thay thế cho hướng tiếp cận ghép nối, *tổng hợp tiếng nói tham số thống kê* (Statistical Parametric Speech Synthesis - SPSS) là một hướng tiếp cận TTS khác đã trở nên cực kỳ phổ biến trong lĩnh vực công nghệ tiếng nói. Lý do là vì nó giải quyết được hạn chế cốt lõi của các hệ thống ghép nối - sự thiếu linh hoạt - bằng cách tạo ra tiếng nói sử dụng các mô hình thống kê thay vì dựa vào các đoạn âm ghi sẵn. Các mô hình thống kê này được học từ các kho ngữ liệu tiếng nói bằng các kỹ thuật học máy, mã hóa thông tin về sự biến đổi của tiếng nói theo thời gian trong ngữ cảnh của văn bản đầu vào. Xét trên khía cạnh này, các hệ thống SPSS có thể được coi là ảnh phản chiếu của các hệ thống [nhận dạng tiếng nói tự động (ASR)](content:asr): trong khi một hệ thống ASR cố gắng chuyển đổi tiếng nói từ các đặc trưng âm học thành một chuỗi các từ bằng các mô hình học máy, thì một hệ thống SPSS cố gắng chuyển đổi một chuỗi các từ thành các đặc trưng âm học hoặc chuyển trực tiếp thành dạng sóng âm học nhờ các mô hình học máy. Cả hệ thống ASR và SPSS thông thường đều được huấn luyện trên một lượng lớn dữ liệu tiếng nói đi kèm với văn bản dịch tương ứng, tạo ra một tập hợp các *tham số* mô tả các *đặc trưng thống kê* của dữ liệu tiếng nói (đó là lý do tại sao nó có tên gọi là tổng hợp tiếng nói "tham số thống kê").

  
![SPSS_basic_pipeline.png](attachments/175517696.png)
**Hình 1:** Sơ đồ tổng quan về một hệ thống SPSS.

  

Một hệ thống SPSS hoàn chỉnh gồm các mô-đun phân tích văn bản (text analysis), tạo đặc trưng (feature generation) và tạo dạng sóng (waveform generation). Hướng tiếp cận cổ điển đối với SPSS dựa trên sự kết hợp của kiến trúc *Mô hình Markov ẩn hỗn hợp Gauss* (Hidden Markov Model - Gaussian Mixture Model, viết tắt là HMM-GMM) để tạo đặc trưng và một *bộ vocoder* để tạo dạng sóng, các thành phần này sẽ được thảo luận chi tiết hơn ở phần dưới. Các tiến bộ gần đây trong SPSS dựa trên mạng nơ-ron sẽ được điểm lại ở phần cuối.

  

### Tạo đặc trưng (Feature generation)

Dựa trên mô tả ngôn ngữ học của văn bản cần tổng hợp, mục đích của việc tạo đặc trưng là chuyển đổi các đặc trưng ngôn ngữ học thành một mô tả tương ứng của tín hiệu âm học. Tương tự như trong ASR, thành phần đóng vai trò trung gian giữa hai cấp độ này được gọi là *mô hình âm học (acoustic model)*. Về mặt kỹ thuật, mô hình âm học chuyển đổi đầu vào ngôn ngữ học thành một chuỗi các đặc trưng âm học ở một tốc độ khung hình cố định (ví dụ: một khung đặc trưng mỗi 10 ms) bằng cách sử dụng một ánh xạ xác suất (probabilistic mapping) giữa hai bên. Ánh xạ này được học từ một kho ngữ liệu tiếng nói huấn luyện.

Một hướng tiếp cận tiêu chuẩn cho ánh xạ xác suất là sử dụng HMM-GMM làm mô hình tham số thống kê. Tương tự như trong hệ thống ASR HMM-GMM, các trạng thái $s$ của HMM tương ứng với các phần của đơn vị dưới cấp từ (ví dụ: các phần của một âm, diphone hoặc triphone). Xác suất chuyển trạng thái $P(s | s_{t-1})$ mô tả cách tiếng nói tiến triển qua từng đơn vị dưới cấp từ và từ đơn vị này sang đơn vị khác. Các đặc tính âm học liên kết với mỗi trạng thái được mô hình hóa bằng một GMM, trong đó GMM mô tả một phân phối xác suất $P(y | s)$ trên các vector đặc trưng âm học khả thi ở trạng thái đó. Từ một chuỗi các đơn vị dưới cấp từ mong muốn (như được chỉ thị bởi các đặc trưng ngôn ngữ học), mô hình có thể được lấy mẫu theo kiểu ngẫu nhiên (stochastically) hoặc xác định (deterministically) để tạo ra một chuỗi các đặc trưng âm học. Các đặc trưng này sau đó được đưa vào *mô-đun tạo dạng sóng (waveform generation module)* để tạo ra tín hiệu tiếng nói thực tế. Ở dạng cơ bản nhất, việc tự chuyển trạng thái từ một trạng thái HMM về chính nó đại diện cho thời lượng nằm ở trạng thái đó (tức là nội dung âm học đó cần được lặp lại bao nhiêu khung hình). Tuy nhiên, các mô hình thời lượng (duration models) tiên tiến hơn thường được sử dụng riêng biệt để khắc phục các hạn chế của chuỗi Markov bậc một trong việc mô hình hóa các phụ thuộc thời gian và các đặc tính thời lượng của tiếng nói.

  
![synthesis_HMM_GMM.png](attachments/175518368.png)

**Hình 2:** Minh họa trực quan về quá trình tạo đặc trưng tiếng nói dựa trên HMM-GMM. Chuỗi trạng thái $s = \{s_1, s_2,...,s_{10}\}$ cần thiết cho từ "cat" (/k ae t/) được hiển thị ở trên cùng, trong đó mỗi âm vị gồm ba trạng thái: trạng thái đầu, trạng thái giữa và trạng thái cuối (ví dụ: $k_{1}$, $k_{2}$ và $k_{3}$).
Mỗi trạng thái liên kết với một mô hình hỗn hợp Gauss (GMM) $N$ chiều, trong đó $N$ là số chiều của các đặc trưng tiếng nói $y$. Tại mỗi bước thời gian, GMM của trạng thái hiện hoạt được lấy mẫu để rút ra một vector đặc trưng $y_{t}$. Sau đó, một sự chuyển trạng thái có thể xảy ra sang trạng thái tiếp theo hoặc quay lại trạng thái hiện tại, giúp kiểm soát các khía cạnh thời lượng của tiếng nói.


### Tạo dạng sóng với bộ vocoder

Một dạng sóng tiếng nói chất lượng cao điển hình bao gồm các giá trị biên độ "liên tục" (ví dụ: lượng tử hóa 16-bit) được lấy mẫu ở tần số 16 kHz. Ngoài ra, hình dạng của dạng sóng chịu ảnh hưởng bởi nhiều yếu tố không trực tiếp đóng góp vào độ tự nhiên hay độ rõ của tiếng nói, chẳng hạn như độ lợi tín hiệu (signal gain) hoặc các đặc tính pha và biên độ của chuỗi ghi âm và truyền dẫn. Điều này có nghĩa là chỉ 80 miligiây của dạng sóng thô - độ dài điển hình của một nguyên âm - sẽ tương ứng với $0.08 \text{ s} \times 16 \text{ kHz} = 1280$ chiều của vector biên độ, và vector này có thể có vô số hình dạng khác nhau đối với các âm thanh mà tai người cảm nhận là cực kỳ giống nhau. Hơn nữa, các giá trị được mã hóa trong vector này sẽ có độ tương quan rất cao với nhau (xem [LPC](content:linearprediction)). Do số chiều lớn, tính biến thiên cao và bản chất dư thừa của biểu diễn tín hiệu dạng sóng, nó không phải là một đối tượng lý tưởng cho việc mô hình hóa tham số thống kê bằng các kỹ thuật học máy cổ điển (tuy nhiên hãy xem phần SPSS dựa trên Mạng nơ-ron dưới đây).

Tuy nhiên, như chúng ta đã biết từ phần trích xuất đặc trưng tiếng nói (xem [STFT](stft)), tín hiệu tiếng nói có thể được coi là gần dừng (quasi-stationary) trong các cửa sổ ngắn có thời lượng khoảng 10–30 ms. Nội dung tiếng nói của tín hiệu trong các cửa sổ ngắn này có thể được mô tả bằng một tập hợp các đặc trưng nguồn và đặc trưng phổ (như [MFCC](content:mfcc) và [F0](content:f0)) được giả định là không đổi trong cửa sổ đó. Khi trích xuất các đặc trưng trong một cửa sổ trượt với các bước cửa sổ ngắn (ví dụ: 10 ms), cấu trúc tổng thể của tín hiệu có thể được nắm bắt bằng một biểu diễn có số chiều thấp hơn nhiều và ít biến thiên hơn nhiều so với dạng sóng thực tế. **Một bộ vocoder là một thuật toán có thể: 1) tham số hóa một dạng sóng tiếng nói thành một tập hợp các đặc trưng mô tả gọn nhẹ hơn theo thời gian, đồng thời 2) tổng hợp lại tiếng nói từ các đặc trưng đó với tổn thất tối thiểu về chất lượng**. Bên cạnh đó, nhiều bộ vocoder sử dụng các đặc trưng có thể diễn giải được dưới góc độ cơ chế phát âm hoặc âm học tiếng nói, cho phép phân tích và can thiệp vào tín hiệu tiếng nói để quan sát hoặc tạo ra các hiện tượng nhất định trong tiếng nói. 

Tính gọn nhẹ và tính bất biến của biểu diễn tín hiệu âm học cũng là lý do tại sao vocoder được sử dụng trong các hệ thống SPSS: thay vì tạo trực tiếp dạng sóng tiếng nói, mô-đun tạo đặc trưng trước tiên sẽ tạo ra một tập hợp các đặc trưng vocoder có số chiều thấp hơn để đặc trưng hóa tín hiệu tiếng nói bằng các thuộc tính thiết yếu của nó. Bộ vocoder sau đó nhận các đặc trưng này làm đầu vào và tạo ra dạng sóng tương ứng bằng một chuỗi các thao tác xử lý tín hiệu. Các thao tác này về cơ bản là quá trình ngược của quá trình trích xuất đặc trưng ban đầu, kết hợp với một số cơ chế bổ sung để tái tạo (hoặc bù đắp) các thông tin bị mất đi trong quá trình trích xuất đặc trưng (chẳng hạn như pha tín hiệu vốn bị loại bỏ khỏi các đặc trưng phổ tiêu chuẩn).

Ví dụ, khi sử dụng bộ vocoder STRAIGHT phổ biến (Kawahara và cộng sự, 1999), mô hình HMM-GMM trước tiên sẽ tạo ra một chuỗi các vector đặc trưng mã hóa bao phổ (spectral envelope), F0, và đặc tính tuần hoàn của tín hiệu tiếng nói cần tạo ra theo chỉ dẫn từ mô-đun phân tích văn bản. Các đặc trưng này sau đó được đưa vào STRAIGHT để tổng hợp dạng sóng tiếng nói cuối cùng.

  
![vocoder_basic_structure](attachments/175517700.png)
 **Hình 3:**
Sơ đồ mô tả bộ vocoder và các ứng dụng điển hình của các đặc trưng vocoder.  
Khi được sử dụng như một phần của hệ thống SPSS, các đặc trưng vocoder được tạo ra bởi mô hình thống kê tham số trong quá trình tổng hợp.

  

### Huấn luyện hệ thống SPSS

Việc huấn luyện hệ thống SPSS đề cập đến quá trình ước lượng mô hình âm học tham số (ví dụ: HMM-GMM) chịu trách nhiệm ánh xạ các đặc trưng ngôn ngữ học sang các đặc trưng tạo dạng sóng (vocoder) tương ứng. Điều này được thực hiện bằng cách sử dụng một kho ngữ liệu tiếng nói, trong đó mỗi phát ngôn đi kèm với văn bản tương ứng của những gì đã được nói, và có thể có thêm chú giải ngữ âm mô tả các đơn vị ngữ âm và vị trí thời gian của chúng trong dạng sóng. Đầu tiên, mô-đun phân tích văn bản được sử dụng để tạo ra các đặc trưng ngôn ngữ học của phát ngôn huấn luyện, trong khi bộ vocoder được sử dụng để trích xuất các đặc trưng vocoder từ dạng sóng tiếng nói tương ứng. Sau đó, mô hình thống kê được huấn luyện để giảm thiểu sai số dự báo của các đặc trưng vocoder cho trước khi các đặc trưng ngôn ngữ học được sử dụng làm đầu vào. Việc tiếp cận chú giải ngữ âm cho phép căn chỉnh thời gian (temporal alignment) chính xác hơn giữa các đặc trưng ngôn ngữ học và tín hiệu tiếng nói. Vì kiến trúc HMM được sử dụng rộng rãi cho mô hình hóa âm học không lý tưởng cho việc mô hình hóa thời lượng của đoạn tiếng nói, một *mô hình thời lượng (duration model)* riêng biệt thường được huấn luyện để căn chỉnh các đặc trưng ngôn ngữ học (vốn độc lập với tốc độ nói và nhịp điệu trong dữ liệu tiếng nói thực tế) với các đơn vị ngữ âm được thực hiện trong tín hiệu tiếng nói âm học. Trong hình dưới đây, cả mô hình âm học và mô hình thời lượng đều được ký hiệu chung bằng khối mô hình thống kê tham số (parametric statistical model).

  

![SPSS_training_pipeline.png](attachments/175517698.png)
**Hình 4:** Sơ đồ quy trình huấn luyện hệ thống SPSS.

  

### Ưu điểm và nhược điểm của SPSS dựa trên HMM-GMM so với tổng hợp ghép nối

Vì các "chỉ thị" cho việc tạo tiếng nói được mã hóa bởi các tham số của mô hình SPSS, mô hình có thể dễ dàng được thích ứng để tạo ra tiếng nói với các đặc trưng khác nhau. Ví dụ, các đặc trưng đường dẫn tiếng của người nói huấn luyện được mã hóa bởi kỳ vọng và phương sai của các phân phối Gauss trong mỗi trạng thái HMM, trong khi các đặc trưng thời lượng được mã hóa bởi ma trận xác suất chuyển trạng thái của HMM. Do đó, hệ thống có thể thích ứng với những người nói khác bằng cách đơn giản là thích ứng mô hình HMM-GMM đã được huấn luyện trước bằng cách sử dụng tiếng nói của những người nói mới. Trong trường hợp này, các kỹ thuật tiêu chuẩn như thích ứng cực đại hóa hậu nghiệm (Maximum A Posteriori - MAP) hoặc hồi quy tuyến tính hợp lý cực đại (Maximum Likelihood Linear Regression - MLLR) có thể được sử dụng để cập nhật các tham số mô hình. Thêm vào đó, vì các tham số của HMM-GMM thường có thể diễn giải được dưới dạng bao phổ tiếng nói hoặc các đặc tính phát âm, chúng ta hoàn toàn có thể sửa đổi các mô hình hoặc hậu xử lý các đặc trưng âm học thu được để đạt được các hiệu ứng mong muốn. Ví dụ, việc thay đổi cao độ giọng nói có thể thực hiện bằng cách điều chỉnh tham số F0, trong khi việc giảm thiểu một số lỗi tổng hợp như chất lượng âm thanh bị nghẹt (muffled) do trung bình hóa thống kê có thể được giải quyết bằng cách điều chỉnh các tham số GMM thông qua một phép biến đổi được lựa chọn.

  

Các nhược điểm tiềm ẩn của hướng tiếp cận thống kê bao gồm các vấn đề về chất lượng âm thanh (ví dụ: giọng bị nghẹt) do sự làm mịn thống kê (statistical smoothing) diễn ra trong mô hình tạo sinh ngẫu nhiên, giới hạn chất lượng âm thanh của bộ vocoder được sử dụng, và các vấn đề tiềm ẩn trong việc ước lượng mô hình thống kê bền vững từ lượng dữ liệu hữu hạn.

  

### SPSS dựa trên mạng nơ-ron (Neural SPSS)

Các tiến bộ gần đây trong mạng nơ-ron nhân tạo (ANN) cũng dẫn đến những bước phát triển mới cho SPSS vượt ra ngoài khuôn khổ HMM-GMM cổ điển. Về khía cạnh vocoder, WaveNet (van den Oord và cộng sự, 2016) là một bộ tạo dạng sóng bằng mạng nơ-ron có ảnh hưởng lớn, có khả năng tạo ra tiếng nói chất lượng rất cao. Nó dựa trên kiến trúc mạng nơ-ron tích chập tự hồi quy (autoregressive CNN) và hoạt động trực tiếp trên các dạng sóng tiếng nói. Dựa trên lịch sử các mẫu dạng sóng trước đó cùng một số thông tin "điều kiện hóa" (conditioning) về loại tín hiệu cần tạo ra, mô hình dự đoán mẫu dạng sóng tiếp theo có khả năng xảy ra cao nhất tại mỗi bước thời gian. Ví dụ, WaveNet có thể được huấn luyện để tạo ra tiếng nói từ các đặc trưng phổ như năng lượng log-Mel và thông tin F0. Mặc dù WaveNet có thể đạt tới độ tự nhiên gần như người thật của tiếng nói được tạo ra (với một số hạn chế nhất định), quá trình xử lý tự hồi quy ở cấp độ dạng sóng đòi hỏi chi phí tính toán cực kỳ lớn. Do đó, việc phát triển các bộ vocoder mạng nơ-ron chất lượng cao hoạt động linh hoạt về mặt tính toán vẫn đang là một lĩnh vực nghiên cứu rất sôi động.

Bên cạnh vocoder, các mạng nơ-ron đã trở thành sự thay thế phổ biến cho HMM-GMM trong giai đoạn tạo đặc trưng. Ví dụ, các mạng truyền thẳng sâu (deep feed-forward networks) hoặc LSTM có thể được sử dụng trong việc tạo đặc trưng. Vì LSTM đặc biệt giỏi trong việc mô hình hóa các phụ thuộc thời gian, về mặt lý thuyết chúng có thể xử lý các nhập nhằng thời gian lớn hơn và sự biến thiên giữa các đặc tả ngôn ngữ học đầu vào và các đặc trưng vocoder mục tiêu.

Nếu có đủ dữ liệu huấn luyện, chúng ta hoàn toàn có thể triển khai toàn bộ quy trình từ văn bản viết cho đến dạng sóng tổng hợp bằng một hệ thống mạng nơ-ron duy nhất. Tacotron 2 là một ví dụ điển hình cho hệ thống này, nơi văn bản đầu vào được xử lý bằng một mô hình ANN sequence-to-sequence để trực tiếp tạo ra phổ đồ log-Mel (log-Mel spectrogram) tương ứng với văn bản đầu vào (nghĩa là không cần mô-đun phân tích văn bản chuyên dụng). Phổ đồ sau đó được đưa vào mô-đun WaveNet (xem ở trên) để tạo ra tín hiệu tiếng nói. Kết quả là, Tacotron 2 kết hợp cùng vocoder WaveNet có thể đạt được chất lượng tiếng nói cực kỳ ấn tượng. Ưu điểm của các hướng tiếp cận đầu-cuối (end-to-end) này là giảm bớt các giả định về việc loại biểu diễn trung gian nào tốt cho tác vụ, hạn chế nguy cơ các thao tác và biểu diễn được chỉ định sẵn làm mất đi thông tin hữu ích trong quy trình. Nó cũng không đòi hỏi sự hiểu biết sâu sắc về cấu trúc ngôn ngữ học của ngôn ngữ viết và nói, hay quyền truy cập vào các công cụ phân tích văn bản sẵn có, giúp việc triển khai hệ thống khả thi cho bất kỳ ngôn ngữ nào có đủ dữ liệu huấn luyện (văn bản và tiếng nói tương ứng). Vì tất cả các thành phần đều dựa trên các phép toán mạng nơ-ron khả vi, chúng ta có thể tối ưu hóa đồng thời toàn bộ chuỗi từ tạo dạng sóng đến xử lý văn bản. Về nguyên tắc, các hệ thống SPSS mạng nơ-ron cũng cực kỳ linh hoạt, vì hầu như bất kỳ loại thông tin bổ trợ nào cũng có thể được đưa vào hệ thống để điều chỉnh các đặc trưng của tiếng nói được tạo ra.

Tuy nhiên, các hệ thống mạng nơ-ron cũng có những nhược điểm riêng. Lượng dữ liệu và tài nguyên tính toán cần thiết để huấn luyện các hệ thống này là rất lớn. Các yêu cầu tính toán khi chạy thực tế (runtime) của các vocoder mạng nơ-ron cũng có thể là một trở ngại lớn trong một số ứng dụng, mặc dù các tiến bộ gần đây về vocoder và tính toán song song đã mang lại những bước tiến đáng kể trong vấn đề này. Một vấn đề khác là tính thiếu khả năng diễn giải (interpretability) và độ minh bạch của các tham số mô hình: trong khi các tham số của các mô hình cổ điển như HMM và GMM có mối quan hệ tương đối rõ ràng với đầu vào và đầu ra của hệ thống, điều này lại không đúng đối với các ANN có nhiều lớp ẩn. Điều này làm cho việc thấu hiểu hành vi của mô hình trở nên khó khăn hơn nhiều, đặc biệt là khi cố gắng khắc phục các vấn đề về hiệu suất của mô hình. Việc thiếu tính minh bạch và khả năng diễn giải cũng đồng nghĩa với việc điều khiển thủ công các đặc tính của tiếng nói tạo ra sẽ phức tạp hơn. Cuối cùng, việc thích ứng mô hình với dữ liệu mới (ví dụ: người nói mới hoặc phong cách nói mới) không thể sử dụng các giải pháp toán học đã được hiểu rõ của các mô hình cổ điển. Thay vào đó, việc thiết kế, huấn luyện và thích ứng các ANN phần lớn mang tính kinh nghiệm (heuristics-driven), tương tự như việc sử dụng ANN trong bất kỳ lĩnh vực học máy nào khác.

  

  

### Tài liệu đọc thêm

Kawahara, K., Masuda-Katsuse, I., và de Cheveigné , A. (1999). Restructuring speech representations using a pitch-adaptive time–frequency smoothing and an instantaneous-frequency-based F0 extraction: Possible role of a repetitive structure in sounds, *Speech Communication*, 27, 187–207. (STRAIGHT vocoder)

Yamagishi, J. (2006). An introduction to HMM-based speech synthesis. 
<https://wiki.inf.ed.ac.uk/pub/CSTR/TrajectoryModelling/HTS-Introduction.pdf>  (Giới thiệu về SPSS dựa trên HMM)

Shen, J. và cộng sự (2017). Natural TTS synthesis by conditioning WaveNet on Mel spectrogram predictions. ArXiV pre-print: <https://arxiv.org/abs/1712.05884> (Tacotron 2)

Tokuda, K., Nankaku, Y., Toda, T., Zen, H., Yamagishi, Y., và Oura, K. (2013). Speech synthesis based on hidden Markov models. *Proceedings of the IEEE*, 101, 1234–1252. (Giới thiệu về SPSS)

van den Oord, A., Dieleman, S., Zen, H., Simonyan, K., Vinyals, O., Graves, A., Kalchbrenner, N., Senior, A., và Kavukcuoglu, K. (2016). *WaveNet: A generative model for raw audio.* ArXiV pre-print: <https://arxiv.org/pdf/1609.03499.pdf> (Bài báo gốc về WaveNet)

Wu, Z., Watts, O., và King, S. (2016). Merlin: An Open Source Neural Network Speech Synthesis System. *In Proc. 9th ISCA Speech Synthesis Workshop (SSW9)*, September 2016, Sunnyvale, CA, USA  <https://github.com/CSTR-Edinburgh/merlin> (Bộ công cụ Merlin dùng cho tổng hợp tiếng nói)

Zen, H., Tokuda, K., và Black, A. W. (2009). Statistical parametric speech synthesis. **Speech Communication**, 51, 1039–1064. <https://www.sciencedirect.com/science/article/pii/S0167639309000648> (Giới thiệu về SPSS)
