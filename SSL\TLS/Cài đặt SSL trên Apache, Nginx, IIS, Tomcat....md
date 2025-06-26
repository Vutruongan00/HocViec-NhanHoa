># CÀI ĐẶT SSL 

# Các bước chung trước khi cài đặt

### Bước 1: Tạo Private Key và CSR

```bash!
# Tạo private key
openssl genrsa -out antvpro.key 2048

# Tạo CSR
openssl req -new -key antvpro.key -out antvpro.csr
```
- Ví dụ như sau:

![image](https://github.com/user-attachments/assets/327e2b66-5456-4d19-b0d4-f8c44a91f0fa)

### Bước 2: Gửi CSR cho CA
- Gửi nội dung file `antvpro.csr` lên trang quản lý **CA** - nơi cung cấp SSL

- **CA** xác minh và gửi lại các file:
    
    - **`certificate.crt`** (chứng chỉ chính)
    - **`intermediate.crt`** hoặc **`ca-bundle.crt`** (chuỗi chứng chỉ)
    - (tùy chọn) **`root.crt`**

- Ví dụ: Upload file `.csr` vừa tạo (Hoặc chọn tự động tạo CSR)

![image](https://github.com/user-attachments/assets/acd38834-d431-44a3-8012-dbcee074b0ff)


### Bước 3: Chuẩn bị chứng chỉ và file cấu hình
- Tổ chức lưu các file,(bạn có thể dấu chúng ở bất kỳ đâu nhưng phải nhớ vị trí lưu nhé), ví dụ tôi lưu ở đây

```swift
/etc/ssl/certificate.crt  
/etc/ssl/ca_bundle.crt
/etc/ssl/antvpro.key
```
![image](https://github.com/user-attachments/assets/44dc40e4-92ff-405a-91f3-31d1b91be975)


> Gộp chuỗi chứng chỉ nếu cần:
> ```
> cat yourdomain.crt intermediate.crt > fullchain.crt
>```


## Cài đặt SSL trên APACHE

- Cài Module cần thiết: `mod_ssl`:
    - Trên Debian/Ubuntu: `sudo a2enmod ssl`
    - Trên CentOS/RHEL: `sudo yum install mod_ssl`

- Cấu hình trong file Virtual Host cho HTTPS của website:
    - Ví dụ ở đây mình cấu hình 1 file `.conf` riêng cho port 443 https

![image](https://github.com/user-attachments/assets/8cfe6ebf-a9a5-4dd8-9c50-f353001d44ba)

```apache!
<VirtualHost *:443>
    ServerName antvpro.io.vn
    DocumentRoot /var/www/antvpro.io.vn/... 

    SSLEngine on
    SSLCertificateFile      /etc/ssl/certificate.crt    
    SSLCertificateKeyFile   /etc/ssl/private/antvpro.key
    SSLCertificateChainFile /etc/ssl/certs/ca_bundle.crt    #-->Vị trí lưu các file chứng chỉ

    # Tùy chọn bảo mật bổ sung
    SSLProtocol all -SSLv2 -SSLv3
    SSLCipherSuite HIGH:!aNULL:!MD5
    Header always set Strict-Transport-Security "max-age=63072000; includeSubDomains; preload"
</VirtualHost>
```

![image](https://github.com/user-attachments/assets/c1513d3e-1db1-4f47-89d9-59c3ec706209)


-  Kích hoạt site và khởi động lại Apache
```bash!
sudo a2ensite yourdomain-ssl
sudo systemctl reload apache2
```
![image](https://github.com/user-attachments/assets/88b2e1ff-193a-411a-9479-c7cd1685d4a2)

## Cài đặt SSL trên NGINX
## Cài đặt SSL trên IIS 
## Cài đặt SSL trên Tomcat
## Cài đặt SSL trên CPanel
## Cài đặt SSL trên Plesk
## Cài đặt SSL trên DirectAdmin
## SSL cho tên miền chính và subdomain
## Tự động gia hạn (Auto-renewal) với Let's Encrypt
