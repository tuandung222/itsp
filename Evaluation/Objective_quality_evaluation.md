(content:objectiveevaluation)=
# Đánh giá chất lượng khách quan

## Các bộ ước lượng khách quan cho chất lượng cảm nhận

Khi nói đến "đánh giá khách quan", ta thường đề cập đến các *bộ ước lượng chất lượng cảm nhận*, trong đó mục tiêu là dự đoán kết quả trung bình của một bài kiểm tra [nghe chủ quan](content:subjectiveevaluation) bằng một thuật toán. Nghĩa là, ta muốn máy tính lắng nghe một mẫu âm thanh và cố gắng "đoán" xem một người nghe sẽ nói gì về chất lượng của nó (trung bình).

Từ đó rõ ràng rằng [*đánh giá chủ quan*](content:subjectiveevaluation) luôn là thước đo "thực" về hiệu suất, và đánh giá khách quan là một phép xấp xỉ của nó. Theo nghĩa này, đánh giá chủ quan "tốt hơn". Có nhiều ví dụ trong đó các bộ ước lượng chất lượng khách quan cho kết quả ngược lại với sự ưa thích chủ quan {cite:p}`manocha2022audio`. Tuy nhiên, có nhiều lý do chính đáng để sử dụng đánh giá khách quan thay vì đánh giá chủ quan:

-   *Đánh giá chủ quan tốn kém*; một bài kiểm tra yêu cầu một số lượng lớn người nghe các mẫu âm thanh, vừa tốn thời gian vừa cần cơ sở hạ tầng. Đánh giá khách quan được thực hiện trên máy tính, sao cho nói chung ta có thể kiểm tra một số lượng lớn mẫu âm thanh trong thời gian ngắn.
-   *Đánh giá chủ quan có nhiễu*; ngay cả với một số lượng lớn người nghe chuyên gia, thường rất khó để đạt được kết quả hoàn toàn giống nhau trong hai bài kiểm tra liên tiếp. Đánh giá khách quan luôn cho cùng một điểm số cho cùng một đầu vào, sao cho việc kiểm tra nhất quán và đáng tin cậy. Điều này đặc biệt quan trọng đối với khả năng tái lập khoa học; một phòng thí nghiệm độc lập có thể xác minh và xác nhận kết quả của bạn, và thước đo khách quan luôn cho cùng một đầu ra. Với đánh giá chủ quan, các nhà nghiên cứu độc lập có thể nhận được kết quả khác nhau, và bạn không bao giờ có thể chắc chắn 100% sự khác biệt đến từ đâu. Có phải một trong các nhà nghiên cứu đã mắc lỗi, hay chỉ là người nghe chủ quan luôn cho kết quả hơi khác nhau?

Một số thước đo khách quan được sử dụng thường xuyên nhất bao gồm:

-   [PESQ](https://en.wikipedia.org/wiki/PESQ) có lẽ là phương pháp đánh giá khách quan được sử dụng thường xuyên nhất và được định nghĩa trong [Khuyến nghị ITU-T P.862: Đánh giá chất lượng tiếng nói cảm nhận (PESQ): Phương pháp khách quan để đánh giá chất lượng tiếng nói đầu cuối của mạng điện thoại dải hẹp và codec tiếng nói](https://www.itu.int/rec/T-REC-P.862/en) (2001) {cite}`rix2001perceptual`. Do đó đây là phương pháp đánh giá được thiết kế rõ ràng cho các ứng dụng viễn thông. Nó ước lượng điểm trung bình của bài kiểm tra P.800 ACR.
    PESQ chỉ chấp nhận đầu vào dải hẹp và *không áp dụng trực tiếp* cho các băng thông khác. Các loại suy giảm mà PESQ có thể dự đoán đáng tin cậy bao gồm
    -   Mức đầu vào tiếng nói của codec

    -   Lỗi kênh truyền dẫn

    -   Mất gói và che giấu mất gói với codec CELP

    -   Tốc độ bit nếu codec có nhiều chế độ tốc độ bit

    -   Chuyển mã (transcoding)

    -   Nhiễu môi trường ở phía gửi

    -   Ảnh hưởng của trễ biến thiên trong các bài kiểm tra chỉ nghe

    -   Biến dạng thời gian ngắn hạn của tín hiệu âm thanh

    -   Biến dạng thời gian dài hạn của tín hiệu âm thanh

    Lưu ý rằng các méo khác với những loại được liệt kê ở trên có thể cho kết quả không đáng tin cậy. Một tính năng quan trọng bị thiếu là các méo gây ra bởi xử lý phổ, chẳng hạn như nhiễu nhạc (musical noise). Cụ thể, ví dụ, sử dụng PESQ để đánh giá các phương pháp [nâng cao chất lượng tiếng nói](content:enhancement) dựa trên xử lý trong miền [STFT](stft) *có thể cho kết quả không đáng tin cậy*.
-   Đánh giá chất lượng nghe khách quan cảm nhận ([POLQA](https://en.wikipedia.org/wiki/POLQA "POLQA")) là phiên bản kế thừa của PESQ và được định nghĩa trong [Khuyến nghị ITU-T P.863: Đánh giá chất lượng nghe khách quan cảm nhận](http://www.itu.int/rec/T-REC-P.863/en) {cite}`beerends2013perceptual`. Điều quan trọng cần lưu ý là đối với hầu hết các mục đích thực tiễn, POLQA tốt hơn PESQ. Nó có phạm vi ứng dụng rộng hơn và các loại suy giảm chấp nhận được nhiều hơn, và đầu ra đáng tin cậy hơn. Tuy nhiên, từ góc độ khoa học, điều vô cùng đáng tiếc là các triển khai POLQA là sản phẩm thương mại và *đắt đỏ*, khiến việc áp dụng POLQA trong công việc khoa học thông thường trở nên bất khả thi. Ngay cả khi một nhóm cá nhân có đủ khả năng mua giấy phép POLQA, việc xác minh kết quả POLQA bởi các phòng thí nghiệm nghiên cứu độc lập chỉ khả thi nếu họ cũng mua giấy phép POLQA. Bất chấp những hạn chế của nó, PESQ do đó vẫn là tiêu chuẩn khoa học trong đánh giá khách quan tiếng nói.
-   Đánh giá chất lượng âm thanh cảm nhận ([PEAQ](https://en.wikipedia.org/wiki/PEAQ "PEAQ")) đánh giá không chỉ tiếng nói mà cả các loại mẫu âm thanh khác {cite}`thiede2000peaq`. Do đó, nó kém chính xác hơn đối với các méo đặc thù của tín hiệu tiếng nói, nhưng khái quát hóa tốt hơn sang các âm thanh khác, như nhạc và nhiễu nền. Thước đo được định nghĩa trong <a href="http://www.itu.int/rec/R-REC-BS.1387/en" rel="nofollow">Khuyến nghị ITU-R BS.1387</a>: Phương pháp đo lường khách quan chất lượng âm thanh cảm nhận (PEAQ).
-   Thước đo [độ rõ ràng khách quan ngắn hạn (STOI)](https://ieeexplore.ieee.org/document/5713237) tập trung vào mức độ *dễ hiểu* của một mẫu tiếng nói {cite}`taal2011algorithm`. Do đó nó tập trung rõ ràng vào các kịch bản chất lượng thấp hơn, nơi tiếng nói bị hỏng đến mức khó hiểu những gì được nói. Giống như tất cả các thước đo khách quan, nó không phải là ước lượng chất lượng hoàn toàn đáng tin cậy nhưng có thể hữu ích khi kết hợp với các thước đo khác. Một ưu điểm của STOI là có [sẵn triển khai](http://amtoolbox.sourceforge.net/amt-0.9.5/doc/speech/taal2011_code.php).


## Các tiêu chí hiệu suất khách quan khác

Có nhiều trường hợp các tiêu chí hiệu suất khác được sử dụng thay vì chỉ dự đoán kết quả kiểm tra nghe chủ quan. Thông thường nhất, các tiêu chí này được áp dụng khi không có người dùng tham gia, chẳng hạn như nhận dạng tiếng nói, hoặc khi ta muốn có sự đặc trưng hóa chi tiết hơn về hiệu suất so với những gì các bộ dự đoán kiểm tra nghe chủ quan cung cấp.

Một số ví dụ về các tiêu chí hiệu suất như vậy bao gồm:

-   *[Tỷ lệ lỗi từ (WER)](https://en.wikipedia.org/wiki/Word_error_rate)* được sử dụng trong nhận dạng tiếng nói để đo tỷ lệ từ được nhận dạng đúng từ một tín hiệu kiểm tra.
-   *[Tỷ lệ tín hiệu trên nhiễu (SNR)](https://en.wikipedia.org/wiki/Signal-to-noise_ratio)* được sử dụng để đo tỷ lệ giữa tín hiệu tiếng nói mong muốn và các thành phần nhiễu không mong muốn (bao gồm ví dụ nhiễu nền, méo gây ra bởi các thuật toán xử lý và truyền dẫn, cũng như các người nói cạnh tranh không mong muốn). Với phổ đầu vào sạch $X_k$ và phiên bản bị méo $\hat X_k$, SNR được định nghĩa là
    
    $$
    D_{SNR} = \frac{ \sum_{k=0}^{N-1} |X_k|^2 }{ \sum_{k=0}^{N-1} |X_k - \hat X_k|^2 }.
    $$
    
    Thông thường, SNR được trình bày theo đơn vị decibel, thu được bằng $10\log_{10} D_{SNR}$. Động lực của SNR là nó phản ánh tỷ lệ năng lượng bị méo. Bằng cách sử dụng tỷ số, ta chuẩn hóa sai số để phản ánh *độ chính xác*, thay vì năng lượng sai số.
-   *Tỷ lệ tín hiệu trên nhiễu cảm nhận (pSNR)* đo SNR trong một miền được thúc đẩy bởi cảm nhận. Về cơ bản, các méo được trọng số sao cho chúng xấp xỉ tương ứng với cảm nhận của con người. Điều này tương tự như các bộ dự đoán kiểm tra nghe chủ quan ở trên nhưng cũng hoạt động trên các đoạn tiếng nói nhỏ. Nó có thể được sử dụng để phân tích chi tiết các méo, ví dụ, phần nào của tín hiệu chứa các méo không mong muốn.
    Với các hệ số trọng số cảm nhận $w_k$, pSNR được định nghĩa là
    
    $$
    D_{pSNR} = \frac{ \sum_{k=0}^{N-1} w_k |X_k|^2 }{ \sum_{k=0}^{N-1} w_k |X_k - \hat X_k|^2 }.
    $$
    
-   *Chỉ số méo tiếng nói (SDI)* đo mức độ mà tín hiệu tiếng nói mong muốn bị méo. Trong [nâng cao chất lượng tiếng nói](content:enhancement), nó thường được sử dụng kết hợp với *hệ số khử nhiễu* (NAF), đo mức độ mà nhiễu không mong muốn được loại bỏ. Rõ ràng rằng bằng cách không làm gì, ta đạt được SDI hoàn hảo, và bằng cách đặt đầu ra về 0, ta đạt được NAF hoàn hảo. Cả hai kết quả đều thường không thỏa đáng. Do đó, thường không rõ sự cân bằng đúng giữa hai thước đo là gì.
-   Tỷ lệ nhớ lại trung bình không trọng số và có trọng số (UAR, WAR) thường được sử dụng để đo hiệu suất trong các tác vụ phân loại tiếng nói, chẳng hạn như phân loại một đoạn tiếng nói vào một trong số hữu hạn các cảm xúc có thể. UAR được định nghĩa là trung bình của các tỷ lệ nhớ lại theo lớp (tỷ lệ mẫu của lớp được nhận dạng đúng) trong khi WAR là tỷ lệ tổng thể các mẫu được nhận dạng đúng trên tất cả các lớp (đôi khi còn được gọi là *độ chính xác*). UAR thường được ưa chuộng hơn WAR trong các thí nghiệm có sự mất cân bằng lớp đáng kể trong dữ liệu kiểm tra, và khi điều quan trọng là có các hệ thống nhạy cảm với cả các lớp ít xuất hiện hơn.
-   [Đường cong đặc trưng hoạt động thu nhận (ROC)](https://en.wikipedia.org/wiki/Receiver_operating_characteristic) và các dẫn xuất của nó, chẳng hạn như [diện tích dưới đường cong (AUC)](https://en.wikipedia.org/wiki/Receiver_operating_characteristic#Area_under_the_curve) hoặc tỷ lệ lỗi bằng nhau (EER) thường được sử dụng để báo cáo hiệu suất của các hệ thống có một số loại ngưỡng phát hiện có thể thay đổi, và khi hiệu suất cho mỗi giá trị ngưỡng được đo bằng [độ chính xác và độ nhớ lại](https://en.wikipedia.org/wiki/Precision_and_recall). Ví dụ, hiệu suất của các hệ thống xác minh người nói thường được đánh giá bằng các chỉ số như vậy.
    
- [Khoảng cách phổ logarit](https://en.wikipedia.org/wiki/Log-spectral_distance) hoặc méo phổ logarit (LSD) đo sai số phổ của phổ log-biên độ $10\log_{10} P(\omega)$, trong đó $P(\omega)=|X(\omega)|^2$ là công suất (năng lượng) của phổ tín hiệu sạch $X(\omega)$. LSD khi đó được định nghĩa sử dụng phổ bị hỏng $\hat P(\omega)$ như sau

   $$
    D_{LS}
    =\sqrt {{\frac {1}{2\pi }}\int _{-\pi }^{\pi }\left[10\log _{10}{\frac {P(\omega )}{{\hat {P}}(\omega )}}\right]^{2}\,d\omega } =\sqrt {{\frac {1}{2\pi }}\int _{-\pi }^{\pi }\left[10\log _{10} {P(\omega )}-10\log _{10}{\hat {P}}(\omega )\right]^{2}\,d\omega }.
    $$

    Trong các ứng dụng thực tế, tích phân cần được thay bằng một tổng, chẳng hạn như
    
    $$
    D_{LS}
    =\sqrt {{\frac {1}{N }}\sum _{k=0}^{N-1 }\left[10\log _{10}{\frac {P_k}{{\hat {P}}_k}}\right]^{2} }, $$
    
    trong đó $N$ là số thành phần phổ. Lưu ý rằng LSD do đó tương ứng với trung bình của sai số bình phương trong miền logarit. LSD được thúc đẩy bởi thực tế rằng cảm nhận của con người về méo là xấp xỉ logarit {cite}`gray1976distance`.
    
    

## Tài liệu tham khảo

```{bibliography}
:filter: docname in docnames
```

