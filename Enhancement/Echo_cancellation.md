# Triệt tiếng vọng (Echo cancellation)

Một dạng biến dạng thường xuyên xảy ra trong các kịch bản viễn thông thoại là tiếng vọng (echo), xuất phát từ nguyên nhân điện học hoặc âm học. Tiếng vọng điện học (electric echo) xuất hiện trong các mạng tương tự (analogue) tại những điểm không tương thích trở kháng (impedance mismatch). Do phần lớn các mạng viễn thông ngày nay đã được số hóa, các tiếng vọng điện học này chủ yếu mang tính chất lịch sử và không được thảo luận thêm ở đây. Ngược lại, tiếng vọng âm học (acoustic echo) là một vấn đề ngày càng trở nên quan trọng, đặc biệt là với sự gia tăng của các dịch vụ hội nghị truyền hình (teleconferencing). Tiếng vọng âm học xuất hiện khi tiếng nói của người A được phát qua loa cho người B nghe, khiến âm thanh từ loa này bị micrô của người B thu lại và truyền ngược về cho người A. Nếu độ trễ chỉ ở mức vài mili giây, người A sẽ cảm nhận tín hiệu phản hồi này như một hiệu ứng vang (reverberation) và *không* quá khó chịu (trong một số trường hợp, hiệu ứng này—được gọi là *side-talk* hay tự âm—thậm chí còn là một tính năng hữu ích cho biết micrô đang ghi âm). Tuy nhiên, thông thường độ trễ truyền dẫn vượt quá 100 ms, khiến tín hiệu phản hồi được cảm nhận rõ ràng là một tiếng vọng, gây khó chịu và gián đoạn cuộc hội thoại. Tệ hơn nữa, đôi khi tín hiệu vọng âm học này tạo thành một vòng lặp phản hồi kín (feedback loop) và chạy tuần hoàn liên tục. Nếu tín hiệu phản hồi bị suy hao sau mỗi vòng lặp, tiếng vọng sẽ giảm dần và tắt đi; nhưng nếu vòng phản hồi khuếch đại tín hiệu, nó có thể nhanh chóng leo thang đến giới hạn vật lý của phần cứng hoặc làm hỏng loa. Do đó, việc triệt tiêu tiếng vọng là yêu cầu bắt buộc trong các hệ thống truyền thông thực tế.

May mắn thay, *triệt tiếng vọng* (echo cancellation) là một bài toán đã được nghiên cứu kỹ lưỡng với các giải pháp chuẩn hóa sẵn có. Nói một cách ngắn gọn, các phương pháp này dựa trên việc ước lượng đáp ứng xung phòng âm học (Room Impulse Response - RIR), lọc tín hiệu loa bằng RIR ước lượng để dự đoán tiếng vọng âm học, và trừ đi tiếng vọng ước lượng này khỏi tín hiệu thu được tại micrô. Mặc dù đây là một bài toán kinh điển đã có các giải pháp thương mại, nó vẫn ẩn chứa nhiều thách thức lớn, bao gồm:

-   RIR không phải là một đặc trưng dừng (stationary) và do đó phải được ước lượng trực tuyến (on-line) thông qua một *bộ lọc thích ứng* (adaptive filter). RIR thay đổi bất cứ khi nào cửa ra vào hoặc cửa sổ được đóng/mở, bàn ghế dịch chuyển, con người di chuyển trong phòng, hoặc thậm chí khi nhiệt độ phòng thay đổi. Sự thay đổi này còn diễn ra nhanh hơn đối với các thiết bị di động, cầm tay hoặc thiết bị đeo, nơi vị trí của thiết bị có thể thay đổi liên tục. Trong trường hợp xấu nhất, micrô và loa nằm trên hai thiết bị khác nhau, khiến khoảng cách âm học giữa chúng biến đổi không ngừng.
-   Khi RIR có đuôi dài (tức là phòng có thời gian âm vang dài), các bộ lọc tương ứng phải có bậc rất lớn, làm tăng đáng kể *độ phức tạp tính toán* (computational complexity). Yêu cầu thích ứng nhanh với sự thay đổi của RIR cũng đòi hỏi tài nguyên tính toán cao hơn.

Cần lưu ý rằng các phương pháp *triệt* tiếng vọng (echo *cancellation*) thực hiện phép *trừ* tiếng vọng ước lượng khỏi tín hiệu micrô. Ngược lại, các phương pháp [suy giảm nhiễu](Noise_attenuation) (noise attenuation) thực hiện phép *nhân* tín hiệu micrô với một đại lượng vô hướng dương, sao cho tín hiệu đầu ra tiệm cận với tín hiệu sạch không có tiếng vọng. Sự khác biệt cốt lõi là các phương pháp nhân thường chỉ ước lượng năng lượng/biên độ của tín hiệu, trong khi các phương pháp trừ cố gắng khớp cả biên độ lẫn pha. Ưu điểm của các phương pháp trừ là chất lượng âm thanh đầu ra thường tốt hơn, vì cơ chế loại bỏ mô phỏng chính xác quá trình vật lý tạo ra tín hiệu. Tuy nhiên, các phương pháp nhân lại tỏ ra mạnh mẽ (robust) hơn nhiều; cụ thể, nếu pha bị ước lượng sai, phương pháp trừ có thể làm *tăng* mức độ nhiễu trong tín hiệu. Trong trường hợp xấu nhất, bộ triệt tiếng vọng không khớp tốt với đáp ứng phòng có thể gây ra vòng phản hồi thảm họa, trong khi các phương pháp nhân có thể được thiết kế để không bao giờ làm tăng năng lượng nhiễu.

Các phương pháp *nén tiếng vọng* (echo suppression) tận dụng đặc điểm này để loại bỏ tiếng vọng âm học bằng phương pháp nhân, tương tự như phương pháp trừ phổ (spectral subtraction) hoặc bộ lọc Wiener được sử dụng trong suy giảm nhiễu.

## Các giải pháp triệt tiếng vọng

Như đã đề cập ở trên, bài toán mà triệt tiếng vọng cần giải quyết là loại bỏ ảnh hưởng của phòng trên đường truyền từ loa đến micrô. Điều này có nghĩa là tín hiệu phát ra từ loa sẽ đi vào hệ thống với một độ trễ nhất định và thông qua nhiều đường phản xạ khác nhau (đáp ứng xung phòng - RIR). Từ đây, chúng tôi gọi tín hiệu nhận được phát ra từ loa là tín hiệu đầu xa (far-end, $x(n)$), và tín hiệu tiếng nói hữu ích từ người dùng tại chỗ là tín hiệu đầu gần (near-end, $s(n)$). Hỗn hợp âm thanh ghi lại bởi micrô để gửi đến đầu kia của cuộc gọi được biểu diễn như sau:

$$ d(n) = s(n) + h(n)*x(n) + v(n) $$

trong đó $v(n)$ đại diện cho nhiễu cộng tính trong phòng và để đơn giản, sẽ được coi là một phần của $s(n)$. Ảnh hưởng của phòng đối với tín hiệu đầu xa được mô hình hóa bằng một bộ lọc FIR $h(n)$, tác động lên tín hiệu đầu xa thông qua phép tích chập (convolution). Ý tưởng của triệt tiếng vọng là tìm một ước lượng của bộ lọc FIR này, áp dụng nó lên tín hiệu đầu xa nhận được, rồi trừ kết quả đó khỏi tín hiệu micrô $d(n)$ để trích xuất tín hiệu đầu gần hữu ích.

Việc ước lượng mô hình đường tiếng vọng được thực hiện bằng cách *tối thiểu hóa sai số bình phương trung bình* (Minimum Mean Square Error - MMSE) giữa tín hiệu ghi được và tín hiệu đầu xa đã qua lọc ước lượng, giả định rằng không có sự hiện diện của tín hiệu đầu gần $s(n)$. Tuy nhiên, việc chỉ thực hiện một lần ước lượng cố định là bất khả thi do các yếu tố sau:

1.  Đường tiếng vọng chưa được biết trước khi bắt đầu cuộc hội thoại; ngoài ra, bất kỳ sự thay đổi nào trong đáp ứng phòng cũng có thể phá hỏng hoàn toàn kết quả ước lượng trước đó.
2.  Tính phi dừng (non-stationarity) của tín hiệu tiếng nói khiến việc ước lượng đáp ứng tiếng vọng trở thành một tác vụ cực kỳ phức tạp.
3.  Rất có thể tín hiệu đầu gần $s(n)$ xuất hiện đồng thời trong tín hiệu micrô ghi được, làm sai lệch quá trình ước lượng (hiện tượng nói đồng thời - Double-talk).

Ba vấn đề này đòi hỏi hệ thống phải liên tục giám sát chất lượng ước lượng và cập nhật bộ lọc định kỳ để phản ánh chính xác đường tiếng vọng thực tế. Vì lý do đó, phương pháp MMSE phải được tính toán lặp (iterative) khi chúng ta nhận thêm thông tin từ tín hiệu đầu xa và tín hiệu micrô. Thuật toán phổ biến nhất để tính toán bộ lọc thích ứng này là thuật toán bình phương trung bình tối thiểu (Least Mean Squares - LMS) và các biến thể của nó. Hàm mục tiêu cần tối thiểu hóa là:

$$ MMSE = \min \|d(n) - \hat{h}(n)*x(n)\|^2 $$

Mục tiêu là tìm các hệ số của bộ lọc ước lượng sao cho tối thiểu hóa phương trình trên. Thuật toán LMS lặp được sử dụng để cập nhật bộ lọc có thể chia thành ba bước đơn giản như sau:

-   Ước lượng tín hiệu tiếng vọng ghi được:

$$ \hat{y}(n) = \sum_{i} \hat{h}(n-i)*x(n-i) $$

-   Ước lượng sai số (error):

$$ e(n) = d(n) - \hat{y}(n) $$

-   Cập nhật trọng số bộ lọc (filter weights):

$$ \hat{h}_{t+1}(n) = \hat{h}_{t}(n) + \mu \cdot e^H(n) \cdot x(n) $$

Tham số $\mu$ đại diện cho tốc độ học (learning rate) của thuật toán. Giá trị $\mu$ lớn giúp thuật toán hội tụ nhanh hơn nhưng có thể dao động quanh giá trị sai số lớn hơn, trong khi tốc độ học nhỏ hơn sẽ hội tụ chậm hơn và có thể không bắt kịp các thay đổi nhanh của đường tiếng vọng. Thực tế đã chứng minh rằng tốc độ học thích ứng (adaptive learning rate) mang lại kết quả tốt nhất, và hầu hết các biến thể của phương pháp này tập trung vào việc điều chỉnh tốc độ học thích ứng theo các yêu cầu kỹ thuật cụ thể. Như có thể thấy trong phương trình thứ hai, sai số ước lượng của thuật toán thích ứng cũng chính là tín hiệu được sử dụng làm đầu ra, và trong điều kiện tối ưu, nó sẽ chứa tín hiệu đầu gần $s(n)$ cần giữ lại.

Cách tiếp cận đầu tiên này của bộ lọc thích ứng LMS được đề xuất áp dụng trên miền thời gian (time domain) của tín hiệu âm thanh. Nếu bộ lọc cập nhật trên từng mẫu của tín hiệu âm thanh nhận được, việc mô hình hóa chính xác đáp ứng xung phòng sẽ đòi hỏi một bộ lọc có độ dài lên tới vài nghìn mẫu. Việc cập nhật bộ lọc trên mỗi mẫu mới trở nên cực kỳ tốn kém về mặt tính toán, và vì lý do đó, các phương pháp khác đã được đề xuất dựa trên hướng tiếp cận này:

-   **BlockLMS:** Giảm tần suất cập nhật trong thuật toán, sao cho việc cập nhật chỉ được áp dụng sau một số lượng mẫu nhất định (theo khối). Điều này giúp giảm đáng kể độ phức tạp tính toán của thuật toán. Tuy nhiên, xử lý theo khối có thể ảnh hưởng đến khả năng hội tụ và làm giảm độ nhạy bén (sự phản hồi nhanh chóng) của thuật toán trước các thay đổi đột ngột.
-   **Bộ lọc thích ứng trên miền tần số (Frequency Domain Adaptive Filters - FDAF):** Trường hợp đơn giản nhất của FDAF là chuyển đổi tín hiệu âm thanh sang miền tần số bằng phép biến đổi Fourier thời gian ngắn (STFT), sau đó áp dụng một bộ lọc LMS độc lập trên từng thành phần tần số của tín hiệu. Cách làm này cho phép biểu diễn đường tiếng vọng với số lượng mẫu nhỏ hơn nhiều. Do thuật toán LMS có độ phức tạp bình phương theo chiều dài bộ lọc, việc sử dụng nhiều bộ lọc ngắn sẽ hiệu quả hơn nhiều so với việc chỉ sử dụng một bộ lọc dài duy nhất trên miền thời gian.

Cuối cùng, như đã đề cập ở trên, thách thức lớn nhất mà các bộ lọc thích ứng phải đối mặt trong các ứng dụng liên lạc thực tế là hiện tượng nói đồng thời (double-talk). Trong một cuộc hội thoại thông thường giữa hai người, rất có thể cả hai người sẽ nói chuyện cùng một lúc. Trong khung làm việc của triệt tiếng vọng, điều đó có nghĩa là cả tín hiệu đầu xa $y(n)$ (sau khi truyền qua phòng) và tín hiệu đầu gần $s(n)$ sẽ cùng xuất hiện trong hỗn hợp micrô. Vì bộ lọc thích ứng cố gắng tối thiểu hóa sai số giữa tín hiệu đầu xa và tín hiệu micrô ghi được, khi xảy ra nói đồng thời, bộ lọc thích ứng sẽ có xu hướng phân kỳ (diverge) và gây méo tín hiệu đầu gần thay vì làm giảm tiếng vọng phản hồi.

Để giảm thiểu ảnh hưởng của hiện tượng nói đồng thời trong quá trình thích ứng, việc phát hiện thời điểm bắt đầu nói đồng thời để tạm dừng quá trình cập nhật bộ lọc là vô cùng quan trọng. Giả định rằng bộ lọc đã hội tụ trước phân đoạn nói đồng thời, tín hiệu đầu xa vẫn sẽ bị loại bỏ hiệu quả, trong khi giọng nói của người nói đầu gần sẽ được truyền qua nguyên vẹn. Nhiều phương pháp đã được đề xuất để kiểm soát tốc độ học của bộ lọc thích ứng dựa trên việc phát hiện nói đồng thời, phần lớn dựa trên ba ý tưởng chính:

-   **Phát hiện dựa trên năng lượng (Geigel detector):**
    -   Bộ phát hiện này giả định rằng tỷ số năng lượng giữa tín hiệu đầu xa và tín hiệu micrô ghi được sẽ duy trì gần như không đổi cho đến khi có thêm giọng nói đầu gần xuất hiện. Khi tỷ số này thay đổi đáng kể, chúng ta có thể giả định rằng hiện tượng nói đồng thời đang xảy ra.
    -   Đây là một phương pháp rất đơn giản và hầu như không làm tăng thêm độ phức tạp tính toán cho hệ thống.
    -   Tuy nhiên, bộ phát hiện này giả định mức tiếng vọng khác biệt rõ rệt so với tiếng nói đầu gần. Do đó, phương pháp này chịu ảnh hưởng rất mạnh bởi nhiễu nền và sự lệch pha/lệch thời gian giữa các tín hiệu.
    -   Phương pháp này cũng yêu cầu phải tinh chỉnh tham số cho từng cấu hình phần cứng cụ thể, và những thay đổi trong đường tiếng vọng trong lúc đàm thoại có thể kích hoạt nhầm bộ phát hiện nói đồng thời.
-   **Tương quan chéo chuẩn hóa (Normalized Cross-Correlation - NCC):**
    -   Phương pháp này đo lường độ tương quan giữa tín hiệu micrô $d(n)$ và tín hiệu sai số đã xử lý $e(n)$. Khi có mặt $s(n)$ trong $d(n)$, độ tương quan giữa hai tín hiệu này sẽ cao hơn vì $s(n)$ vẫn tồn tại trong tín hiệu sai số (nếu $\hat{y}(n)$ được ước lượng chính xác).
    -   Ngược lại, nếu không có nói đồng thời (tức là $s(n)$ bằng 0) và bộ lọc đã hội tụ, $e(n)$ sẽ chỉ chứa nhiễu nền, dẫn đến độ tương quan thấp.
    -   Phương pháp này hoạt động mạnh mẽ và ít nhạy cảm với nhiễu hơn so với phương pháp so sánh mức năng lượng đơn thuần.
    -   Giá trị NCC sẽ gần bằng 1 khi xảy ra nói đồng thời và gần bằng 0 khi không xảy ra. Do đó, NCC có thể được sử dụng trực tiếp làm hệ số tỷ lệ để điều chỉnh tốc độ học $\mu$.

$$ NCC = \frac{\sigma^{2}_{ed}(n)}{\sigma_{e}(n)\sigma_{d}(n)} $$ 

$$ \sigma^{2}_{x} = E[xx^H] $$

$$ \sigma^{2}_{x}(n) = \lambda\sigma^{2}_{x}(n - 1) + (1 - \lambda)\sigma^{2}_{x} $$

$$ \mu_{NCC} = (1 - NCC)\mu_{max} $$

-   **Triệt tiếng vọng hai đường (Two-path Echo Cancellation):**
    -   Hai bộ lọc—bộ lọc nền (background filter) và bộ lọc tiền cảnh (foreground filter)—cùng xử lý tín hiệu tiếng vọng đồng thời. Bộ lọc nền liên tục thích ứng và cập nhật hệ số tại mỗi bước, trong khi bộ lọc tiền cảnh được giữ cố định. Một mô-đun điều khiển sẽ quyết định xem có hiện tượng nói đồng thời hay không và đưa ra giải pháp tối ưu là cập nhật các hệ số của bộ lọc tiền cảnh bằng các hệ số của bộ lọc nền, hoặc giữ nguyên chúng dựa trên so sánh sai số đầu ra của cả hai bộ lọc.
    -   Đây là phương pháp hoạt động mạnh mẽ và ổn định nhất trong các phương pháp được giới thiệu.
    -   Đổi lại, phương pháp này yêu cầu độ phức tạp tính toán cao hơn, vì tín hiệu đầu xa cần được lọc qua cả hai bộ lọc trước khi đưa ra quyết định về hiện tượng nói đồng thời.


Vòng phản hồi âm học trong các ứng dụng viễn thông.

![echocancellation.png](attachments/175509295.png)
  

Mô hình đường truyền tiếng vọng và bộ lọc ước lượng

![image](attachments/203126383.png)
  

Vòng phản hồi dùng trong triệt tiếng vọng

![image2](attachments/203126386.png)


Mô hình triệt tiếng vọng hai đường (Two-path)

![twopath](attachments/203126388.png)
  

  

 