> # Giao thức & Chuẩn liên quan

## 1. HTTPS (SSL over HTTP)

- **Khái niệm:** **HTTPS** (Hypertext Transfer Protocol Secure) chính là giao thức **HTTP** được mã hóa bằng **SSL/TLS**. Thay vì HTTP hoạt động trên cổng 80, HTTPS hoạt động mặc định trên cổng **443**.
- **Cách hoạt động:**
  1. Khi nhập `https://yourdomain.com` vào trình duyệt, trình duyệt sẽ cố gắng thiết lập kết nối tới cổng 443 của máy chủ.
  2. Máy khách và máy chủ thực hiện quy trình **TLS Handshake** (như đã giải thích ở mục 2.2) để thống nhất về phiên bản TLS, bộ mã hóa và thiết lập một khóa phiên đối xứng.
  3. Sau khi handshake thành công, tất cả các yêu cầu và phản hồi HTTP sẽ được mã hóa bằng khóa phiên này trước khi gửi và giải mã khi nhận.
- **Mục đích:**
  - **Bảo mật dữ liệu:** Ngăn chặn việc nghe lén, thay đổi hoặc giả mạo dữ liệu giữa trình duyệt và máy chủ. Điều này cực kỳ quan trọng cho thông tin nhạy cảm như thông tin đăng nhập, chi tiết thẻ tín dụng.
  - **Xác thực danh tính:** Xác minh quá trình  giao tiếp với máy chủ chính xác, không phải kẻ tấn công.
  - **Toàn vẹn dữ liệu:** Đảm bảo dữ liệu không bị thay đổi trong quá trình truyền tải.
- **Tầm quan trọng:** HTTPS đã trở thành tiêu chuẩn bắt buộc cho mọi trang web hiện đại. Các trình duyệt hiện nay sẽ cảnh báo người dùng về các trang HTTP không an toàn, và Google cũng ưu tiên HTTPS trong xếp hạng tìm kiếm.

## 9.2. Ứng dụng SSL/TLS trong Email và các giao thức khác

SSL/TLS không chỉ giới hạn ở giao tiếp web. Nó là nền tảng cho nhiều giao thức ứng dụng khác cần bảo mật dữ liệu.

- **SMTPS (Simple Mail Transfer Protocol Secure):**
  - **Khái niệm:** SMTPS là giao thức **SMTP** được bảo mật bằng SSL/TLS. SMTP dùng để gửi email đi.
  - **Cổng mặc định:** Thường là cổng **465** (Implicit TLS) hoặc cổng **587** (Explicit TLS/STARTTLS).
  - **Mục đích:** Mã hóa quá trình gửi email từ máy khách email đến máy chủ email, hoặc giữa các máy chủ email với nhau, bảo vệ nội dung email và thông tin đăng nhập.
- **IMAPS (Internet Message Access Protocol Secure):**
  - **Khái niệm:** IMAPS là giao thức **IMAP** được bảo mật bằng SSL/TLS. IMAP dùng để truy cập và quản lý email trên máy chủ.
  - **Cổng mặc định:** Thường là cổng **993**.
  - **Mục đích:** Mã hóa quá trình truy cập hộp thư đến, đọc, xóa, sắp xếp email trên máy chủ, bảo vệ nội dung email và thông tin đăng nhập.
- **POP3S (Post Office Protocol 3 Secure):**
  - **Khái niệm:** POP3S là giao thức **POP3** được bảo mật bằng SSL/TLS. POP3 dùng để tải email từ máy chủ về máy khách và xóa chúng trên máy chủ.
  - **Cổng mặc định:** Thường là cổng **995**.
  - **Mục đích:** Mã hóa quá trình tải email về máy tính, bảo vệ nội dung email và thông tin đăng nhập.
- **Các ứng dụng khác:** SSL/TLS cũng được sử dụng để bảo mật nhiều giao thức khác như:
  - **FTPS (File Transfer Protocol Secure):** FTP qua SSL/TLS để truyền tệp an toàn (cổng 990 hoặc cổng 21 với FTPS Explicit).
  - **LDAPS (Lightweight Directory Access Protocol Secure):** LDAP qua SSL/TLS để truy cập dịch vụ thư mục (cổng 636).
  - **VPN (Virtual Private Network):** Một số giải pháp VPN sử dụng TLS để tạo đường hầm an toàn.

## 9.3. TLS Versions và Security Policy (Ví dụ: Tắt TLS 1.0/1.1)

Phiên bản của giao thức TLS được sử dụng là một yếu tố cực kỳ quan trọng đối với bảo mật. Các phiên bản cũ hơn có thể chứa lỗ hổng bảo mật đã biết.

- **Lịch sử các phiên bản:**
  - **SSL 1.0, 2.0, 3.0:** Đã lỗi thời hoàn toàn và có nhiều lỗ hổng nghiêm trọng. **Không bao giờ nên sử dụng.**
  - **TLS 1.0 (RFC 2246):** Ra mắt năm 1999. Mặc dù đã từng là tiêu chuẩn, nhưng nó có các lỗ hổng đã biết (như POODLE, BEAST) và không còn được coi là an toàn.
  - **TLS 1.1 (RFC 4346):** Ra mắt năm 2006. Cải thiện một số điểm so với TLS 1.0 nhưng vẫn có những điểm yếu và không còn được khuyến nghị.
  - **TLS 1.2 (RFC 5246):** Ra mắt năm 2008. Là tiêu chuẩn vàng trong một thời gian dài, cung cấp bảo mật mạnh mẽ và khả năng chống lại nhiều cuộc tấn công đã biết. Hầu hết các trang web vẫn đang sử dụng TLS 1.2.
  - **TLS 1.3 (RFC 8446):** Ra mắt năm 2018. Đây là phiên bản mới nhất và an toàn nhất. Nó đơn giản hóa handshake, loại bỏ các tính năng không an toàn (như RSA key exchange cũ, các cipher suite yếu), và tăng cường bảo mật đáng kể.
- **Chính sách bảo mật (Security Policy):**
  - **Mục đích:** Định cấu hình máy chủ chỉ sử dụng các phiên bản TLS và bộ mã hóa an toàn nhất.
  - **Ví dụ về việc tắt TLS 1.0/1.1:**
    - Các trình duyệt lớn (Chrome, Firefox, Edge, Safari) đã ngừng hỗ trợ TLS 1.0 và TLS 1.1 vào đầu năm 2020.
    - Việc tắt các phiên bản này trên máy chủ là cần thiết để:
      - Loại bỏ các lỗ hổng bảo mật liên quan đến các phiên bản cũ.
      - Đảm bảo tuân thủ các tiêu chuẩn bảo mật hiện hành.
      - Nâng cao điểm số SSL Rating.
    - **Cách thực hiện (như đã đề cập ở mục 7):**
      - **Apache:** `SSLProtocol All -SSLv2 -SSLv3 -TLSv1 -TLSv1.1` (Chỉ cho phép TLS 1.2 và cao hơn).
      - **NGINX:** `ssl_protocols TLSv1.2 TLSv1.3;`
      - **IIS:** Cấu hình thông qua Registry Editor hoặc công cụ IISCrypto để bật/tắt các giao thức và cipher suites.
      - **Tomcat:** `sslProtocol="TLSv1.2+TLSv1.3"` trong Connector.
- **Lợi ích của việc áp dụng chính sách bảo mật chặt chẽ:**
  - **Bảo mật tối đa:** Giảm thiểu bề mặt tấn công.
  - **Hiệu suất tốt hơn:** Các phiên bản TLS mới hơn thường được tối ưu hóa về hiệu suất.
  - **Tuân thủ:** Đáp ứng các yêu cầu bảo mật ngày càng tăng của ngành và các quy định.
