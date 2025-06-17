# Cấu hình Virtual Host và Triển Khai nhiều Website Apache trên CentOS

### Cài đặt Apache Web Server:
```
sudo yum install httpd -y
```

### Khởi động và bật Apache khi khởi động hệ thống:
```bash!
sudo systemctl start httpd
sudo systemctl enable httpd
```

### Cấu hình Tường lửa (Firewall)
```bash!
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload
```
### Tạo thư mục chứa website

- Giả sử để chạy hai website:

    site1.com
    site2.com
- Tạo thư mục và file cho từng website:
```bash!
sudo mkdir -p /var/www/site1.com/public_html
sudo mkdir -p /var/www/site2.com/public_html
```
- Phân quyền:
```bash!
sudo chown -R $USER:$USER /var/www/site1.com/public_html
sudo chown -R $USER:$USER /var/www/site2.com/public_html
sudo chown -R apache:apache /var/www/site1.com
sudo chown -R apache:apache /var/www/site2.com
sudo chmod -R 755 /var/www/site1.com
sudo chmod -R 755 /var/www/site2.com
```

- Tạo file index.html để kiểm tra:
```bash!
echo "<h1>Welcome to site1.com</h1>" | sudo tee /var/www/site1.com/public_html/index.html
echo "<h1>Welcome to site2.com</h1>" | sudo tee /var/www/site2.com/public_html/index.html
```
### Tạo Virtual Host file
- Mặc định trên CentOS, cấu hình Apache nằm ở `/etc/httpd/conf.d`
```bash!
sudo nano /etc/httpd/conf.d/site1.com.conf
```
-  Dán nội sau dung vào file cấu hình:
```bash!
<VirtualHost *:80>
    ServerAdmin admin@site1.com
    ServerName site1.com
    ServerAlias www.site1.com
    DocumentRoot /var/www/site1.com/public_html
    ErrorLog /var/log/httpd/site1_error.log
    CustomLog /var/log/httpd/site1_access.log combined
</VirtualHost>
```

**- Tương tự cho `site2.com`**
```
sudo nano /etc/httpd/conf.d/site2.com.conf
```
Nội dung:
```bash!
<VirtualHost *:80>
    ServerAdmin admin@site2.com
    ServerName site2.com
    ServerAlias www.site2.com
    DocumentRoot /var/www/site2.com/public_html
    ErrorLog /var/log/httpd/site2_error.log
    CustomLog /var/log/httpd/site2_access.log combined
</VirtualHost>
```

### Kiểm tra cấu hình và khởi động lại Apache
```bash!
sudo apachectl configtest
sudo systemctl restart httpd
```

### Cập nhật file hosts (nếu dùng máy ảo hoặc test nội bộ)

- Nếu không có domain thật mà chỉ đang test nội bộ, thêm vào máy (client) hoặc máy chủ file hosts: `sudo nano /etc/hosts`
```bahs!
192.168.137.133   site1.com site2.com
```
>`192.168.137.133` là ip máy chủ CentOS cài Apache Webserver


### Truy cập thử nghiệm

- Từ trình duyệt máy Client, truy cập `http://site1.com` hoặc `http://site2.com`
![image](https://github.com/user-attachments/assets/c5c862c1-a5e6-41ee-9fe4-d89449d294df)

