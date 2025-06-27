> # Lỗi thường gặp

# 1. Mixed content

- **Lỗi Mixed Content** xảy ra khi một trang web được tải qua kết nối bảo mật HTTPS, nhưng lại có một số tài nguyên (ví dụ: hình ảnh, video, stylesheet, script, font, iframe) trên trang đó được tải qua kết nối không an toàn HTTP.

![image](https://github.com/user-attachments/assets/05f42c1e-df28-43b5-88f2-2d99abcd2aa2)

- **Cách khắc phục:**
    - **Kiểm tra mã nguồn**: Duyệt qua mã nguồn HTML, CSS, và JavaScript của trang web để tìm và thay thế tất cả các URL `http://` bằng `https://`
    - Sử dụng tìm kiếm & thay thế trong cơ sở dữ liệu: Đối với các hệ thống quản lý nội dung (CMS) như WordPress, các URL thường được lưu trong cơ sở dữ liệu. Sử dụng plugin (như Better Search Replace) hoặc các script chuyên dụng để thay thế `http://yourdomain.com` bằng `https://yourdomain.com` trong cơ sở dữ liệu.
    - **Sử dụng công cụ kiểm tra**: Các công cụ kiểm tra Mixed Content trực tuyến (ví dụ: Why No Padlock) hoặc các công cụ dành cho nhà phát triển trong trình duyệt (Console tab) có thể giúp bạn xác định các tài nguyên gây lỗi.

# 2. ERR_CERT_COMMON_NAME_INVALID

- **Mô tả:** Lỗi này xảy ra khi tên miền trên chứng chỉ SSL/TLS không khớp với tên miền mà người dùng đang cố gắng truy cập. Trình duyệt không thể xác minh danh tính của trang web.

![image](https://github.com/user-attachments/assets/47e7706d-fee2-4855-93a1-43231b7ab88d)

- **Cách khắc phục:**
    - Kiểm tra tên miền trên chứng chỉ
    - Cấp lại hoặc mua chứng chỉ mới: Nếu tên miền không khớp, cần cấp lại chứng chỉ (reissue) để bao gồm tất cả các tên miền và tên miền phụ cần thiết (sử dụng chứng chỉ SAN hoặc Wildcard phù hợp).
    - Kiểm tra chuyển hướng: Đảm bảo rằng tất cả các biến thể của tên miền (có www và không www) đều được chuyển hướng đến cùng một địa chỉ HTTPS được bảo vệ bởi chứng chỉ.
    
# 3. SSL handshake failure

- **Mô tả:** Đây là một lỗi chung xảy ra khi máy khách (trình duyệt) và máy chủ không thể thiết lập một kết nối TLS an toàn trong quá trình bắt tay. Điều này có thể do nhiều nguyên nhân khác nhau.

![image](https://github.com/user-attachments/assets/571e4f06-4b5b-4afa-8d79-0cd26dd645d6)

- **Nguyên nhân:**
    - Không tương thích giao thức/mã hóa: Máy chủ chỉ hỗ trợ các giao thức hoặc bộ mã hóa mà máy khách không hỗ trợ (hoặc ngược lại). Ví dụ, máy chủ chỉ bật TLS 1.3 nhưng máy khách cũ chỉ hỗ trợ TLS 1.2.
    - Cấu hình SSL sai trên máy chủ: Có lỗi trong file cấu hình SSL của web server (thiếu file, sai đường dẫn, quyền truy cập).
    - Lỗi chuỗi chứng chỉ (Certificate Chain): Máy chủ không gửi đầy đủ các chứng chỉ trung gian hoặc gửi sai thứ tự, khiến trình duyệt không thể xây dựng chuỗi tin cậy đến Root CA.
    - Khóa riêng tư không khớp: Khóa riêng tư được sử dụng trên máy chủ không khớp với khóa công khai trong chứng chỉ.
    - Tường lửa/Proxy: Tường lửa hoặc proxy chặn các cổng hoặc giao thức liên quan đến SSL/TLS.


- **Cách khắc phục:**
    - Kiểm tra cấu hình máy chủ: 
        - Đảm bảo SSLEngine on (Apache), listen 443 ssl (NGINX), hoặc tương đương đã được bật.
        - Kiểm tra đường dẫn tới `SSLCertificateFile`, `SSLCertificateKeyFile`, và `SSLCertificateChainFile` (Apache) hoặc `ssl_certificate`, `ssl_certificate_key`, `ssl_trusted_certificate` (NGINX) là chính xác và có quyền truy cập.
        - Đảm bảo các file .key, .crt, .pem không bị hỏng hoặc lỗi định dạng.
    - **Sử dụng SSL Labs**: Chạy kiểm tra SSL Labs SSL Server Test. Nó sẽ chỉ ra chính xác nguyên nhân lỗi bắt tay, bao gồm các vấn đề về giao thức, cipher suite và chuỗi chứng chỉ.
    - **Cập nhật cấu hình giao thức/cipher suite**: Cấu hình máy chủ để hỗ trợ một dải giao thức và cipher suite rộng hơn nhưng vẫn an toàn (ví dụ: TLS 1.2 và TLS 1.3, với các cipher suite mạnh).
    - **Kiểm tra nhật ký lỗi**: Xem nhật ký lỗi của web server (ví dụ: error.log của Apache/NGINX) để tìm các thông báo lỗi liên quan đến SSL/TLS.
    - **Tái tạo khóa/chứng chỉ**: Nếu nghi ngờ khóa riêng tư không khớp, hãy tạo lại CSR với khóa riêng tư mới và yêu cầu CA cấp lại chứng chỉ.

# 4. Expired certificate (Chứng chỉ hết hạn)
![image](https://github.com/user-attachments/assets/a1af2cb7-0d10-4239-983d-d83581c3850f)

- **Cách khắc phục:**
    - Gia hạn chứng chỉ
    - Cài đặt chứng chỉ mới
    - Kiểm tra tự động gia hạn (đối với Let's Encrypt)

# 5. Không tin cậy do thiếu intermediate cert (Lỗi chuỗi chứng chỉ)
