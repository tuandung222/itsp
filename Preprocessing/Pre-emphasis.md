# Nhấn mạnh trước (Pre-emphasis)

![speech_avg_db.png](attachments/149888832.png)

Hình trên minh họa phổ biên độ trung bình của một tín hiệu tiếng nói. Ta quan sát thấy rằng phần lớn năng lượng tập trung ở phần thấp của phổ. Thực tế, dưới dạng xấp xỉ tuyến tính, ta thấy rằng trong ví dụ cụ thể này, năng lượng giảm với tốc độ 2,2 dB/kHz. Tốc độ giảm chính xác thay đổi tùy theo từng người nói và phụ thuộc vào nhiều yếu tố. Một giả định an toàn và thường được sử dụng là năng lượng giảm khoảng 2 dB/kHz.

Sự giảm năng lượng nhanh chóng này dẫn đến các vấn đề thực tiễn trong triển khai. Ví dụ, nếu ta triển khai biến đổi Fourier rời rạc với [số học điểm cố định](https://en.wikipedia.org/wiki/Fixed-point_arithmetic), thì độ chính xác sẽ rất khác nhau ở các phần khác nhau của phổ. Thông thường, phổ tại 6kHz thấp hơn 15dB so với tại 0Hz. Trên thang tuyến tính, 15dB tương ứng với hệ số 6. Nói cách khác, trên CPU 16-bit, nếu ta sử dụng toàn bộ dải của biểu diễn 15-bit có dấu cho các tần số thấp nhất, thì ta chỉ sử dụng hiệu quả dải 12-bit cho các thành phần tần số tại 6kHz.

Một công cụ tiền xử lý phổ biến được sử dụng để bù đắp cho hình dạng phổ trung bình là *nhấn mạnh trước* (pre-emphasis), nhấn mạnh các tần số cao hơn. Thông thường, nhấn mạnh trước được áp dụng dưới dạng bộ lọc FIR trong miền thời gian với một tham số tự do, ví dụ, trong mã hóa tiếng nói ở tần số lấy mẫu 8kHz hoặc 12,8kHz, ta sử dụng bộ lọc nhấn mạnh trước $ P(z)=1-0.68 z^{-1} $ {cite:p}`backstrom2017speech`. Phổ của bộ lọc này được minh họa bên dưới. Sau khi áp dụng bộ lọc, phổ phẳng hơn và ta có thể áp dụng số học điểm cố định với độ chính xác thấp hơn, từ đó tối ưu hóa tốt hơn việc tiêu thụ CPU.

Có nhiều cách khác nhau để điều chỉnh nhấn mạnh trước. Thứ nhất, mặc dù phổ trung bình đang giảm, các âm xát (fricative) vô thanh thường có *nhiều* năng lượng hơn ở các tần số cao. Nhấn mạnh trước quá mức do đó sẽ gây ra vấn đề cho các âm xát. Nhấn mạnh trước cũng có ảnh hưởng đến cả mô hình cảm nhận và mô hình thống kê cũng như ước lượng các mô hình dự báo tuyến tính. Do đó, mức độ nhấn mạnh trước tối ưu phụ thuộc rất nhiều vào ứng dụng và chi tiết triển khai.

![pre_emph_db.png](attachments/149888831.png) 


# Tài liệu tham khảo
```{bibliography}
:filter: docname in docnames
```
