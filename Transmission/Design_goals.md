# Mục tiêu thiết kế (Design goals)


Nói ngắn gọn, mục tiêu của các phương pháp mã hóa tiếng nói (speech coding) chủ yếu là cho phép giao tiếp bằng giọng nói một cách tự nhiên và hiệu quả qua khoảng cách địa lý, dưới các ràng buộc về tài nguyên sẵn có. Nói cách khác, chúng ta muốn có thể trò chuyện với một người ở xa thông qua sự hỗ trợ của công nghệ. Thông thường, "khoảng cách" đề cập đến vị trí địa lý, nhưng mã hóa tiếng nói cũng có thể (và thường xuyên) được sử dụng để lưu trữ tín hiệu tiếng nói (trong trường hợp này, khoảng cách được hiểu là khoảng cách về mặt *thời gian*). {cite:p}`backstrom2017speech`

Cụ thể, các khía cạnh chất lượng có thể đưa vào mục tiêu thiết kế bao gồm:

-   **Chất lượng âm học (Acoustic quality):** Tín hiệu âm học được tái tạo phải tương đồng với tín hiệu gốc (được đo bằng tỷ lệ tín hiệu trên nhiễu SNR hoặc một biến thể có trọng số cảm nhận của nó).
-   **Tính trong suốt cảm giác (Perceptual transparency):** Đặc tính của các hệ thống mã hóa độ chính xác cao, nơi người nghe không thể nhận ra sự khác biệt giữa tín hiệu gốc và tín hiệu tái tạo. Khi thảo luận về tính trong suốt, chúng ta cần xác định chính xác phương pháp luận đo lường. Cụ thể, nếu so sánh các đoạn nhỏ của tín hiệu gốc và tái tạo, ta có thể dễ dàng nghe thấy những khác biệt cực nhỏ vốn không bao giờ được nhận thấy trong các tình huống sử dụng thực tế.
-   **Độ rõ (Intelligibility):** Người nghe có thể hiểu được ý nghĩa ngôn ngữ học của tín hiệu được tái tạo.
-   **Độ trễ (Delay):** Độ trễ từ đầu-đến-đầu (end-to-end) trên đường truyền thông tin phải nằm trong giới hạn hợp lý (ví dụ: dưới 150 ms). Độ trễ cao hơn có thể làm giảm tính tự nhiên của cuộc hội thoại.
-   **Độ ồn (Noisiness):** Tiếng ồn gây ra bởi việc lượng tử hóa độ chính xác thấp và tiếng ồn nền cần được giảm thiểu.
-   **Sự méo tiếng (Distortions):** Mức độ tín hiệu tiếng nói gốc bị cảm nhận là đã thay đổi do quá trình xử lý. 
-   **Độ tự nhiên (Naturalness):** Mức độ âm thanh của tín hiệu tiếng nói nghe tự nhiên hay phi tự nhiên. Ví dụ, một số giọng nói phi tự nhiên có thể nghe giống robot, giọng kim (metallic) hoặc bị nghẹt (muffled).
-   **Nỗ lực nghe (Listening effort):** Nỗ lực mà người nghe cần bỏ ra khi sử dụng hệ thống viễn thông, và nỗ lực này cần được giảm thiểu tối đa. Nỗ lực này có thể xuất phát từ giao diện người dùng, nhưng quan trọng hơn là từ việc người nghe phải cố gắng để hiểu một tín hiệu tiếng nói bị méo.
-   **Sự khó chịu (Annoyance):** Liên quan chặt chẽ đến nỗ lực nghe, ở chỗ một tín hiệu dù có thể hiểu được nhưng lại bị méo, ồn hoặc trễ truyền dẫn quá mức đến mức gây khó chịu thực sự. Sự khó chịu thường làm tăng nỗ lực nghe của người dùng.
-   **Cảm nhận về khoảng cách (Perception of distance):** Cảm giác của người tham gia về khoảng cách giữa các bên đối thoại. Yếu tố chính đóng góp vào cảm nhận này có lẽ là âm học phòng (room acoustics), nghĩa là nếu khoảng cách giữa người nói và micrô lớn, người nghe sẽ có cảm giác người nói đang ở rất xa. Điều này ảnh hưởng trực tiếp đến sự thân mật của cuộc hội thoại (rất khó để cảm thấy thân mật nếu khoảng cách cảm nhận được quá lớn).

Rõ ràng là các khía cạnh chất lượng khác nhau sẽ nổi bật ở các mức độ chính xác mã hóa khác nhau, vốn là một hàm của tốc độ bit (bitrate) sẵn có (băng thông). Như một mô tả khái quát, với công nghệ hiện nay, các vấn đề chất lượng mà chúng ta tối ưu hóa ở các tốc độ bit khác nhau là:

-   **Ở tốc độ bit cực thấp (dưới 1 kbps):** Chúng ta không thể kỳ vọng mã hóa tiếng nói với chất lượng cao. Mục tiêu tốt nhất có thể hy vọng là duy trì **độ rõ** (intelligibility). 
-   **Ở tốc độ bit rất thấp (1-2 kbps):** Độ rõ thường có thể được bảo toàn, nhưng tín hiệu tiếng nói vẫn có thể bị méo và ồn, do đó chúng ta muốn giảm thiểu **nỗ lực nghe**.
-   **Ở tốc độ bit thấp (3-8 kbps):** Chúng ta thường phải cân bằng giữa việc tránh tiếng ồn, méo tiếng, sự khó chịu và duy trì độ tự nhiên. Nói cách khác, chúng ta có thể giảm tiếng ồn bằng cách làm cho tín hiệu bị nghẹt hơn, nhưng điều đó lại làm giảm độ tự nhiên. Việc lựa chọn sự cân bằng nào là tốt nhất lúc này phụ thuộc nhiều vào cảm nhận cá nhân.
-   **Ở tốc độ bit trung bình (8-16 kbps):** Tín hiệu tiếng nói đã có thể được mã hóa với chất lượng cao, do đó chúng ta có thể cố gắng giảm thiểu số lượng méo tiếng có thể nghe thấy (cảm nhận được). Ở các tốc độ này, các hệ thống viễn thông trong thực tế thường đạt độ trong suốt cảm giác ở mức người dùng không nhận biết được bất kỳ méo tiếng nào, ngay cả khi họ có thể nhận ra sự khác biệt nếu so sánh trực tiếp với tín hiệu gốc.
-   **Ở tốc độ bit cao (trên 16 kbps):** Về tổng thể, ta có thể mã hóa tín hiệu tiếng nói ở mức trong suốt cảm giác. Tuy nhiên, nếu tài nguyên tính toán bị hạn chế hoặc trong các ứng dụng yêu cầu độ trễ cực thấp, méo tiếng vẫn có thể nghe thấy được.

Hiệu suất của một bộ codec tuy vậy luôn là sự thỏa hiệp giữa chất lượng và tài nguyên. Bằng cách tăng tài nguyên tính toán (hoặc băng thông), chúng ta có thể nâng cao chất lượng đến vô hạn. Các tài nguyên bị giới hạn quan trọng nhất bao gồm:

-   **Băng thông (Bandwidth):** Tức là tốc độ bit truyền dữ liệu. Nó bị giới hạn bởi:
    -   Các ràng buộc vật lý như băng tần vô tuyến sẵn có,
    -   Mức tiêu thụ điện năng (pin, khía cạnh sinh thái và giá cả),
    -   Dung lượng cơ sở hạ tầng (vốn đầu tư, độ phức tạp, năng lượng).
-   **CPU:** Tức là số lượng phép tính trên giây có thể thực hiện được, bị giới hạn thêm bởi:
    -   Chi phí đầu tư,
    -   Mức tiêu thụ điện năng (pin, khía cạnh sinh thái và giá cả).
-   **Bộ nhớ (Memory):** Tức là lượng RAM và ROM cần thiết cho hệ thống, bị giới hạn bởi:
    -   Chi phí đầu tư,
    -   Mức tiêu thụ điện năng (pin, khía cạnh sinh thái và giá cả).

Hơn nữa, kịch bản sử dụng (use-case) của bộ codec tiếng nói (và âm thanh) dự kiến cũng có nhiều ảnh hưởng quan trọng đến thiết kế tổng thể. Ví dụ, cấu hình hệ thống có thể là một trong những dạng sau:

-   **Một-đối-một (One-to-one):** Cuộc hội thoại điện thoại cổ điển, nơi hai thiết bị truyền tiếng nói qua lại giữa chúng. 
-   **Một-đối-nhiều (One-to-many):** Có thể áp dụng trong phát thanh truyền hình hoặc lưu trữ dữ liệu, nơi chúng ta chỉ mã hóa một lần nhưng có thể có nhiều bộ thu/giải mã nhận được. Vì có nhiều bộ nhận, chúng ta mong muốn việc giải mã tín hiệu không tiêu tốn nhiều tài nguyên. Trong thực tế, điều này có nghĩa là phía gửi (bộ mã hóa - encoder) có thể sử dụng tỷ lệ tài nguyên lớn hơn.
-   **Nhiều-đối-nhiều (Many-to-many):** Ứng dụng hội nghị trực tuyến điển hình, thường được triển khai thông qua một máy chủ đám mây, giúp trộn giọng nói của các cá nhân tập trung để tiết kiệm băng thông truyền đến nhiều người nhận.
-   **Nhiều-đối-một (Many-to-one):** Kịch bản mảng cảm biến phân tán, nơi nhiều thiết bị trong phòng cùng ghi âm tiếng nói. Vì có nhiều bộ mã hóa, chúng phải rất đơn giản, và phần lớn trí tuệ nhân tạo cũng như tài nguyên tính toán sẽ được tập trung ở đầu thu (giải mã).

Thiết kế tổng thể cũng chịu ảnh hưởng bởi loại liên kết truyền dẫn. Cụ thể, một vài thế hệ điện thoại di động kỹ thuật số đầu tiên hoạt động trên mạng chuyển mạch kênh ([circuited-switched](https://en.wikipedia.org/wiki/Circuit_switching)), nơi một lượng băng thông cố định được phân bổ cho mỗi kết nối. Tuy nhiên, các mạng mới hơn dựa trên thiết kế chuyển mạch gói ([packet-switched](https://en.wikipedia.org/wiki/Packet_switching)), nơi dữ liệu được truyền qua Internet và dung lượng cũng như định tuyến được tối ưu hóa liên tục. Mạng chuyển mạch gói trong thực tế có thể tối ưu tốt hơn nhiều về chi phí tổng thể và hiệu suất. Tuy nhiên, mạng chuyển mạch gói không thể đảm bảo một luồng gói tin ổn định, khiến đầu thu phải chấp nhận cả các gói tin bị trễ hoặc bị mất cũng như các gói tin đến không đúng thứ tự. Rõ ràng điều này ảnh hưởng đến cả độ trễ truyền dẫn tổng thể của hệ thống, đồng thời làm tăng độ phức tạp tính toán của đầu thu. Tuy nhiên, các chi phí này thường được bù đắp bởi lợi ích tiết kiệm có được từ việc tối ưu hóa mạng.

Một khía cạnh quan trọng khác là các giả định về việc mất gói tin nói chung. Trong nhiều ứng dụng lưu trữ và phát sóng, chúng ta có thể giả định rằng các gói tin không bị mất và tất cả dữ liệu đều sẵn có ở đầu thu. Tuy nhiên, việc giả định một tỷ lệ mất gói tin nhất định là phổ biến hơn nhiều. Một trong những hệ quả quan trọng nhất của việc mất gói tin đối với thiết kế là khi giải mã tín hiệu, chúng ta không thể giả định mình có quyền truy cập vào các gói tin trước đó. Cụ thể, nếu việc giải mã gói tin hiện tại phụ thuộc vào gói tin trước đó, thì một gói tin bị mất sẽ khiến chúng ta không thể giải mã bất kỳ gói tin tiếp theo nào. Rõ ràng, sự nhạy cảm với việc mất gói tin như vậy là không thể chấp nhận được trong hầu hết các hệ thống truyền dẫn thực tế. Tuy nhiên, chúng ta có thể mã hóa tiếng nói với hiệu suất cao hơn nhiều nếu được phép sử dụng các gói tin trước đó để dự báo gói tin hiện tại. Do đó, khả năng mất gói tin sẽ quyết định sự thỏa hiệp giữa độ nhạy cảm với việc mất gói và hiệu suất mã hóa (nén) dữ liệu.


## Tài liệu tham khảo
```{bibliography}
:filter: docname in docnames
```