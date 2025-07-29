
> # DirectAdmin - Overview

# 1. Giới thiệu DirectAdmin

- **DirectAdmin** là một trong những bảng điều khiển (control panel) quản lý web hosting phổ biến nhất hiện nay, được sử dụng để đơn giản hóa việc quản lý máy chủ web, các tài khoản hosting, website, email, cơ sở dữ liệu và nhiều thứ khác. Nó nổi bật với sự đơn giản, tốc độ và tính ổn định.

- DirectAdmin là một phần mềm trả phí, chạy trên các hệ điều hành dựa trên Unix (như CentOS, Ubuntu, Debian, FreeBSD, CloudLinux) để cung cấp giao diện đồ họa thân thiện với người dùng, giúp **quản trị viên (admin)**, **đại lý (reseller)** và **người dùng cuối (user)** dễ dàng quản lý các dịch vụ web hosting mà không cần kiến thức chuyên sâu về dòng lệnh.

### Tính năng nổi bật:

* **Dễ sử dụng:** Giao diện của DirectAdmin được thiết kế trực quan, gọn gàng, giúp người dùng dễ dàng làm quen và thao tác, ngay cả với những người mới bắt đầu.
* **Tốc độ cao:** DirectAdmin được tối ưu để sử dụng ít tài nguyên hệ thống, giúp máy chủ hoạt động nhanh chóng và hiệu quả, lý tưởng cho cả VPS cấp thấp và máy chủ chuyên dụng.
* **Độ ổn định cao:** Nó có khả năng tự động khôi phục từ các sự cố, giảm thiểu thời gian ngừng hoạt động (downtime) và gửi thông báo cho quản trị viên khi có vấn đề.
* **Phân quyền người dùng linh hoạt:** DirectAdmin cung cấp ba cấp độ truy cập chính:

  * **Admin:** Có toàn quyền quản lý máy chủ, tạo và quản lý các tài khoản đại lý (reseller) và người dùng (user), cấu hình các dịch vụ hệ thống (Apache, PHP, MySQL), quản lý IP, sao lưu/khôi phục dữ liệu, v.v.
  * **Reseller:** Có quyền tạo và quản lý các tài khoản người dùng, thiết lập gói tài nguyên, quản lý DNS và email cho khách hàng của mình.
  * **User:** Quản lý website, email, cơ sở dữ liệu, tài khoản FTP, DNS, xem thống kê tài nguyên, v.v. cho các tên miền của họ.
* **Quản lý tên miền và DNS:** Cho phép thêm, xóa, sửa đổi tên miền, tên miền phụ (subdomain), bản ghi DNS (A, CNAME, MX, NS, PTR) một cách dễ dàng.
* **Quản lý Email:** Tạo tài khoản email POP/IMAP, địa chỉ chuyển tiếp (forwarder), danh sách gửi thư (mailing list), thư trả lời tự động (autoresponder), và truy cập webmail. Hỗ trợ lọc email, xác thực SMTP.
* **Quản lý FTP:** Tạo, chỉnh sửa, xóa tài khoản FTP, thiết lập quyền truy cập.
* **Quản lý cơ sở dữ liệu:** Hỗ trợ MySQL và MariaDB, cho phép tạo và quản lý các cơ sở dữ liệu, người dùng cơ sở dữ liệu thông qua phpMyAdmin tích hợp.
* **Sao lưu và khôi phục:** Cho phép người dùng tạo các bản sao lưu đầy đủ hoặc từng phần của website và khôi phục chúng khi cần.
* **Bảo mật:** Tích hợp các tính năng bảo mật như tường lửa (CSF), quản lý chống tấn công Brute Force, hỗ trợ Let's Encrypt cho SSL miễn phí.
* **Tùy biến cao:** Cho phép tùy chỉnh giao diện (skin) và các cài đặt khác để phù hợp với nhu cầu.

### Yêu cầu hệ thống tối thiểu:**

* **Bộ xử lý (CPU):** 500 MHz
* **RAM:** 1GB (khuyến nghị 2GB trở lên), với ít nhất 2GB swap
* **Dung lượng ổ cứng:** Tối thiểu 2GB trống (sau khi cài đặt hệ điều hành Linux)
* **Hệ điều hành:** Tương thích với CloudLinux, Red Hat, Fedora Core, Red Hat Enterprise Linux, CentOS, Ubuntu và Debian.

> - Mặc dù cấu hình tối thiểu này đủ để chạy DirectAdmin, nhưng lượng tài nguyên dành cho website có thể hạn chế, gây chậm trễ và quá tải khi có nhiều traffic. 
> - Để có trải nghiệm mượt mà, đề xuất sử dụng VPS với ít nhất 2 CPU, RAM từ 2GB, và ổ cứng từ 15GB trở lên.

### Ưu điểm nổi bật so với các Control Panel khác (ví dụ cPanel):**

* **Đơn giản và nhẹ nhàng hơn:** Giao diện ít rườm rà, tiêu thụ ít tài nguyên hơn.
* **Chi phí hợp lý:** Thường có giá cả cạnh tranh và chính sách cấp phép linh hoạt.
* **Tốc độ xử lý nhanh:** Được tối ưu để hoạt động hiệu quả.
* **Ổn định và đáng tin cậy:** Khả năng tự động phục hồi giúp duy trì hoạt động liên tục.

### Nhược điểm:

- **Tính năng và Plugin ít hơn:** Không phong phú bằng các đối thủ (như cPanel), đôi khi cần cài đặt thêm bên ngoài.
- Cần kiến thức kỹ thuật khi tùy biến sâu: Để tối ưu hoặc cấu hình phức tạp, bạn vẫn cần hiểu biết về Linux và cấu hình server.

---

# 2. Giao diện tổng quan

## Những cấp độ user trong DirectAdmin

### Administrator:

* Cấp độ user cao nhất của DirectAdmin.
* Quyền chỉnh sửa và thay đổi cấu hình của toàn bộ hệ thống.
* Có thể tạo và quản lý Reseller và User.

### ***Reseller:***

* Cấp bậc sau Administrator.
* Chỉ có quyền quản trị và thay đổi cấu hình của nhóm User mà Reseller tạo ra.
* Không có quyền can thiệp vào cấu hình của các Reseller khác.

### ***User:***

* Cấp độ có quyền hạn thấp nhất.
* Tài khoản được tạo ra bởi Admin hoặc Reseller.
* Chỉ có quyền thay đổi thông tin liên quan đến tài khoản của mình.

---

## Giao diện Menu chính
- Đây là nơi quản trị viên có thể theo dõi tình trạng hoạt động của các dịch vụ và tài nguyên hệ thống. 

- Click vào biểu tượng DirectAdmin ở góc trên bên trái

<img width="1897" height="910" alt="image" src="https://github.com/user-attachments/assets/b4b72e43-10be-4a87-b1db-f5ab1a8aeea9" />

- **Menu**:
    - Hiển thị toàn bộ các nhóm chức năng cấu hình trong hệ thống.
    - Cho phép truy cập nhanh đến các khu vực như: Account Manager, Server Manager, Admin Tools, System Info & Files, v.v. 

- **Widget**: Khu vực hiển thị các khối thông tin nhanh (widget) về tài nguyên và dịch vụ

- <img width="1429" height="813" alt="image" src="https://github.com/user-attachments/assets/5487c541-6177-4e6f-b138-f1c014bf388f" />

    - **SERVICE**: Tên dịch vụ (ví dụ: dovecot, exim, httpd, mysqlid, php-fpm, sshd, v.v.)
    - **PID(S)**: Mã tiến trình (Process ID) tương ứng với từng dịch vụ
    - **Memory Usage**: Dung lượng RAM mà dịch vụ đang sử dụng (hiển thị theo MB)

- **Bên phải: Các nút chức năng thống kê và giám sát**: Gồm các nút chính, mỗi nút có tùy chọn **"VIEW MORE"** để xem chi tiết:

    * **Reseller Stats** – Thống kê tài khoản đại lý
    * **List Users** – Danh sách người dùng bạn đang quản lý
    * **Admin Stats** – Thống kê tài khoản quản trị viên
    * **All Users** – Danh sách toàn bộ tài khoản trên máy chủ
    * **Mail Queue** – Hàng đợi email đang chờ gửi
    * **Services** – Giám sát tất cả dịch vụ đang chạy

---

## My Messages - Thông báo
Hiển thị các thông báo hệ thống, cảnh báo hoặc cập nhật.

<img width="1902" height="673" alt="image" src="https://github.com/user-attachments/assets/54881dd2-53e3-4755-a0b1-18c0c150d369" />

---

## User Profile - Thông tin người dùng
Truy cập thông tin tài khoản đang đăng nhập:

<img width="1895" height="925" alt="image" src="https://github.com/user-attachments/assets/5cf35c6e-21ff-45e1-9dfd-46b9772773ff" />

- **Thông tin cá nhân (Personal Information)**
- **Quản lý mật khẩu (Password Management)**
- **Cài đặt thông báo (Notification Settings)**
- **Two-Step Authentication**: bật xác thực 2 lớp 
- **Skin Options**: Tùy chỉnh giao diện hiển thị (giao diện sáng/tối, bố cục...).

<img width="1892" height="918" alt="image" src="https://github.com/user-attachments/assets/aa0f8b22-da73-4a85-9027-c6742640be02" />

## Language - tùy chọn ngôn ngữ

