# Reverse Proxy là gì? 
- **Reverse proxy** là một máy chủ trung gian đứng giữa **người dùng (client)** và **máy chủ đích (Apache)**. 
- Khi client gửi request, request sẽ đi qua **reverse proxy** (ở đây là **Nginx**) trước, rồi mới chuyển đến Apache xử lý và trả kết quả lại cho client.
# Lợi ích khi dùng Nginx reverse proxy cho Apache
| Lợi ích            | Giải thích                                                               |
| --------------------- | ------------------------------------------------------------------------ |
| **Tăng hiệu suất**    | Nginx xử lý tĩnh (HTML, ảnh, JS, CSS...) rất nhanh, giảm tải cho Apache. |
| **Load balancing**    | Nginx có thể phân phối tải đến nhiều backend (nhiều Apache server).      |
| **Bảo mật**           | Nginx che giấu backend Apache, giảm nguy cơ bị tấn công.                 |
| **SSL termination**   | Nginx xử lý HTTPS, Apache chỉ xử lý HTTP nội bộ.                         |
| **Ghi log tập trung** | Dễ quản lý và phân tích log từ Nginx.                                    |

# CẤU HÌNH
## 1. Cài đặt Nginx
## 2. Cài đặt Apache
```yum install httpd```
## 3. Cấu hình Nginx reverse proxy cho Apache
- Trước tiên ta cũng cần backup file `nginx.conf` và thay đổi cấu hình của nó như sau:
 ```
 mv /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bak
 ```
```
 sudo nano /etc/nginx/nginx.conf
``` 
```cat=
worker_processes 4;
pid /var/run/nginx.pid;
 
events {
        worker_connections 768;
}
 
http {
 
        sendfile on;
        tcp_nopush on;
        tcp_nodelay on;
        keepalive_timeout 65;
        types_hash_max_size 2048;
 
        include /etc/nginx/mime.types;
        default_type application/octet-stream;
 
        access_log /var/log/nginx/access.log;
        error_log /var/log/nginx/error.log;
 
        gzip on;
        gzip_disable "msie6";
        gzip_min_length  1100;
        gzip_buffers  4 32k;
        gzip_types    text/plain application/x-javascript text/xml text/css;
 
        open_file_cache          max=10000 inactive=10m;
        open_file_cache_valid    2m;
        open_file_cache_min_uses 1;
        open_file_cache_errors   on;
 
        ignore_invalid_headers on;
        client_max_body_size    8m;
        client_header_timeout  3m;
        client_body_timeout 3m;
        send_timeout     3m;
        connection_pool_size  256;
        client_header_buffer_size 4k;
        large_client_header_buffers 4 32k;
        request_pool_size  4k;
        output_buffers   4 32k;
        postpone_output  1460;
 
 
        include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/sites-enabled/*;
}
```


- Tạo file Vhost cho nginx, file cấu hình của vhost sẽ được lưu trong đường dẫn `/etc/nginx/conf.d/` và `/etc/nginx/sites-enabled/`

- Cấu hình mặc định cho Nginx lắng nghe cổng :80 để không bị trùng với Apache chạy trên cổng :8080
```
sudo nano /etc/nginx/conf.d/truongan12345.com.conf
```
```cat=
server {
        listen    80;
        server_name  truongan12345.com www.truongan12345.com;
        access_log off;
        error_log  /var/log/httpd/truongan12345.com-error_log crit;
 
location ~* .(gif|jpg|jpeg|png|ico|wmv|3gp|avi|mpg|mpeg|mp4|flv|mp3|mid|js|css|html|htm|wml)$ {
        root /var/www/html/truongan12345.com;
        expires 30d;
        }
 
location / {
        client_max_body_size    10m;
        client_body_buffer_size 128k;
 
        proxy_send_timeout   90;
        proxy_read_timeout   90;
        proxy_buffer_size    128k;
        proxy_buffers     4 256k;
        proxy_busy_buffers_size 256k;
        proxy_temp_file_write_size 256k;
        proxy_connect_timeout 30s;
 
        proxy_redirect  http://www.truongan12345.com:8080   http://www.truongan12345.com;
        proxy_redirect  http://truongan12345.com:8080   http://truongan12345.com;
 
        proxy_pass   http://127.0.0.1:8080/;
 
        proxy_set_header   Host   $host;
        proxy_set_header   X-Real-IP  $remote_addr;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        }
}
```

- Copy file Vhost vừa tạo ở trên tới thư mục /etc/nginx/sites-available\
```
cp /etc/nginx/conf.d/truongan12345.com.conf /etc/nginx/sites-available/truongan12345.com.conf
```
- Tạo symlink `sites-available` tới `sites-enabled`
```
sudo ln -s /etc/nginx/sites-available/truongan12345.com.conf /etc/nginx/sites-enabled/truongan12345.com.conf
```

- Start lại service nginx:
![image](https://github.com/user-attachments/assets/7f03eec6-6635-4d0a-baac-54bd38094a76)



## 4. Cấu hình Apache:

- Sửa file /etc/httpd/conf/httpd.conf để tạo thêm Vhost cho domain truongan12345.com trên apache: 
```
sed -i 's/Listen 80/Listen 8080/g' /etc/httpd/conf/httpd.conf 
```

- Tạo file vhost trong Apache: 
`sudo nano /etc/httpd/conf.d/truongan12345.com`
 
và thêm đoạn cấu hình sau:
```cat=
<VirtualHost *:8080>
ServerAdmin webmaster@truongan12345.com
    DocumentRoot /var/www/html/truongan12345.com
    ServerName truongan12345.com
    ServerAlias www.truongan12345.com
    <Directory "/var/www/html/truongan12345.com">
               AllowOverride All
               Order allow,deny
               Allow from all
    </Directory>
       RewriteEngine on
    ErrorLog logs/truongan12345.com-error_log
    CustomLog logs/truongan12345.com-access_log common
</VirtualHost> 
```

-  Tạo thư mục chứa code của website truongan12345.com theo file cấu hình ở trên trong `/var/www/html/truongan12345.com` (đã tạo)
```
sudo mkdir -p /var/www/html/truongan12345.com
```
![image](https://github.com/user-attachments/assets/3eb4388a-ef77-4bda-8ee1-d5ec0881ddf4)



-->  Truy cập trang web từ máy client:
![image](https://github.com/user-attachments/assets/f40a73ea-93c0-489b-981f-b13958347e97)


----
Do bài lab này  làm trên nền của bài Triển khai virtualhost trong NGINX
>**Những Lưu ý ở bài này:**
> 1. Kiểm tra Apache có đang phục vụ thư mục đúng:
> `sudo nano /etc/httpd/conf/httpd.conf`
> Tìm phần `DocumentRoot`. Đảm bảo nó trỏ đúng đến
> `DocumentRoot "/var/www/html/truongan12345.com"`
>2.  Kiểm tra quyền truy cập:
>`sudo chown -R apache:apache /var/www/html/truongan12345.com`
>`sudo chmod -R 755 /var/www/html/truongan12345.com`
>3. Để Apache đọc được file `.php` cụ thể là file `proxy.php` mình tạo thì phải **kiểm tra Apache có module PHP chưa** và cài PHP cho Apache (nếu chưa có)
>- Kiểm tra: `httpd -M | grep php`
>- Cài PHP Module cho Apache(centOS7):
> ```
> sudo yum install php php-cli php-common php-mysql php-mbstring php-gd php-devel php-pdo php-fpm php-soap php-xml php-intl php-zip php-bcmath php-opcache -y
> 
> sudo systemctl restart httpd

4. Bảo đảm trong file cấu hình Apache `/etc/httpd/conf/httpd.conf` có dòng sau:
```
AddType application/x-httpd-php .php
AddHandler php-script .php
```
5. Sau mỗi lưu ý trên cần **Khởi động lại Apache** cho chắc!



