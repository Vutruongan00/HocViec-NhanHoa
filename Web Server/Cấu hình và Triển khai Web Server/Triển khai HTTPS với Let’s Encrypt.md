# Triển khai HTTPS với Let’s Encrypt 

>**CHUẨN BỊ**:
> - Tên miền thật: antvpro.io.vn
> - Cấu hình DNS cho anvtpro.io.vn đã trỏ đúng IP VPS 
![image](https://github.com/user-attachments/assets/00cfbec5-0c3b-478f-bf64-989b7eb8e766)


## APACHE

### Cài đặt Apache (nếu chưa có)
```bash!
sudo dnf install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
```

### Cài đặt Certbot và plugin Apache trên CentOS
```bash!
sudo dnf install epel-release -y
sudo dnf install certbot python3-certbot-apache -y
```
### Tạo thư mục web và cấu hình VirtualHost cho domain
```bash!
sudo mkdir -p /var/www/anvtpro.io.vn/public_html
sudo chown -R $USER:$USER /var/www/anvtpro.io.vn/public_html
```
- Tạo file test:
```
echo "<h1>Welcome to anvtpro.io.vn</h1>" | sudo tee /var/www/anvtpro.io.vn/public_html/index.html
```

- Tạo file cấu hình Apache VirtualHost::
```bash!
sudo nano /etc/httpd/conf.d/anvtpro.io.vn.conf
```
Với nội dung:
```bash!
<VirtualHost *:80>
    ServerName anvtpro.io.vn
    ServerAlias www.anvtpro.io.vn
    DocumentRoot /var/www/anvtpro.io.vn/public_html

    ErrorLog /var/log/httpd/anvtpro.io.vn-error.log
    CustomLog /var/log/httpd/anvtpro.io.vn-access.log combined
</VirtualHost>
```

### Cấu hình FIREWALL
- Mở port `80` và `443`
```bash!
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload
```
### Kiểm tra cấu hình Apache và restart
```bash!
sudo apachectl configtest
sudo systemctl restart httpd
```
###  Chạy Certbot để cấp và cài SSL
```
sudo certbot --apache
```
### Chạy Kiểm tra HTTPS

![image](https://github.com/user-attachments/assets/2bf43850-34b9-4111-964f-f2e773ae37f5)
