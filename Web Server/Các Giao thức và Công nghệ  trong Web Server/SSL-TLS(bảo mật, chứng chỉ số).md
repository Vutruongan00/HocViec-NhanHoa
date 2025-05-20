# SSL/TLS là gì?
**SSL（Secure Sockets Layer**）/**TLS（Transport Layer Security)** là kỹ thuật mã hóa truyền tin trên internet. Sử dụng SSL/TLS , bằng việc mã hóa data truyền tin giữa máy tính và server thì có thể phòng tránh bên thứ ba nghe trộm hoặc giả mạo data. Đây là công nghệ đứng sau HTTPS.

- TLS: Là phiên bản nâng cấp và thay thế SSL, do IETF phát triển.

# **Phân loại**

**Có 5 SSL **

- Domain Validation (DV SSL)
  - Chứng thư số SSL chứng thực cho Domain Name – Website. Khi 1 Website sử dụng DV SSL thì sẽ được xác thực tên domain, website đã được mã hoá an toàn khi trao đổi dữ liệu.
- Organization Validation (OV SSL)
  - Chứng thư số SSL chứng thực cho Website và xác thực doanh nghiệp đang sở hữu website đó 
- Extended Validation (EV SSL)
  - Cho khách hàng của bạn thấy Website đang sử dụng chứng thư SSL có độ bảo mật cao nhất và được rà soát pháp lý kỹ càng.
- Subject Alternative Names (SANs SSL)
  - Nhiều tên miền hợp nhất trong 1 chứng thư số:
  - Một chứng thư số SSL tiêu chuẩn chỉ bảo mật cho duy nhất một tên miền đã được kiểm định. Lựa chọn thêm SANs chỉ với chứng thư duy nhất bảo đảm cho nhiều tên miền con. SANs mang lại sự linh hoạt cho người sử dụng, dễ dàng hơn trong việc cài đặt, sử dụng và quản lý chứng thư số SSL. Ngoài ra, SANs có tính bảo mật cao hơn Wildcard SSL, đáp ứng chính xác yêu cầu an toàn đối với máy chủ và làm giảm tổng chi phí triển khai SSL tới tất cả các tên miền và máy chủ cần thiết.
  - Chứng thư số SSL SANs có thể tích hợp với tất cả các loại chứng thư số SSL của GlobalSign bao gồm: Chứng thực tên miền (DV SSL), Chứng thực tổ chức doanh nghiệp (OV SSL) và Chứng thực mở rộng cao cấp (EV SSL).
- Wildcard SSL Certificate (Wildcard SSL)
  - Sản phẩm lý tưởng dành cho các cổng thương mại điện tử. Mỗi e-store là một sub-domain và được chia sẻ trên một hoặc nhiều địa chỉ IP. Khi đó, để triển khai giải pháp bảo bảo mật giao dịch trực tuyến (đặt hàng, thanh toán, đăng ký & đăng nhập tài khoản,…) bằng SSL, chúng ta có thể dùng duy nhất một chứng chỉ số Wildcard cho tên miền chính của website và tất cả sub-domain.

# Cơ chế kỹ thuật của SSL/TLS

Khi người dùng gửi yêu cầu kết nối đến server đối tượng, server gửi SSL server certificate có chứa `puplic key` . Ở trình duyệt của người dùng sử dụng root certificate được cài đặt sẵn trong trình duyệt để kiểm chứng SSL server certificate. Nếu không có vấn đề gì thì mã hóa khóa chung của chính nó bằng `puplic key` được gửi từ server rồi gửi đến server. Phía server thì bằng cách giải mã bằng `secret key` , lấy được khóa chung và thực hiện truyền tin bằng cách sử dụng khóa chung này.

 ![image](https://github.com/user-attachments/assets/5ca6faac-898d-4ac4-928a-8534a27626db)

# **Tại sao nên sử dụng SSL?**

Khi ta đăng ký tên miền để sử dụng các dịch vụ website, email v.v… luôn có những lỗ hổng bảo mật cho hacker tấn công, SSL bảo vệ website và khách hàng của chúng ta
- An toàn dữ liệu: dữ liệu không bị thay đổi bởi hacker.
- Bảo mật dữ liệu: dữ liệu được mã hóa và chỉ người nhận đích thực mới có thể giải mã.
- Chống chối bỏ: đối tượng thực hiện gửi dữ liệu không thể phủ nhận dữ liệu của mình.

Tiêu chuẩn xác thực – SSL chỉ được cung cấp bởi các đơn vị cấp phát chứng thư (CA) có uy tín trên toàn thế giới sau khi đã thực hiện xác minh thông tin về chủ thể đăng ký rất kỹ càng mang lại mức độ tin cậy cao cho người dùng Internet và tạo nên giá trị cho các website, doanh nghiệp cung cấp dịch vụ.

# **Lợi ích khi sử dụng SSL là gì?**

- Xác thực website, giao dịch.
- Nâng cao hình ảnh, thương hiệu và uy tín doanh nghiệp.
- Bảo mật các giao dịch giữa khách hàng và doanh nghiệp, các dịch vụ truy nhập hệ thống.
- Bảo mật webmail và các ứng dụng như Outlook Web Access, Exchange, và Office Communication Server.
- Bảo mật các ứng dụng ảo hó như Citrix Delivery Platform hoặc các ứng dụng điện toán đám mây.
- Bảo mật dịch vụ FTP.
- Bảo mật truy cập control panel.
- Bảo mật các dịch vụ truyền dữ liệu trong mạng nội bộ, file sharing, extranet.
- Bảo mật VPN Access Servers, Citrix Access Gateway …
- Website không được xác thực và bảo mật sẽ luôn ẩn chứa nguy cơ bị xâm nhập dữ liệu, dẫn đến hậu quả khách hàng không tin tưởng sử dụng dịch vụ.
