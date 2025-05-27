# TỔNG QUAN VỀ FTP SERVER

## 1. FTP là gì? (Giao thức truyền file - File Transfer Protocol)
- **Định nghĩa**: FTP (File Transfer Protocol) là một giao thức chuẩn của mạng máy tính được sử dụng để truyền tải file giữa máy tính client và server trên một mạng TCP/IP. Đây là một trong những giao thức lâu đời nhất và được sử dụng rộng rãi trên Internet.
- **Mục đích**: Mục đích chính của FTP là cho phép người dùng tải lên (upload) và tải xuống (download) file từ/đến một máy chủ từ xa, cũng như quản lý các file và thư mục trên server (ví dụ: tạo thư mục, xóa file, đổi tên).
- **Cổng mặc định**: FTP sử dụng hai cổng TCP riêng biệt:
    - **Cổng 21** (Control Port): Dùng cho kênh điều khiển, để gửi các lệnh (commands) như xác thực người dùng, liệt kê thư mục, yêu cầu tải file, v.v. Kênh này duy trì hoạt động trong suốt phiên làm việc.
    - **Cổng 20** hoặc cổng ngẫu nhiên (Data Port): Dùng cho kênh dữ liệu, để thực sự truyền tải dữ liệu file. Cổng này được mở và đóng cho mỗi lần truyền file.

## 2. Vai trò của FTP Server trong việc truyền dữ liệu
FTP Server (Máychủ FTP) đóng vai trò trung tâm trong quá trình truyền tải dữ liệu bằng giao thức FTP. Các vai trò chính bao gồm:
- **Lưu trữ tập trung:** Cung cấp một không gian lưu trữ tập trung cho các file mà người dùng có thể truy cập từ xa.
- **Quản lý quyền truy cập:** Xác thực người dùng (tên đăng nhập và mật khẩu) và quản lý quyền hạn của họ (ai được phép đọc, ghi, xóa file/thư mục nào). Điều này đảm bảo an toàn và riêng tư cho dữ liệu.
- **Truyền tải file hai chiều**: Cho phép cả việc tải lên (upload) file từ client lên server và tải xuống (download) file từ server về client.
- **Hỗ trợ đa nền tảng:** FTP server và client có thể chạy trên nhiều hệ điều hành khác nhau (Windows, Linux, macOS, Unix), cho phép truyền file giữa các hệ thống không đồng nhất.
- **Tự động hóa:** Thường được sử dụng trong các script hoặc chương trình để tự động hóa việc truyền file (ví dụ: upload báo cáo, download bản cập nhật).
- **Chia sẻ dữ liệu:** Là phương tiện phổ biến để chia sẻ tập tin trong nội bộ mạng hoặc với đối tác/khách hàng.

## **3. Các thành phần chính**

**a. FTP Server (Máy chủ FTP):**
- Là chương trình phần mềm chạy trên 1 máy tính(Server) luôn lắng nghe các yêu cần kết nối FTP đến, quản lý lưu trữ file, xử lý yêu cầu của các client, xác thực người dùng và áp dụng các quyền truy cập.
- **Chức năng**:
    - **Lắng nghe kết nối:** Lắng nghe trên cổng 21 cho các yêu cầu kết nối từ client.
    - **Xác thực:** Kiểm tra tên người dùng và mật khẩu được cung cấp bởi client.
    - **Quản lý hệ thống file**: Cung cấp quyền truy cập vào các file và thư mục, thực hiện các thao tác như đọc, ghi, xóa, liệt kê.
    - **Quản lý phiên**: Duy trì trạng thái của mỗi phiên làm việc với client.
    
**b. FTP Client**
- Là chương trình phần mềm chạy trên máy tính của người dùng (client) dùng để khởi tạo và quản lý kết nối với FTP server.
- **Chức năng:**
    - Kết nối: Mở kết nối đến cổng điều khiển của FTP server.
    - Gửi lệnh: Gửi các lệnh FTP (ví dụ: USER, PASS, LIST, GET, PUT, CWD) đến server.
    - Nhận/Gửi dữ liệu: Quản lý việc truyền tải dữ liệu file qua kênh dữ liệu.
    - Giao diện người dùng: Cung cấp giao diện đồ họa (GUI) hoặc dòng lệnh (CLI) để người dùng tương tác.
- Ví dụ phần mềm: FileZilla Client, WinSCP, Cyberduck, ForkLift (GUI clients), hoặc lệnh ftp trên terminal (CLI client), thậm chí nhiều trình duyệt web cũng có thể hoạt động như một FTP client cơ bản để tải xuống file.

**c. Kết nối mạng (Network Connection):**

Là các đường hầm thông tin giữa FTP client và FTP server, bao gồm hai kênh riêng biệt:
- **Kênh Điều khiển (Control Channel)**:
    - **Cổng**: Luôn sử dụng cổng TCP **21** trên server.
    - **Mục đích**: Dùng để gửi các lệnh FTP từ client đến server và nhận các phản hồi về trạng thái (status codes) từ server. Ví dụ: lệnh đăng nhập, lệnh liệt kê file, lệnh yêu cầu bắt đầu truyền file.
    - **Đặc điểm**: Kênh này duy trì hoạt động trong suốt thời gian của phiên làm việc.

- **Kênh Dữ liệu (Data Channel):**

    - **Mục đích**: Dùng để truyền tải dữ liệu file thực tế (upload/download) và danh sách thư mục.
    - **Cổng**: Cổng này thay đổi tùy thuộc vào chế độ truyền tải:
        - Chế độ Active (Active Mode FTP): Server khởi tạo kết nối dữ liệu đến client.
            - **Client**: Gửi lệnh PORT cho server, cung cấp địa chỉ IP và cổng mà client sẽ lắng nghe để nhận kết nối dữ liệu từ server.
            - **Server**: Mở kết nối từ cổng 20 của nó đến cổng mà client cung cấp.
            - **Vấn đề**: Thường gặp khó khăn với firewall của client vì server cố gắng khởi tạo kết nối vào một cổng ngẫu nhiên trên client.
        - Chế độ Passive (Passive Mode FTP): Client khởi tạo kết nối dữ liệu đến server. Đây là chế độ phổ biến hơn và thân thiện với firewall hơn.
            - **Client**: Gửi lệnh PASV cho server, yêu cầu server mở một cổng ngẫu nhiên để lắng nghe kết nối dữ liệu.
            - **Server**: Mở một cổng ngẫu nhiên (thường là cổng cao >1023) và gửi thông tin về cổng đó cho client.
            - **Client**: Mở kết nối từ một cổng ngẫu nhiên của nó đến cổng ngẫu nhiên mà server vừa cung cấp.
            - **Ưu điểm**: Client luôn là bên khởi tạo kết nối, giúp dễ dàng vượt qua firewall của client.

## Các phương thức truyền dữ liệu trong FTP
Quá trình truyền dữ liệu được thiết lập, dữ liệu sẽ được truyền từ máy Client đến máy Server và ngược lại. FTP có 3 phương thức truyền tải dữ liệu là stream mode, block mode và compressed mode.

- **Stream mode**: Phương thức này hoạt động dựa vào tính tin cậy trong việc truyền dữ liệu trên giao thức TCP. Dữ liệu được truyền đi dưới dạng các byte có cấu trúc không liên tiếp.
- **Block mode:** Là phương thức truyền dữ liệu mang tính quy chuẩn. Dữ liệu được chia thành nhiều block nhỏ và đóng gói thành các FTP blocks.
- **Compressed mode**: Là phương thức truyền dữ liệu kỹ thuật nén dữ liệu khá đơn giản run-length encoding. Các đoạn dữ liệu bị lặp sẽ được phát hiện và loại bỏ.
## . So sánh FTP với các giao thức khác (SFTP, FTPS, HTTP, SCP).


| **Tiêu chí**       | **FTP (File Transfer Protocol)**                                         | **SFTP (SSH File Transfer Protocol)**                                   | **FTPS (FTP Secure)**                                                  | **HTTP (Hypertext Transfer Protocol)**                                 | **SCP (Secure Copy Protocol)**                                         |
|--------------------|-------------------------------------------------------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------------|------------------------------------------------------------------------|------------------------------------------------------------------------|
| **Bảo mật**        | Không mã hóa dữ liệu và thông tin xác thực. Kém an toàn.                | Mã hóa toàn bộ dữ liệu và thông tin xác thực (qua SSH). Rất an toàn.   | Mã hóa dữ liệu và thông tin xác thực (qua SSL/TLS). An toàn.          | Có thể mã hóa (HTTPS) hoặc không (HTTP).                              | Mã hóa toàn bộ (qua SSH). Rất an toàn.                                |
| **Giao thức nền**  | TCP/IP                                                                  | SSH (Secure Shell)                                                     | TCP/IP + SSL/TLS                                                      | TCP/IP (Web)                                                          | SSH (Secure Shell)                                                     |
| **Cổng sử dụng**   | 21 (control), 20/random (data). Hai kênh riêng biệt.                     | 22 (SSH). Một kênh duy nhất.                                            | 21 (control), 20/random (data) hoặc cổng riêng (implicit TLS). Hai kênh. | 80 (HTTP), 443 (HTTPS). Một kênh.                                     | 22 (SSH). Một kênh duy nhất.                                           |
| **Cách truyền file** | Active/Passive mode. Yêu cầu hai kết nối.                             | Luôn là một kênh duy nhất.                                              | Active/Passive mode. Yêu cầu hai kết nối.                              | Dạng yêu cầu/phản hồi web.                                            | Chép file trực tiếp từ/đến server.                                    |
| **Quản lý thư mục**| Có đầy đủ các lệnh quản lý file/thư mục từ xa (ls, cd, mkdir, rmdir).   | Có, hỗ trợ quản lý file/thư mục từ xa.                                  | Có, hỗ trợ quản lý file/thư mục từ xa.                                 | Hạn chế, chủ yếu download. Không quản lý thư mục.                    | Hạn chế, không có lệnh quản lý thư mục từ xa.                          |
| **Độ phức tạp**    | Khá đơn giản.                                                           | Tương đối đơn giản.                                                    | Phức tạp hơn FTP do quản lý chứng chỉ SSL/TLS.                         | Đơn giản cho duyệt web.                                               | Đơn giản cho việc chép file.                                          |
| **Khi nào dùng**   | Truyền file nội bộ mạng an toàn, hoặc khi bảo mật không phải ưu tiên.   | Rất phổ biến cho truyền file an toàn trên Internet.                     | Thay thế an toàn cho FTP truyền thống.                                 | Truy cập web, download file từ website.                               | Chép file nhanh chóng giữa các hệ thống Linux/Unix.                   |


