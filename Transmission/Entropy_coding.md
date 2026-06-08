# Mã hóa entropy (Entropy coding)


Trong truyền dẫn và lưu trữ dữ liệu, việc cực tiểu hóa số lượng bit cần thiết để biểu diễn duy nhất (uniquely represent) đầu vào là rất hữu ích. Thuật ngữ **mã hóa entropy** (entropy coding) đề cập đến các phương pháp sử dụng thống kê để nén dữ liệu. Mục tiêu của nó là mã hóa **không tổn hao** (lossless), nơi dữ liệu gốc có thể được tái cấu trúc hoàn hảo từ biểu diễn đã nén. Tương tự, mã hóa **có tổn hao** (lossy) đề cập đến việc nén khi chúng ta có số lượng bit hạn chế để sử dụng và cố gắng tái tạo một tín hiệu giống với tín hiệu gốc nhất có thể, nhưng không nhất thiết phải giống hệt nhau. Trong xử lý tiếng nói và âm thanh, mã hóa thường đề cập đến mã hóa có tổn hao. Mục tiêu là nén tín hiệu được mã hóa sao cho tai người nghe không thể phân biệt được với tín hiệu gốc, hoặc giảm thiểu tối đa ảnh hưởng cảm thụ của việc lượng tử hóa. Cách mã hóa như vậy được gọi là **[mã hóa cảm giác (perceptual coding)](Perceptual_modelling_in_speech_and_audio_coding)**. Thông thường, mã hóa cảm giác được thực hiện qua hai bước: 1) lượng tử hóa tín hiệu sao cho ảnh hưởng suy giảm chất lượng cảm thụ được giảm thiểu tối đa, và 2) mã hóa không tổn hao cho tín hiệu đã lượng tử hóa đó. Theo nghĩa này, mặc dù mã hóa không tổn hao và có tổn hao là hai phương pháp hoàn toàn khác nhau, một mô-đun mã hóa không tổn hao thường vẫn được tích hợp bên trong các bộ codec có tổn hao.

```{sidebar} Mã hóa ngây thơ (Naïve coding)
Cách mã hóa ngây thơ cho một tập hợp số, với 2 bit cho mỗi ký hiệu.

| số | ký hiệu | chuỗi bit | độ dài |
|---|---|---|---|
| -1 | a | 00 | 2 |
| 0 | b | 01 | 2 |
| +1 | c | 10 | 2 |
```

Mã hóa entropy hoạt động ở cấp độ trừu tượng, nghĩa là nó có thể hoạt động trên bất kỳ tập hợp ký hiệu nào miễn là chúng ta có thông tin về xác suất xuất hiện của từng ký hiệu. Ví dụ, xét một đại lượng vô hướng nhận giá trị nguyên $x$ có thể nhận các giá trị -1, 0 và +1 với xác suất tương ứng là 0.25, 0.5 và 0.25. Nghĩa là, nếu chúng ta lấy mẫu lặp đi lặp lại các giá trị $x$ từ phân phối này, thì trung bình 25% trong số chúng sẽ là -1. Khi đó, giá trị số của $x$ không còn quan trọng, chúng ta có thể đặt tên tương đương cho các phần tử phân biệt này theo các ký hiệu của bảng chữ cái là $a, b$ và $c$.  
Bảng bên phải mô tả một cách mã hóa khả thi cho các con số này. Rõ ràng chúng ta cần nhiều hơn 1 bit để mã hóa ba ký hiệu, và do đó cách mã hóa này sử dụng 2 bit cho mỗi ký hiệu. Tuy nhiên, lưu ý rằng chuỗi bit 11 không được sử dụng, nghĩa là cách mã hóa này chưa hiệu quả.



## Mã hóa vector (Vector coding)

```{sidebar} Mã hóa vector (Vector coding)
Cách mã hóa vector ngây thơ cho một tập hợp số, với 2 bit cho mỗi ký hiệu.

| số | ký hiệu | chuỗi bit | độ dài |
|---|---|---|---|
| -1, -1, -1 | aaa | 00000 | 5 |
| -1, -1, 0 | aab | 00001 | 5 |
| -1, -1, +1 | aac | 00010 | 5 |
| -1, 0 -1 | aba | 00011 | 5 |
| -1, 0, 0 | abb | 00100 | 5 |
| ... | ... | ... | ... |
| +1, +1, +1 | ccc | 11011 | 5 |
```

Để tận dụng tốt hơn các bit, thay vì xét từng ký hiệu đơn lẻ, chúng ta có thể xem xét một vector các ký hiệu $x_1, x_2, x_3$. Với 3 ký hiệu khả thi cho mỗi phần tử, chúng ta có $3^3 = 27$ tổ hợp khả thi. Do đó để mã hóa, chúng ta cần $\text{ceil}(\log_2(27)) = 5$ bit, tương đương 1.66 bit cho mỗi mẫu. So với cách mã hóa ban đầu là 2 bit/mẫu ở trên, đây là một sự cải tiến rõ rệt. Tuy nhiên, chúng ta vẫn còn 5 chuỗi bit chưa được sử dụng, cho thấy cách mã hóa này vẫn chưa hoàn toàn tối ưu.

Lưu ý rằng có những điểm tương đồng trực tiếp giữa phương pháp này với **[lượng tử hóa vector (VQ)](content:vq)** mặc dù hai phương pháp này không phải là một. Nói ngắn gọn, lượng tử hóa vector là mã hóa có tổn hao (lossy coding), giúp tìm ra phép lượng tử hóa tốt nhất với một tập hợp ký hiệu cho trước, trong khi mã hóa vector là mã hóa không tổn hao (lossless coding) cho các vector ký hiệu.





## Mã hóa độ dài biến thiên và mã hóa Huffman

```{sidebar} Mã hóa Huffman (Huffman coding)
Minh họa cách mã hóa cho một tập hợp số, với 1.5 bit cho mỗi ký hiệu.

| số | ký hiệu | xác suất $P_{k}$ | chuỗi bit | độ dài $L_{k}$ |
|---|---|---|---|---|
| -1 | a | 0.25 | 00 | 2 |
| 0 | b | 0.5 | 1 | 1 |
| +1 | c | 0.25 | 01 | 2 |
```

Một khái niệm trung tâm trong mã hóa entropy là mã hóa **độ dài biến thiên** (variable length coding), nơi các ký hiệu có thể được mã hóa bằng các chuỗi bit có độ dài khác nhau. Trong ví dụ trên của chúng ta, một cách mã hóa tối ưu (tức là các chuỗi bit tối ưu) được liệt kê ở bảng bên phải.

Số lượng bit trung bình cho mỗi ký hiệu là tổng độ dài của mỗi chuỗi bit nhân với xác suất tương ứng của nó:

$$ E[bits/symbol] = \sum_{k\in\{a,b,c\}}P_k L_k = 0.25\times 2 + 0.5\times 1 + 0.25\times 2 = 1.5. $$

Từ các chuỗi bit 00, 01 và 1, chúng ta hoàn toàn có thể giải mã được các ký hiệu gốc; nếu bit đầu tiên là 1, ký hiệu đó là $b$, ngược lại, bit thứ hai sẽ quyết định ký hiệu là $a$ hoặc $c.$

Các mã độ dài biến thiên như vậy có thể dễ dàng được xây dựng khi các xác suất là lũy thừa âm của 2. Đây là một hướng tiếp cận kinh điển được gọi là **mã hóa Huffman** (Huffman coding). Nó rất đơn giản để triển khai, làm cho nó trở thành một lựa chọn hấp dẫn khi các xác suất là các lũy thừa âm của 2. Tuy nhiên, khi xác suất của các ký hiệu là các giá trị bất kỳ, mã hóa Huffman không còn áp dụng được nữa nếu không có các phép xấp xỉ xác suất, điều này làm cho việc mã hóa không còn tối ưu.




## Mã hóa số học (Arithmetic coding)

```{sidebar} Mã hóa số học 1
Minh họa tập hợp các ký hiệu và xác suất tương ứng của chúng.

| ký hiệu $k$| xác suất $P_k$ | khoảng $s_k \dots s_{k+1}$ | số bit mỗi ký hiệu $\log_2(P_k)$ |
| ---- | ---- | ---- | ---- |
| 0 | 0.40 | 0.00 ... 0.40 | 1.32 |
| 1 | 0.27 | 0.40 ... 0.67 | 1.89 |
| 2 | 0.12 | 0.67 ... 0.79 | 3.05 |
| 3 | 0.08 | 0.79 ... 0.87 | 3.64 |
| 4 | 0.07 | 0.87 ... 0.94 | 3.84 |
| 5 | 0.06 | 0.94 ... 1.00 | 4.06 |
```


Để cải thiện hơn nữa hiệu suất mã hóa, chúng ta có thể kết hợp mã hóa vector với mã hóa Huffman trong một phương pháp được gọi là **mã hóa số học** (arithmetic coding) {cite}`rissanen1979arithmetic`. Phương pháp này sử dụng xác suất của các ký hiệu để mã hóa đồng thời một chuỗi các ký hiệu. Ví dụ, xét tập hợp các ký hiệu từ 0 đến 5 ở bên phải với xác suất xuất hiện tương ứng là $P_k$. Giả sử thêm rằng chúng ta cần mã hóa chuỗi ký tự "130". Bước đầu tiên là gán mỗi ký hiệu cho một đoạn duy nhất $[s_k, s_{k+1}]$ nằm trong khoảng $[0, 1]$ sao cho độ rộng của đoạn khớp với xác suất của ký hiệu đó: $P_k = s_{k+1}-s_k$.


```{sidebar} Mã hóa số học 2
Các khoảng của ký hiệu thứ hai.

| ký hiệu $k$ | khoảng $s_k' \dots s_{k+1}'$|
| ---- | ---- |
| 0 | 0.4000 ... 0.5080 |
| 1 | 0.5080 ... 0.5809 |
| 2 | 0.5809 ... 0.6133 |
| 3 | 0.6133 ... 0.6349 |
| 4 | 0.6349 ... 0.6538 |
| 5 | 0.6538 ... 0.6700 |
```


Ký hiệu đầu tiên là "1", qua đó chúng ta được gán vào khoảng 0.40 ... 0.67, khoảng này ta sẽ gọi là khoảng hiện tại. Ý tưởng trung tâm của mã hóa số học là ký hiệu tiếp theo sẽ được mã hóa bên trong khoảng hiện tại này. Nghĩa là chúng ta tịnh tiến và tỉ lệ hóa (shift and scale) các giá trị $s_k$ sao cho chúng khớp hoàn hảo bên trong khoảng hiện tại 0.40 ... 0.67. Về mặt toán học, nếu ký hiệu hiện tại là $h$, thì khoảng hiện tại là $s_h ... s_{h+1}$, và các khoảng của ký hiệu tiếp theo được dịch chuyển và tỉ lệ hóa dưới dạng:

$$ s'_k = s_h + s_k(s_{h+1}-s_h) = s_h + s_kP_h. $$

*(Lưu ý: Công thức ở trên sử dụng $P_h$ thay cho $P_k$ của ký hiệu hiện tại để tỉ lệ hóa khoảng, trong đó $P_h = s_{h+1} - s_h$)*

Ký hiệu thứ hai là "3", làm cho khoảng hiện tại trở thành 0.6133 ... 0.6349. Đối với ký hiệu thứ ba, các khoảng sau đó tiếp tục được dịch chuyển và tỉ lệ hóa thành:

$$ s''_k = s'_3 + s'_k(s'_4-s'_3) = 0.6133 + s'_k \times (0.6349 - 0.6133). $$

Các khoảng mới thu được được liệt kê ở bảng bên phải. Ký hiệu thứ ba là "0", do đó khoảng hiện tại cuối cùng sẽ là 0.6133 ... 0.6219, chúng ta ký hiệu khoảng này là $ s_{left} ... s_{right} $.

```{sidebar} Mã hóa số học 3
Các khoảng của ký hiệu thứ ba.

| ký hiệu $k$ | khoảng $s_k'' \dots s_{k+1}''$|
| ---- | ---- |
| 0 | 0.6133 ... 0.6219 |
| 1 | 0.6219 ... 0.6278 |
| 2 | 0.6278 ... 0.6304 |
| 3 | 0.6304 ... 0.6321 |
| 4 | 0.6321 ... 0.6336 |
| 5 | 0.6336 ... 0.6439 |
```

Bước còn lại là chuyển đổi khoảng cuối cùng này thành một chuỗi các bit. Hãy chia toàn bộ khoảng 0 ... 1 thành $2^{B}$ mức lượng tử hóa trên một lưới đều, sao cho mức thứ $k$ là $k 2^{-B}$. Sau đó, chúng ta tìm số bit $B$ nhỏ nhất (tương ứng độ phân giải lớn nhất) sao cho tồn tại một số nguyên $k$ để giá trị $k 2^{-B}$ nằm gọn bên trong đoạn hiện tại cuối cùng $s_{left} ... s_{right}$, tức là: $s_{left} \le k 2^{-B} \le s_{right}$. Khi đó, $k$ chính là chỉ số của vị trí lượng tử hóa của chúng ta, nó biểu diễn duy nhất cho khoảng đó và từ đó biểu diễn duy nhất cho chuỗi ký hiệu "130". Cụ thể, với $B=7$, chúng ta thấy rằng $k=79$ thỏa mãn tiêu chí:

$$ s_{left}=0.6133 \leq k2^{-B} = 0.6172 \leq s_{right} = 0.6219.
$$

Quá trình giải mã chuỗi này sau đó ở bộ giải mã là vô cùng dễ dàng và tường minh.

Một vài điểm lưu ý bổ sung:

-   Tốc độ bit trung bình (entropy của nguồn tin) là $ -\sum_k P_k \log_2 P_k \approx 2.2 $ bit cho mỗi mẫu. Trong ví dụ trên, chúng ta cần $B=7$ bit để mã hóa 3 mẫu, tương đương với 2.3 bit cho mỗi mẫu. Số lượng bit thực tế do đó không hoàn toàn trùng khớp với tốc độ bit trung bình lý thuyết.

-   Trong triển khai thực tế của bộ giải mã, chúng ta cần biết số lượng ký hiệu hoặc số lượng bit, truyền riêng số lượng ký hiệu/bit đó đi, hoặc sử dụng một ký hiệu đặc biệt để đánh dấu kết thúc chuỗi (end-of-string).

-   Thông thường, phân đoạn hiện tại cuối cùng không khớp hoàn toàn với $k 2^{-B}$, điều này có nghĩa là có các khoảng trống nhỏ không được sử dụng giữa chuỗi bit và phân đoạn hiện tại cuối cùng. Đây là một sự kém hiệu quả cố hữu của mã hóa số học. Về mặt trực quan, chúng ta bắt buộc phải gửi một số nguyên các bit, nhưng dữ liệu có thể không tương ứng với chính xác một số nguyên các bit. Tổn hao này luôn nhỏ hơn 1 bit cho toàn bộ chuỗi ký tự, đây là mức chấp nhận được khi chúng ta truyền một lượng lớn các ký hiệu trong một chuỗi.

-   Việc triển khai trực tiếp mô tả trên sẽ gặp nhiều khó khăn vì các khoảng số nhanh chóng trở nên nhỏ hơn khả năng biểu diễn của số học dấu phẩy động hoặc số học dấu phẩy tĩnh (fixed or floating point). Do đó, các thuật toán thường được thiết kế để sử dụng một khoảng trung gian, từ đó ta có thể xuất ra các bit kết quả ngay khi chúng được xác định. Ví dụ, trong ví dụ trên, ngay sau ký hiệu đầu tiên, chúng ta đã biết khoảng nằm trên 0.5, nên bit đầu tiên chắc chắn phải là 1.

-   Việc triển khai chi tiết khá phức tạp và rất nhạy cảm với các lỗi tính toán.

-   Độ phức tạp thuật toán của bộ mã hóa số học nhìn chung là hợp lý, với điều kiện xác suất $P_k$ được cung cấp sẵn. Nếu xác suất cần được tính toán trực tuyến (online) bằng mô hình tham số, độ phức tạp tính toán sẽ tăng lên đáng kể.




## Mã hóa tham số (Parametric coding)

Trong các ví dụ trên, chúng ta đã sử dụng một bảng cố định để liệt kê xác suất của từng ký hiệu. Điều này dẫn đến một hệ thống cứng nhắc, không thể thích ứng với những thay đổi của tín hiệu. Nếu chúng ta muốn sử dụng một mô hình cảm giác để lựa chọn độ chính xác lượng tử hóa cho các mẫu khác nhau, chúng ta cần khả năng điều chỉnh độ rộng của các khoảng lượng tử hóa (quantization bin widths), và do đó là điều chỉnh xác suất tương ứng của từng khoảng lượng tử hóa. Một hướng tiếp cận đơn giản là mô hình hóa phân phối xác suất của tín hiệu và tính toán xác suất của từng ký hiệu trực tuyến. Do đó, chúng ta sử dụng một mô hình tham số (parametric model) của phân phối xác suất, và việc mã hóa sử dụng các mô hình tham số như vậy được gọi là **mã hóa tham số** (parametric coding).

Trong mã hóa tiếng nói, mã hóa tham số thường được sử dụng trong mã hóa miền tần số để mã hóa các thành phần phổ riêng lẻ. Chúng ta có thể giả định rằng các thành phần phổ tuân theo phân phối Laplace và tính toán ra các xác suất $P_k$ dựa trên phân phối đó. Các phương án tinh vi hơn bao gồm việc sử dụng các [mô hình hỗn hợp Gauss (Gaussian Mixture Models - GMMs)](content:gmm).


## Mã hóa đại số (Algebraic coding)

```{sidebar} Mã hóa đại số (Algebraic coding)
Minh họa một vector đầu vào với các phần tử $x_k$ và các mã hóa tương ứng của chúng.

| $x_1$ | $x_2$ | $x_3$ | $x_4$ | mã hóa |
| ---- | ---- | ---- |---- |----: |
| +1 | 0 | 0 | 0 | 0 |
| -1 | 0 | 0 | 0 | 1 |
|  0 |+1 | 0 | 0 | 2 |
|  0 |-1 | 0 | 0 | 3 |
|  0 | 0 |+1 | 0 | 4 |
|  0 | 0 |-1 | 0 | 5 |
|  0 | 0 | 0 |+1 | 6 |
|  0 | 0 | 0 |-1 | 7 |
```

Giả sử chúng ta muốn mã hóa một chuỗi như "00010000", nơi các số "0" xuất hiện với xác suất rất cao và chỉ có duy nhất một số "1". Chúng ta có thể sử dụng mã hóa số học và mã hóa tham số để mã hóa xác suất của các số "0" và "1" nhằm tạo ra chuỗi bit đầu ra. Tuy nhiên, việc đó sẽ nhanh chóng trở nên cực kỳ phức tạp, trong khi cách đơn giản hơn nhiều là chỉ cần mã hóa vị trí của số "1" duy nhất đó. Trong trường hợp này, nếu vị trí đầu tiên đánh số từ 0, thì số "1" nằm ở vị trí thứ 3. Có tổng cộng 8 vị trí, do đó chúng ta cần $\log_2(8) = 3$ bit để mã hóa vị trí. Vô cùng đơn giản.

Hướng tiếp cận này có thể dễ dàng được mở rộng để bao gồm cả các giá trị +1 và -1. Chỉ cần mã hóa dấu (âm hay dương) bằng 1 bit bổ sung. Cũng có các thuật toán đơn giản để bao gồm nhiều phần tử khác không, miễn là số lượng các phần tử khác không này nhỏ.

Các thuật toán mã hóa như vậy được gọi là **mã hóa đại số** (algebraic coding), nơi chúng ta sử dụng một quy tắc đại số hoặc thuật toán tường minh để mã hóa chuỗi ký hiệu. Đây là một dạng của mã hóa vector, vì nó mã hóa đồng thời cả một chuỗi ký hiệu. Thông thường mã hóa đại số là mã hóa tốc độ bit cố định (fixed bitrate coding), vì số lượng bit đã được quyết định từ trước.

Mã hóa đại số hoạt động hiệu quả khi số lượng phần tử khác không nhỏ và số lượng mức lượng tử hóa rất ít. Đáng tiếc là các phương pháp này trở nên phức tạp hơn rất nhiều khi yêu cầu độ chính xác cao hơn. Hơn nữa, đối với lượng tử hóa độ chính xác cao, việc tìm ra phép lượng tử hóa tốt nhất cho một vector $x$ cho trước cũng trở nên khó khăn hơn. Mặc dù vậy, nhờ tính đơn giản và hiệu quả ở tốc độ bit thấp, mã hóa đại số cực kỳ phổ biến trong mã hóa tiếng nói đến mức loại codec được sử dụng phổ biến nhất được gọi là [Dự báo tuyến tính kích thích bằng bảng mã đại số (ACELP)](Code-excited_linear_prediction_CELP.md), vì nó sử dụng mã hóa đại số để mã hóa tín hiệu phần dư. {cite}`backstrom2017speech`



## Tài liệu tham khảo

```{bibliography}
:filter: docname in docnames
```