(content:CELP)=
# Dự báo tuyến tính kích thích bằng mã (Code-excited linear prediction - CELP)


Mô hình (paradigm) mã hóa tiếng nói nổi tiếng nhất là **Dự báo tuyến tính kích thích bằng mã** (Code-Excited Linear Prediction - CELP). Được phát minh lần đầu tiên vào năm 1985, CELP là nền tảng của tất cả các bộ codec chuyên dụng cho tiếng nói phổ biến hiện nay. Biến thể nổi bật nhất của nó là CELP đại số (Algebraic CELP - ACELP), sử dụng một bảng mã đại số (algebraic codebook) để mã hóa phần dư nhiễu (noise residual). Các bộ codec như [AMR](https://en.wikipedia.org/wiki/Adaptive_Multi-Rate_audio_codec), [EVS](https://en.wikipedia.org/wiki/Enhanced_Voice_Services), [G.718](https://en.wikipedia.org/wiki/G.718) và [Speex](https://en.wikipedia.org/wiki/Speex) (đã được thay thế bởi [Opus](https://en.wikipedia.org/wiki/Opus_(audio_format))) đều dựa trên các biến thể của CELP.


Tổng quan về mặt lý thuyết, CELP dựa trên mô hình nguồn-bộ lọc (source-filter model) của tiếng nói, trong đó [dự báo tuyến tính (linear prediction)](content:linearprediction) được sử dụng để mô hình hóa hiệu ứng lọc của đường dẫn tiếng (vocal tract - và các hiệu ứng khác), và bộ lọc này được kích thích (excited) bởi nguồn tiếng nói, cụ thể là nguồn thanh môn (glottal source) và nhiễu hỗn loạn (turbulent noise). Thông thường, mô hình [cao độ hoặc tần số cơ bản (F0)](content:f0) là một bộ lọc dự báo dài hạn (Long-Term Prediction - LTP), thực chất là một bộ dự báo tuyến tính với độ trễ lớn. Để mô hình hóa nhiễu, các bộ codec CELP thường sử dụng một [bảng mã vector (vector codebook)](content:vq). Thành phần đóng góp của bảng mã thường được tối ưu hóa bằng phương pháp **phân tích bằng tổng hợp** (analysis-by-synthesis), trong đó đầu ra của các phép lượng tử hóa khác nhau được tổng hợp lại, và các đầu ra tổng hợp này được đánh giá để chọn ra phép lượng tử hóa tối ưu nhất. Quá trình đánh giá này sử dụng [mô hình hóa cảm giác](Perceptual_modelling_in_speech_and_audio_coding) để chất lượng cảm thụ chủ quan có thể được so sánh trực tiếp.

Vì phần [dự báo tuyến tính](content:linearprediction) và [mô hình hóa tần số cơ bản](content:f0) đã được mô tả chi tiết ở các phần khác, dưới đây chúng ta sẽ chỉ thảo luận về cấu trúc tổng thể của bộ mã hóa/giải mã, đánh giá chất lượng cảm thụ, mô hình hóa nhiễu và phân tích bằng tổng hợp.



![celp.png](attachments/175511854.png) 
*Mô hình nguồn-bộ lọc của các bộ codec CELP.*


## Cấu trúc bộ mã hóa/giải mã

Bộ giải mã (decoder, xem hình bên dưới) triển khai rất sát ý tưởng của mô hình nguồn-bộ lọc (đã nêu ở trên). Điểm tinh chỉnh duy nhất là hai phép nhân với các độ lợi vô hướng (scalar gains), trong đó thành phần đóng góp của bảng mã nhiễu (noise codebook) và cao độ (pitch) được nhân tỷ lệ (scaled) về độ lớn mong muốn. Lưu ý rằng ở đây chúng ta viết tắt mã hóa dự báo tuyến tính là LPC (Linear Predictive Coding).

Bộ mã hóa và giải mã thông thường hoạt động trên các khung hình (frames) có độ dài 20 ms, các khung này lại được chia nhỏ thành các phân khung (subframes) có độ dài 5 ms. Các thao tác mô tả ở trên do đó được thực hiện trên các vector có độ dài tương ứng với 5 ms, ở tần số lấy mẫu 12.8 kHz tương ứng với 64 mẫu.

Bộ mã hóa (encoder, xem hình bên dưới) trước tiên ước lượng mô hình dự báo tuyến tính (LPC), sau đó loại bỏ ảnh hưởng của nó khỏi tín hiệu đầu vào. Nói cách khác, vì dự báo tuyến tính là bộ lọc phản hồi vô hạn (IIR - Infinite Impulse Response), chúng ta có thể loại bỏ ảnh hưởng này khỏi tín hiệu tiếng nói bằng một bộ lọc phản hồi hữu hạn (FIR - Finite Impulse Response) tương ứng để thu được phần dư LPC (LPC-residual). Sau đó, chúng ta có thể ước lượng tần số cơ bản (F0) tương tự từ phần dư này và tiếp tục loại bỏ ảnh hưởng của nó để thu được phần dư F0 (F0-residual).

Phần dư F0 có dạng rất gần với nhiễu trắng (tuân theo phân phối Laplace - Laplacian distribution). Do đó, chúng ta có thể lượng tử hóa nó bằng một bộ lượng tử hóa nhiễu (mô tả bên dưới) cũng như lượng tử hóa các độ lợi cao độ và độ lợi nhiễu. Để đánh giá chất lượng đầu ra của tín hiệu, chúng ta tiến hành giải mã tín hiệu đã lượng tử hóa và tính toán sai số có trọng số cảm thụ (perceptually weighted error). Tuy nhiên, vì quá trình lọc LPC là tự hồi quy (IIR), nó có ảnh hưởng phi tuyến tính lên đầu ra, khiến cho việc lượng tử hóa cũng có tác động phi tuyến tính lên đầu ra. Do đó, chúng ta không thể biết phép lượng tử hóa nào là tốt nhất nếu không thử nghiệm **tất cả các trường hợp khả thi**. Để đạt hiệu suất tốt nhất có thể, về mặt lý thuyết, chúng ta phải thử mọi phép lượng tử hóa! Tuy nhiên, trong thực tế, chúng ta chọn ra một nhóm các phép lượng tử hóa có khả năng tốt và tìm ra phương án tối ưu nhất trong số đó. Đây được gọi là phương pháp **phân tích bằng tổng hợp** (analysis-by-synthesis).

Phân tích bằng tổng hợp là một phương pháp rất nổi tiếng vì nó cho phép tối ưu hóa CELP và đạt được chất lượng tương đối cao. Vì CELP được chứng minh là hiệu quả hơn các bộ codec miền tần số đối thủ, và phân tích bằng tổng hợp giúp tối ưu hóa CELP, nên phương pháp này cực kỳ quan trọng. Dẫu vậy, hãy lưu ý rằng đây là một phương pháp vét cạn (brute-force), vốn dĩ phải trả giá bằng độ phức tạp tính toán rất cao.


![celp2.png](attachments/175511877.png)

*Cấu trúc bộ giải mã CELP*

  

![celp3.png](attachments/175511879.png)
*Cấu trúc bộ mã hóa CELP*

## Đánh giá chất lượng cảm thụ

Chất lượng cảm thụ trong các bộ codec CELP được đánh giá bằng một chuẩn có trọng số (weighted norm). Giả sử $W$ là ma trận chập tương ứng với bộ lọc trọng số cảm nhận, thì chuẩn có trọng số giữa tín hiệu phần dư thực tế $x$ và tín hiệu phần dư đã lượng tử hóa $\hat x$ tương ứng được tính bằng:

$$ d_W(x,\hat x):=\left\| W(x-\hat x)\right\|^2 = (x-\hat x)^T W^T W (x-\hat x). $$

Mặc dù đây là một dạng toàn phương (quadratic form) có việc cực tiểu hóa khá đơn giản, nhưng cần lưu ý rằng chúng ta đang xét các vector đã lượng tử hóa, do đó việc cực tiểu hóa trở thành một bài toán tối ưu hóa số nguyên, vốn không có nghiệm giải tích (analytic solution).

Hơn nữa, tín hiệu lượng tử hóa là tổng của các thành phần đóng góp từ cao độ và nhiễu, cả hai đều được nhân với các hệ số tỷ lệ tương ứng:

$$ \hat x := \gamma_{F0} x_{F0} + \gamma_{noise} x_{noise}. $$

Khi ước lượng F0, chúng ta có thể đặt thành phần đóng góp của nhiễu bằng không, từ đó cực tiểu hóa biểu thức:

$$ \arg\min_{x_{F0}}\, d_W(x,\gamma_{F0}x_{F0}):= \arg\min_{x_{F0}}(x-\gamma_{F0}x_{F0})^T W^T W (x-\gamma_{F0}x_{F0}). $$

Để so sánh các thành phần đóng góp cao độ khác nhau, chúng ta cần loại bỏ độ lợi ra khỏi bài toán, điều này được thực hiện bằng cách cho đạo hàm theo $\gamma_{F0}$ bằng không (xem như bài tập tự giải), qua đó thu được độ lợi tối ưu là:

$$ \gamma_{F0}^* = \frac{x^TW^T W x_{F0}}{x_{F0}^TW^T W x_{F0}}. $$

Thay ngược lại bài toán gốc, sau khi loại bỏ các hằng số, ta thu được:

$$ x_{F0}^*:=\arg\min_{x_{F0}}\, d_W(x,\gamma_{F0}^*x_{F0}):= \arg\max_{x_{F0}} \frac{\left(x^TW^T W x_{F0}\right)^2}{x_{F0}^TW^T W x_{F0}}. $$

Có thể thấy phương trình này đánh giá mối tương quan có trọng số giữa tín hiệu gốc $x$ và thành phần đóng góp cao độ. Nói cách cách khác, các giá trị F0 khác nhau có thể được đánh giá bằng hàm số này, và giá trị nào cho độ tương quan cao nhất sẽ được chọn làm F0.

Một khi F0 đã được chọn, chúng ta tính toán độ lợi tối ưu và trừ nó khỏi tín hiệu phần dư gốc, thu được $x':=x - \gamma_{F0}^* x_{F0}^*$. Phần dư F0 này sau đó xấp xỉ nhiễu trắng và có thể được mô hình hóa bằng bảng mã nhiễu. Tương tự như trên, chúng ta giả định độ lợi nhiễu là tối ưu để bảng mã nhiễu có thể được tối ưu hóa bằng:

$$ \gamma_{noise}^* = \frac{x^TW^T W x_{noise}}{x_{noise}^TW^T W x_{noise}} $$

và

$$ x_{noise}^*:=\arg\min_{x_{noise}}\, d_W(x',\gamma_{noise}^*x_{noise}):= \arg\max_{x_{noise}} \frac{\left(x^TW^T W x_{noise}\right)^2}{x_{noise}^TW^T W x_{noise}}. $$

Như vậy, chúng ta đã lượng tử hóa các thành phần đóng góp của cao độ và nhiễu, nhưng đối với hai độ lợi, chúng ta chỉ có các giá trị tối ưu liên tục chứ chưa phải là các giá trị *lượng tử hóa* tối ưu. Một lần nữa, vì các giá trị lượng tử hóa không liên tục, chúng ta không có nghiệm giải tích mà phải tìm kiếm lượng tử hóa tốt nhất trong tất cả các giá trị khả thi. Bài toán tối ưu hóa lúc này là:

$$ \arg\min_{\gamma_{F0},\gamma_{noise}}\, d_W(x,\gamma_{F0}x_{F0}^* + \gamma_{noise}x_{noise}^*) = \arg\min_{\gamma_{F0},\gamma_{noise}}\,(x-\gamma_{F0}x_{F0}^* - \gamma_{noise}x_{noise}^*)^T W^T W (x-\gamma_{F0}x_{F0}^* - \gamma_{noise}x_{noise}^*). $$

Chúng ta nhận thấy phương trình trên là một đa thức của hai độ lợi vô hướng, và tất cả các thành phần vector và ma trận đều được rút gọn thành hằng số, sao cho:

$$ \arg\min_{\gamma_{F0},\gamma_{noise}}\, d_W(x,\gamma_{F0}x_{F0}^* + \gamma_{noise}x_{noise}^*) = c_0 + \gamma_{F0}c_1 + \gamma_{F0}^2c_2 +\gamma_{noise}c_3 + \gamma_{noise}\gamma_{F0}c_4 + \gamma_{noise}^2c_5. $$

Khác với việc tối ưu hóa các vector phần dư, việc tối ưu hóa này tương đối đơn giản về mặt tính toán, cho phép chúng ta tìm kiếm vét cạn để chọn ra độ lợi tốt nhất. Các độ lợi thường được lượng tử hóa với 8 đến 10 bit, nghĩa là chỉ cần thực hiện 256 đến 1024 phép đánh giá đa thức.  
Phần dư lượng tử hóa cuối cùng sẽ là:

$$ \hat x^* = \gamma_{F0}^* x_{F0}^* + \gamma_{noise}^* x_{noise}^*. $$



## Mô hình hóa nhiễu và mã hóa đại số

Như đã đề cập ở trên, phần dư sau khi lọc LPC và mô hình hóa F0 xấp xỉ là nhiễu trắng dừng (stationary white noise), tức là có phương sai không đổi và các mẫu không tương quan với nhau. Chúng ta mong muốn lượng tử hóa tín hiệu này một cách hiệu quả. Tuy nhiên, các tín hiệu nhiễu trắng không còn bất kỳ cấu trúc nào khác ngoài phân phối xác suất của chúng. Chúng ta có thể giả định rằng các mẫu phần dư $\xi_k$ tuân theo phân phối Laplace với kỳ vọng bằng không:

$$ f(\xi_k)= C \exp\left(-\frac{\|\xi_k\|}{s}\right). $$

Hàm log-likelihood đồng thời là:

$$ \log\prod_k f(\xi_k)= C'- \sum_k\frac{\|\xi_k\|}{s} = C' - \frac1s \|x_{noise}\|_1, $$

trong đó $x_{noise}:= [\xi_1,\dotsc,\,\xi_{K}]$ và $\|x\|_1$ là chuẩn 1 (1-norm, tức là tổng giá trị tuyệt đối). Nói cách khác, việc mô hình hóa các vector có xác suất không đổi $x_{noise}$ tương đương với việc mô hình hóa các vector có chuẩn 1 không đổi: $\|x_{noise}\|_1=\text{constant}$. Do đó, chúng ta có thể xây dựng một bảng mã có chuẩn 1 không đổi. Ví dụ, nếu lượng tử hóa thành các giá trị nguyên, thì tổng tuyệt đối của tín hiệu lượng tử hóa sẽ là một số nguyên cố định.

Trong trường hợp đơn giản nhất, chúng ta có thể lượng tử hóa $x_{noise}$ để chỉ có một xung có dấu (signed pulse) tại vị trí $k$, và tất cả các mẫu khác bằng không. Vị trí của xung có thể được mã hóa bằng $\log_2 K$ bit, và dấu của xung được mã hóa bằng 1 bit, sao cho tổng lượng bit tiêu thụ là $1+\log_2 K$ bit. Chiến lược mã hóa này có thể dễ dàng mở rộng bằng cách thêm nhiều xung hơn. Tuy nhiên, lượng bit tiêu thụ của các vector đa xung (multi-pulse) sẽ trở nên phức tạp hơn. Vấn đề là nếu áp dụng cách mã hóa ngây thơ (naive encoding) bằng việc mã hóa trực tiếp vị trí và dấu của từng xung, chúng ta sẽ tiêu tốn nhiều bit hơn cần thiết vì hai lý do. Thứ nhất, nếu hai xung chồng lấp nhau, chúng bắt buộc phải có cùng dấu, nếu không chúng sẽ triệt tiêu lẫn nhau. Thứ hai, các xung là không thể phân biệt được, nghĩa là thứ tự của chúng không quan trọng. Nếu mã hóa từng xung một, việc thay đổi thứ tự của chúng sẽ tạo ra các luồng bit khác nhau mặc dù tín hiệu được mã hóa vẫn giữ nguyên. Cả hai điều này đều ngụ ý rằng chúng ta đang lãng phí bit khi sử dụng phương pháp mã hóa như vậy cho nhiều xung. Đã có các giải pháp mã hóa tối ưu cho các vector đa xung này, nhưng thuật toán thực hiện sẽ phức tạp hơn.

Trong mọi trường hợp, kết quả là chúng ta có thể tạo ra các phép lượng tử hóa của tín hiệu phần dư bằng các phương pháp thuật toán. Nghĩa là chúng ta có một thuật toán hoặc một quy tắc đại số để xác định mọi phép lượng tử hóa khả thi, và do đó, phép lượng tử hóa như vậy được gọi là **mã hóa đại số** (algebraic coding). Cách mã hóa này có thể được chọn để có lượng bit tiêu thụ tối ưu cho một số lượng xung cho trước, và do đó (với các giả định lỏng lẻo) đây là phép lượng tử hóa tốt nhất có thể cho vector phần dư khi sử dụng tốc độ bit cố định. Nó rất hiệu quả về mặt tính toán vì các vector phần dư chứa hầu hết các số không, giúp cho việc đánh giá hàm tối ưu hóa diễn ra nhanh chóng. Nó cũng hiệu quả ở chỗ các bảng mã không cần phải lưu trữ sẵn mà có thể được tạo trực tiếp khi chạy chương trình (on the fly) bởi thuật toán.

Mã hóa đại số đóng vai trò trung tâm trong các bộ codec CELP đến mức các bộ codec CELP sử dụng mã hóa đại số được gọi là CELP đại số (Algebraic CELP - ACELP). Hầu hết các bộ codec phổ biến hiện nay như AMR, EVS và USAC đều sử dụng ACELP. Một số bộ codec có thể sử dụng thêm các bảng mã phần dư khác, nhưng ngay cả khi đó, mã đại số vẫn luôn là lựa chọn hàng đầu.
