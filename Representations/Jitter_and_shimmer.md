# Jitter và shimmer

Hệ thống sản sinh tiếng nói không phải là một cỗ máy cứng nhắc, cơ khí, mà được cấu tạo từ một tập hợp các thành phần mô mềm. Do đó, mặc dù một số phần của tín hiệu tiếng nói có vẻ dừng (stationary), luôn có những dao động nhỏ trong đó, vì dao động của dây thanh (vocal fold) không hoàn toàn tuần hoàn. Các biến thiên về tần số và biên độ tín hiệu được gọi lần lượt là jitter và shimmer. Jitter và shimmer là các đặc trưng âm thanh của tín hiệu giọng nói, và chúng được gây ra bởi sự rung không đều của dây thanh. Chúng được cảm nhận như độ thô (roughness), độ thở (breathiness), hoặc độ khàn (hoarseness) trong giọng nói. Tất cả tiếng nói tự nhiên đều chứa một mức jitter và shimmer nào đó, nhưng việc đo lường chúng là một cách phổ biến để phát hiện các bệnh lý giọng nói. Thói quen cá nhân như hút thuốc hoặc uống rượu có thể làm tăng mức jitter và shimmer trong giọng nói. Tuy nhiên, nhiều yếu tố khác cũng có thể ảnh hưởng, chẳng hạn như độ to của giọng nói, ngôn ngữ, hoặc giới tính. Vì jitter và shimmer đại diện cho các đặc trưng giọng nói cá nhân mà con người có thể sử dụng để nhận ra giọng nói quen thuộc, các đại lượng này thậm chí có thể hữu ích cho các hệ thống nhận dạng người nói.

Có nhiều cách khác nhau để đo jitter và shimmer. Ví dụ, khi phát hiện rối loạn giọng nói, chúng được đo dưới dạng phần trăm của chu kỳ trung bình, trong đó các giá trị vượt quá ngưỡng nhất định có khả năng liên quan đến giọng nói bệnh lý. Jitter và shimmer được phát hiện rõ nhất từ các nguyên âm dài, duy trì.

Một giá trị jitter thường dùng là jitter tuyệt đối. Đại lượng này biểu thị hiệu tuyệt đối trung bình giữa các chu kỳ liên tiếp.

$$ Jitter(absolute) = \frac{1}{N-1}\sum_{i=1}^{N-1}\|T_i-T_{i+1}\|
$$

trong đó Ti là độ dài chu kỳ F0 được trích xuất và N là số chu kỳ F0 được trích xuất.

Khi chia giá trị này cho chu kỳ trung bình, ta thu được một đại lượng phổ biến khác là jitter tương đối.

$$ Jitter(relative) =
\frac{\frac{1}{N-1}\sum_{i=1}^{N-1}\|T_i-T_{i+1}\|}{\frac{1}{N}\sum_{i=1}^{N}T_i}
$$

trong đó Ti là độ dài chu kỳ F0 được trích xuất và N là số chu kỳ F0 được trích xuất.

Một giá trị shimmer thường dùng, ở đây gọi là Shimmer(dB), biểu thị giá trị trung bình của logarit cơ số 10 tuyệt đối của hiệu giữa biên độ của các chu kỳ liên tiếp nhân với 20.

$$ Shimmer(dB) =
\frac{1}{N-1}\sum_{i=1}^{N-1}\|20\log(A_{i+1}/A_i)\| $$

trong đó Ai là dữ liệu biên độ đỉnh-đỉnh được trích xuất và N là số chu kỳ tần số cơ bản được trích xuất.

Shimmer tương đối biểu thị hiệu tuyệt đối trung bình giữa biên độ của các chu kỳ liên tiếp chia cho biên độ trung bình.

$$ Shimmer(relative) =
\frac{\frac{1}{N-1}\sum_{i=1}^{N-1}\|A_i-A_{i+1}\|}{\frac{1}{N}\sum_{i=1}^{N}A_i}
$$

trong đó Ai là dữ liệu biên độ đỉnh-đỉnh được trích xuất và N là số chu kỳ tần số cơ bản được trích xuất.

  

  
