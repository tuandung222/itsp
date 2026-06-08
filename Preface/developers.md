(content:developers)=
# Hướng dẫn cho Nhà phát triển

## Bắt đầu

1. Tải mã nguồn từ [https://github.com/Speech-Interaction-Technology-Aalto-U/itsp](https://github.com/Speech-Interaction-Technology-Aalto-U/itsp). 
2. Cài đặt các gói cần thiết, ví dụ:

   ```bash
   pip install jupyter-book   
   ```
   ![]()
3. Nhiều chương yêu cầu các gói bổ sung, cần cài đặt nếu bạn muốn biên dịch toàn bộ sách (không phải lúc nào cũng bắt buộc).

    ```bash
    pip install numpy scipy matplotlib ipython ipywidgets jupyterlab
    pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
    pip install itikz
    pip install librosa
    pip install speechbrain
    ```
    ![]()

   Một số chương cũng cần Texlive. Lưu ý rằng speechbrain khá kén phiên bản python và 
    
5. Cách sử dụng phổ biến là viết markdown (.md) và notebook (.ipynb) bằng [JupyterLab](https://jupyter.org/), có thể khởi chạy (trong thư mục dự án) bằng

    ```bash
    jupyter lab &
    ```
    ![]()

   Cách dễ nhất để bắt đầu là viết [file Markdown](https://jupyterbook.org/en/stable/file-types/markdown.html#file-types-markdown) tĩnh. Nhược điểm là khi đó bạn không thể sử dụng ví dụ lập trình trực tiếp trong tài liệu. Tuy nhiên, việc nâng cấp từ markdown lên notebook khá dễ dàng.
    
6. Khi đã tương đối hài lòng với nội dung mới, biên dịch sách HTML (trong thư mục dự án) bằng

    ```bash
    jupyter-book build .
    ```
    ![]()    
    Kiểm tra xem sách HTML hiển thị đúng như mong muốn — một số thành phần có thể hoạt động hơi khác.
    
    
## Mẹo và thủ thuật

### Âm thanh

Ghi âm bằng python/jupyter rất phiền phức, nhưng phát âm thanh hoạt động rất tốt khi sử dụng [IPython](https://ipython.org/) và [ipywidgets](https://ipywidgets.readthedocs.io/en/latest/). Xem [Vocoder](content:vocoder) để biết ví dụ. 

### Thành phần tương tác

Một số minh họa phát huy hiệu quả tốt nhất khi người đọc được tự thay đổi tham số và quan sát kết quả. [IPython](https://ipython.org/) và [ipywidgets](https://ipywidgets.readthedocs.io/en/latest/) cho phép thực hiện điều đó. Tuy nhiên, một số thành phần tương tác nâng cao hiện yêu cầu kernel jupyter/python đang chạy (tức là phải chạy notebook trên máy chủ).
Để xem ví dụ, hãy tham khảo [Biểu diễn/Phổ đồ và STFT](stft).

### Đồ thị luồng và trực quan hóa tĩnh khác

[Tikz và PGF](https://tikz.dev/) là bộ công cụ mạnh mẽ để tạo trực quan hóa bên trong tài liệu [LaTeX](https://www.latex-project.org/). Chúng có thể được tích hợp vào notebook jupyter thông qua [itikz](https://pypi.org/project/itikz/). Xem ví dụ tại [Bảo mật và Quyền riêng tư](../Security_and_privacy.ipynb).

*Mẹo:* Trong một số trường hợp, ký tự có thể bị lỗi khi có nhiều thành phần itikz trong cùng một tài liệu. Nếu gặp vấn đề này, hãy xem giải pháp tại [thảo luận trên Itikz-github](https://github.com/jbn/itikz/issues/28).

### Khối mã ẩn và bị xóa trong notebook Jupyter

Các khối mã có thể được sử dụng như thành phần sư phạm, nhưng thường có những khối mã chỉ dùng để vẽ đồ thị, không thực sự thú vị đối với người đọc. Khi đó nên *ẩn* khối mã để tránh rối mắt. Một nút "*nhấn để hiện*" sẽ xuất hiện, cho phép người đọc xem cách phần backend hoạt động. 

Một lựa chọn khác là *xóa* hoàn toàn khối mã khỏi output. Cách này phù hợp nhất khi khối mã không có mục đích sư phạm, như khi tạo đồ thị luồng bằng *itikz* (xem ở trên). 

Cả hai chức năng đều có thể thực hiện bằng "*tags*" trong notebook jupyter. Có thể tìm thấy bằng cách nhấn vào biểu tượng "*Property inspector*" ở phía ngoài cùng bên phải của notebook. Tại đó, có thể thêm "*hide-input*" hoặc "*remove-input*" cho các chức năng tương ứng. Để biết thêm thông tin, xem [Jupyter-book/Ẩn hoặc xóa nội dung](https://jupyterbook.org/interactive/hiding.html).

Để xem ví dụ sử dụng cả khối mã ẩn và bị xóa, tham khảo [Nâng cao chất tiếng nói/Giảm nhiễu](../Enhancement/Noise_attenuation.ipynb).

### Trích dẫn và tham chiếu

Chúng tôi sử dụng phương pháp dựa trên bibtex được lưu tại [references.bib](../references.bib). Để biết hướng dẫn, xem [Jupyter-book/trích dẫn và tham chiếu](https://jupyterbook.org/en/stable/content/citations.html). Để xem ví dụ, tham khảo [Mô hình tính toán về xử lý ngôn ngữ người](../Computational_models_of_human_language_processing.md).


### Chạy notebook / Huy hiệu MyBinder

Để cho phép người dùng chạy mã trong notebook, cần có máy chủ jupyter. Máy chủ này có thể được cài đặt cục bộ (xem [Sử dụng tài liệu này](Using_this_document.ipynb)), nhưng quan trọng hơn, có các máy chủ công cộng có thể sử dụng. Cách đơn giản nhất là sử dụng [mybinder.org](https://mybinder.org). Để thuận tiện sử dụng, vui lòng đặt huy hiệu ở đầu notebook, cho phép chạy trực tiếp notebook trên máy chủ theo các bước sau:
1. Tìm URL github, thư mục và tên file.
2. Nhập thông tin trên vào biểu mẫu tại [mybinder.org](https://mybinder.org).
3. Hệ thống sẽ tự động tạo lệnh tạo huy hiệu; sao chép mã đó vào đầu notebook jupyter của bạn.
4. Nhớ kiểm tra bằng cách nhấn vào huy hiệu.
Để xem ví dụ, tham khảo trang [Giảm nhiễu](../Enhancement/Noise_attenuation.ipynb).
