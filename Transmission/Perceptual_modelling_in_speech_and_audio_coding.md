# Mô hình hóa cảm giác trong mã hóa âm thanh và tiếng nói (Perceptual modelling in speech and audio coding)


Con người thông thường là đối tượng tiếp nhận cuối cùng của tín hiệu tiếng nói trong viễn thông, do đó chất lượng của quá trình truyền dẫn nên được đánh giá dựa trên mức độ cảm thụ âm thanh thực tế của người nghe. **Mô hình cảm giác** (perceptual models) đề cập đến các phương pháp cố gắng xấp xỉ hoặc dự đoán cách đánh giá chất lượng thính giác được cảm nhận bởi người nghe. Trong các ứng dụng mã hóa, chúng ta có thể định nghĩa mô hình cảm giác là các **mô hình đánh giá**, qua đó ta xấp xỉ hóa ảnh hưởng cảm thụ của các dạng méo tiếng (distortions).

Một loại mô hình khác thường được sử dụng trong mã hóa tiếng nói là **mô hình nguồn** (source models), mô tả các đặc tính nội tại của nguồn phát - chính là tín hiệu tiếng nói. Bạn có thể hình dung mô hình nguồn thông qua ví dụ về: 1) các mô hình vật lý mô tả quá trình sinh lý tạo ra âm thanh tiếng nói, hoặc 2) phân phối xác suất của tín hiệu tiếng nói. Điểm khác biệt quan trọng là mô hình nguồn không quan tâm đến đối tượng quan sát, mà chỉ mô tả thực tế khách quan của tín hiệu. Ngược lại, mô hình cảm giác được áp dụng khi **chúng ta quan sát một cách chủ quan** tín hiệu, nhằm đánh giá các đặc tính cảm nhận của tín hiệu đó.

Trong các ứng dụng mã hóa âm thanh và tiếng nói, hầu như tất cả các méo tiếng gây ra bởi thuật toán đều xuất phát từ việc lượng tử hóa tín hiệu. Mục tiêu của mô hình hóa cảm giác khi đó là lựa chọn độ chính xác lượng tử hóa sao cho ảnh hưởng suy giảm chất lượng cảm nhận được giảm thiểu tối đa. Nói một cách đơn giản, những thành phần tín hiệu quan trọng hơn đối với tai người nghe sẽ được lượng tử hóa với độ chính xác cao hơn những thành phần ít quan trọng hơn.


## Hiện tượng che phủ tần số (Frequency masking)

Nếu chúng ta phát đồng thời hai sóng hình sin với tần số hơi khác nhau một chút, thì sóng sin lớn hơn có thể **che phủ** (mask) sóng sin thứ hai khiến cho tai người không thể nghe thấy nó nữa. Hiệu ứng này được gọi là **hiện tượng che phủ tần số** (frequency masking). Nói cách khác, con người ít nhạy cảm hơn với các âm thanh có tần số gần với các âm thanh khác lớn hơn. Cụ thể, khi lượng tử hóa một tín hiệu, chúng ta có thể sử dụng độ chính xác lượng tử hóa thấp hơn ở các vùng tần số có năng lượng cao hơn. Hiệu ứng che phủ này sẽ giảm dần khi chúng ta ở cách xa tần số che phủ đó.

Trong thực tế, các mô hình che phủ tần số tương tự như các bao phổ (spectral envelopes) về năng lượng. Nghĩa là, hình dạng của mô hình che phủ tần số tương đồng với bao phổ, nhưng là phiên bản được làm mịn và ít nhọn hơn. Các phiên bản chính xác hơn của mô hình này có thể được xây dựng dựa trên lý thuyết [âm học tâm lý (psychoacoustics)](https://en.wikipedia.org/wiki/Psychoacoustics).

Các mô hình che phủ tần số được sử dụng theo hai cách chính:

-   **Trong các bộ codec miền tần số:** Nơi biểu diễn miền tần số của tín hiệu được lượng tử hóa, chúng ta lựa chọn độ chính xác lượng tử hóa ở các vùng khác nhau của phổ dựa trên một mô hình cảm giác. Thông thường, các vùng năng lượng cao (nơi có hiệu ứng che phủ mạnh) sẽ được lượng tử hóa với độ phân giải thấp hơn so với các vùng năng lượng thấp.
-   **Trong các bộ codec miền thời gian như CELP:** Chúng ta thường sử dụng một vòng lặp phân tích bằng tổng hợp (analysis-by-synthesis), nơi các phiên bản lượng tử hóa khác nhau được tổng hợp lại, và sai số giữa tín hiệu gốc và tín hiệu lượng tử hóa được xác định bằng trọng số cảm giác. Trọng số này dựa trên mô hình che phủ tần số. Trong số các phương án lượng tử hóa khả thi, phương án có sai số có trọng số cảm giác nhỏ nhất sẽ được lựa chọn.



## Thang đo tần số (Frequency scale)

Độ nhạy cảm của thính giác con người phụ thuộc vào dải tần số nơi âm thanh xuất hiện. Chúng ta nhạy cảm hơn ở các vùng tần số "thấp" và kém nhạy cảm hơn ở các tần số "cao". Tuy nhiên, có sự khác biệt trong cách định nghĩa độ nhạy này, dẫn đến hai cách giải thích nổi bật:

-   **Trong ốc tai (cochlea) của tai người:** Âm thanh được xử lý trong các dải phổ (spectral bands) độc lập, sao cho âm thanh ở các dải khác nhau không gây nhiễu lẫn nhau, nhưng các âm thanh trong cùng một dải **sẽ gây nhiễu** lên sự cảm nhận của nhau. Hiện tượng này được gọi là hiện tượng che phủ thính giác (auditory masking). Độ rộng của các dải này phụ thuộc vào tần số và tăng lên khi tần số tăng. Khía cạnh cảm thụ này đã được xấp xỉ bằng một số mô hình, bao gồm các thang đo [Bark](https://en.wikipedia.org/wiki/Bark_scale) và [ERB](https://en.wikipedia.org/wiki/Equivalent_rectangular_bandwidth) (Equivalent Rectangular Bandwidth).
-   **Cảm nhận về khoảng cách cao độ:** Được cảm nhận khác nhau tùy thuộc vào tần số của chúng. Nói ngắn gọn, một bước thay đổi nhỏ về cao độ cảm nhận được (đo bằng tần số vật lý) ở tần số cao sẽ lớn hơn nhiều so với ở tần số thấp. Khía cạnh cảm thụ này có thể được xấp xỉ bằng [thang đo Mel (Mel scale)](https://en.wikipedia.org/wiki/Mel_scale).



## Hiện tượng che phủ thời gian (Temporal masking)

Sự biến đổi về độ chính xác và độ nhạy của cảm giác cũng diễn ra vô cùng thú vị theo thời gian. Cụ thể, một âm thanh lớn có thể làm cho một âm thanh thứ hai yếu hơn xuất hiện sau đó trở nên không thể nghe thấy. Chẳng hạn, nếu chúng ta có hai xung âm thanh nối tiếp nhau theo thời gian, trong đó xung thứ hai yếu hơn và khoảng cách thời gian giữa chúng đủ ngắn, chúng ta sẽ không thể nghe thấy xung thứ hai. Đáng ngạc nhiên là hiện tượng che phủ thời gian này cũng có thể xảy ra theo chiều ngược lại: một âm thanh lớn xuất hiện **sau** có thể che phủ một âm thanh yếu hơn xuất hiện **trước** đó.
