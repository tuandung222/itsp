# Năng lượng tín hiệu, độ to và decibel

## Năng lượng tín hiệu

Khi nói đến năng lượng tín hiệu, ta thường đề cập đến phương sai (variance) của tín hiệu, tức là độ lệch bình phương trung bình so với giá trị trung bình $
Energy(x)=var(x)=E[(x-\mu)^2], $ trong đó $ \mu=E[x] $ là giá trị trung bình của tín hiệu *x*. Phương sai là một đại lượng đo năng lượng nếu ta diễn giải *x* như độ dịch chuyển của một con lắc.



Vì biên độ của tín hiệu dao động thay đổi theo chu kỳ dao động, việc ước lượng năng lượng tức thời thường không có ý nghĩa, mà chỉ nên tính trung bình trên một [cửa sổ](Windowing) nào đó. Tuy nhiên, cần lưu ý rằng hàm cửa sổ làm giảm năng lượng trung bình (nó nhân tín hiệu với một đại lượng nhỏ hơn 1), điều này gây ra độ chệch (bias) cần được hiệu chỉnh nếu cần ước lượng năng lượng tuyệt đối. Tuy nhiên, thông thường độ chệch này nhất quán trong toàn bộ tập dữ liệu và có thể bỏ qua.

Các phương pháp ước lượng năng lượng thay thế điển hình:

-   Năng lượng có thể được tính trên các *băng tần phổ* (spectral bands), thường gọi là băng năng lượng, tức là một dải tần số trong [biến đổi thời gian-tần số](Spectrogram_and_the_STFT), chẳng hạn 0 đến 1000 Hz, 1000 Hz đến 2000 Hz, v.v. cho các băng 1 kHz. Lưu ý rằng các băng phải đủ rộng để chứa "nhiều" thành phần tần số bên trong sao cho phương sai có thể được ước lượng. Cách biểu diễn này tương đương với một mô hình bao phổ (spectral envelope) của tín hiệu.
-   Trong biểu diễn thời gian-tần số, năng lượng của một thành phần tần số đơn lẻ có thể được ước lượng theo thời gian. Nghĩa là, ta có thể lấy năng lượng trung bình của một thành phần tần số qua nhiều khung (frame) hoặc cửa sổ liên tiếp.

Lưu ý rằng vì cả hai phương pháp biểu diễn này đều được tính từ tín hiệu đã qua cửa sổ, chúng cũng bị chệch tương tự như bản thân cửa sổ.



## Decibel

Đơn vị thường dùng cho năng lượng tín hiệu là [decibel](https://en.wikipedia.org/wiki/Decibel) (dB). Công thức chuyển đổi giá trị năng lượng tín hiệu $ \sigma^2 $ sang decibel là

$$ 10\,\log_{10}\sigma^2. $$

Decibel do đó là một đại lượng đo năng lượng theo thang logarit. Một cách hiển nhiên, ta cũng có thể chuyển đổi công thức này thành $ 20\,\log_{10}\sigma $ để liên hệ *biên độ* tín hiệu (độ lệch chuẩn) $ \sigma $ với thang decibel.

Lợi ích của việc sử dụng decibel là năng lượng tín hiệu thường có dải giá trị rất rộng. Bằng cách lấy logarit, ta thu được một biểu diễn mà việc trực quan hóa trở nên dễ dàng hơn nhiều (xem hình minh họa bên phải). Hơn nữa, decibel cũng gần hơn với cảm nhận của con người về năng lượng âm thanh.

### Chuẩn hóa năng lượng, độ to, dBFS và dBov

Trong hầu hết các ứng dụng dữ liệu âm thanh, ta cần chuẩn hóa âm thanh sao cho chúng có cùng âm lượng xấp xỉ hoặc ít nhất là âm lượng đã biết. Ví dụ, hãy xem xét một chương trình truyền hình và các quảng cáo. Hầu hết mọi người sẽ cảm thấy rất khó chịu nếu quảng cáo to hơn nhiều so với chương trình chính (xem thêm [loudness wars](https://en.wikipedia.org/wiki/Loudness_war)). Do đó, ta cần chuẩn hóa quảng cáo cho phù hợp với âm lượng của chương trình chính. Chuẩn hóa năng lượng trung bình của quảng cáo cho khớp với chương trình chính là một cách thô sơ để làm điều đó. Tuy nhiên, cần lưu ý rằng cảm nhận năng lượng khác nhau ở các dải tần số khác nhau, nên năng lượng và độ to cảm nhận không phải là cùng một thứ. Để đo độ to, ta cần mô hình hóa cảm nhận chủ quan. Đây là một chủ đề phức tạp và không được thảo luận thêm ở đây. Tuy nhiên, các ứng dụng thực tế vẫn cần một số chuẩn hóa để tránh các vấn đề cơ bản như cắt xén (clipping).

Các đại lượng năng lượng decibel-to-overload (*dBov*) và decibel-to-full-scale (*dBFS*) liên quan đến dải động (dynamic range) của định dạng lưu trữ hoặc truyền dẫn tín hiệu. Giả sử biên độ cực đại mà một biểu diễn số có thể thể hiện là $x_{ov}$. Nếu ta cố biểu diễn một biên độ lớn hơn, tín hiệu sẽ bị cắt xén (méo). dBov đo lượng tín hiệu nằm dưới biên độ cực đại (cách bao xa so với cắt xén). Giả sử $P_{0}$ là năng lượng của sóng vuông biên độ cực đại. Khi đó, dBov của một tín hiệu có năng lượng $P$ được định nghĩa là

$$ L_{\text{ov}}=10\,\log _{10}\left({\frac
{P}{P_{0}}}\right). $$

Vì năng lượng của một sóng sin có biên độ cực đại bằng $\sqrt{\frac12}$ so với sóng vuông biên độ cực đại, nên dBov của nó là $-3.01$. Lưu ý rằng giá trị dBov luôn âm. dBFS là một đại lượng tương tự.

Trong các trường hợp điển hình, tín hiệu tiếng nói đầu vào được chuẩn hóa về $-26$ dBov để việc xử lý tín hiệu ở mức vừa phải ít có khả năng gây ra cắt xén.

  
![spectrum](attachments/149885445.png)
![db spectrum](attachments/149885446.png)

Năng lượng (công suất) của phổ tín hiệu tiếng nói (hình trên) và logarit của nó trên thang decibel (hình dưới).








