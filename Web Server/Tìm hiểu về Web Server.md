
# WEB SERVER

## 1. Định nghĩa Web Server là gì?

**Web Server (máy chủ web)** là một phần mềm hoặc phần cứng dùng để lưu trữ, xử lý và phân phối nội dung web (ví dụ: trang HTML, hình ảnh, video...) đến các thiết bị người dùng thông qua giao thức HTTP hoặc HTTPS.

- Phổ biến nhất hiện nay là các phần mềm Web Server như:

    - Apache HTTP Server

    - Nginx

    - Microsoft IIS

    - LiteSpeed

> Tóm lại: Web Server là “người phục vụ” nội dung web khi người dùng truy cập bằng trình duyệt.

## 2. Vai trò của Web Server trong mô hình Client - Server
- Trong mô hình **Client-Server**:

    - **Client** là thiết bị của người dùng (PC, điện thoại...) gửi yêu cầu (request) truy cập một tài nguyên web (ví dụ: www.example.com).

    - **Web Server (Server)** tiếp nhận yêu cầu, xử lý và gửi phản hồi (response) trở lại cho client.

- **Vai trò chính của Web Server:**

    - Nhận và xử lý các yêu cầu HTTP/HTTPS từ client.

    - Phục vụ các tài nguyên như: file HTML, CSS, JS, hình ảnh, video…
 
    - Giao tiếp với các thành phần khác như cơ sở dữ liệu, ứng dụng phía server để xử lý yêu cầu động (dynamic content).

    - Bảo mật, phân quyền, ghi log, quản lý phiên truy cập.

## 3. Cách thức hoạt động của Web Server

- Web server hoạt động dựa trên mô hình kiến trúc client-server, trong đó máy tính khách (client) gửi yêu cầu đến máy chủ (server) và máy chủ xử lý yêu cầu và trả về kết quả tương ứng.
- Quá trình hoạt động gồm 3 bước chính:

**Bước 1: Nhận Request từ Client**
- Người dùng nhập URL trên trình duyệt →   Trình duyệt gửi **HTTP Request** đến địa chỉ IP của máy chủ web
- Yêu cầu này có thể là GET (lấy tài nguyên), POST (gửi dữ liệu), PUT, DELETE...

**Bước 2: Xử lý Request**
- Web Server tiếp nhận request.

- Nếu là nội dung tĩnh (static content) → đọc file HTML, CSS, ảnh từ ổ cứng.

- Nếu là nội dung động (dynamic) → chuyển yêu cầu đến backend (PHP, ASP.NET, Python...) để xử lý.

**Bước 3: Trả Response**
- Web Server tạo HTTP Response, bao gồm:

    -    Status Code (ví dụ: 200 OK, 404 Not Found)

    -    Header (thông tin phản hồi)

    -    Body (nội dung trang web)

- Gửi về trình duyệt để hiển thị cho người dùng.

## 4. Các loại Web Server phổ biến
### 4.1 Apache HTTP Server
**Đặc điểm:**
- Do Apache Software Foundation phát triển.

- Là Web Server phổ biến nhất trong nhiều năm.

- Hoạt động theo mô hình process/thread-based.

- Hỗ trợ nhiều hệ điều hành: Linux, Unix, Windows, macOS.

**Ưu điểm:**
- Linh hoạt cao: hỗ trợ nhiều module mở rộng như mod_php, mod_ssl, mod_rewrite...

- Tài liệu phong phú, cộng đồng lớn.

- Tương thích tốt với hệ thống LAMP (Linux – Apache – MySQL – PHP).

**Nhược điểm:**
- Hiệu suất giảm khi xử lý nhiều kết nối đồng thời (vì mỗi kết nối tạo một tiến trình/luồng).

- Cấu hình phức tạp hơn so với một số lựa chọn hiện đại

### 4.2 Nginx
**Đặc điểm:**
- Phát triển bởi Igor Sysoev (Nga), năm 2004.

- Kiến trúc event-driven, non-blocking (phi đồng bộ).

- Thiết kế cho hiệu suất cao, tối ưu tài nguyên.

**Ưu điểm:**
- Hiệu suất cao, đặc biệt với các trang có nhiều kết nối đồng thời.

 - Sử dụng ít tài nguyên CPU và RAM hơn Apache.

- Phù hợp làm Reverse Proxy, Load Balancer, Web Server, API Gateway.

- Cấu hình đơn giản, dễ đọc.

**Nhược điểm:**
- Không hỗ trợ module động như Apache (một số tính năng cần biên dịch lại).

- Xử lý PHP gián tiếp thông qua FastCGI (không tích hợp sẵn mod_php như Apache).

### 4.3 Microsoft IIS (Internet Information Services)
**Đặc điểm:**
- Do Microsoft phát triển, chỉ chạy trên hệ điều hành Windows Server.

- Giao diện cấu hình đồ họa thân thiện (GUI).

- Tích hợp tốt với ASP.NET, .NET Core, SQL Server, Active Directory.

**Ưu điểm:**
- Tích hợp chặt chẽ với hệ sinh thái Windows.

- Dễ quản lý qua GUI hoặc PowerShell.

- Hỗ trợ tính năng bảo mật Windows như Kerberos, NTFS Permissions, SSL...

**Nhược điểm:**
- Chỉ hỗ trợ Windows, không thể chạy trên Linux.

- Ít phổ biến hơn trong cộng đồng mã nguồn mở.

- Chi phí bản quyền Windows Server.

### 4.4 Các lựa chọn khác

**a. Lighttpd**
- Nhẹ, tối ưu hiệu năng, dùng cho hệ thống tài nguyên thấp.

- Cấu trúc event-driven như Nginx.

- Hạn chế về tính năng nâng cao.

**b. Caddy**
- Web Server hiện đại, tự động cấp SSL (Let's Encrypt).

- Dễ cấu hình, hỗ trợ HTTP/2, gRPC...

- Cấu hình bằng Caddyfile – đơn giản và dễ đọc.

**c. LiteSpeed**
- Web Server thương mại (có bản miễn phí: OpenLiteSpeed).

- Tương thích 100% Apache config (htaccess, mod_rewrite...).

- Tốc độ cao, tối ưu cho các website WordPress.

| Web Server    | Kiến trúc      | Hệ điều hành | Ưu điểm chính                  | Nhược điểm chính                       |
| ------------- | -------------- | ------------ | ------------------------------ | -------------------------------------- |
| Apache        | Process/Thread | Linux/Win    | Mở rộng mạnh, phổ biến         | Tài nguyên cao, chậm với nhiều kết nối |
| Nginx         | Event-driven   | Linux/Win    | Hiệu suất cao, ít tài nguyên   | Không hỗ trợ module động               |
| Microsoft IIS | Thread/Async   | Windows      | Tích hợp Windows tốt           | Chỉ dùng trên Windows                  |
| Lighttpd      | Event-driven   | Linux        | Nhẹ, nhanh                     | Hạn chế tính năng                      |
| Caddy         | Event-driven   | Linux/Win    | Tự động SSL, cấu hình đơn giản | Cộng đồng nhỏ hơn, ít phổ biến         |
| LiteSpeed     | Event-driven   | Linux        | Hiệu suất cao, hỗ trợ Apache   | Bản thương mại, ít người dùng miễn phí |

## 5. So sánh Web Server mã nguồn mở và bản thương  mại

| Tiêu chí                      | **Mã nguồn mở (Open Source)**               | **Phần mềm thương mại (Commercial)**                   |
| ----------------------------- | ------------------------------------------- | ------------------------------------------------------ |
| **Ví dụ Web Server**          | Apache, Nginx, Lighttpd, OpenLiteSpeed      | Microsoft IIS, LiteSpeed (bản Enterprise)              |
| **Ví dụ HĐH/Khác**            | Linux (Ubuntu, CentOS), MySQL, Zimbra       | Windows Server, cPanel, Oracle DB                      |
| **Chi phí**                   | Miễn phí hoặc chi phí rất thấp              | Có phí bản quyền, chi phí cao                          |
| **Truy cập mã nguồn**         | Có thể xem, chỉnh sửa, phân phối lại        | Không được truy cập mã nguồn                           |
| **Tính linh hoạt & tùy biến** | Cao – có thể chỉnh sửa theo nhu cầu         | Thấp – phụ thuộc vào nhà cung cấp                      |
| **Hỗ trợ kỹ thuật**           | Dựa vào cộng đồng, tài liệu                 | Có đội ngũ hỗ trợ chuyên nghiệp                        |
| **Bảo mật**                   | Nhanh phát hiện và vá lỗi nhờ cộng đồng lớn | Kiểm soát tốt từ nhà phát triển, nhưng vá lỗi chậm hơn |
| **Tốc độ cập nhật**           | Nhanh, thường xuyên cập nhật                | Có lộ trình cập nhật rõ ràng nhưng chậm hơn            |
| **Độ ổn định/Dễ dùng**        | Có thể khó cấu hình, đòi hỏi kỹ thuật       | Giao diện thân thiện, hỗ trợ GUI đầy đủ                |
| **Giấy phép sử dụng**         | GPL, MIT, Apache...                         | Giấy phép độc quyền từ nhà cung cấp                    |

