# Tổng hợp tiếng nói ghép nối (Concatenative speech synthesis)

*Tổng hợp tiếng nói ghép nối* (Concatenative Speech Synthesis - CSS), hay còn gọi là *tổng hợp tiếng nói chọn đơn vị* (unit selection speech synthesis), là một trong hai kỹ thuật tổng hợp tiếng nói hiện đại chính cùng với [tổng hợp tiếng nói tham số thống kê (statistical parametric speech synthesis)](Statistical_parametric_speech_synthesis). Đúng như tên gọi của nó, CSS dựa trên việc ghép nối các đoạn tiếng nói đã được ghi âm sẵn để tạo ra tiếng nói tổng hợp chất lượng cao và rõ ràng. Ưu điểm của hướng tiếp cận này là độ tự nhiên cực kỳ cao của tiếng nói được tạo ra, miễn là hệ thống được thiết kế tốt và có sẵn dữ liệu tiếng nói phù hợp cho việc phát triển. Nhược điểm của nó là tính linh hoạt hạn chế vì tất cả các đoạn tiếng nói được sử dụng đều phải được ghi âm trước, làm giới hạn khả năng lựa chọn giọng nói của người nói hoặc thực hiện các sửa đổi khác đối với biểu cảm giọng nói.

Hệ thống CSS đơn giản nhất có thể tưởng tượng được là ghép nối các dạng sóng của các từ đã được ghi âm sẵn. Tuy nhiên, như {cite}`rabiner2007introduction` đã lưu ý, hướng tiếp cận này sẽ gặp phải hai vấn đề lớn. Thứ nhất, việc ghép nối các dạng sóng ở cấp độ từ sẽ nghe rất mất tự nhiên, vì các hiệu ứng đồng cấu âm (coarticulatory effects) giữa các từ sẽ không tồn tại trong dữ liệu. Ngoài ra, hệ thống sẽ bị giới hạn trong các kịch bản rất hạn chế, vì bất kỳ ngôn ngữ nào cũng có thể có hàng chục hoặc hàng trăm nghìn từ vựng và hàng triệu tên riêng - nhiều hơn nhiều so với những gì một người nói có thể ghi âm lại một cách hợp lý. Vấn đề này thậm chí còn tồi tệ hơn đối với các ngôn ngữ chắp dính (agglutinative languages) như tiếng Phần Lan, nơi ý nghĩa của từ được xây dựng và điều chỉnh bằng cách thêm các hậu tố vào dạng gốc của từ. Hơn nữa, vì các từ có thể xuất hiện trong phát ngôn ở các vị trí và vai trò khác nhau, nên các đặc trưng siêu đoạn tính/ngữ điệu (ví dụ: tần số cơ bản $F_0$) của cùng một từ có thể khác biệt tùy theo ngữ cảnh. Điều này có nghĩa là việc ghi âm trước tất cả các từ khả thi không phải là một lựa chọn thực tế.

Để giải quyết các vấn đề về khả năng mở rộng (scalability) và sự đồng cấu âm, các hệ thống CSS hiện đại trong thực tế dựa trên các đơn vị dưới cấp từ (sub-word units). Về nguyên tắc, mỗi ngôn ngữ chỉ có một vài âm (phones) (ví dụ: khoảng 40–50 âm đối với tiếng Anh), nhưng đặc tính âm học của chúng lại phụ thuộc rất nhiều vào ngữ cảnh ngữ âm xung quanh do hiện tượng đồng cấu âm. Do đó, các đơn vị phụ thuộc ngữ cảnh như diphone (cặp âm) hoặc triphone (bộ ba âm) được sử dụng. Để xây dựng một hệ thống CSS, tập dữ liệu tiếng nói trước hết phải được chú giải và phân đoạn cẩn thận thành các đơn vị cần quan tâm. Các phân đoạn này sau đó có thể được lưu trữ dưới dạng các tham số âm học (ví dụ: tham số codec tiếng nói) để tiết kiệm dung lượng và cho phép dễ dàng mô tả đặc tính cũng như thao tác xử lý.

### Các bước của tổng hợp tiếng nói ghép nối

Khi đã có cơ sở dữ liệu về các đơn vị âm học, quá trình tổng hợp với CSS gồm các bước cơ bản sau: **1)** *chuyển đổi văn bản đầu vào thành đặc tả mục tiêu (target specification)*, bao gồm chuỗi các âm cần tổng hợp cùng với các đặc tả ngữ điệu bổ sung như cao độ (pitch), thời lượng (duration) và năng lượng (power), **2)** *chọn đơn vị (unit selection)* cho từng đoạn âm theo đặc tả, và **3)** *hậu xử lý (post-processing)* để giảm thiểu tác động của các lỗi ghép nối (concatenation artifacts) tiềm ẩn.

Trong khi giai đoạn xử lý văn bản phần lớn tương tự như tiền xử lý trong [các hệ thống tổng hợp tiếng nói tham số thống kê](Statistical_parametric_speech_synthesis), phần cốt lõi của CSS là thực hiện chọn đơn vị sao cho tiếng nói đầu ra khớp với đặc tả đồng thời giữ được độ tự nhiên cao của âm thanh. Như mô tả của {cite}`hunt1996unit`, việc chọn đơn vị được thực hiện bằng cách giảm thiểu chi phí (cost minimization) sử dụng hai hàm chi phí (Hình 1): *chi phí mục tiêu (target cost)* $C^{t}(u_{i},t_{i})$ và *chi phí ghép nối (concatenation cost)* $C^{c}(u_{i-1},u_{i})$. Chi phí mục tiêu mô tả sự không khớp giữa đặc tả đơn vị tiếng nói mục tiêu $t_{i}$ và đơn vị ứng viên $u_{i}$ từ cơ sở dữ liệu. Chi phí ghép nối mô tả sự không khớp (ví dụ: về mặt âm học hoặc cảm thụ) tại điểm nối giữa đơn vị ứng viên $u_{i}$ và đơn vị đứng trước $u_{i-1}$. Nói cách khác, một giải pháp lý tưởng sẽ tìm thấy tất cả các đơn vị mục tiêu theo đúng đặc tả mà không tạo ra các điểm bất liên tục âm học ở ranh giới của các đơn vị được ghép nối.

  

![CSS_cost_schematic](attachments/CSS_cost_schematic.png)
**Hình 1:** Minh họa về chi phí lựa chọn $C^t$ và chi phí ghép nối $C^c$ trong việc chọn đơn vị dựa trên diphone để tổng hợp từ "cat" [k ae t]. Trong thực tế, các diphone có các âm khởi đầu khác nhau nhưng có hiệu ứng đồng cấu âm tương tự lên âm mục tiêu có thể được xem xét trong quá trình lựa chọn để khắc phục vấn đề khan hiếm dữ liệu. Phỏng theo {cite}`hunt1996unit`.

  

Vì đặc tả mục tiêu bao gồm nhiều đặc tính như danh tính của âm mục tiêu và (các) âm ngữ cảnh, cao độ, thời lượng và năng lượng, nên chi phí mục tiêu có thể được chia thành nhiều chi phí thành phần (subcosts) $j$ dưới dạng $C_{j}^{t}(t_{i},u_{i})$. Ví dụ, (các) âm ngữ cảnh (như [ae] trong đơn vị cuối [ae t] của từ "cat" ở Hình 1) có thể được biểu diễn bằng một số đặc trưng mô tả phương thức và vị trí cấu âm, nhờ đó đặc tả có thể được so sánh với các đơn vị ứng viên khác nhau trong cơ sở dữ liệu {cite:p}`hunt1996unit`. Điều này cho phép sử dụng các âm ngữ cảnh khác nhau nhưng có hiệu ứng đồng cấu âm tương tự lên âm mục tiêu. Tương tự, chi phí cho năng lượng và cao độ của đoạn âm có thể được đo lường, ví dụ: dựa trên sự khác biệt về log-năng lượng trung bình và F0 trung bình. Chi phí cho âm mục tiêu ([t] trong [ae t] ở Hình 1) thường là một chỉ báo nhị phân (binary indicator) bắt buộc danh tính âm vị của đơn vị được chọn phải trùng khớp với đặc tả mục tiêu. Tổng chi phí mục tiêu có thể được viết dưới dạng:

$$
C^{t}(t_i,u_i)=\sum_{j=1}^{P}w_{j}^{t}C_{j}^{t}(t_{j},u_{i}) $$

       (1) 

trong đó $w^{t} = [w^{t}_{1}, w^{t}_{2}, ..., w^{t}_{P}]$ là các trọng số tương đối của từng chi phí thành phần.

Chi phí ghép nối $C^{c}(u_{i-1},u_{i})$ có thể được rút ra theo cách tương tự Phương trình (1) bằng cách phân tích tổng chi phí thành $Q$ chi phí thành phần, rồi tính tổng có trọng số của các chi phí thành phần đó:

$$
C^{c}(u_{i-1},u_i)=\sum_{j=1}^{Q}w_{j}^{c}C_{j}^{c}(u_{i-1},u_{i})
$$         

(2)

  

Lưu ý rằng các chi phí thành phần và trọng số $w^{c}$ của chúng đối với $C^{c}$ không cần phải trùng khớp với các chi phí của $C^{t}$, vì chi phí ghép nối tập trung cụ thể vào tính tương thích âm học của các đơn vị kế tiếp nhau. Do đó, các chi phí thành phần $C^{c}_{j}(u_{i-1},u_{i})$ liên quan đến tính liên tục của phổ (hoặc phổ cepstrum), năng lượng đoạn âm, và cao độ trong đoạn âm và/hoặc tại điểm ghép nối cần được xem xét.

  

Tổng chi phí của quá trình lựa chọn là tổng của chi phí mục tiêu và chi phí ghép nối trên toàn bộ $n$ đơn vị:

$$
                  \begin{split}
C(t_{1}^{n},u_{1}^{n})&=\sum_{i=1}^{n}C^t(t_i,u_i)+\sum_{i=2}^{n}C^c(u_{i-1},u_i)+C^c(\#,u_1)+C^c(u_n,\#)
\\&
                  =
\sum_{i=1}^{n}\sum_{j=1}^{P}w_{j}^{t}C_{j}^{t}(t_{i},u_{i})+\sum_{i=2}^{n}\sum_{j=1}^{Q}w_{j}^{c}C_{j}^{c}(u_{i-1},u_{i})+C^c(\#,u_1)+C^c(u_n,\#)
\end{split}
$$      

(3)

trong đó $t_{1}^{n}$ là các mục tiêu, $u_{1}^{n}$ là các đơn vị được chọn, và ký hiệu \# biểu thị khoảng lặng (silence) {cite:p}`hunt1996unit`. Hai thành phần bổ sung đại diện cho sự chuyển tiếp từ khoảng lặng phía trước vào phát ngôn và từ phát ngôn ra khoảng lặng phía sau. Mục tiêu của quá trình lựa chọn là tìm ra chuỗi đơn vị $\bar{u}_{1}^{n}$ sao cho giảm thiểu tối đa tổng chi phí trong Phương trình (3). Quá trình lựa chọn này có thể được biểu diễn dưới dạng một lưới tìm kiếm liên kết đầy đủ (trellis), như chỉ ra ở Hình 3, trong đó mỗi cạnh dẫn đến một nút chứa chi phí cơ bản để chọn nút đó (chi phí mục tiêu) và một chi phí bổ sung phụ thuộc vào đơn vị đứng trước nó (chi phí ghép nối). Dựa trên lưới trellis này, việc lựa chọn tối ưu có thể được thực hiện bằng *[thuật toán tìm kiếm Viterbi](https://en.wikipedia.org/wiki/Viterbi_algorithm)* - một thuật toán quy hoạch động giúp tính toán đường đi có chi phí thấp nhất qua lưới trellis. Để việc tìm kiếm khả thi về mặt tính toán đối với các cơ sở dữ liệu lớn, các ứng viên ít khả thi hơn cho từng mục tiêu có thể được cắt tỉa (pruned) khỏi lưới trellis. Ngoài ra, thuật toán *[tìm kiếm chùm (beam search)](https://en.wikipedia.org/wiki/Beam_search)* chỉ giữ lại một số lượng cố định các nút có khả năng cao nhất ở mỗi bước có thể được áp dụng để tăng tốc độ tính toán.  

  
![CSS_search_trellis.png](attachments/180303620.png)

**Hình 3:** Ví dụ về lưới tìm kiếm chọn đơn vị cho từ "*cat*" [*k ae t*]. Mỗi cạnh gắn liền với chi phí mục tiêu cơ bản của đơn vị được chọn và chi phí ghép nối phụ thuộc vào đơn vị trước đó. Phỏng theo {cite:p}`rabiner2007introduction`

Sau khi các đơn vị tiếng nói được ghép nối để tạo thành phát ngôn mong muốn, các kỹ thuật hậu xử lý có thể được sử dụng để làm mịn các điểm bất liên tục tiềm ẩn về F0, năng lượng và phổ tại ranh giới của các đơn vị. Cần lưu ý rằng việc lạm dụng xử lý tín hiệu để biến đổi mạnh các đoạn âm thường có xu hướng làm giảm độ tự nhiên của âm thanh thu được. Do đó, việc sửa đổi trực tiếp các tham số âm học của đoạn âm (ví dụ: bằng cách sử dụng vocoder) không phải là chiến lược được khuyến khích để khắc phục các vấn đề do chọn đơn vị kém hoặc dữ liệu nguồn chất lượng thấp.

### Huấn luyện hệ thống CSS

Công thức trên cho phép thực hiện quá trình chọn đơn vị tối ưu và có cơ sở toán học rõ ràng từ một cơ sở dữ liệu tiếng nói cho trước. Tuy nhiên, kết quả tổng hợp phụ thuộc rất nhiều vào việc lựa chọn các đặc trưng và hàm số dùng cho các chi phí thành phần, cũng như các trọng số được chọn cho từng đặc trưng. Trong khi các hàm chi phí và các đặc trưng nền tảng có thể được thiết kế dựa trên các kiến thức về xử lý tín hiệu và xử lý tiếng nói, các trọng số cần phải được tinh chỉnh thông qua thử sai (trial and error) hoặc tối ưu hóa tự động bằng một tiêu chí chất lượng nào đó.

Để làm ví dụ cho việc ước lượng trọng số tự động, {cite}`hunt1996unit` đề xuất hai phương pháp thay thế để tự động tìm các trọng số của hàm chi phí:

1\) Sử dụng phương pháp [tìm kiếm lưới (grid search)](https://en.wikipedia.org/wiki/Hyperparameter_optimization#Grid_search) trên các giá trị trọng số khác nhau bằng cách tổng hợp các phát ngôn theo đặc tả mục tiêu của các phát ngôn được giữ lại (held-out utterances) từ cơ sở dữ liệu huấn luyện, sau đó so sánh dạng sóng tổng hợp thu được với dạng sóng thực tế của phát ngôn đó bằng một độ đo khách quan. Các trọng số mang lại hiệu suất tổng thể tốt nhất sẽ được lựa chọn.

2\) Sử dụng các mô hình hồi quy để dự đoán các giá trị trọng số tối ưu. {cite}`hunt1996unit` chỉ ra rằng khoảng cách cepstrum (cepstral distance) và sự khác biệt năng lượng tại điểm ghép nối có thể được dùng làm các biến dự báo (predictors) cho chất lượng cảm thụ của điểm nối trong một mô hình hồi quy tuyến tính, và do đó các trọng số hồi quy tuyến tính có thể được sử dụng trực tiếp làm các trọng số chi phí dựa trên cảm nhận thực tế. Đối với các trọng số mục tiêu, họ đề xuất hướng tiếp cận xem xét từng đơn vị trong cơ sở dữ liệu như một đặc tả mục tiêu tại mỗi thời điểm, và tìm kiếm $K$ đơn vị khác phù hợp nhất với mục tiêu đó bằng cách sử dụng một độ đo khoảng cách khách quan. Tiếp theo, các chi phí thành phần giữa mục tiêu và $K$ đơn vị khớp này được tính toán và ghi lại. Quá trình này được lặp lại cho toàn bộ các mẫu (exemplars) của cùng một đơn vị ngữ âm trong cơ sở dữ liệu, ghi lại $K \times Q$ chi phí thành phần và $K$ khoảng cách liên quan cho mỗi mẫu. Hồi quy tuyến tính sau đó được áp dụng để dự báo các khoảng cách cảm thụ khách quan đã ghi lại bằng cách sử dụng các chi phí thành phần liên quan; các hệ số hồi quy tuyến tính thu được sẽ biểu thị các trọng số tối ưu cho từng chi phí thành phần. Một ưu điểm nổi bật của hướng tiếp cận hồi quy trong việc ước lượng trọng số chi phí thành phần là nó cho phép ước lượng các trọng số đặc thù cho từng âm vị (phoneme-specific weights) đối với mỗi chi phí thành phần, vì các dấu hiệu cảm thụ quan trọng có thể khác nhau tùy theo ngữ cảnh ngữ âm.

  

  

## Tài liệu đọc thêm



```{bibliography}
:filter: docname in docnames
```

  
<!--
![CSS_cost_schematic.png](attachments/180303393.png)
![CSS_cost_schematic.png](attachments/180303625.png)
![CSS_search_trellis.png](attachments/180303624.png)
-->
