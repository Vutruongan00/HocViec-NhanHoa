**BÁO CÁO KẾT QUẢ CÔNG VIỆC**

1. **NHỮNG VIỆC ĐƯỢC TRIỂN KHAI**
1. **Tìm hiểu Các loại Logs quan trọng trên Linux**
1. **Tìm hiểu các công cụ quản lý logs** 
1. **journalctl,** 
1. **logrotate** 
1. **syslog** 
1. **Security cơ bản trên Linux**
1. **Phân tích logs để phát hiện tấn công cơ bản**
1. **ĐÃ HOÀN THÀNH**
1. **Tìm hiểu Các loại Logs quan trọng trên Linux**
   1. **Hệ thống logs (System Logs)**
   1. **Đường đẫn chính**

\``` /var/log/syslog  (Ubuntu, Debian)

/var/log/messages (CentOS, RHEL) ```

1. **Chức năng**
- Ghi nhận thông tin tổng quát từ toàn hệ thống: kernel, tiến trình, dịch vụ nền (daemons). Nó chủ yếu được sử dụng để lưu trữ các thông tin liên quan đến hệ thống. Về cơ bản, file log này lưu trữ tất cả dữ liệu hoạt động trên toàn hệ thống như mail, cron, daemon, kern, auth,...
- Dùng để chẩn đoán lỗi hệ điều hành, lỗi dịch vụ, hoặc theo dõi hoạt động tổng thể.
- Đây là file log đầu tiên mà các quản trị viên Linux nên kiểm tra nếu có sự cố trên hệ thống.

  **Ví dụ:** Lệnh sau sẽ cho phép theo dõi log hệ thống theo thời gian thực:

![](Aspose.Words.2de371a4-6a42-46cd-8a82-542887225f0d.001.png)

1. **Log dịch vụ cụ thể (Service-specific Logs)**
1. **Apache (Web server)**

   ``` 

   /var/log/apache2/access.log  - ghi lại tất cả các request từ client.

   /var/log/apache2/error.log  - ghi các lỗi khi xử lý request

   ```

1. Nginx

   ``` 

   /var/log/nginx/access.log

   /var/log/nginx/error.log

   ` ````

1. **MySQL/MariaDB**

   ``` /var/log/mysql/error.log  hoặc /var/log/mysqld.log

   Tùy theo distro và cấu hình mà đường dẫn có thể thay đổi.

\````![](Aspose.Words.2de371a4-6a42-46cd-8a82-542887225f0d.002.png)

1. **SSH**
- **Đường dẫn:** 

  ``` /var/log/auth.log (Ubuntu)  hoặc /var/log/secure (CentOS) ```

- **Chức năng**: Ghi lại hoạt động đăng nhập, xác thực người dùng từ xa.

  ![](Aspose.Words.2de371a4-6a42-46cd-8a82-542887225f0d.003.png)

  1. **Log đăng nhập (Authentication/Login Logs)**
1. **Đường dẫn chính**

\``` 

/var/log/auth.log	 (Ubuntu/Debian)

/var/log/secure	(CentOS/RHEL)

\```

1. **Chức năng**
- Ghi nhận tất cả hoạt động liên quan đến xác thực: đăng nhập, sudo, su, SSH login, v.v.
1. **Lệnh ví dụ dùng để phát hiện đăng nhập sai:**

   ``` grep "Failed password" /var/log/auth.log ```

   1. **Log reboot/shutdown (Khởi động/Tắt máy)**
1. **Công cụ sử dụng: last**
- File log: /var/log/wtmp
  - Lệnh kiểm tra lịch sử reboot: 	``` last reboot ```
  - Lệnh kiểm tra các sự kiện shutdown:	``` last -x | grep shutdown ```
1. **Dùng journalctl**
- Nếu hệ thống dùng systemd, có thể dùng lệnh

\```

journalctl --list-boots

journalctl -b -1   # Xem log boot trước đó

\```

1. **Công cụ quản lý logs**
1. **journalctL**
- **journalctl** là công cụ dòng lệnh dùng để xem, truy vấn và lọc log được ghi bởi **systemd-journald**.
- **systemd**-**journald**  thay thế cho các hệ thống logging truyền thống như syslog trên các hệ điều hành dùng systemd (Ubuntu, Debian, CentOS 7+, RHEL 7+,…
- Log được lưu trong **định dạng nhị phân** tại  /var/log/journal/ , và không thể đọc trực tiếp bằng cat, phải dùng **journalctl**
- **Ưu điểm** của journalctl:
  - Truy vấn log cực kỳ linh hoạt, mạnh mẽ.
  - Lọc theo thời gian, dịch vụ, mức độ ưu tiên.
  - Tích hợp sâu với systemd và dịch vụ hệ thống.
  - Có thể dùng làm nguồn dữ liệu cho các hệ thống giám sát tập trung (như ELK, Graylog...).


- **Cách sử dụng cơ bản**

|**Mục đích**|**Lệnh**|
| :-: | :-: |
|Xem toàn bộ log|journalctl|
|Xem log mới nhất (giống tail -f)|journalctl -f|
|Xem log hệ thống kể từ khi boot|journalctl -b|
|Xem log của boot trước|journalctl -b -1|
|Xem log theo thời gian|journalctl --since "2025-05-01" --until "2025-05-02"|
|Xem log gần đây (ví dụ 10 phút)|journalctl --since "10 min ago"|

1. **Cách Lọc log theo tiêu chí cụ thể**
1. **Theo dịch vụ -u (unit)**
- Cú pháp :  ``` journalctl -u <tên\_dịch\_vụ>.service ```
- Ví dụ: 

  ``` journalctl -u ssh.service ```

![](Aspose.Words.2de371a4-6a42-46cd-8a82-542887225f0d.004.png)

1. **Theo mức đọ ưu tiên (-p) (priority)** 

|**Mức**|**Mô tả**|
| :-: | :-: |
|0|Emergency|
|1|Alert|
|2|Critical|
|3|Error|
|4|Warning|
|5|Notice|
|6|Info|
|7|Debug|

- Ví dụ: để xem **log lỗi Erro** :

  ``` journalctl -p 3 -b     # Log lỗi từ lần boot hiện tại ```

![](Aspose.Words.2de371a4-6a42-46cd-8a82-542887225f0d.005.png)

1. **Theo user hoặc PID**
- Theo UID:  ``` journalctl  \_UID=1000```
- Theo PID: ``` journalctl \_PID=1234```
  1. **Các tùy chọn hiển thị hữu ích**

|**Tùy chọn**|**Mô tả**|
| :-: | :-: |
|-f|Theo dõi log mới giống tail -f|
|-n <số\_dòng>|Hiển thị số dòng gần nhất|
|--no-pager|Hiển thị log không phân trang|
|--output=json|Hiển thị log ở dạng JSON (phân tích máy)|

1. **Quản lý dung lượng log**
- Kiểm tra dung lượng log: 

  ``` journalctl --disk-usage ```

![](Aspose.Words.2de371a4-6a42-46cd-8a82-542887225f0d.006.png)

- Xoá log cũ (ví dụ log > 1MB)

  ``` journalctl --vacuum-size=1M ```

- Xoá log cũ hơn 3 ngày:

  ``` journalctl --vacuum-time=3d ```



1. **Logrotate**
- logrotate là một công cụ trong Linux dùng để **quản lý kích thước và vòng đời của file log**. 
- Nó giúp **tự động xoay vòng (rotate)**, **nén (compress)**, **xóa (remove)**, hoặc **gửi log đi nơi khác**, giúp tránh việc log chiếm đầy ổ đĩa.
- File cấu hình chính: 

  ``` /etc/logrotate.conf ```

- Thư mục cấu hình chi tiết theo dịch vụ: 

  ``` /etc/logrotate.d/ ``` 

  ~ mỗi dịch vụ thường có 1 file riêng ở đây, ví dụ: apache2, rsyslog, mysql,...

  1. **Ví dụ cấu hình đơn giản trong mysql:**

![](Aspose.Words.2de371a4-6a42-46cd-8a82-542887225f0d.007.png)

`	`**Giải thích:**

1. daily : xoay vòng  hàng ngày
1. rotate 7 : Giữ lại **7 bản log cũ**, sau đó xóa các bản cũ hơn
1. missingok: **Không báo lỗi** nếu file log không tồn tại
1. create 640 mysql adm: Sau khi xoay log, tạo **file log mới** với:quyền 640 (chỉ owner đọc/ghi, group đọc, người khác không truy cập)- owner: msql – group: adm
1. …

1. **Các tùy chọn thường dùng**

|**Tùy chọn**|**Ý nghĩa**|
| :-: | :-: |
|daily|Xoay vòng hàng ngày|
|weekly|Xoay vòng hàng tuần|
|monthly|Xoay vòng hàng tháng|
|rotate N|Giữ lại N bản log cũ|
|compress|Nén file log cũ (.gz)|
|delaycompress|Nén từ bản log thứ 2 trở đi (tránh nén file đang dùng)|
|notifempty|Không xoay nếu file log rỗng|
|missingok|Không báo lỗi nếu file log không tồn tại|
|create MODE OWNER GROUP|Tạo file log mới với quyền và người sở hữu cụ thể|

1. **SYSLOG**
- Syslog là một giao thức chuẩn để gửi và xử lý log trên Unix/Linux.
- Có thể ghi log cục bộ (local) hoặc gửi đến máy chủ log từ xa (remote logging).
  1. **Phương Thức Hoạt Động của Syslog**
- Syslog hoạt động dựa trên mô hình client-server, trong đó các thiết bị hoặc ứng dụng (client) gửi thông điệp log đến một máy chủ (server) tập trung, nơi các thông điệp được lưu trữ và phân tích. Dưới đây là các thành phần chính trong hệ thống syslog:
  - Syslog Client: Là các thiết bị hoặc ứng dụng gửi thông điệp log. Client có thể là hệ điều hành, ứng dụng, thiết bị mạng hoặc bất kỳ phần mềm nào khác có khả năng tạo log.
  - Syslog Server: Là máy chủ nhận và lưu trữ thông điệp log từ các client. Syslog server thường được cấu hình để lưu trữ log vào file, cơ sở dữ liệu, hoặc hệ thống quản lý log tập trung.
  - Syslog Daemon: Là phần mềm chạy trên hệ điều hành giống Unix, chịu trách nhiệm nhận và xử lý thông điệp log từ các ứng dụng và hệ thống. Ví dụ phổ biến là rsyslog, syslog-ng, và systemd-journald.

1. **Vai trò của Syslog** 
- Syslog mang lại nhiều lợi ích cho quản trị hệ thống và bảo mật:
  - Tập trung hóa quản lý log: Syslog cho phép tập trung tất cả các log từ các thiết bị và ứng dụng vào một vị trí duy nhất, giúp dễ dàng quản lý và phân tích.
  - Phát hiện sự cố nhanh chóng: Bằng cách theo dõi log từ nhiều nguồn khác nhau, syslog giúp phát hiện sự cố sớm và xử lý kịp thời.
  - Tuân thủ quy định: Nhiều quy định và tiêu chuẩn bảo mật yêu cầu lưu trữ và quản lý log. Syslog giúp đảm bảo tuân thủ các yêu cầu này.
  - Bảo mật: Syslog có thể cấu hình để gửi log qua các kết nối bảo mật (ví dụ như TLS), bảo vệ thông tin log khỏi bị truy cập trái phép.
  1. **Hai công cụ chính sử dụng giao thức syslog**

|**Công cụ**|**Mục tiêu / Đặc điểm chính**|
| :-: | :-: |
|rsyslog|Mặc định trên hầu hết các distro (Ubuntu, CentOS, RHEL…)|
|syslog-ng|Phiên bản nâng cao hơn, linh hoạt, mở rộng tốt hơn|

1. **Rsyslog**
- Rsyslog là một trong những daemon syslog phổ biến nhất, được biết đến với hiệu suất cao và tính linh hoạt. Nó hỗ trợ gửi log qua TCP và UDP, có thể cấu hình để lưu log vào nhiều định dạng khác nhau, và hỗ trợ các tính năng như lọc log, gửi log từ xa, và nhiều hơn nữa.
  1. Cấu hình chính của rsyslog

     ``` /etc/rsyslog.conf

     /etc/rsyslog.d/\*.conf ```

  1. Cấu trúc dòng cấu hình
     1. **Syslog-ng**
- Syslog-ng là một daemon syslog mạnh mẽ khác, cung cấp nhiều tính năng mở rộng như lưu trữ log vào cơ sở dữ liệu, lọc và chuyển đổi log, và gửi log qua các kết nối bảo mật. Syslog-ng được thiết kế để xử lý một lượng lớn log với hiệu suất cao.

1. **Security cơ bản trên Linux**
1. Bảo mật đăng nhập
1. Quản lý user và quyền
1. Firewall (Tường lửa)
1. Cập nhật hệ thống
1. Giám sát hệ thống
