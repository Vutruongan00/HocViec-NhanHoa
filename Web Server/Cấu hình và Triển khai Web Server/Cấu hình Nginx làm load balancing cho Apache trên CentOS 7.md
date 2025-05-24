# Cấu hình Nginx làm load balancing cho Apache trên CentOS 7

## Tổng quan
- **Nginx** sẽ đóng vai trò **Load Balancer** (ở trước), phân phối request đến nhiều **Apache HTTP Server** (backend).

### Môi trường LAB:
Gồm 3 máy CentOS 7:
- **Nginx Load Balancer**: `192.168.137.133`

- **Apache Backend 1**: `192.168.137.141`

- **Apache Backend 2**: `192.168.137.142`

---
### Các bước cấu hình

#### Bước 1: Cài đặt Apache trên các máy backend:
---
- Trên `192.168.137.141` và `192.168.137.142`:

```bash!
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
```
- Tạo trang test khác nhau để phân biệt:
```bash!
echo "Apache Server 1" | sudo tee /var/www/html/index.html   # trên máy Backend 1 /.141
echo "Apache Server 2" | sudo tee /var/www/html/index.html   # trên máy Backend 2 /.142
```
- Mở Firewall:
```bash!
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --reload
```

#### Bước 2: Cài đặt và cấu hình Nginx trên Load Balancer

- Cài đặt Nginx:

```bash!
sudo yum install epel-release -y
sudo yum install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```
- ** Cấu hình Nginx làm Load Balancer:**

    - Sửa file cấu hình: `sudo vi /etc/nginx/nginx.conf
`
    - Thêm đoạn sau vào phần `http { ... }`:

```bash!
http {
    upstream apache_backend {
        server 192.168.137.141;
        server 192.168.137.142;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://apache_backend;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
        }
    }
}
```

>- `upstream apache_backend` là cụm backend chứa nhiều Apache Server.
>- `proxy_pass` dùng để chuyển tiếp request đến backend.
>- Các dòng `proxy_set_header` giúp truyền IP client đến backend.


![image](https://github.com/user-attachments/assets/836619e3-1841-457e-8c02-021b17d2ffc7)


- Kiểm tra và khởi động lại Nginx:

```bash!
sudo nginx -t      # Kiểm tra cấu hình
sudo systemctl restart nginx
```
 
 output như này là oke:
 ```bash!
 nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```
    
    
#### Bước 3: Kiểm tra hoạt động 
- dùng trình duyệt truy cập http://192.168.137.133/index.html  

F5 Reload lại trang web sẽ thấy nội dung lần lượt trả về từ **Apache Server 1** và **Apache Server 2**

![image](https://github.com/user-attachments/assets/5d176438-7b70-4212-b541-247e5c3d8d16)


![image](https://github.com/user-attachments/assets/037ee485-d4e1-417d-a5b1-fcb4ec2ac853)


**--> Cấu hình chia tải thành công!**


