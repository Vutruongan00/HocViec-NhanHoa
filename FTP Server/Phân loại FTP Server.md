## Phân loại FTP SERVER (theo phương thức truy cập và bảo mật)

### 1. Anonymous FTP (FTP ẩn danh)
---
### 2. Authenticated FTP (FTP được xác thực)
---
### 3. FTPS (FTP Secure)
---
####  Định nghĩa:
FTPS là một **phần mở rộng bảo mật của giao thức FTP**, sử dụng **SSL/TLS** để **mã hóa kênh điều khiển và/hoặc kênh dữ liệu**.

---

#### ⚙️ Cách thức hoạt động:
- Vẫn giữ nguyên **hai kênh** (Control + Data) như FTP.
- Sau khi thiết lập kết nối, **SSL/TLS sẽ được thêm vào** để mã hóa các lệnh và dữ liệu truyền tải.
- Tất cả thông tin đăng nhập, lệnh FTP và file sẽ được **mã hóa hoàn toàn**.

---

####  Các loại FTPS:
- **Implicit FTPS**:
  - Kết nối đến cổng **990**
  - Mã hóa **ngay lập tức** khi kết nối
  - **Ít phổ biến**
  
- **Explicit FTPS (FTPS-AUTH TLS/SSL)**:
  - Kết nối đến cổng **21** như FTP thông thường
  - Client gửi lệnh `AUTH TLS` hoặc `AUTH SSL` để yêu cầu mã hóa
  - **Phổ biến và được khuyến nghị**

---

####  Mục đích sử dụng:
- Truyền tải **dữ liệu nhạy cảm**
- **Tuân thủ các tiêu chuẩn bảo mật** như PCI DSS
- Ví dụ: Hệ thống ngân hàng, tổ chức tài chính

---

####  Ưu điểm:
- **Mã hóa mạnh** với SSL/TLS
- Giữ nguyên toàn bộ **tính năng FTP** (liệt kê file, chuyển file, phân quyền,...)

####  Nhược điểm:
- **Firewall khó cấu hình** do dùng hai kết nối (đặc biệt là Active Mode)
- **Phải quản lý chứng chỉ SSL/TLS**

---

###  4. SFTP (SSH File Transfer Protocol)

####  Định nghĩa:
**SFTP không phải là FTP**. Nó là một giao thức truyền file **hoàn toàn khác**, được xây dựng trên nền tảng của **SSH (Secure Shell)**.

---

####  Cách thức hoạt động:
- Dùng **một kết nối duy nhất**, thường qua cổng **TCP 22** (cổng SSH)
- **Toàn bộ phiên làm việc được mã hóa**, bao gồm:
  - Thông tin đăng nhập
  - Lệnh điều khiển
  - Dữ liệu file
- Là một **subsystem của SSH**, rất phổ biến trên máy chủ Linux/Unix

---

####  Mục đích sử dụng:
- Truyền tải **file an toàn** qua Internet
- Quản lý file từ xa trên **Linux Server**
- Tích hợp vào **script tự động sao lưu**, CI/CD

---

####  Ưu điểm:
- **Bảo mật cao**: Mã hóa toàn bộ phiên làm việc
- **Dễ cấu hình firewall**: Chỉ cần mở một cổng duy nhất (22)
- Hỗ trợ **xác thực bằng password hoặc SSH key**
- Hỗ trợ **quản lý file**: copy, rename, delete, mkdir,...

####  Nhược điểm:
- **Không tương thích với FTP** truyền thống
- Cần client/server hỗ trợ **SFTP riêng**
- **Hiệu suất có thể thấp hơn FTP/FTPS** do chi phí mã hóa


 

