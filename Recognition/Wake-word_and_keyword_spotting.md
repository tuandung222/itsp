# Phát hiện từ đánh thức và từ khóa (Wake-word and keyword spotting)

Nhận dạng tiếng nói từ vựng lớn đầy đủ (large-vocabulary speech recognition) là không cần thiết và quá tốn kém cho nhiều ứng dụng thực tế. Đối với một chiếc đèn bàn, sẽ là khá đủ nếu nó hiểu được lệnh "Tắt đèn" và "Bật đèn". Bên cạnh đó, hầu hết thời gian, các thiết bị tiếng nói chỉ ở đó chờ đợi hướng dẫn khi không có ai nói chuyện. Hơn nữa, các thuật toán nhận dạng tiếng nói sử dụng rất nhiều tài nguyên tính toán, vì vậy sẽ thật lãng phí nếu để chúng liên tục phân tích âm thanh khi không có tiếng nói xuất hiện. [Phát hiện hoạt động tiếng nói (Voice activity detection - VAD)](content:vad) đảm nhận phần đầu tiên này, đó là phát hiện xem có tiếng nói hay không, nhờ đó tất cả các hoạt động tính toán khác có thể được duy trì ở chế độ ngủ (sleep mode) khi không có tiếng nói.

Phát hiện từ đánh thức và từ khóa (Wake-word and keyword spotting) đề cập đến các tác vụ nhận dạng tiếng nói từ vựng nhỏ (small-vocabulary speech recognition). Chúng được sử dụng trong các ứng dụng rất đơn giản nơi nhận dạng tiếng nói đầy đủ là phức tạp không cần thiết (phát hiện từ khóa), hoặc trong các tác vụ tiền xử lý nơi chúng ta muốn tiết kiệm tài nguyên bằng cách chờ đợi một khẩu lệnh như "Hey computer!". Trong tác vụ thứ hai này, việc phát hiện từ đánh thức đóng vai trò là tác nhân kích hoạt (trigger) cho các tác vụ xử lý tiếng nói phức tạp hơn ở phía sau. Mặc dù hai tác vụ này có mục tiêu khá khác nhau, công nghệ cốt lõi của chúng lại rất giống nhau và sẽ được thảo luận chung ở đây.

Thông thường nhất, các thuật toán phát hiện từ đánh thức và từ khóa chạy trên các thiết bị có tài nguyên hạn chế. Chúng có thể bị giới hạn về dung lượng bộ nhớ và tài nguyên tính toán (năng lực CPU) hoặc thường là cả hai. Việc tăng dung lượng bộ nhớ hoặc sử dụng CPU lớn hơn sẽ làm tăng giá thành thiết bị (chi phí đầu tư), đồng thời cũng đòi hỏi nhiều năng lượng hơn (chi phí vận hành). Trong các thiết bị nhỏ, những chi phí biên như vậy chiếm một phần rất đáng kể trong tổng chi phí của thiết bị.

Cấu trúc tổng quát của các thuật toán phát hiện từ khóa được minh họa dưới đây. Tín hiệu tiếng nói đầu vào trước tiên được chuyển đổi thành một biểu diễn đặc trưng, chẳng hạn như [MFCC](content:mfcc), sau đó được đưa vào một [mạng nơ-ron (neural network)](content:nn), và đầu ra thu được là khả năng xảy ra (likelihood) của từng từ khóa.

![keyword_flow.png](attachments/155474775.png)

Được điều chỉnh từ {cite:p}`zhang2017hello`

Nói cách khác, các bộ phát hiện từ khóa và từ đánh thức có một tập hợp nhỏ các từ khóa được chấp nhận, được lập trình sẵn (hard-coded) vào phần mềm. Nếu chúng ta có $N$ từ khóa khả thi, thì mạng nơ-ron sẽ có $N$ đầu ra tương ứng với xác suất đầu vào là mỗi từ khóa đó. Đầu ra sau đó được áp ngưỡng so sánh để chọn từ khóa có xác suất lớn nhất.

Việc lựa chọn cấu trúc cho mạng nơ-ron phụ thuộc rất lớn vào các ràng buộc của thiết bị cũng như mục tiêu, môi trường hoạt động và bối cảnh của ứng dụng. Thông thường, cấu trúc được sử dụng là một mạng nơ-ron sâu (deep neural network) kết hợp các thành phần hồi quy (recurrent), tích chập (convolutional) và bộ nhớ ngắn hạn dài (LSTM).

Thách thức trung tâm trong việc huấn luyện các thuật toán phát hiện từ khóa là tìm kiếm và lựa chọn dữ liệu huấn luyện. Để đạt được chất lượng tốt, bạn thường cần tới vài chục nghìn lượt phát âm của các từ khóa, được nói bởi nhiều người nói khác nhau và trong các môi trường khác nhau. Ví dụ: bộ dữ liệu ["Speech Commands Dataset" của Google](https://ai.googleblog.com/2017/08/launching-speech-commands-dataset.html) chứa khoảng 65.000 lượt phát âm của 30 từ ngắn. Tuy nhiên, việc lựa chọn từ khóa tự nhiên phụ thuộc vào các chức năng mà bộ phát hiện từ khóa cần kích hoạt, hoặc từ đánh thức mong muốn. Đối với các ứng dụng thực tế, chúng ta thường không thể sử dụng các bộ dữ liệu được thu thập sẵn, mà phải tự thu thập dữ liệu của riêng mình. Bạn có thể hình dung được khối lượng công việc khổng lồ cần thiết để thu thập 65.000 lượt phát âm từ hơn một nghìn người nói!

## Tài liệu tham khảo

```{bibliography}
:filter: docname in docnames
```
