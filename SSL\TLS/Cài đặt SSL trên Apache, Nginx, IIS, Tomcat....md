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

---

## Cài đặt SSL trên NGINX

### Bước 1: Cấu hình block cho HTTPS trong 
`/etc/nginx/sites-available/yourdomain`

> Gộp chuỗi chứng chỉ (nếu cần):
> ```
> cat certificate.crt certificateca_bundle.crt > fullchain.crt
>```

```nginx!
server {
    listen 443 ssl;
    server_name yourdomain.com;

    root /var/www/html;

    ssl_certificate     /etc/ssl/fullchain.crt;
    ssl_certificate_key /etc/ssl/antvpro.key;

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;

    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload" always;
}
```

### Bước 2: Kiểm tra và reload
```
sudo nginx -t
sudo systemctl reload nginx
```

---


## Cài đặt SSL trên IIS (Windows Server)

### **Bước 1: Mở IIS Manager:** 
- Mở `Server Manager` -> `Tools` -> `Internet Information Services (IIS) Manager`.

### **Bước 2: Tạo CSR (Nếu chưa có):**
   - Chọn tên máy chủ trong khung bên trái.
   - Trong khung giữa, click chọn `Server Certificates`.
![image](https://github.com/user-attachments/assets/8219f88c-2f66-4e9c-b0f7-722d7aaad816)

   - Trong khung `Actions` bên phải, nhấp vào `Create Certificate Request...`.
   - Điền thông tin yêu cầu và chọn độ dài khóa (thường là 2048-bit trở lên) và thuật toán mã hóa (SHA256).
![image](https://github.com/user-attachments/assets/64c118a3-072d-4b6a-a1d6-5a0139a0fb02)

![image](https://github.com/user-attachments/assets/71a822a1-1bef-48b8-bd69-a77193a8dad9)

   - Lưu CSR vào một tệp văn bản.


### **Bước 3: Hoàn tất Yêu cầu Chứng chỉ:**
   - Sau khi CA cấp chứng chỉ (thường là tệp `.cer` hoặc `.crt`), trở lại `Server Certificates`.
   - Trong khung `Actions`, nhấp vào `Complete Certificate Request...`.
   - Duyệt đến tệp chứng chỉ đã nhận từ CA.
   - Cung cấp một tên thân thiện (Friendly name) cho chứng chỉ để dễ quản lý.
   - Chứng chỉ sẽ được nhập vào kho lưu trữ chứng chỉ của máy chủ.
 
 
> - Nếu có file `.crt` rồi, cần convert sang định dạng file `.PFX`. Sử dụng **Openssl**  hoặc convert online:
>     - chạy lệnh sau bằng `cmd` trong thư mục chứa file cert
> ```
> openssl pkcs12 -export -out certificate.pfx -inkey antvpro.key -in certificate.crt -certfile ca_bundle.crt
> ```
> - Sau khi conver file có định dạng như sau
![image](https://github.com/user-attachments/assets/942a8284-2f96-4ddf-bf77-4686f2458b16)
>
>- Import chứng chỉ vào máy chủ
![image](https://github.com/user-attachments/assets/2ef4fd8f-e4a3-4fda-b2a6-8d5ae81da365)



### **Bước 4: Gán chứng chỉ cho trang web:**
   - Trong khung bên trái, điều hướng đến `Sites` và chọn trang web cần cấu hình.
   - Trong khung `Actions` bên phải, nhấp vào `Bindings...`.
   - Trong hộp thoại `Site Bindings`, nhấp vào `Add...`.
   - Chọn `Type` là `https`.
   - Đặt `IP address` là `All Unassigned` hoặc địa chỉ IP cụ thể.
   - Đặt `Port` là `443`.
   - Trong `SSL certificate`, chọn chứng chỉ vừa nhập bằng `Friendly name` của nó.
   - (Tùy chọn) Chọn `Require Server Name Indication (SNI)` nếu có nhiều trang web HTTPS trên cùng một địa chỉ IP.

![image](https://github.com/user-attachments/assets/94b4a643-ec2a-4920-b074-165b6ec4f028)

   - Nhấp `OK` và sau đó `Close`.
 
![image](https://github.com/user-attachments/assets/af95fded-ea8f-415b-9ade-04954f9687ef)

### **Bước5: Chuyển hướng HTTP sang HTTPS (Tùy chọn):**

>Dùng HSTS:

![image](https://github.com/user-attachments/assets/0cbb2d75-4d0f-42ea-946f-ab0444542974)

>Hoặc dùng module URL Rewrite
- Cài đặt module URL Rewrite (nếu chưa có).
- Chọn trang web cần cấu hình trong IIS Manager.
- Trong khung giữa, click chọn `URL Rewrite`.
- Trong khung `Actions`, nhấp vào `Add Rule(s)...` và chọn `Blank Rule` cho `Inbound Rules`.

- Cấu hình rule:
    - `Name`: `HTTP to HTTPS Redirect`
    - `Match URL`:
       - `Requested URL`: `Matches the Pattern`
       - `Using`: `Regular Expressions`
       - `Pattern`: `(.*)`
     - `Conditions`:
       - `Add...`: `Condition input`: `{HTTPS}` `Pattern`: `^OFF$`
     - `Action`:
       - `Action type`: `Redirect`
       - `Redirect URL`: `https://{HTTP_HOST}/{R:1}`
       - `Redirect type`: `Permanent (301)`
   - Nhấp `Apply`.

---
---

---

## Cài đặt SSL trên Tomcat
>- **Vị trí cấu hình**: Cấu hình SSL/TLS được thực hiện trong tệp `conf/server.xml` 
>- **Chuẩn bị Keystore**: Tomcat thường sử dụng định dạng keystore Java (JKS hoặc PKCS12) để lưu trữ chứng chỉ và khóa riêng tư. Cần nhập khóa riêng tư và chứng chỉ vào một keystore.

- **Bước 1: Tạo Java Keystore và Private Key**
```bash!
keytool -genkey -alias tomcat -keyalg RSA -keysize 2048 -keystore antvpro.jks
```
>- `tomcat`: bí danh (alias) cho khóa
>- `antvpro.jks`: tên file keystore sẽ tạo ra

- **Bước 2: Tạo CSR (Certificate Signing Request)**
```bash!
keytool -certreq -alias tomcat -file antvpro.csr -keystore antvpro.jks
```

- **Bước 3: Gửi CSR cho CA để mua chứng chỉ SSL**
> Sau khi xác minh, họ sẽ gửi lại cho bạn các file `.crt` (server certificate) và `.ca-bundle` (intermediate CA).

- **Bước 4: Nhập chứng chỉ vào keystore:** 
    - Nếu nhận được file  `certificate.crt` và `ca_bundle.crt`
    ```bash!
    keytool -import -trustcacerts -alias tomcat -file ca_bundle.crt -keystore antvpro.jks
    keytool -import -trustcacerts -alias tomcat -file certificate.crt -keystore antvpro.jks
    ```
    - Nếu CA gửi bạn file `.p7b`:
    ```bash!
    keytool -import -trustcacerts -alias tomcat -file certificate.p7b -keystore antvpro.jks
    ```
    
- **Bước 5: Cấu hình SSL trong server.xml của Tomcat**
    - Mở file conf/server.xml và thêm đoạn sau:
```xml!
<Connector port="443" protocol="org.apache.coyote.http11.Http11NioProtocol"
           maxThreads="150" SSLEnabled="true">
    <SSLHostConfig>
        <Certificate certificateKeystoreFile="path/to/antvpro.jks"
                     type="JKS"
                     certificateKeystorePassword="qwerty123@" />
    </SSLHostConfig>
</Connector>
```

- **Khởi động lại Tomcat và kiểm tra**
`https://antvpro.io.vn:8443`
```bash!
sudo systemctl restart tomcat
```

---

//// HOẶC NẾU ĐÃ CÓ SẴN 3 FILE `.crt` và `.key`
- **Bước 1: Gộp các file .crt và .key thành một file .pfx (PKCS#12)**

```bash!
openssl pkcs12 -export \
  -in /etc/ssl/certificate.crt \
  -inkey /etc/ssl/antvpro.key \
  -certfile /etc/ssl/ca_bundle.crt \
  -out /etc/ssl/antvpro.pfx 
  -passout pass:qwerty123@
 ```
> --> Đặt mật khẩu cho file `.pfx`: `qwerty123@` . Nhớ mật khẩu này để dùng trong bước tiếp theo.

- **Bước 2: Chuyển .pfx thành Java Keystore (.jks)**

```bash!
keytool -importkeystore \
  -deststorepass your_keystore_password \
  -destkeypass your_key_password \
  -destkeystore antvpro.jks \
  -srckeystore antvpro.pfx \
  -srcstoretype PKCS12 \
  -srcstorepass qwerty123@ \
  -alias tomcat
```
- **Cấu hình trong server.xml của Tomcat** (như trên)
