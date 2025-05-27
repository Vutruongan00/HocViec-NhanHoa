# Nguyên lý hoạt động của FTP
## Hoạt động theo mô hình Client-Serer
 **FTP Server (máy chủ)**

- Là một **chương trình phần mềm (daemon)** chạy trên một máy tính server.
- **Lắng nghe các yêu cầu kết nối** từ phía client.
- **Quản lý kho lưu trữ file**, xác thực người dùng và xử lý các lệnh từ client như:
  - Tải lên (upload)
  - Tải xuống (download)
  - Liệt kê thư mục (list)
  - Xóa file (delete)
- Luôn ở trạng thái **"chờ đợ

 **FTP Client** 

- Là một **ứng dụng phần mềm** chạy trên máy tính người dùng.
- **Khởi tạo kết nối** đến FTP server.
- Cung cấp giao diện dòng lệnh hoặc giao diện đồ họa cho người dùng tương tác.
- Gửi các lệnh FTP đến server như:
  - `GET`: tải file về
  - `PUT`: tải file lên
  - `LS`: liệt kê nội dung thư mục
  - `CD`: thay đổi thư mục
- Là bên **chủ động** bắt đầu phiên làm việc.


 **Mối quan hệ Client - Server**
- **Client gửi yêu cầu** (request).
- **Server phản hồi** (response).
- Quá trình giao tiếp được quản lý thông qua **hai kênh kết nối TCP độc lập**:
  - **Kênh điều khiển (Control Channel)** – qua cổng 21
  - **Kênh dữ liệu (Data Channel)** – qua cổng 20 hoặc cổng ngẫu nhiên tùy chế độ


##  Cơ chế kết nối: Kênh điều khiển và Kênh dữ liệu

Giao thức FTP hoạt động dựa trên mô hình cơ bản của việc truyền và nhận dữ liệu từ máy Client đến Server. Quá trình truyền và nhận dữ liệu giữa Client và Server được tạo nên từ TCP logic là 
- **Control Connection**: Là phiên làm việc TCP logic đầu tiên được tạo ra khi quá trình truyền dữ liệu bắt đầu. Nhưng trong tiến trình này chỉ kiểm soát được các thông tin điều khiển đi qua nó. Quá trình này sẽ được duy trì trong suốt quá trình phiên làm việc diễn ra.
- **Data Connection**: Là một kết nối dữ liệu TCP được tạo ra với mục đích riêng là truyền dữ liệu giữa Client và Server. Quá trình truyền tải dữ liệu hoàn tất kết nối dữ liệu này sẽ tự động ngắt kết nối.
    -  Kênh dữ liệu có thể được thiết lập theo hai chế độ khác nhau: **Active Mode** và **Passive Mode**.

### FTP Active Mode
- **Nguyên lý**: Trong chế độ Active, FTP server là bên khởi tạo kết nối kênh dữ liệu đến FTP client.

- **Quy trình**: 
    - **Client**: Kết nối đến cổng 21 của server (kênh điều khiển).
    **Client**: Sau khi xác thực hoặc trước khi truyền dữ liệu, client mở một cổng ngẫu nhiên trên máy tính của mình (ví dụ: cổng N > 1023) và gửi lệnh PORT (ví dụ: PORT <client_IP>, <N>) qua kênh điều khiển để thông báo cho server biết địa chỉ IP và cổng mà client sẽ lắng nghe để nhận kết nối dữ liệu.
    **Server**: Server sau đó khởi tạo một kết nối mới từ cổng 20 của mình đến cổng N mà client đã chỉ định. Đây là kênh dữ liệu.
    **Truyền dữ liệu**: Dữ liệu file hoặc danh sách thư mục được truyền qua kênh này.
Đóng kênh: Sau khi truyền xong, kênh dữ liệu đóng lại. Kênh điều khiển vẫn mở. 
    
- **Nhược điểm**: Chế độ Active thường gặp vấn đề với firewall của client. Vì server là bên khởi tạo kết nối đến một cổng ngẫu nhiên trên client, firewall của client có thể coi đó là một kết nối đến không được yêu cầu và chặn nó, dẫn đến việc truyền file bị lỗi.

    
### FTP Passive Mode
- **Nguyên lý:** Trong chế độ Passive, FTP client là bên khởi tạo kết nối kênh dữ liệu đến FTP server. Đây là chế độ phổ biến hơn và thân thiện với firewall hơn.

- **Quy trình:**

    - Client: Kết nối đến cổng 21 của server (kênh điều khiển).
    - Client: Sau khi xác thực hoặc trước khi truyền dữ liệu, client gửi lệnh PASV qua kênh điều khiển cho server.
    Server: Server mở một cổng ngẫu nhiên trên chính nó (ví dụ: cổng `P > 1023`) và gửi phản hồi chứa địa chỉ IP của server và số cổng `P` đó (ví dụ: `227 Entering Passive Mode (<server_IP>, <P>)` trở lại client qua kênh điều khiển. Server lúc này đang lắng nghe trên cổng `P`.
Client: Client sau đó khởi tạo một kết nối mới từ một cổng ngẫu nhiên của mình đến cổng P mà server đã cung cấp. Đây là kênh dữ liệu.
Truyền dữ liệu: Dữ liệu file hoặc danh sách thư mục được truyền qua kênh này.
Đóng kênh: Sau khi truyền xong, kênh dữ liệu đóng lại. Kênh điều khiển vẫn mở.
    - **Ưu điểm**: Chế độ Passive thân thiện hơn với firewall của client vì client luôn là bên khởi tạo kết nối, và firewall của client thường cho phép các kết nối đi ra và nhận phản hồi. Tuy nhiên, firewall của server cần được cấu hình để cho phép các kết nối đến trên một dải cổng ngẫu nhiên được sử dụng cho chế độ passive.
    
##  3. Quy trình xác thực (Anonymous vs. Authenticated Users)

Khi một FTP client kết nối đến FTP server, bước đầu tiên thường là **xác thực người dùng** để kiểm soát quyền truy cập vào file và thư mục. Có hai hình thức xác thực chính:

---

###  a) Anonymous FTP (FTP ẩn danh)

- **Mục đích:** Cho phép truy cập công cộng mà không cần tài khoản cụ thể. Thường dùng để:
  - Phân phối file công khai
  - Cung cấp phần mềm, tài liệu không yêu cầu xác thực

- **Cách hoạt động:**
  1. Client gửi username là `anonymous` hoặc `ftp`
  2. Mật khẩu thường là địa chỉ email của người dùng (không được xác minh)
  3. Server cấp quyền truy cập hạn chế, **chỉ cho phép đọc** và **tải xuống**, thường từ thư mục `/pub` hoặc tương tự

- **Ưu điểm:**
  - Truy cập dễ dàng, không cần quản lý tài khoản
  - Thích hợp cho môi trường chia sẻ công khai

- **Nhược điểm:**
  - **Bảo mật thấp**
  - Không kiểm soát được ai đang truy cập nếu không có logging

---

### b) Authenticated Users (Người dùng được xác thực)

- **Mục đích:** Cung cấp quyền truy cập cá nhân, được kiểm soát chặt chẽ tới các khu vực cụ thể trên server

- **Cách hoạt động:**
  1. Server yêu cầu `username` và `password` hợp lệ
  2. Client gửi thông tin này qua **kênh điều khiển**
  3. Server kiểm tra thông tin với cơ sở dữ liệu người dùng:
     - Có thể là tài khoản hệ thống
     - Hoặc từ cơ sở dữ liệu riêng của FTP server
  4. Nếu xác thực thành công, người dùng được cấp quyền truy cập dựa trên cấu hình:
     - Ví dụ: `read/write/delete` trong thư mục `/home/user_a`

- **Ưu điểm:**
  - **Bảo mật tốt hơn**
  - Cho phép kiểm soát chi tiết truy cập theo người dùng

- **Nhược điểm:**
  - Yêu cầu quản lý tài khoản người dùng trên server
  - Thông tin xác thực không được mã hóa nếu dùng FTP thuần (có thể cải thiện bằng SFTP hoặc FTPS)

---

 **So sánh nhanh:**

| Tiêu chí          | Anonymous FTP               | Authenticated Users           |
|------------------|-----------------------------|-------------------------------|
| Cần tài khoản     | ❌ Không                    | ✅ Có                         |
| Dữ liệu xác thực  | Địa chỉ email (giả)        | Username & Password thật      |
| Mức độ truy cập   | Hạn chế (chỉ đọc)           | Tuỳ cấu hình: đọc/ghi/xóa     |
| Bảo mật           | Thấp                        | Cao hơn                       |
| Ứng dụng          | Tải file công khai          | Quản lý file cá nhân/có phân quyền |
