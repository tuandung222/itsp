(content:synthesis)=
# Tổng hợp tiếng nói (Speech Synthesis)

Các hệ thống tổng hợp tiếng nói hướng tới việc tạo ra tín hiệu tiếng nói rõ ràng, dễ hiểu từ một dạng biểu diễn ngôn ngữ đầu vào nào đó. Các hệ thống tổng hợp cũng thường được gọi là hệ thống chuyển văn bản thành tiếng nói (Text-To-Speech - TTS), vì văn bản viết là một cách tự nhiên để chỉ thị loại phát ngôn cần được tạo ra.

Một hệ thống TTS điển hình gồm hai mô-đun xử lý cơ bản: mô-đun phân tích văn bản (text analysis) và mô-đun tổng hợp tiếng nói (speech synthesis).  

**Phân tích văn bản (Text analysis)**

Mô-đun đầu tiên trong quy trình TTS, mô-đun phân tích văn bản, có nhiệm vụ chuyển đổi văn bản đầu vào thành một biểu diễn ngôn ngữ học (linguistic representation) mã hóa các thông tin về cách văn bản đó được phát âm.

Trong bước đầu tiên, mô-đun phải xử lý văn bản thành định dạng chuẩn hóa bằng cách giải quyết các ký tự đặc biệt hoặc các điểm không nhất quán khác. Tiếp theo, văn bản phải được phân tích cấu trúc để xác định các thành phần cú pháp của câu. Mọi sự bất tương thích giữa ngôn ngữ viết và ngôn ngữ nói (ví dụ: các từ viết tắt và chữ viết tắt chữ đầu, tên riêng, số) phải được phát hiện và chuyển đổi sang định dạng chính xác (ví dụ: chuyển chuỗi "100 sq ft" thành "*one hundred square feet*" [một trăm feet vuông], chứ không phải "*one-zero-zero sq ft*").  

Bước thứ hai bao gồm việc suy diễn cách phát âm chính xác của các từ, còn được gọi là chuyển đổi tự vị sang âm vị (grapheme-to-phoneme - G2P). Do mối quan hệ giữa ngôn ngữ viết và ngôn ngữ nói không phải lúc nào cũng tương ứng trực tiếp (ví dụ: trong tiếng Anh), mô-đun phân tích văn bản phải chuyển đổi các chuỗi ký tự chữ viết đầu vào thành các chuỗi âm vị (phonemes) dựa trên các quy ước phát âm của ngôn ngữ đó. Điều này đòi hỏi phải giải quyết nhiều từ nhập nhằng phổ biến trong ngôn ngữ, chẳng hạn như cách xử lý các từ đồng tự dị âm (homographs - các từ có cách viết giống nhau nhưng cách phát âm khác nhau). Các từ ngoại lai và từ mượn cũng cần được xử lý phù hợp.

Ở giai đoạn cuối cùng, các đặc trưng siêu đoạn tính (suprasegmental characteristics) của tiếng nói chuẩn bị tạo ra phải được đưa vào biểu diễn ngôn ngữ. Thời lượng của các âm vị và các khoảng dừng trung gian phải được xác định rõ, mặc dù thông tin này không tồn tại trong văn bản gốc. Nhằm giúp tiếng nói tổng hợp nghe tự nhiên và dễ hiểu, quá trình này cũng bao gồm việc đưa các cấu trúc nhịp điệu (rhythmic structure), khuôn mẫu trọng âm (stress patterns) và gợi ý ngữ điệu (intonation) tiềm năng vào biểu diễn ngôn ngữ. Để làm được điều này, mô-đun phân tích văn bản phải diễn giải các thuộc tính cú pháp của văn bản đầu vào. Chẳng hạn, hệ thống phải phân biệt được các loại câu khác nhau như câu hỏi và câu trần thuật, đồng thời suy diễn xem từ nào nên được nhấn mạnh (focus) trong ngữ cảnh của câu.  

**Thuật toán tổng hợp (Synthesis algorithms)**

Mô-đun quan trọng thứ hai là bộ tổng hợp tiếng nói (speech synthesizer). Đầu vào của bộ tổng hợp là biểu diễn ngôn ngữ học do khối phân tích văn bản tạo ra, trong khi đầu ra là dạng sóng âm học của tiếng nói tổng hợp. Có một số hướng tiếp cận kỹ thuật tiềm năng để tạo ra dạng sóng tiếng nói từ các chỉ thị ngôn ngữ học. Về mặt lịch sử, các phương pháp như tổng hợp formant (formant synthesis) hoặc tổng hợp cấu âm (articulatory synthesis) đã từng được sử dụng (trong đó phương pháp tổng hợp cấu âm vẫn còn được dùng trong nghiên cứu tiếng nói). Tuy nhiên, các bộ tổng hợp tiếng nói thương mại hiện đại chủ yếu dựa trên một trong hai kỹ thuật thay thế: [tổng hợp ghép nối (concatenative synthesis)](Synthesis/Concatenative_speech_synthesis.md) hoặc [tổng hợp tham số thống kê (statistical parametric speech synthesis)](Synthesis/Statistical_parametric_speech_synthesis.md). Cả hai phương pháp này được mô tả chi tiết hơn trong các phần phụ tương ứng.  

![synthesis_basic_schematic](Synthesis/attachments/175517689.png)
**Hình 1:** Cấu trúc cơ bản của một hệ thống tổng hợp tiếng nói.  

Nhìn chung, các thuật toán tổng hợp tiếng nói hướng tới việc tạo ra tiếng nói đầu ra giống với tiếng nói tự nhiên nhất có thể, không có tiếng ồn và các biến dạng âm thanh (artifacts), đồng thời có độ rõ nét cao. Các đặc trưng khác có thể bao gồm khả năng sử dụng các giọng người nói khác nhau hoặc các phong cách nói khác nhau để phù hợp với các bối cảnh sử dụng và sở thích của người dùng. Trong thực tế sử dụng, độ phức tạp tính toán của hệ thống cũng có thể là một yếu tố thiết kế quan trọng. Điều này đặc biệt đúng nếu hệ thống phải hỗ trợ tạo tiếng nói trong thời gian thực và/hoặc phục vụ nhiều người dùng đồng thời. Ví dụ, việc tổng hợp tiếng nói trên một thiết bị di động tiêu chuẩn hoặc trong hệ thống giải trí của ô tô phải hoạt động dưới những ràng buộc nghiêm ngặt về độ trễ và độ phức tạp tính toán.

Chất lượng tiếng nói và độ rõ của bộ tổng hợp tiếng nói thường được đánh giá bằng các [bài kiểm tra nghe chủ quan (subjective listening tests)](content:subjectiveevaluation).  

**Tài liệu đọc thêm và tài nguyên (Further readings & material)**

Simon King - Sử dụng tổng hợp tiếng nói để mang lại cho mỗi người một giọng nói riêng (Using Speech Synthesis to give Everyone their own Voice), Đại học Edinburgh. Video Youtube ([link](https://www.youtube.com/watch?v=xzL-pxcpo-E)). *Bao gồm tổng quan về phương pháp chọn đơn vị (unit selection) và tổng hợp tham số thống kê kèm theo các phần minh họa âm thanh.*

Kim Silverman - Bài giảng về tổng hợp tiếng nói tại ICSI, Berkeley. Video Youtube ([link](https://www.youtube.com/watch?v=7mjh0PSUv0M)).

Danh sách trực tuyến các tài nguyên nhập môn về tổng hợp tiếng nói của Sai Krishna Rallabandi ([link](http://www.cs.cmu.edu/~srallaba/Learn_Synthesis/intro.html))
