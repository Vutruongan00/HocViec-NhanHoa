# Cấu hình & kiểm tra SSL

## 1. Cấu hình HTTPS Redirect (Chuyển hướng 301)

Chuyển hướng HTTP sang HTTPS là một bước cần thiết để đảm bảo tất cả lưu lượng truy cập đến trang web của bạn đều được mã hóa. Chúng ta sử dụng chuyển hướng 301 (Moved Permanently) để thông báo cho trình duyệt và công cụ tìm kiếm rằng địa chỉ mới là HTTPS, giúp duy trì SEO.

- **Mục đích:** Buộc mọi kết nối từ HTTP sang HTTPS.
- **Lợi ích:** Đảm bảo bảo mật toàn diện cho người dùng, tránh nội dung trùng lặp cho SEO.

### a. Apache HTTP Server:**

- Thêm cấu hình này vào `VirtualHost` cho cổng 80: `RedirectMatch 301 ^/(.*)$ https://yourdomain.com/$1`

```Apache!
<VirtualHost *:80>
    ServerName yourdomain.com
    ServerAlias www.yourdomain.com blog.yourdomain.com

    RedirectMatch 301 ^/(.*)$ https://yourdomain.com/$1
  
    RewriteEngine On
    RewriteCond %{HTTPS} off
    RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
</VirtualHost>
```

- Sau khi cấu hình, khởi động lại Apache: 
```
sudo systemctl restart apache2
```



### b. NGINX:**

- Thêm một khối `server` riêng cho cổng 80:

```nginx!
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;   #Liệt kê tất cả các tên miền muốn chuyển hướng

    return 301 https://$host$request_uri; 
}
```

- Sau khi cấu hình, kiểm tra và tải lại NGINX: `sudo nginx -t` và `sudo systemctl reload nginx`.

### c. IIS (Internet Information Services):**

Trong IIS Manager, có thể thiết lập chuyển hướng HTTP sang HTTPS bằng module **URL Rewrite**.

1. **Cài đặt URL Rewrite Module:** Nếu chưa có, tải và cài đặt từ trang web của Microsoft.
2. **Mở IIS Manager:** 
3. **Thêm Rule:** click chọn **URL Rewrite**. Trong khung `Actions` --> **Add Rule(s)...** -->  `Blank Rule` --> `Inbound Rules`.
4. **Cấu hình Rule:**
   - **Name:** `HTTP to HTTPS Redirect`
   - **Match URL:**
     - `Requested URL`: `Matches the Pattern`
     - `Using`: `Regular Expressions`
     - `Pattern`: `(.*)`
   - **Conditions:**
     - `Logical grouping`: `Match All`
     - `Add...`:
       - `Condition input`: `{HTTPS}`
       - `Pattern`: `^OFF$` (Kiểm tra nếu kết nối không phải HTTPS)
   - **Action:**
     - `Action type`: `Redirect`
     - `Redirect URL`: `https://{HTTP_HOST}/{R:1}`
     - `Redirect type`: `Permanent (301)`
5. Click **Apply**.


### d. Apache Tomcat:

Cấu hình chuyển hướng trong tệp `server.xml`.

```xml!
<Connector port="80" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="443" /> <Connector port="443" 
```

Khởi động lại Tomcat sau khi thay đổi: `sudo systemctl restart tomcat`.

---
## 2. Cấu hình HSTS (HTTP Strict Transport Security)

**HSTS** là một tiêu đề phản hồi HTTP cho phép web server thông báo cho trình duyệt rằng nó chỉ nên tương tác với trang web này bằng HTTPS, ngay cả khi người dùng nhập `http\://` hoặc nhấp vào liên kết HTTP cũ.

- **Mục đích:** Buộc trình duyệt chỉ sử dụng HTTPS cho các truy cập sau này, chống lại các cuộc tấn công hạ cấp giao thức (protocol downgrade attacks) và các tấn công đánh cắp cookie.
- **Lợi ích:** Tăng cường bảo mật đáng kể.

**a. Apache HTTP Server:**

- Thêm vào VirtualHost cho cổng 443 (trong phần `SSLEngine on`):

```Apache!
Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains; preload"
```

> - `max-age`: Thời gian (tính bằng giây) trình duyệt sẽ nhớ chỉ truy cập trang web bằng HTTPS (ví dụ: 31536000 giây = 1 năm).
> - `includeSubDomains`: Áp dụng HSTS cho tất cả các tên miền phụ.
> - `preload`: Cho phép bạn đăng ký tên miền của mình vào danh sách tải trước HSTS của các trình duyệt lớn (để truy cập lần đầu tiên cũng là HTTPS).

- Cần đảm bảo `mod_headers` đã được bật: `sudo a2enmod headers`. Khởi động lại Apache.

### b. NGINX:

- Thêm vào khối `server` cho cổng 443:

```Nginx!
add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload" always;
```

- Reload lại NGINX.

### c. IIS:

1. **Mở IIS Manager:** Chọn site cần cấu hình
2. **Mở HTTP Response Headers:** --> `HTTP Response Headers`.
3. **Thêm Header:** Trong khung `Actions` bên phải, chọn `Add...`.
   - `Name`: `Strict-Transport-Security`
   - `Value`: `max-age=31536000; includeSubDomains; preload`
4. Nhấp `OK`.
5. **Lưu ý:** Header này sẽ được áp dụng cho cả HTTP và HTTPS. Để chỉ áp dụng cho HTTPS, cần sử dụng URL Rewrite module để thiết lập điều kiện.

### d. Apache Tomcat:

- Tomcat cung cấp bộ lọc có tên HttpHeaderSecurityFilter, cho phép thêm các tiêu đề HTTP bảo mật như Strict-Transport-Security, X-Frame-Options và X-Content-Type-Options vào phản hồi.
- Bộ lọc này có thể được thêm và cấu hình như các bộ lọc khác thông qua tệp web.xml. Ví dụ cấu hình trong tệp conf/web.xml.


XML

```
<filter>
    <filter-name>HSTSFilter</filter-name>
    <filter-class>org.apache.catalina.filters.HttpHeaderSecurityFilter</filter-class>
    <init-param>
        <param-name>hstsEnabled</param-name>
        <param-value>true</param-value>
    </init-param>
    <init-param>
        <param-name>hstsMaxAgeSeconds</param-name>
        <param-value>31536000</param-value>
    </init-param>
    <init-param>
        <param-name>hstsIncludeSubDomains</param-name>
        <param-value>true</param-value>
    </init-param>
    <init-param>
        <param-name>hstsPreload</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>HSTSFilter</filter-name>
    <url-pattern>/*</url-pattern>
    <dispatcher>REQUEST</dispatcher>
    <dispatcher>ERROR</dispatcher>
</filter-mapping>
```
- Khởi động lại Tomcat.

---

## 3. Cấu hình OCSP Stapling

**OCSP Stapling** là một tính năng giúp tăng tốc độ xác minh chứng chỉ và bảo vệ quyền riêng tư của người dùng.

- **Mục đích:** Cho phép máy chủ của bạn gửi phản hồi OCSP (Online Certificate Status Protocol) được ký bởi CA cùng với chứng chỉ của mình trong quá trình TLS Handshake.
- **Lợi ích:**
  - **Tăng tốc độ:** Trình duyệt không cần phải tự mình liên hệ với máy chủ OCSP của CA, giảm thời gian thiết lập kết nối.
  - **Bảo vệ quyền riêng tư:** CA không biết ai đang truy cập trang web của bạn.
  - **Đáng tin cậy hơn:** Đảm bảo rằng phản hồi OCSP đến từ máy chủ của bạn chứ không phải một bên thứ ba.

**a. Apache HTTP Server:**

- Yêu cầu Apache 2.4.x trở lên. Thêm vào VirtualHost cho cổng 443:
- Thông số cache: Cấu hình nơi lưu cache, đặt bên ngoài cặp <VirtualHost> </VirtualHost>
```apache!
SSLUseStapling on
SSLStaplingCache "shmcb:/var/run/ocsp(128000)" # Đường dẫn và kích thước bộ nhớ cache
```

- Có thể cần cấu hình thêm `SSLStaplingResponderTimeout` và `SSLStaplingReturnResponderErrors`. Khởi động lại Apache.

**b. NGINX:**
- OCSP stapling được hỗ trợ kể từ phiên bản Nginx 1.3.7+. trở đi.
- Thêm vào khối `server` cho cổng 443:

```Nginx!
ssl_stapling on;
ssl_stapling_verify on;
resolver 8.8.8.8 8.8.4.4 valid=300s; 
resolver_timeout 5s;
```

- Kiểm tra và Reload NGINX.

**c. IIS:**

- Mặc định IIS tự động bật OCSP Stapling nếu nó được hỗ trợ bởi chứng chỉ và máy chủ.

**d. Apache Tomcat:**

Tomcat 8 trở lên hỗ trợ OCSP Stapling. Cấu hình trong `server.xml`:

```xml!
<Connector port="8443" protocol="org.apache.coyote.http11.Http11NioProtocol"
           ...
           sslProtocol="TLS"
           ciphers="..."
           sslUseStapling="true"
           sslStaplingCacheSize="100" />

```

- Restart Tomcat.
