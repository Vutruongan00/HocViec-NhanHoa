# Triển khai 2 Website trên NGINX (CentOS 7)

## 1. Cài đặt NGINX
- Tắt selinux:
```
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g'  /etc/selinux/config
```
- Tắt firewall trên server:
```
systemctl disable firewalld
systemctl stop firewalld
```
- Update OS cho webserver:
```
yum update -y
init 6
```
- truy cập vào đường dẫn repo:
`cd /etc/yum.repos.d/`
- Tạo file repo cho nginx:
```
cat >> nginx.repo << "EOF"
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=0
enabled=1
EOF
```
- Cài đặt và restart nginx:
`yum install nginx -y`
`service nginx start`

## 2. Cài đặt PHP
- Để Vhost có thể xử lý được file php chúng ta cần cài đặt php, php-fpm và các extention liên quan.Chúng ta truy cập ssh vào máy chủ webserver và chạy lệnh cài đặt bên dưới:
> yum install -y php-common php-bcmath php-cli php-devel php-mcrypt php-mysql php-password-compat php-pclzip php-pdo php-gd php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc php-dba php-embedded php-enchant php-mbstring php-intl libssh2 php-pecl-ssh2 php-pecl-memcached php-pecl-memcache php-fpm
- Trên Centos 7 nếu lỗi, tham khảo link này:
https://github.com/AtlasGondal/centos7-eol-repo-fix
- Sửa file cấu hình của php-fpm:
```
sed -i 's/user = apache/user = nginx/g' /etc/php-fpm.d/www.conf
sed -i 's/group = apache/group = nginx/g' /etc/php-fpm.d/www.conf
service php-fpm restart
```

## 3. Cấu hình Virtual Host
- Truy cập vào đường dẫn chứa file config của nginx:
`cd /etc/nginx/`

- Backup lại file nginx.conf:
`mv nginx.conf nginx.conf.bak`

- Sửa file cấu hình Nginx:
```cat >> nginx.conf << "EOF"
user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}
EOF
```
- Truy cập vào đường dẫn chung chứa code của các website và tạo thư mục riêng chứa mã nguồn của website **truongan12345.com**
```
cd /var/www/html

mkdir truongan12345.com
```
- Tạo 1 file `info.php` trong thư mục **truongan12345.com** và thêm nội dung sau để trang web hiển thị:

```
<?php
echo "Website11111 xin chao anh VU TRUONG AN";
phpinfo();
?>
```

- Truy cập đường dẫn chứa các file cấu hình của virtualhost và Backup file cấu hình default
```
cd /etc/nginx/conf.d/

mv default.conf default.conf.bak
```

- Tạo virtualhost trong Nginx:
```
cat >> truongan12345.com.conf << "EOF"
server {
    listen       80;
    server_name  truongan12345.com;
    access_log /var/log/nginx/truongan12345.com-access_log;
    error_log /var/log/nginx/truongan12345.com-error_log;

location / {
        root   /var/www/html/truongan12345.com;
        index  index.html index.php index.htm;
    }

location ~ \.php$ {
        root           /var/www/html/truongan12345.com;
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }
}
EOF
```
- Tạo thư mục chứa log cho Vhost và cấp quyền cho **user Nginx** có quyền đọc/ghi vào thư mục `log`
```
mkdir -p /var/log/nginx

chown -R nginx:nginx /var/log/nginx
```

- Restart lại service nginx: 
`service nginx restart`
- --> **Truy cập website vừa tạo trên trình duyệt của máy Client (tôi dùng WINDOWS)**
- Lưu ý cấu hình file hosts trong: `C:\Windows\System32\drivers\etc\hosts
![image](https://github.com/user-attachments/assets/88ba894d-9d26-41b8-926d-3c2513ddb8d8)

--> **http://truongan12345/info.php trả về kết quả như hình bên dưới**

![image](https://github.com/user-attachments/assets/de9c191c-669a-4529-aeda-ccb5e2fbfc43)

## 4. Thêm một Virtual Host khác trên Web Server (Site2)

- Tương tự tạo site2: truongan.dragon.com
```
cd /var/www/html
mkdir truongan.dragon.com
```

- Tạo 1 file `test.php` và Thêm nội dung cần hiển thị trên website, ví dụ đơn giản như sau:
```
<?php
echo "Website222222 xinchao An! ";
?>
```

- Truy cập đường dẫn chứa các file cấu hình của virtualhost:
`cd /etc/nginx/conf.d/`
- Tạo virtualhost trong Nginx:

![image](https://github.com/user-attachments/assets/b4dca3ae-7657-42e3-b7ae-c35c204fb956)
```
cat >> truongan.dragons.com.conf << "EOF"
server {
    listen       80;
    server_name  truongan.dragons.com;
    access_log /var/log/nginx/truongan.dragons.com-access_log;
    error_log /var/log/nginx/truongan.dragons.com-error_log;

location / {
        root   /var/www/html/truongan.dragons.com;
        index  index.html index.php index.htm;
    }

location ~ \.php$ {
        root           /var/www/html/truongan.dragons.com;
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }
}
EOF
```
- Restart lại service nginx

--> **Truy cập website vừa tạo trên trình duyệt của máy Client **
- Lưu ý cấu hình file hosts trong: `C:\Windows\System32\drivers\etc\hosts`
--> **http://truongan.dragon.com/test.php trả về kết quả như hình bên dưới**
![image](https://github.com/user-attachments/assets/1ad69607-4c28-467e-8897-e36986c36809)

