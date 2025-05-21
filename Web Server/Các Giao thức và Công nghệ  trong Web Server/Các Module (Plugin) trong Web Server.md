# Module/Plugin trong Web Server
Module (hay Plugin) trong Web Server là các thành phần mở rộng (extension components) cho phép bổ sung hoặc thay đổi chức năng của Web Server mà không cần sửa đổi lõi (core) của phần mềm.

>  Hiểu đơn giản: Web Server giống như một khung xương chính, còn Module là các bộ phận gắn thêm vào để nó làm được nhiều việc hơn (như hỗ trợ SSL, rewrite URL, xác thực, nén dữ liệu, xử lý ngôn ngữ lập trình…).

## CÁCH HOẠT ĐỘNG CỦA MODULE
- Khi Web Server khởi động, nó sẽ nạp các module đã cấu hình.

- Các module sẽ chèn (hook) vào quá trình xử lý request → can thiệp vào các bước như:
    - Xác thực người dùng
    - Ghi log
    - Phân tích URL
    - Kết nối đến backend (PHP, Python…)
    - Thêm tiêu đề HTTP
    - Nén dữ liệu trước khi gửi

## PHÂN LOẠI MODULE
### 1. Core Module (mặc định, không thể tắt)
- Gắn liền với hệ thống.
- Ví dụ: http_core trong Apache.

### 2. Dynamic Module (có thể bật/tắt)
- Nạp vào khi cần.
- Ví dụ: mod_ssl, mod_rewrite, ngx_http_gzip_module.

### 3. Third-party Module (bên ngoài)
- Do cộng đồng hoặc bên thứ ba phát triển.
- Cần biên dịch hoặc cấu hình riêng.
- Ví dụ: ngx_pagespeed, mod_security, brotli, passenger

----------
## CÁC MODULE TRONG APACHE
| Module           | Mục đích                            |
| ---------------- | ----------------------------------- |
| `mod_ssl`        | Kích hoạt HTTPS thông qua SSL/TLS   |
| `mod_rewrite`    | Viết lại URL (rewrite rule)         |
| `mod_auth_basic` | Xác thực người dùng bằng Basic Auth |
| `mod_php`        | Cho phép Apache xử lý mã PHP        |
| `mod_headers`    | Thêm/sửa header HTTP trong response |

### 1. Mod_rewrite
- `mod_rewrite` là một module của Apache cho phép viết lại URL trên server một cách linh hoạt bằng cách sử dụng biểu thức chính quy (regular expressions).
- Nó không thay đổi nội dung thật sự của trang, chỉ thay đổi cách URL được hiển thị hoặc xử lý.

 **Ví dụ:**
 -  URL gốc:
 `http://domain.com/product.php?id=123
`
- URL sau khi dùng mod_rewrite:
`http://domain.com/product/123
`
#### a. Mục đích của mod_rewrite
| Mục đích                     | Giải thích                                                       |
| ---------------------------- | ---------------------------------------------------------------- |
| **Tạo URL đẹp (pretty URL)** | Giúp URL dễ đọc, dễ SEO                                          |
| **Ẩn cấu trúc nội bộ**       | Không để lộ tên file thật (ví dụ `.php`, `.html`)                |
| **Chuyển hướng (redirect)**  | Nếu bạn đổi đường dẫn cũ sang mới, vẫn giữ liên kết cũ hoạt động |
| **Bảo mật nhẹ**              | Giấu thông tin thật về hệ thống (tên file, tham số)              |

#### b. Cách hoạt động
- Khi một client gửi yêu cầu HTTP, mod_rewrite can thiệp vào URL trong quá trình xử lý yêu cầu, và chuyển hướng hoặc viết lại URL theo các quy tắc được cấu hình trong .htaccess hoặc file cấu hình của Apache.
- Mod_rewrite hoạt động trên toàn bộ đường dẫn URL bao gồm các data được nhập trên URL. Các rewrite rule có thể được định nghĩa trong các file httpd.conf trên hệ thống, file .htaccess trên source. Các rewrite rule có thể dẫn đến truy vấn theo chuỗi, khởi chạy chương trình nội bộ, gửi request ra bên ngoài hệ thống, hoặc gọi các proxy nội bộ.
#### c. Cú pháp cơ bản 
Trong `.htaccess` hoặc VirtualHost, sử dụng:
```
RewriteEngine On
RewriteRule [pattern] [substitution] [flags]
```
- `RewriteEngine On`: Bật chức năng rewrite.

- `RewriteRule`: Xác định quy tắc.

- `pattern`: Biểu thức chính quy để khớp URL gốc.

- `substitution`: URL đích hoặc tập tin đích (có thể là đường dẫn vật lý hoặc logic).

- `flags`: Tham số điều khiển hành vi (tùy chọn).

**Ví dụ**: Viết lại URL **động** thành **tĩnh**
```
RewriteEngine On
RewriteRule ^sanpham/([0-9]+)/?$ product.php?id=$1 [L]
```
URL người dùng thấy:`https://example.com/sanpham/123 `

Apache thực sự xử lý: `https://example.com/product.php?id=123
`

### **Các flag thường gặp**

| Flag    | Ý nghĩa                                            |
| ------- | -------------------------------------------------- |
| `L`     | Dừng áp dụng các rule tiếp theo (Last rule)        |
| `R=301` | Redirect (301 là mã chuyển hướng vĩnh viễn)        |
| `F`     | Forbidden – trả về lỗi 403                         |
| `QSA`   | Giữ nguyên chuỗi truy vấn (?key=value) gốc         |
| `NC`    | Không phân biệt chữ hoa/thường (No Case)           |
| `OR`    | Dùng trong `RewriteCond` để "hoặc" nhiều điều kiện |

### Các chỉ thị mod_rewrite

| **Chỉ thị**       | **Description** |
|-------------------|-----------------|
| **RewriteEngine** | Bật/tắt Rewrite. Mặc định không hoạt động, cần bật thủ công trong từng thư mục.<br>**Ví dụ:** `RewriteEngine on` |
| **RewriteBase**   | Thiết lập URL cơ sở cho thư mục.<br>**Cú pháp:** `RewriteBase /URL-path` |
| **RewriteOptions**| Thiết lập tùy chọn cho Rewrite. Một số tùy chọn phổ biến:<br>• `inherit`: kế thừa cấu hình từ thư mục cha<br>• `AllowAnyURI`: cho phép thay đổi toàn bộ URI<br>• `MergeBase`: áp dụng RewriteBase cho thư mục con chưa có RewriteBase |
| **RewriteMap**    | Định nghĩa tập tin ánh xạ URL.<br>**Ví dụ:**<br>`RewriteMap examplemap txt:/path/to/file/map.txt`<br>`RewriteRule ^/ex/(.*) ${examplemap:$1}`<br>**Định dạng map:**<br>`\${MapName:LookupKey}`<br>`\${MapName:LookupKey|DefaultValue}` |
| **RewriteRule**   | Luật thay thế URL khi khớp Pattern.<br>**Cú pháp:** `RewriteRule Pattern Substitution [flags]`<br>**Ví dụ:**<br>`RewriteRule ^welcome\.html$ /html/index.html [L]`<br>**Thành phần:**<br>• `Pattern`: biểu thức chính quy để kiểm tra URL<br>• `Substitution`: URL thay thế (hoặc `-` để giữ nguyên)<br>• `Flags`: một số cờ:<br> • `[L]`: dừng áp dụng các rule khác<br> • `[QSA]`: nối thêm query string<br> • `[F]`: trả về lỗi 403<br> • `[G]`: trả về lỗi 410<br> • `[N]`: lặp lại rule<br> • `[R=code]`: redirect với mã HTTP |
| **RewriteCond**   | Điều kiện áp dụng cho RewriteRule kế tiếp.<br>**Cú pháp:** `RewriteCond TestString CondPattern [flags]`<br>**TestString**: biến kiểm tra (`%{VARIABLE}`, `%{ENV:var}`, `%{HTTP:Header}`)<br>**CondPattern**: regex hoặc các điều kiện đặc biệt:<br>• `-s`: là file<br>• `-l`: là symbolic link<br>• `-d`: là thư mục<br>**Flags**:<br>• `[NC]`: không phân biệt hoa thường<br>• `[OR]`: điều kiện logic "hoặc"<br>**Ví dụ:**<br>`RewriteCond "%{HTTP_USER_AGENT}" "(iPhone|Android)"`<br>`RewriteRule ...` |


### 2. Mod_ssl
`mod_ssl` là một module của Apache Web Server giúp kích hoạt và xử lý giao thức HTTPS (SSL/TLS) trên website. Module này sử dụng thư viện OpenSSL để thực hiện mã hóa và giải mã dữ liệu, đảm bảo tính bảo mật cho thông tin truyền tải giữa máy chủ và người dùng.


#### a. Chức năng chính của `mod_ssl`
- Kích hoạt giao thức HTTPS (cổng 443)

- Sử dụng chứng chỉ SSL/TLS để mã hóa dữ liệu

- Xác thực server và/hoặc client

- Thiết lập các tham số bảo mật như cipher, giao thức (TLS 1.2, 1.3...)
#### b. Thành phần cấu hình chính
Khi cấu hình `mod_ssl` trong Apache để bật HTTPS, bạn sẽ làm việc chủ yếu với các chỉ thị (`directive`) trong file cấu hình. Các thành phần chính bao gồm:
 
- **SSLEngine**
    - Cú pháp: `SSLEngine on`

    -    Chức năng: Bật chế độ SSL/TLS cho VirtualHost hiện tại.

    - Ghi chú: Nếu không có dòng này, Apache sẽ không xử lý các kết nối HTTPS dù đã có chứng chỉ.

- **SSLCertificateFile**:
    - Cú pháp: `SSLCertificateFile /đường_dẫn/đến/file.crt`
    - Chức năng: Trỏ đến file chứa chứng chỉ SSL đã được cấp cho tên miền (domain)
    - Ví dụ:
```
    SSLCertificateFile /etc/ssl/certs/example.com.crt
```

-  **SSLCertificateKeyFile**:
    - Cú pháp: `SSLCertificateKeyFile /đường_dẫn/đến/private.key`
    - Chức năng: Trỏ đến khóa riêng tư (private key) tương ứng với chứng chỉ phía trên.
    - **Lưu ý**: File **.key** này cần được giữ an toàn tuyệt đối.
    - Ví dụ:
    ```
    SSLCertificateKeyFile /etc/ssl/private/example.com.key
    ```
- **SSLCertificateChainFile (tùy chọn, dùng với chứng chỉ từ CA)**:
    - Cú pháp: `SSLCertificateChainFile /đường_dẫn/đến/chain.pem`
    - Chức năng: Dùng để chỉ ra chuỗi chứng chỉ trung gian nếu nhà cung cấp chứng chỉ yêu cầu.
    - Ví dụ:
```
SSLCertificateChainFile /etc/ssl/certs/chain.pem
```
- **SSLProtocol**: Chỉ định các phiên bản giao thức SSL/TLS được phép sử dụng.

- **SSLCipherSuite**: Quy định tập hợp thuật toán mã hóa (cipher) được phép dùng trong kết nối.


## Quy trình cài đặt và cấu hình `mod_ssl`
Để thực hiện tạo chứng chỉ SSL trên Apache trước tiên cần đảm bảo đã cài đặt cấu hình đầy đủ máy chủ web Apache trên server 

**Bước 1: Cài đặt `mod_ssl`**

- Trên Ubuntu:  `sudo a2enmod ssl`
- Trên Centos/RHEL: `sudo yum install httpd mod_ssl
`

**Bước 2: Tạo chứng chỉ SSL tự ký mới:**

- Thư mục `/etc/ssl/certs` được sử dụng để chứa chứng chỉ công khai đã có trên server. 
- **Tạo một thư mục `/etc/ssl/private`** để chứa file khóa riêng tư:
    >Tính bảo mật của khóa này vô cùng quan trọng. Chính vì vậy, việc khóa quyền truy cập để ngăn chặn truy cập trái phép là rất cần thiết.
    ```
    sudo mkdir /etc/ssl/private

    sudo chmod 700 /etc/ssl/private
    ```
- Tạo cặp khóa và chứng chỉ self-signed với OpenSSL bằng lệnh sau:
``` 
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt 
```
trong đó: 
`openssl req`: Công cụ OpenSSL được sử dụng với chứng năng là tạo yêu cầu chứng chỉ.

`-x509`: Là một chuẩn cơ bản về quản lý khóa công khai mà cả SSL và TLS sử dụng để kiểm soát khóa và chứng chỉ. Giúp tạo chứng chỉ ký, không yêu cầu chứng nhận bên ngoài.

`-nodes`: Cho phép OpenSSL biết bỏ qua tùy chọn bảo vệ chứng chỉ bằng mật khẩu. Apache có thể đọc file mà không cần đến sự can thiệp của người dùng mỗi khi server khởi động lại. Mật khẩu giữ nhiệm vụ ngăn cản điều này xảy ra do cần nhập mật khẩu mỗi lần khởi động lại.

`-days 365`: là xác định thời hạn có hiệu lực của chứng chỉ 365 ngày()

 `-newkey rsa:2048`: tạo ra một chứng chỉ và khóa mới cùng lúc. Nếu chưa tạo khóa được yêu cầu để ký chứng chỉ trong 1 bước trước đó, cần tạo khóa cùng với chứng chỉ. Phần `rsa:2048` chỉ định tạo ra một khóa RSA với độ dài 2048 bit.
 
  `-keyout /etc/ssl/private/apache-selfsigned.key`: nơi đặt file khóa riêng tư được tạo ra.
  
  -`out`: Dòng này cho OpenSSL biết nơi đặt chứng chỉ được tạo ra
  
-> Nhập tên miền hoặc địa chỉ IP công khai của server khi được yêu cầu**, nhập Common Name cần khớp chính xác với cách người dùng truy cập vào website 
 
Output tương tự như sau,
```
Country Name (2 letter code) [XX]:US

State or Province Name (full name) []:Example

Locality Name (eg, city) [Default City]:Example 

Organization Name (eg, company) [Default Company Ltd]:Example Inc

Organizational Unit Name (eg, section) []:Example Dept

Common Name (eg, your name or your server's hostname) []: [your_domain_or_ip]

Email Address []: [webmaster@example.com]
```

- Khi sử dụng OpenSSL, bạn cũng nên tạo một nhóm Diffie-Hellman sử dụng trong việc đàm phán Forward Secrecy tuyệt đối với các client:
```sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048```

**Bước 3: Cấu hình Apache để sử dụng SSL**
- **Trên Ubuntu:**
    - Mở file cấu hình mặc định cho SSL:
```
sudo nano /etc/apache2/sites-available/default-ssl.conf
```
sửa lại như sau: 
```
<IfModule mod_ssl.c>
<VirtualHost _default_:443>
    ServerAdmin webmaster@localhost #thay bằng emai của quản trị viên hệ thống hoặc emai thật
    DocumentRoot /var/www/html

    SSLEngine on
    SSLCertificateFile      /etc/ssl/certs/selfsigned.crt
    SSLCertificateKeyFile   /etc/ssl/private/selfsigned.key
</VirtualHost>
</IfModule>
```
Kích hoạt cấu hình SSL:
```
sudo a2ensite default-ssl.conf
sudo systemctl reload apache2
```

- **Trên Centos**:
    - Mở file cấu hình ``` sudo nano  /etc/httpd/conf.d/ssl.conf ```
    - Sửa các dòng tương ứng

```
 ServerName your_domain_or_ip

    DocumentRoot /var/www/html

    SSLEngine on

    SSLCertificateFile /etc/ssl/certs/apache-selfsigned.crt

    SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key
```
Khởi động lại apache:
`sudo systemctl restart httpd`

**Bước 4: Mở cổng 443 (nếu có tường lửa)**






-----

## CÁC MODULE TRONG NGINX
| Module                       | Mục đích                                          |
| ---------------------------- | ------------------------------------------------- |
| `ngx_http_ssl_module`        | Hỗ trợ HTTPS                                      |
| `ngx_http_gzip_module`       | Nén dữ liệu khi gửi                               |
| `ngx_http_rewrite_module`    | Viết lại URL bằng các rule logic                  |
| `ngx_http_auth_basic_module` | Xác thực truy cập bằng user/pass                  |
| `ngx_http_proxy_module`      | Chuyển tiếp request đến server khác (proxy\_pass) |



### ngx_http_core_module 

`ngx_http_core_module` là module lõi mặc định trong Nginx, chịu trách nhiệm chính trong việc xử lý request HTTP, quản lý location, thiết lập root, index, và nhiều chỉ thị (directive) quan trọng khác trong cấu hình của server.

...

### ngx_http_ssl_module
- Cung cấp sự hỗ trợ cần thiết cho HTTPS. Module này yêu cầu thư viện OpenSSL 
		- Gồm nhiều chỉ thị khác nhau nổi bật cần chú ý:  ssl_certificate, ssl_certificate_key, ssl_protocols phục vụ cấu hình SSL/TLS cơ bản 
		- Ví dụ: Cấu hình Nginx để phục vụ nội dung qua HTTPS, bật mã hóa SSL.
```
ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
ssl_ciphers AES128-SHA:AES256-SHA:RC4-SHA:DES-CBC3-SHA:RC4-MD5;
ssl_certificate /usr/local/nginx/conf/cert.pem;
ssl_certificate_key /usr/local/nginx/conf/cert.key;
```













## CÁC MODULE TRONG NGINX
| Module                       | Mục đích                                          |
| ---------------------------- | ------------------------------------------------- |
| `ngx_http_ssl_module`        | Hỗ trợ HTTPS                                      |
| `ngx_http_gzip_module`       | Nén dữ liệu khi gửi                               |
| `ngx_http_rewrite_module`    | Viết lại URL bằng các rule logic                  |
| `ngx_http_auth_basic_module` | Xác thực truy cập bằng user/pass                  |
| `ngx_http_proxy_module`      | Chuyển tiếp request đến server khác (proxy\_pass) |

