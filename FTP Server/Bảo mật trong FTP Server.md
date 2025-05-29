# Bảo mật FTP Server 

## I. Các rủi ro bảo mật chính của FTP Server

#### 1. Truyền dữ liệu không mã hóa (Unencrypted Data Transmission)
- Username, password và dữ liệu được gửi qua mạng dưới dạng văn bản thuần (plain text).

- Dễ bị đánh cắp bằng công cụ như Wireshark (sniffing).
- Ví dụ: Khi một người dùng kết nối FTP qua Wi-Fi công cộng mà không mã hóa TLS sẽ dễ bị hacker chặn gói tin và lấy được thông tin đăng nhập.

#### 2. Tấn công Brute Force
- Hacker tự động thử hàng loạt mật khẩu để đăng nhập trái phép.

- Đặc biệt nguy hiểm với tài khoản mặc định (anonymous) hoặc mật khẩu yếu.

#### 3. Truy cập ẩn danh (anonymous)
Nếu không được kiểm soát, anonymous login có thể bị lợi dụng để tải dữ liệu trái phép hoặc tấn công hệ thống.

## II. Các biện pháp bảo vệ FTP Server

Để giảm thiểu các rủi ro bảo mật của FTP, cần triển khai các biện pháp kỹ thuật và quản lý phù hợp.

### 1. Sử dụng các giao thức an toàn: FTPS hoặc SFTP
- **FTPS (FTP Secure)**: Mã hóa kết nối FTP bằng TLS (SSL).

- **SFTP (SSH File Transfer Protocol)**: Truyền file qua SSH, không cần FTP server truyền thống.

### Ví dụ:

#### - **Windows Server – IIS FTP với FTPS:**
    - Mở IIS Manager → FTP SSL Settings → Chọn “Require SSL Connections” → Gán chứng chỉ số (SSL Certificate) từ máy chủ hoặc tự tạo.

![image](https://github.com/user-attachments/assets/0437426f-7147-48a9-8d6b-a889cb2ecbe4)

    
#### - **FileZilla (Windows)**

    - Server > Configurations > Server Listener -> Require Explicit FTP over TLS

![image](https://github.com/user-attachments/assets/da39bb97-4312-44fd-90f2-89a7cc7b3e13)


![image](https://github.com/user-attachments/assets/05ce9d63-959d-4adb-982a-9ef375b87c76)



#### - **vsftpd** dùng FTPS - (Ubuntu Linux)
- Tạo chứng chỉ SSL: `sudo apt install openssl`
    ```bash
    sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.key -out /etc/ssl/certs/vsftpd.crt
    ```
    - Cấu hình trong /etc/vsftpd.conf
    ```bash!
    ssl_enable=YES
    rsa_cert_file=/etc/ssl/certs/vsftpd.crt
    rsa_private_key_file=/etc/ssl/private/vsftpd.key
    force_local_data_ssl=YES
    force_local_logins_ssl=YES
    ```
    
- ProFTPD (Ubuntu)
```...```
---

### 2. Giới hạn IP truy cập

#### - Windows Server – IIS FTP
- Trong **IIS Manager** --> Vào **FTP IP Address and Domain Restrictions** 

![image](https://github.com/user-attachments/assets/6cd7f59f-5bc0-4cb8-baed-dff832c3913f)


- Thêm danh sách IP được phép(Allow) hoặc từ chối(Deny):
![image](https://github.com/user-attachments/assets/6a5071a0-c094-4f14-a8c7-4b18b5bb7151)


![image](https://github.com/user-attachments/assets/f0744d6e-e087-46ff-b58e-4f150b2fc0ff)


#### - FileZilla (Windows)

![image](https://github.com/user-attachments/assets/9edb7e83-bd46-4c5d-a440-f8bb3e37701c)


- hoặc cấu giới hạn/cho phép truy cập cho từng user

![image](https://github.com/user-attachments/assets/3a8fb658-69f0-4a7e-994b-7c506b6737b4)




#### - vsftpd - (Ubuntu)

- Dùng Firewall UFW, ví dụ:

```bash!
sudo ufw allow from 192.168.137.0/24 to any port 21
sudo ufw deny from any to any port 21
```

#### - ProFTPD - (Ubuntu)

- Chỉnh sửa file cấu hìn chính:
```
 nano /etc/proftpd/proftpd.conf
```

- Thêm cấu hình `<limit LOGIN>` chỉ cho phép đăng nhập từ dải mạng...:
```apache!
<Limit LOGIN> 
  Order Deny,Allow
  Deny from all
  Allow from 192.168.1.0/24 
</Limit>
```
![image](https://github.com/user-attachments/assets/1b4e7a2d-7852-4100-8a21-88a0d519855e)



- Kết quả khi cấu hình `<limit LOGIN>` và sau khi bỏ cấu hình:

![image](https://github.com/user-attachments/assets/c732b482-fe0a-4d17-ab2d-e02194a2c205)




### 3. QUẢN LÝ TÀI KHOẢN NGƯỜI DÙNG

#### - **Chính sách mật khẩu mạnh:**
    - Sử dụng phần mềm bên ngoài quản lý người dùng.
    - Đặt chính sách độ dài, độ phức tạp, hết hạn mật khẩu.

#### - **Vô hiệu hóa anonymous access:**
- **Windows IIS FTP:** Vào **FTP Authentication** > Vô hiệu hóa “Anonymous Authentication”.

    ![image](https://github.com/user-attachments/assets/b7159ed4-6645-43ac-b424-21effa3a2a7d)


    ![image](https://github.com/user-attachments/assets/5b4c4011-74e1-4f9e-86a5-f33d31e5ab06)


    - **vsftpd** - (Ubuntu): Cấu hình trong `/etc/vsftpd.conf:`
    ```ini!
    anonymous_enable=NO
    ```
    
    
    - **ProFTPD**: `/etc/proftpd/proftpd.conf`
    
    Thêm vào dòng sau
    ```conf
    <Anonymous ~ftp>
    User ftp
    Group nogroup
    UserAlias anonymous ftp
    <Limit LOGIN>
       DenyAll
    </Limit>
    </Anonymous>
    ```
    hoặc xóa hoặc comment(#) block <Anonymous> 
   
    ![image](https://github.com/user-attachments/assets/4ae8cf4b-1a5c-41cf-891b-8034da99f9c6)




#### - Cách ly người dùng (chroot)

- **IIS FTP Server**(WinDows Server)
- **FileZilla Server**(Windows)
> xem lại file thực hành cấu hình...
    
- **vsftpd** (Ubuntu):
    ```bash!
    sudo adduser user1
    sudo mkdir /home/user1/ftp
    sudo chown nobody:nogroup /home/user1/ftp
    sudo chmod a-w /home/user1/ftp
    ```
    
    - Cấu hình trong file `/etc/vsftpd.conf`:
    ```bash!
    local_enable=YES
    write_enable=YES
    chroot_local_user=YES
    allow_writeable_chroot=YES
    ```

- **ProFTPD (Ubuntu)**
    - File cấu hình `/etc/proftpd/proftpd.conf`:
    
    ```apache!
    DefaultRoot ~
    ```
    
    
