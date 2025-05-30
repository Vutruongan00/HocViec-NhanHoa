# ProFTPD

## Hướng dẫn Cài đặt

```bash!
 sudo apt-get update -y
 sudo apt install proftpd -y
```

- Kiểm tra hoạt động: ` sudo systemctl status proftpd `

![image](https://github.com/user-attachments/assets/4ee19bd4-717e-4e36-a8f3-032f58efb498)


## Cấu hình
- **Vị trí các file cấu hình chính:**

     `/etc/proftpd/proftpd.conf`

- Tạo người dùng và thư mục tương tự như vsftpd và FileZilla:
```bash!
sudo adduser user3

sudo mkdir -p /home/user3/ftp/file3
sudo chown nobody:nogroup /home/user3/ftp
sudo chmod a-w /home/user3/ftp

sudo chown user3:user3 /home/user3/ftp/file3
```

- Chỉnh sửa file cấu hình: `/etc/proftpd/proftpd.conf`

```
 sudo nano /etc/proftpd/proftpd.conf
 ```
 
 - Thêm hoặc sửa (bỏ comment #) các dòng sau:
```apache!
DefaultRoot ~
RequireValidShell off
UseFtpUsers on
```
>- `DefaultRoot`: Chỉ định thư mục gốc mà người dùng sẽ bị "nhốt" (chrooted) vào sau khi đăng nhập thành công vào FTP server. Khi người dùng bị "chroot", họ sẽ không thể điều hướng ra khỏi thư mục này, đảm bảo an toàn cho các phần khác của hệ thống file.
>- `RequireValidShell off`: tắt quyền Shell vào hệ thống
>- `UseFtpUsers on` Kích hoạt việc sử dụng tệp /etc/ftpusers để kiểm soát quyền truy cập FTP của các tài khoản người dùng hệ thống. Tệp /etc/ftpusers thường chứa danh sách các tài khoản người dùng hệ thống bị cấm đăng nhập vào FTP server.

- Tùy chỉnh cổng (cũng trong file cấu hình này):
![image](https://github.com/user-attachments/assets/cefff6e9-c02d-4587-94dd-7e334526445d)


- Thiết lập Passive Mode:


![image](https://github.com/user-attachments/assets/9bf66f66-e532-4a05-85fb-557f8441c5cf)


- Mở port trên UFW (nếu bật tường lửa):
```
sudo ufw allow 49152:49200/tcp
```
- Giới hạn băng thông: 
Thêm vào cuối file `/etc/proftpd/proftpd.conf`
```apache!
IfModule mod_shaper.c>
   ShaperEngine on
   ShaperLog /var/log/proftpd/shaper.log
   ShaperTable /var/log/proftpd/shaper.tab
   # An overall rate (in KB/s) must be set.  This line explicitly
   # sets both the download and upload rates to be the same.
   ShaperAll downrate 100 uprate 100

   # Allow all system users to see shaper info
   ShaperControlsACLs info allow user *

   # Allow FTP admins to alter settings both overall and per-session
   ShaperControlsACLs all,sess allow group ftpadm
 </IfModule>
```
>- `ShaperEngine on`: Bật tính năng giới hạn băng thông.
>- `ShaperLog / ShaperTable`: Chỉ định nơi lưu trữ nhật ký và dữ liệu nội bộ của shaper.
>- `ShaperAll downrate 100 uprate 100`: Đặt tổng băng thông tối đa cho toàn bộ máy chủ là 100 KB/giây cho cả tải xuống và tải lên. Băng thông này sẽ được chia sẻ giữa tất cả các kết nối đang hoạt động.
>- `ShaperControlsACLs info allow user` *: Cho phép tất cả người dùng xem thông tin về giới hạn băng thông.
>- `ShaperControlsACLs all,sess allow group ftpadm`: Cho phép nhóm ftpadm quyền thay đổi cài đặt giới hạn băng thông cho cả toàn bộ máy chủ và từng phiên riêng lẻ


- Cấu hình Log:

![image](https://github.com/user-attachments/assets/64c2e83e-b637-4260-a9b2-6118de1fafba)


- test kết nối từ client

![image](https://github.com/user-attachments/assets/c6f9f763-b295-4083-9eea-8f134ff92269)



- Check log trong server 

![image](https://github.com/user-attachments/assets/678783a2-99e7-4f84-ab22-ab40a69622da)


---

## Cấu hình FTPS - SSL/TLS trong proFTPD

- Cài đặt module mod_tls (nếu chưa có)

```bash!
sudo apt install proftpd-mod-crypto
```

- Bật `mod_tls` trong file cấu hình `/etc/proftpd/modules.conf`

```bash!
sudo nano /etc/proftpd/modules.conf
```

 - Thêm hoặc bỏ comment (#) dòng sau:
```
LoadModule mod_tls.c
```

- Tạo chứng chỉ tự ký:
```bash!
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
 -keyout /etc/ssl/private/proftpd.key \
 -out /etc/ssl/certs/proftpd.crt
```

---
hoặc **Cài đặt Certbot để lấy chứng chỉ Let's Encrypt** - nếu có tên miền đã trỏ vào FTP Server
```bash!
sudo apt install certbot -y
```
 - Lấy chứng chỉ SSL cho ftp.antvpro.io.vn
   ```
   sudo certbot certonly --standalone -d ftp.antvpro.io.vn
   ```
---

**- Cấu hình TLS trong `proftpd.conf`**

`sudo nano /etc/proftpd/proftpd.conf`

Thêm cấu hình sau vào cuối file (hoặc chỉnh nếu đã có)
```bash!
<IfModule mod_tls.c>
  TLSEngine                   on
  TLSLog                     /var/log/proftpd/tls.log
  TLSProtocol                TLSv1.2
  TLSRSACertificateFile      /etc/ssl/certs/proftpd.crt
  TLSRSACertificateKeyFile   /etc/ssl/private/proftpd.key
  TLSOptions                 NoCertRequest
  TLSVerifyClient            off
  TLSRequired                on
</IfModule>
```
- Khởi động lại ProFTPD

```bash!
sudo systemctl restart proftpd

```

### Thử kết nối lại từ FileZilla
- Dùng Site Manager

- Chọn:

    - **Protocol**: `FTP`

    - **Encryption**: `Require explicit FTP over TLS`


![image](https://github.com/user-attachments/assets/6c210915-08f7-4f4d-9fbb-6b40dca45225)

![image](https://github.com/user-attachments/assets/16e385e2-7845-48d8-ab80-1ec8f5e02b04)

![image](https://github.com/user-attachments/assets/5abf66f9-a7a6-4cab-929f-973c2255969f)

