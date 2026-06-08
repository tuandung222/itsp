# Delta và Delta-delta


Trong [các tác vụ nhận dạng](content:recotasks), chẳng hạn như nhận dạng âm tố (phoneme recognition) hoặc phát hiện hoạt động tiếng nói (voice activity detection), một đặc trưng đầu vào kinh điển là [hệ số cepstrum tần số mel](content:mfcc) (MFCC). Chúng mô tả hình dạng bao phổ tức thời của tín hiệu tiếng nói. Tuy nhiên, tín hiệu tiếng nói là tín hiệu biến đổi theo thời gian và luôn ở trạng thái biến động liên tục. Mặc dù trong ngôn ngữ học, ta mô tả tiếng nói như các chuỗi âm tố được nối tiếp nhau, tín hiệu âm thanh thực tế được mô tả chính xác hơn như một chuỗi các quá trình chuyển tiếp giữa các âm tố.

Nhận xét tương tự cũng áp dụng cho các đặc trưng khác của tiếng nói như [tần số cơ bản (F0)](content:f0), vốn mô tả một giá trị tức thời. Tuy nhiên, việc phân tích hình dạng tổng quát của đường F0 thường mang lại nhiều thông tin hơn so với giá trị tuyệt đối. Ví dụ, sự nhấn mạnh trong câu thường được mã hóa bằng sự tương phản cao-thấp rõ rệt trong F0, và câu hỏi trong nhiều ngôn ngữ có đường cong F0 đặc trưng dạng thấp-cao.

Một phương pháp phổ biến để trích xuất thông tin về các quá trình chuyển tiếp này là xác định sai phân bậc nhất của các đặc trưng tín hiệu, gọi là *delta* của một đặc trưng. Cụ thể, đối với đặc trưng $f_k$ tại thời điểm *k*, delta tương ứng được định nghĩa là

$$ \Delta_k = f_k - f_{k-1}. $$

Sai phân bậc hai, hay delta-delta, được định nghĩa tương ứng là

$$ \Delta\Delta_k = \Delta_k - \Delta_{k-1}. $$

Các ký hiệu viết tắt phổ biến cho delta và delta-delta lần lượt là $ \Delta $ và $ \Delta\Delta $. Các đặc trưng trong một hệ thống nhận dạng thường được bổ sung thêm các đặc trưng $ \Delta $ và $ \Delta\Delta $ để tăng gấp ba lần số lượng đặc trưng với chi phí tính toán rất nhỏ.

Một nhận xét/cách diễn giải hiển nhiên về các đặc trưng delta và delta-delta là chúng xấp xỉ đạo hàm bậc nhất và bậc hai của tín hiệu. Với tư cách là các ước lượng đạo hàm, chúng không đặc biệt chính xác, nhưng tính đơn giản có lẽ bù đắp cho điều đó. Vấn đề về độ chính xác là các bộ vi phân (differentiator) có xu hướng khuếch đại nhiễu trắng, trong khi tín hiệu mong muốn không thay đổi. Do đó, tín hiệu đầu ra nhiễu hơn tín hiệu gốc. Phép vi phân được áp dụng hai lần trong đặc trưng delta-delta nên các vấn đề về nhiễu cũng bị tích lũy.

Lưu ý rằng các đặc trưng delta là các phép biến đổi tuyến tính của các đặc trưng đầu vào, do đó nếu chúng được kết hợp với một lớp tuyến tính trong mạng nơ-ron tiếp theo, thì về nguyên tắc, hai lớp tuyến tính liên tiếp là dư thừa. Tuy nhiên, việc sử dụng đặc trưng delta vẫn có thể mang lại lợi ích trong quá trình hội tụ.

Trong mọi trường hợp, các đặc trưng delta và delta-delta là một thành phần kinh điển của các thuật toán học máy. Chúng thành công vì rất đơn giản để tính toán và thường mang lại lợi ích rõ rệt so với các đặc trưng tức thời.

  
