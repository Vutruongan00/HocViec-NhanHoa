# Hiệu suất và tối ưu hóa trong FTP Server

## 1. Tối ưu băng thông và tốc độ truyền tải
**Mục đích:** Kiểm soát lượng băng thông mà FTP server sử dụng để tránh làm quá tải mạng hoặc để phân bổ tài nguyên hợp lý giữa các người dùng.

### a. vsftpd (Ubuntu/Linux)
- **Directive**: `anon_max_rate` (cho người dùng ẩn danh), `local_max_rate` (cho người dùng cục bộ). Giá trị tính bằng byte/giây
- **Ví dụ** cấu hình trong file `/etc/vsftpd/vsftpd.conf`
```bash!
local_max_rate=1048576    #Giới hạn tốc độ download/upload cho người dùng cục bộ là 1 MB/s

anon_max_rate=524288    #Giới hạn tốc độ download/upload cho người dùng ẩn danh là 512 KB/s
```
### ProFTPD (Ubuntu/Linux)
- **Directive**: `mod_shaper`
- **Ví dụ** thêm cấu hình sau vào cuối file `/etcproftpd/proftpd.conf`
```bash!
IfModule mod_shaper.c>
   ShaperEngine on
   ShaperLog /var/log/proftpd/shaper.log
   ShaperTable /var/log/proftpd/shaper.tab
   
   ShaperAll downrate 100 uprate 100

   ShaperControlsACLs info allow user *

   ShaperControlsACLs all,sess allow group ftpadm
 </IfModule>
```

>- `ShaperEngine on`: Bật tính năng giới hạn băng thông.
>- `ShaperLog / ShaperTable`: Chỉ định nơi lưu trữ nhật ký và dữ liệu nội bộ của shaper.
>- `ShaperAll downrate 100 uprate 100`: Đặt tổng băng thông tối đa cho toàn bộ máy chủ là 100 KB/giây cho cả tải xuống và tải lên. Băng thông này sẽ được chia sẻ giữa tất cả các kết nối đang hoạt động.
>- `ShaperControlsACLs info allow user` *: Cho phép tất cả người dùng xem thông tin về giới hạn băng thông.
>- `ShaperControlsACLs all,sess allow group ftpadm`: Cho phép nhóm ftpadm quyền thay đổi cài đặt giới hạn băng thông cho cả toàn bộ máy chủ và từng phiên riêng lẻ

### c. FileZilla Server (Windows)
- **Cấu hình theo người dùng/nhóm**
- **Server -> Configure -> Users(Groups)** -> `user` -> **tab Limits** -> ...

![image](https://github.com/user-attachments/assets/db7c752c-c4df-4497-b816-47b056d6e72b)

- Áp dụng: có thể áp dụng Linh hoạt cho toàn server hoặc từng người dùng/nhóm riêng lẻ.

### d. IIS FTP (Windows)

## 2. Quản lý số lượng kết nối đồng thời

**Mục tiêu**: Ngăn chặn server bị quá tải do quá nhiều kết nối cùng lúc, đảm bảo tài nguyên được phân bổ hợp lý.
### a. vsftpd (Ubuntu/Linux)
- **Directive**: `max_clients`, `max_per_ip`
- **Ví dụ:** cấu hình trong `/etc/vsftpd/vsftpd.conf`
```bash!
# Số lượng kết nối client tối đa đồng thời tới server
max_clients=200

# Số lượng kết nối tối đa từ một địa chỉ IP duy nhất
max_per_ip=5
```

### b.  ProFTPD (Ubuntu/Linux)
- Ví dụ: thêm cấu hình giới hạn kết nối trong `/etc/proftpd/proftpd.conf`

```bash!
MaxInstances 30

<Limit LOGIN>
  # Số lượng đăng nhập tối đa cho một VirtualHost
  MaxClients 20

  # Số lượng kết nối tối đa cho mỗi người dùng từ một máy chủ (IP) duy nhất
  MaxHostsPerUser 2 

  # Số lần thử đăng nhập tối đa trước khi bị chặn
  MaxLoginAttempts 3
</Limit>
```

### c. FileZilla Server (Windows)
- Cấu hình theo người dùng / nhóm:
Chọn user -> trong tab **General** -> tích chọn **Concurrent session limit** -> tùy chỉnh số lượng kết nối

![image](https://github.com/user-attachments/assets/809bc404-b0ff-42df-8f58-2f9b867adf2e)


### d.  IIS FTP (Windows Server)

![image](https://github.com/user-attachments/assets/7b037493-f7f3-4062-a0c9-565c9bebfc19)

## 3. Sử dụng nén file trước khi truyền
- Giảm kích thước file truyền tải, tăng tốc độ truyền và tiết kiệm băng thông.
- Công cụ:

    - Windows: WinRAR, 7-Zip
    - Linux: `tar` , `zip`
 ```
 zip -r backup.zip /var/www/html/
 tar -czvf backup.tar.gz /etc/nginx/
 ```
 ## 4. Giám sát hiệu suất và khắc phục sự cố
- Mục tiêu: Theo dõi hoạt động của server, phát hiện và giải quyết các vấn đề như kết nối chậm, ngắt kết nối hoặc lỗi.

### a. vsftpd (Ubuntu/Linux)
- **Giám sát:**
  
    - Cấu hình File nhật ký: `xferlog_enable=YES`, `xferlog_file=/var/log/vsftpd.log`, `log_ftp_protocol=YES`.
      
    - Kiểm tra log:
      ```
      tail -f /var/log/vsftpd.log` hoặc `grep "FAIL" /var/log/vsftpd.log
      ```
    - Công cụ hệ thống: `top`, `htop` (để xem tài nguyên CPU/RAM), `netstat -putan | grep :21` hoặc `ss -putan | grep :21` (để xem kết nối)
   
- **Khắc phục sự cố (Troubleshooting):**
  
    - **Kiểm tra dịch vụ**: `sudo systemctl status vsftpd`
    - **Kiểm tra log dịch vụ**: `sudo journalctl -xeu vsftpd`
    - **Firewall (UFW)**: Đảm bảo cổng 21 và dải cổng passive mode (ví dụ 20000-21000) được mở: `sudo ufw status`

### b. ProFTPD (Ubuntu/Linux)
- **Giám sát:**
    - File nhật ký: ProFTPD có các loại log khác nhau: `TransferLog` (ghi lại các phiên truyền file), `SystemLog` (ghi lại các sự kiện của server), `ExtendedLog` (log chi tiết hơn).
- Ví dụ cấu hình ghi log trong `/etc/proftpd/proftpd.conf`, bỏ comment hoặc thêm dòng sau:

```bash!
TransferLog /var/log/proftpd/xferlog.log
SystemLog /var/log/proftpd/proftpd.log
```
- Kiểm tra log:
```
tail -f /var/log/proftpd/proftpd.log
```
