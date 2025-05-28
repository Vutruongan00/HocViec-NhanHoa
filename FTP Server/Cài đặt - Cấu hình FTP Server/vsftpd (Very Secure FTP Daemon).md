
# vsftpd (Very Secure FTP Daemon)
**vsftpd** là một trong những máy chủ FTP phổ biến và được tin cậy nhất trong các hệ thống Linux/Unix. Như tên gọi "Very Secure FTP Daemon", nó được thiết kế đặc biệt tập trung vào **bảo mật**, đồng thời vẫn duy trì hiệu suất cao và sự ổn định

**Điểm nổi bật của vsftpd:**
- Bảo mật cao: Đây là ưu tiên hàng đầu trong thiết kế của vsftpd. Nó có nhiều tính năng bảo mật tích hợp sẵn để giảm thiểu rủi ro bị tấn công, như cách ly người dùng (chroot jail), hỗ trợ SSL/TLS (cho FTPS).
- Hiệu suất và nhẹ: vsftpd tiêu thụ rất ít tài nguyên hệ thống (CPU, RAM) và có thể xử lý một lượng lớn kết nối đồng thời một cách hiệu quả.
- Ổn định: Nó là một lựa chọn đáng tin cậy cho các môi trường sản xuất, nơi mà sự ổn định là yếu tố then chốt.
- Hỗ trợ FTPS: Cho phép mã hóa toàn bộ phiên truyền tải file, bao gồm cả thông tin xác thực và dữ liệu, thông qua SSL/TLS.
- Hỗ trợ người dùng cục bộ và ảo: Cho phép xác thực người dùng dựa trên tài khoản hệ thống hoặc thông qua các cơ sở dữ liệu riêng biệt.

## Cài đặt 
> Yêu cầu hệ thống:
> - OS: Linux (Ubuntu, Debian, CentOS, RHEL, Fedora, v.v.)
> - RAM: > 64MB
> - CPU: > 1 core
> - Port: 21(default), Passive Ports: cần mở thêm (ví dụ 30000-31000)

- Cài đặt: ` sudo apt install vsftpd -y`
- Kiểm tra hoạt động: `sudo apt status vsftpd`

![image](https://github.com/user-attachments/assets/5ebd90ed-f661-4341-88b9-9064088c91ae)


## Cấu hình
- Tạo người dùng mới: `sudo adduser user2`
- Tạo thư mục và gán quyền:
```bash!
sudo mkdir -p /home/user2/files_user2
sudo chown nobody:nogroup /home/user2
sudo chmod a-w /home/user2
sudo chown user2:user2 /home/user2/files_user2
```

- Cấu hình file `vsftpd.conf`: 
```
 nano /etc/vsftpd.conf
 ```
Thêm hoặc sửa các dòng như dưới đây:
```bash!
listen=YES
listen_ipv6=NO
listen_port=21

# Cho phép user local
local_enable=YES

# Cho phép ghi file
write_enable=YES

# Chroot để user không thoát ra ngoài thư mục home
chroot_local_user=YES

# Bật passive mode
pasv_enable=YES
pasv_min_port=30000
pasv_max_port=31000

# Tùy chọn bảo mật
user_sub_token=$USER
local_root=/home/$USER/files_user2

# Nhật ký
xferlog_enable=YES
xferlog_std_format=YES
log_ftp_protocol=YES

# Giới hạn user
userlist_enable=YES
userlist_file=/etc/vsftpd.userlist
userlist_deny=NO

```

- Thêm user vừa tạo vào danh sách được phép:
```
echo "user2" | sudo tee -a /etc/vsftpd.userlist
```
- Chặn quyền ghi thư mục `/home/user2`:
```
sudo chown root:root /home/user2
sudo chmod 755 /home/user2
```

**Cấu hình tường lửa (UFW)**
- Mở port 21 và dải port 30000:31000
sudo ufw allow 21/tcp
sudo ufw allow 30000:31000/tcp

- Khởi động lại dịch vụ;
```
sudo systemctl restart vsftpd
sudo systemctl status vsftpd
```

- **Thử kết nối từ Client:**
![image](https://github.com/user-attachments/assets/68bd2182-d68a-4369-a694-417903c5f184)

-->check lại từ Server
![image](https://github.com/user-attachments/assets/7159f8f1-b0c9-4b87-a79d-0cfbdbbb24ca)

