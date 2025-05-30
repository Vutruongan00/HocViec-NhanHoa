# Khắc phục sự cố thường gặp trong FTP server

## 1. Lỗi Kết Nối
### a. **Nguyên nhân phổ biến:**

- Dịch vụ FTP server không chạy.
- Tường lửa (Firewall) trên server hoặc mạng đang chặn các cổng FTP (cổng điều khiển 21 và các cổng dữ liệu cho chế độ Passive).
- Cấu hình chế độ Passive Mode sai (đặc biệt khi server nằm sau NAT/router).
- IP của client bị chặn bởi server (bởi quy tắc cấu hình FTP hoặc tường lửa).
- Lỗi cấu hình SSL/TLS (nếu sử dụng FTPS).
- Mất kết nối mạng giữa client và server.

### b. Cách xử lý

**vsftpd**
- Kiểm tra port:
```bash!
sudo ufw allow 20:21/tcp
sudo ufw allow 40000:50000/tcp  # nếu dùng passive mode
```
- Cấu hình passive trong `/etc/vsftpd.conf`:
```bash!
pasv_enable=YES
pasv_min_port=40000
pasv_max_port=50000
pasv_address=your.public.ip
```
**ProFTPD (Linux)**
- Mở passive port trong firewall:
```
sudo ufw allow 49152:49200/tcp
```
- Cấu hình passive trong `/etc/proftpd/proftpd.conf`
```
PassivePorts 49152 49200
```
**FileZilla Server (Windows)**
- Kiểm tra Windows Defender Firewall:

    - Cho phép port 21 (hoặc port tùy chỉnh).

    - Cho phép dải passive

**IIS FTP (Windows Server)**
- Dùng **Windows Firewall with Advanced Security** để mở port:
```powershell!
New-NetFirewallRule -DisplayName "FTP" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 21
```

## 2. Lỗi quyền truy cập
### a. **Nguyên nhân phổ biến:**
    - File/folder không có quyền đọc/ghi phù hợp.
    - Tài khoản FTP chưa được gán đúng thư mục.
    - Truy cập ẩn danh bị cấm.

### b. Cách xử lý

**vsftpd**:
- Không cho anonymous login: `anonymous_enable=NO
`
- Gán quyền thư mục:
```
sudo chown ftpuser:ftpuser /home/ftpuser
sudo chmod 755 /home/ftpuser
```
**ProFTPD**:
- Kiểm tra quyền user home:
```bash!
<Directory /home/ftpuser>
  Umask 022 022
  AllowOverwrite on
</Directory>
```

## 3.Vấn đề hiệu suất
### a. Nguyên nhân phổ biến:
- Băng thông hạn chế.
- Quá nhiều kết nối đồng thời.
- Cấu hình passive không hợp lý gây timeout.

### Cách xử lý:

**vsftpd (Ubuntu/Linux)**
- Kiểm tra giới hạn tốc độ: `local_max_rate` và `anon_max_rate` trong file cấu hình 
```
sudo nano /etc/vsftpd/vsftpd.conf
```
- Kiểm tra kết nối: `max_clients` và `max_per_ip`
- Kiểm tra tài nguyên hệ thống: `top`, `htop`,... 
- Kiểm tra **Log**: `sudo tail -f /var/log/vsftpd.log`  để xem có quá nhiều kết nối không
-> Xử lý: Tăng giá trị `max_rate`, `max_clients` nếu server có đủ tài nguyên. Nâng cấp phần cứng nếu tài nguyên quá tải.

**ProFTPD**:
- Kiểm tra các vấn đề tương tự như trên*
- Log: `sudo tail -f /var/log/proftpd/proftpd.log` để xem thông tin kết nối cũng như các lỗi

**FileZilla Server (Windows)**
**Microsoft IIS FTP (Windows)**
