># CÀI ĐẶT SSL 

# Các bước chung trước khi cài đặt

### Bước 1: Tạo Private Key và CSR

```bash!
# Tạo private key
openssl genrsa -out yourdomain.key 2048

# Tạo CSR
openssl req -new -key yourdomain.key -out yourdomain.csr
```

### Bước 2: Gửi CSR cho CA
- Gửi nội dung file `yourdomain.csr` lên trang quản lý **CA** - nơi cung cấp SSL

- **CA** xác minh và gửi lại các file:
    
    - **`yourdomain.crt`** (chứng chỉ chính)
    - **`intermediate.crt`** hoặc **`ca-bundle.crt`** (chuỗi chứng chỉ)
    - (tùy chọn) **`root.crt`**

### Bước 3: Chuẩn bị chứng chỉ và file cấu hình
- Tổ chức các file:
```swift
/etc/ssl/certs/yourdomain.crt
/etc/ssl/certs/intermediate.crt
/etc/ssl/private/yourdomain.key
```

> Gộp chuỗi chứng chỉ nếu cần:
> ```
> cat yourdomain.crt intermediate.crt > fullchain.crt
>```


## Cài đặt SSL trên APACHE

- Cài Module cần thiết: `mod_ssl`:
    - Trên Debian/Ubuntu: `sudo a2enmod ssl`
    - Trên CentOS/RHEL: `sudo yum install mod_ssl`

- Cấu hình trong file Virtual Host cho HTTPS của website:
    - Ví dụ ở đây mình cấu hình 1 file .conf riêng cho port 443 https
![image](https://github.com/user-attachments/assets/bbf3b282-aedf-48af-badf-5db6700d8ccc)

```apache!
<VirtualHost *:443>
    ServerName antvpro.io.vn
    DocumentRoot /var/www/html

    SSLEngine on
    SSLCertificateFile      /etc/ssl/certs/antvpro.io.vn.crt
    SSLCertificateKeyFile   /etc/ssl/private/antvpro.io.vn.key
    SSLCertificateChainFile /etc/ssl/certs/intermediate.crt

    # Tùy chọn bảo mật bổ sung
    SSLProtocol all -SSLv2 -SSLv3
    SSLCipherSuite HIGH:!aNULL:!MD5
    Header always set Strict-Transport-Security "max-age=63072000; includeSubDomains; preload"
</VirtualHost>

```

-  Kích hoạt site và khởi động lại Apache
```bash!
sudo a2ensite yourdomain-ssl
sudo systemctl reload apache2
```

## Cài đặt SSL trên NGINX
## Cài đặt SSL trên IIS 
## Cài đặt SSL trên Tomcat
## Cài đặt SSL trên CPanel
## Cài đặt SSL trên Plesk
## Cài đặt SSL trên DirectAdmin
## SSL cho tên miền chính và subdomain
## Tự động gia hạn (Auto-renewal) với Let's Encrypt
