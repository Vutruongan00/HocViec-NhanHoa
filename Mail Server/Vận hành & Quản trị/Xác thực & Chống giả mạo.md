> # Xác thực & Chống giả mạo

- **Xác thực và chống giả mạo email** là tập hợp các kỹ thuật được thiết kế để xác minh danh tính của người gửi email và đảm bảo rằng email không bị giả mạo hoặc thay đổi trên đường truyền. 
- Mục tiêu chính là **giảm thiểu thư rác (spam)**, **lừa đảo (phishing)**, và các cuộc tấn công giả mạo miền (domain spoofing), đồng thời nâng cao độ tin cậy của email gửi từ miền. 

# 1. Cấu hình DKIM SPF DMARC  (Zimbra Mail Server) 
> Thực Hành...
 - **SPF**: có vai trò  giúp ngăn chặn giả mạo email người gửi
     - SPF cho phép chủ sở hữu tên miền chỉ định rõ ràng những máy chủ email(địa chỉ IP) nào được phép gửi email từ tên miền của họ.
     - SPF được tạo dưới dạng một **bản ghi TXT** trong **DNS** 
 - **DKIM (DomainKeys Identified Mail)**: Cung cấp cơ chế xác thực email bằng chữ ký số.
 - **DMARC (Domain-based Message Authentication, Reporting, and Conformance)**

-  **Kiểm tra các bản ghi:** [tại đây](https://dnschecker.org/all-dns-records-of-domain.php?rtype=TXT&dns=google)


# 2. SMTP AUTH - Xác thực người gửi 

* **Vai trò:** **SMTP AUTH** là một phần mở rộng của giao thức SMTP, cho phép người dùng **xác thực danh tính của họ với MTA (Mail Transfer Agent)** trước khi gửi email. Điều này rất quan trọng để ngăn chặn server của bạn bị lợi dụng làm "open relay" (relay mở) - nơi bất kỳ ai cũng có thể gửi email qua server của bạn mà không cần xác thực, dẫn đến việc server của bạn bị liệt vào danh sách đen và bị lạm dụng để gửi spam.
* **Cách hoạt động:**

  1. Khi một ứng dụng email (MUA) muốn gửi thư, thay vì chỉ đơn thuần gửi lệnh `MAIL FROM`, nó sẽ gửi lệnh `AUTH` đến MTA.
  2. MTA yêu cầu người dùng cung cấp tên đăng nhập và mật khẩu.
  3. Thông tin đăng nhập này được gửi đi dưới dạng mã hóa (thường là qua TLS/SSL).
  4. Nếu xác thực thành công, MTA cho phép người dùng gửi email. Nếu không, kết nối sẽ bị từ chối.
* **Tầm quan trọng:**

  * **Ngăn chặn Open Relay:** Đây là biện pháp chính để ngăn chặn việc server của bạn bị lợi dụng để gửi spam bởi những người không được ủy quyền.
  * **Theo dõi người gửi:** Giúp bạn biết ai đã gửi email từ server của mình (thông qua log xác thực).

# 3. SASL (Simple Authentication and Security Layer)
Giao thức hỗ trợ SMTP AUTH.
* **Cách hoạt động:**

  1. Một ứng dụng (ví dụ: MTA như Postfix) muốn thực hiện xác thực sẽ sử dụng thư viện SASL.
  2. Thư viện SASL sẽ giao tiếp với các plugin SASL cụ thể (ví dụ: `saslauthd` để xác thực với hệ thống PAM, hoặc plugin LDAP để xác thực với Active Directory/LDAP).
  3. Tùy thuộc vào plugin được sử dụng, SASL có thể hỗ trợ nhiều cơ chế xác thực khác nhau như PLAIN, LOGIN, CRAM-MD5, DIGEST-MD5, GSSAPI (Kerberos), v.v.
  4. SASL cũng cung cấp các lớp bảo mật (security layers) tùy chọn, cho phép mã hóa hoặc kiểm tra tính toàn vẹn của dữ liệu trong phiên giao tiếp, ngay cả khi giao thức cơ bản không hỗ trợ mã hóa (mặc dù TLS/SSL vẫn được ưu tiên hơn).
* **Tầm quan trọng:**

  * **Cơ chế xác thực linh hoạt:** SASL cho phép MTA (và MDA/MRA) sử dụng nhiều phương pháp xác thực khác nhau (từ hệ thống Linux cục bộ đến cơ sở dữ liệu LDAP, SQL) mà không cần phải viết lại code cho từng phương pháp.
  * **Tăng cường bảo mật:** Bằng cách cung cấp một khuôn khổ tiêu chuẩn cho xác thực và khả năng thêm các lớp bảo mật, SASL là nền tảng cho việc xác thực người gửi an toàn (SMTP AUTH) cũng như xác thực người dùng truy cập IMAP/POP3.
  * **Tích hợp:** Nó là thành phần cốt lõi cho việc tích hợp xác thực trên các hệ thống mail server lớn.
