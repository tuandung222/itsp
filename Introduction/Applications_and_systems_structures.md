# Ứng dụng và cấu trúc hệ thống

## Ứng dụng

Xử lý tiếng nói được sử dụng trong nhiều lĩnh vực, ví dụ:

-   Viễn thông  
    -   Điện thoại và điện thoại di động
    -   Hệ thống hội nghị truyền hình
    -   Voice-over-IP, như
        [Skype](https://en.wikipedia.org/wiki/Skype), [Google
        Hangouts](https://en.wikipedia.org/wiki/Google_Hangouts),
        [Facetime](https://en.wikipedia.org/wiki/FaceTime),
        [Zoom](https://en.wikipedia.org/wiki/Zoom_Video_Communications)
    -   Podcast, radio và TV số
    -   Thực tế ảo và ứng dụng trò chơi
-   Trợ lý ảo điều khiển bằng giọng nói, như
    [Siri](https://en.wikipedia.org/wiki/Siri),
    [Alexa](https://en.wikipedia.org/wiki/Amazon_Alexa), [Google
    Assistant](https://en.wikipedia.org/wiki/Google_Assistant),
    [Mycroft](https://en.wikipedia.org/wiki/Mycroft_(software)),
    [Cortana](https://en.wikipedia.org/wiki/Cortana_%28software%29) v.v.
    -   Cả giao diện tiếng nói của robot
-   Hệ thống dẫn đường có phản hồi bằng giọng nói, như
    [TomTom](https://en.wikipedia.org/wiki/TomTom) và
    [Garmin](https://en.wikipedia.org/wiki/Garmin)
-   Dịch vụ điện thoại tự động (ví dụ: tổng đài của hãng hàng không)
-   Chuyển đổi tự động
    -   Phụ đề Youtube
    -   Ghi chú bác sĩ đã ghi âm
-   Microphone và hệ thống microphone
    -   Tai nghe, có hoặc không có giảm nhiễu, có hoặc không có chống ồn chủ động
    -   Microphone sân khấu (liên quan đến xử lý âm thanh)
-   Hệ thống ghi âm phòng thu (liên quan đến xử lý âm thanh)  
    -   [Autotune](https://en.wikipedia.org/wiki/Auto-Tune)
    -   Giảm/triệt nhiễu
-   Lồng tiếng phim, ví dụ: dịch âm thanh
    -   Cả dịch trực tuyến
-   Dịch vụ tổng hợp tiếng nói
    -   Trình đọc sách điện tử tự động
    -   Giao diện cho người khiếm thị và người khuyết tật
-   Ứng dụng ít phổ biến hơn  
    -   Kiểm soát truy cập và phát hiện gian lận bằng nhận dạng và xác minh người nói
    -   Ẩn danh và làm mờ (ví dụ: bảo vệ nhân chứng) tín hiệu tiếng nói
    -   Bộ tổng hợp tiếng nói cho người khuyết tật (ví dụ: Stephen Hawking)
    -   Phân tích y khoa tín hiệu tiếng nói (ví dụ: phát hiện [Alzheimer](https://en.wikipedia.org/wiki/Alzheimer%27s_disease))

![app1](attachments/165138615.jpeg)
![app2](attachments/165138616.png)
![app4](attachments/165138617.png)
![app3](attachments/165138618.png)
![app3](attachments/165138684.png)


Các ứng dụng này có thể được phân loại theo chức năng, đại khái như sau:

-   Truyền dẫn và lưu trữ tiếng nói
    -   Cho phép giao tiếp với người ở xa
-   Giao diện người dùng điều khiển bằng tiếng nói
    -   Cho phép tương tác bằng giọng nói với máy
-   Trích xuất thông tin từ tín hiệu tiếng nói
    -   Như chuyển đổi tự động để tạo phụ đề cho phim
-   Tổng hợp tiếng nói, tức tạo tiếng nói tự động
-   "Cải thiện" tín hiệu tiếng nói, như  
    -   Giảm nhiễu, dịch thuật

Lưu ý rằng các nhóm này chồng lấp nhau ở nhiều khía cạnh; ví dụ, giảm nhiễu có thể là một phần của bất kỳ hệ thống xử lý tiếng nói nào, và trích xuất thông tin gần như bắt buộc trong giao diện người dùng điều khiển bằng giọng nói.


## Cấu trúc hệ thống

### Truyền dẫn và lưu trữ

Mục tiêu của hệ thống truyền dẫn tiếng nói là nén tín hiệu thành càng ít bit càng tốt, đồng thời giữ chất lượng âm thanh ở đầu ra càng tốt. Điều này đòi hỏi rằng những suy giảm mà ta đưa vào phải được chọn sao cho ảnh hưởng cảm quan (perceptual) của chúng là nhỏ nhất. Nói cách khác, ta không muốn người nghe nhận ra (hoặc nhận ra càng ít càng tốt) rằng tín hiệu đã bị suy giảm. Trong hình minh họa bên phải, tại bộ mã hóa (encoder) phía gửi, ta có một mô hình về tầm quan trọng cảm quan, xác định cách tín hiệu được lượng tử hóa. Tín hiệu đã lượng tử hóa sau đó được nén thành càng ít bit càng tốt. Để nén như vậy, ta sử dụng thông tin thống kê về tín hiệu tiếng nói.

Bộ giải mã (decoder) phía nhận sẽ đảo ngược các bước bằng giải nén và giải lượng tử hóa.

Các thao tác tiền xử lý thường bao gồm giảm nhiễu và phát hiện hoạt động tiếng nói (xem dưới).

![struct1](attachments/165138696.png)

  

### Trích xuất thông tin

Ta có thể trích xuất nhiều loại thông tin từ tín hiệu tiếng nói, như [nội dung văn bản](content:asr) và [danh tính người nói](content:sid). Nhiều dạng thông tin như vậy có thể được phân loại bằng *nhãn*, tức là ta gán một nhãn cho một tín hiệu tiếng nói cụ thể. Nhãn đó có thể là từ đã được phát âm hoặc danh tính người nói. Ngoài ra, thông tin trích xuất cũng có thể có giá trị liên tục, như tuổi hoặc tâm trạng của người nói (bạn vui/buồn thế nào?), nhưng ta có thể xem cả hai loại thông tin đều là nhãn.

Các phương pháp trích xuất thông tin ngày nay chủ yếu là phương pháp học máy. Một cấu hình điển hình được minh họa bên phải, trong đó hệ thống được huấn luyện ngoại tuyến (off-line) với cơ sở dữ liệu tiếng nói và nhãn tương ứng. Sau khi huấn luyện, hệ thống "biết" cách suy ra nhãn từ đầu vào tiếng nói, sao cho khi sử dụng thực tế, nó có thể phân loại tiếng nói đầu vào để đưa ra ước lượng nhãn.

Trong nhiều trường hợp, trích xuất thông tin cũng có thể được thực hiện như một bài toán xử lý tín hiệu, trong đó ta sử dụng kiến thức tiên nghiệm về tín hiệu để thiết kế thuật toán. Ví dụ, để ước lượng [tần số cơ bản](content:f0) (pitch) của tín hiệu tiếng nói, ta có thể sử dụng kiến thức để thiết kế các thuật toán hiệu quả. Những thuật toán này thường đơn giản hơn học máy khoảng một bậc, nhưng nếu bài toán phức tạp, độ chính xác của đầu ra sẽ giảm tương ứng.

![struct2](attachments/165138741.png)

## Tổng hợp tiếng nói

Khi muốn máy tính nói, ta cần bộ tổng hợp tiếng nói (speech synthesiser), nhận văn bản làm đầu vào và xuất ra tiếng nói. Đây là bài toán ngược của trích xuất thông tin, trong đó vai trò của tiếng nói và nhãn (văn bản) được hoán đổi (xem hình bên phải). Cũng như trong trích xuất thông tin, ở đây ta có thể sử dụng phương pháp đơn giản hơn khi phù hợp. Phương pháp cổ điển là tổng hợp nối (concatenative synthesis), trong đó các đoạn tiếng nói từ cơ sở dữ liệu được ghép lại thành câu liên tục. Phương pháp này phổ biến trong các hệ thống thông báo công cộng (ví dụ: nhà ga), nơi phạm vi thông báo có thể biết trước.

![struct3](attachments/165139247.png)


## Giao diện người dùng

Giao diện người dùng có thể sử dụng tiếng nói để nhận lệnh giọng nói và/hoặc phản hồi bằng giọng nói. Ví dụ:

-   Hệ thống dẫn đường ô tô có thể đưa ra hướng dẫn bằng giọng nói, nhưng chỉ nhận lệnh qua giao diện cảm ứng.
-   Cửa tự động có thể nhận lệnh giọng nói ("Cửa, mở") nhưng không phản hồi bằng âm thanh.
-   Giao diện điều khiển hoàn toàn bằng giọng nói, như loa thông minh, vừa nhận lệnh vừa phản hồi bằng giọng nói.

Các hệ thống đơn hướng chỉ có nhận dạng tiếng nói hoặc chỉ tạo tiếng nói là tập con của hệ thống "đầy đủ" điều khiển bằng giọng nói. Ngăn xếp các module của một hệ thống hoàn chỉnh được minh họa bên phải. Ở đây, phần trước âm thanh (acoustic front-end) có thể chứa các phương pháp tiền xử lý được mô tả trong phần tiếp theo. Hiểu ngôn ngữ tự nhiên gán ý nghĩa cho chuỗi từ, quản lý đối thoại ánh xạ ý nghĩa đó thành hành động cụ thể, được thực thi bởi (các) bộ truyền động, và sinh ngôn ngữ tự nhiên tạo câu trả lời dưới dạng văn bản.

![struct4](attachments/165139542.png)

## Xử lý và tiền xử lý

Bất kể ứng dụng nào, hầu hết các hệ thống hoạt động với tín hiệu tiếng nói đều gặp những vấn đề tương tự:

-   Ứng dụng chính tốn kém khi chạy, nên sẽ hữu ích nếu có bộ phận tiền xử lý phát hiện khi nào nên kích hoạt ứng dụng chính. Ví dụ, khi không ai nói, việc chạy bộ nhận dạng tiếng nói là vô nghĩa.
-   Thiết bị được sử dụng trong môi trường thực, nơi tiếng ồn nền và tiếng vang phòng phổ biến. Do đó, việc làm sạch tín hiệu trước khi đưa vào ứng dụng chính là hữu ích.

Những chức năng này thường tạo thành phần trước âm thanh (acoustic front-end).

### Phát hiện hoạt động tiếng nói

Khi không có tiếng nói, hầu hết các thao tác xử lý tiếng nói đều vô nghĩa. [Phát hiện hoạt động tiếng nói](content:vad) (voice activity detection) là việc phân loại các đoạn tín hiệu theo tiêu chí có chứa tiếng nói hay không.

### Phát hiện từ khóa hoặc Wake-word

Trong hầu hết các tình huống thực tế có thiết bị điều khiển bằng giọng nói, ta muốn nói chuyện với người khác chứ không chỉ với thiết bị. Nói cách khác, ta cần biết khi nào người dùng đang nói với thiết bị và khi nào không. Thiết bị khi đó không cần cố hiểu tiếng nói hướng đến người khác. Phát hiện wake-word (hoặc spotting) là quá trình chỉ chờ một từ cụ thể để kích hoạt ứng dụng chính. Ví dụ, loa thông minh Amazon Alexa chờ người dùng nói wake-word "Alexa", và chỉ sau khi nhận diện từ đó, nó mới bắt đầu quá trình chính.

### Nâng cao chất tiếng nói

Ta có thể cố gắng loại bỏ tác động bất lợi của tiếng ồn nền và tiếng vang phòng bằng các phương pháp nâng cao chất tiếng nói. Hãy tưởng tượng việc hiểu ai đó qua điện thoại khó đến mức nào khi người ở đầu dây bên kia đang đứng trên đường phố đông đúc với xe cộ qua lại. Bằng giảm nhiễu, ta có thể giảm những tiếng ồn đó để tiếng nói dễ nghe hơn ở đầu nhận. Bất kỳ quá trình xử lý nào khác cũng trở nên đơn giản hơn nếu tín hiệu tiếng nói sạch hơn. Do đó, giao diện người dùng giọng nói có thể sử dụng giảm nhiễu như tiền xử lý. Tuy nhiên, lưu ý rằng các quá trình khác như phát hiện hoạt động tiếng nói và phát hiện wake-word cũng trở nên dễ dàng hơn trên tín hiệu sạch (xem phương án 1 trong hình bên phải). Tuy nhiên, vì giảm nhiễu có thể tốn tài nguyên tính toán, tốt hơn nên áp dụng khi ta đã biết tín hiệu là lệnh giọng nói (xem phương án 2 trong hình bên phải), mặc dù điều này sẽ giảm độ chính xác của phát hiện hoạt động và wake-word.

### Xử lý tín hiệu khác

Tín hiệu tiếng nói có thể được xử lý thêm bằng một loạt các thuật toán khác nhau tùy theo nhu cầu. Ví dụ:

-   Tín hiệu có thể được chỉnh sửa cho mục đích nghệ thuật bằng các công cụ như auto-tune, trong đó pitch của giọng hát được điều chỉnh để khớp với pitch mong muốn.
-   Tín hiệu tiếng nói có thể được dịch sang ngôn ngữ khác.
-   Danh tính người nói có thể được ẩn (hoặc giả mạo) cho mục đích bảo mật như bảo vệ nhân chứng, hoặc cho hoạt động phi pháp như gian lận.

 
![struct5](attachments/165139583.png)
