># Các Dịch Vụ Bổ Trợ

Ngoài các tính năng email cốt lõi, Zimbra có thể được tích hợp hoặc đi kèm với nhiều dịch vụ bổ trợ để nâng cao trải nghiệm người dùng, bảo mật và khả năng quản lý.

# 1. Giao Diện Webmail 

## 1.1. Roundcube

* **Mô tả:** Roundcube là một ứng dụng webmail mã nguồn mở phổ biến, nhẹ và nhanh chóng. Nó có giao diện người dùng tối giản, hiện đại và tập trung vào chức năng cơ bản của email.
* **Ưu điểm:**

  * Giao diện người dùng sạch sẽ, dễ sử dụng.
  * Hỗ trợ nhiều plugin để mở rộng tính năng.
  * Thường được ưa chuộng cho những người chỉ cần một giao diện email đơn giản, không quá nhiều tính năng như Zimbra Web Client đầy đủ.
* **Cách tích hợp với Zimbra:** Roundcube kết nối với Zimbra thông qua các giao thức tiêu chuẩn như IMAP và SMTP. Bạn sẽ cần cài đặt Roundcube trên một máy chủ web riêng (ví dụ: Apache/Nginx) và cấu hình nó để trỏ đến máy chủ Zimbra của bạn để xác thực và truy xuất email.

## 1.2. RainLoop

* **Mô tả:** RainLoop cũng là một ứng dụng webmail mã nguồn mở, nổi bật với giao diện người dùng rất hiện đại, cảm ứng tốt và khả năng tích hợp mạng xã hội (tùy chọn). Nó được thiết kế để mang lại trải nghiệm người dùng mượt mà và trực quan.
* **Ưu điểm:**

  * Giao diện người dùng đẹp mắt, responsive (thích ứng tốt trên các thiết bị).
  * Hỗ trợ kéo và thả file, xem trước file đính kèm.
  * Tốc độ nhanh, tiêu tốn ít tài nguyên.
* **Cách tích hợp với Zimbra:** Tương tự như Roundcube, RainLoop cũng kết nối qua IMAP/SMTP. Cần cài đặt trên một máy chủ web riêng và cấu hình để tương tác với Zimbra.

# 2. Lịch (Calendar) & Danh Bạ (CardDAV, CalDAV)

Zimbra không chỉ là một máy chủ email; nó còn là một nền tảng cộng tác mạnh mẽ với các tính năng lịch và danh bạ tích hợp. Điều quan trọng là khả năng đồng bộ hóa các dữ liệu này với các ứng dụng bên thứ ba bằng các giao thức mở.

## 2.1. CalDAV (cho Lịch)

* **Mô tả:** CalDAV là một giao thức chuẩn mở cho phép người dùng truy cập và quản lý dữ liệu lịch trên một máy chủ từ xa. Nó cho phép đồng bộ hóa các sự kiện lịch, lời nhắc và lời mời giữa Zimbra và các ứng dụng lịch khác.
* **Cách sử dụng:**

  * Các ứng dụng lịch phổ biến trên máy tính (như Apple Calendar, Mozilla Thunderbird Lightning) và điện thoại (ứng dụng lịch của iOS, ứng dụng bên thứ ba trên Android) có thể được cấu hình để kết nối với lịch Zimbra qua CalDAV.
  * Thông thường, bạn sẽ cần nhập URL CalDAV của Zimbra (thường là `https://your_zimbra_server/dav/user@yourdomain.com/Calendar/`).
* **Lợi ích:** Đảm bảo lịch của bạn luôn được đồng bộ và cập nhật trên mọi thiết bị, giúp quản lý thời gian hiệu quả hơn.

## 2.2. CardDAV (cho Danh Bạ)

* **Mô tả:** CardDAV là một giao thức chuẩn mở để đồng bộ hóa danh bạ cá nhân trên một máy chủ từ xa. Nó cho phép bạn truy cập, thêm, sửa đổi và xóa các liên hệ từ Zimbra và các ứng dụng danh bạ khác.
* **Cách sử dụng:**

  * Các ứng dụng danh bạ trên máy tính (như Apple Contacts) và điện thoại (ứng dụng danh bạ của iOS, ứng dụng bên thứ ba trên Android) có thể được cấu hình để kết nối với danh bạ Zimbra qua CardDAV.
 
* **Lợi ích:** Đảm bảo danh bạ của bạn luôn được đồng bộ và cập nhật trên mọi thiết bị, giúp bạn dễ dàng liên hệ với mọi người.

# 3. Anti-spam Nâng Cao 

Mặc dù Zimbra đã tích hợp SpamAssassin, nhưng đối với môi trường doanh nghiệp với lượng email lớn hoặc yêu cầu bảo mật cao hơn, việc triển khai các giải pháp chống spam nâng cao là cần thiết.

## 3.1. Rspamd

* **Mô tả:** Rspamd là một hệ thống lọc spam và chống virus hiện đại, nhanh chóng và mạnh mẽ. Nó sử dụng nhiều kỹ thuật tiên tiến (như phân tích xác suất, RBLs, DKIM, DMARC, SPF, Neural Networks) để đánh giá và lọc email.
* **Ưu điểm:**

  * Hiệu suất cao, xử lý email rất nhanh.
  * Độ chính xác cao trong việc nhận diện spam.
  * Khả năng cấu hình linh hoạt, có thể tùy chỉnh các quy tắc.
  * Có giao diện web để theo dõi và quản lý.
* **Cách tích hợp với Zimbra:** Rspamd thường được triển khai như một dịch vụ riêng biệt và được tích hợp với Postfix (thành phần MTA của Zimbra) thông qua giao thức Milter hoặc bằng cách sử dụng các bộ lọc Postfix. Zimbra sẽ chuyển email đến Rspamd để phân tích trước khi xử lý tiếp.

## 3.2. MailScanner

* **Mô tả:** MailScanner là một trình quét email mã nguồn mở, hoạt động như một lớp bảo vệ giữa Mail Transfer Agent (Postfix/Sendmail) và các bộ lọc nội dung (như SpamAssassin, ClamAV). Nó kiểm tra tất cả các email đến và đi để tìm spam, virus, lừa đảo và các mối đe dọa khác.
* **Ưu điểm:**

  * Là một giải pháp toàn diện cho cả spam và virus.
  * Khả năng tích hợp với nhiều công cụ khác nhau (SpamAssassin, ClamAV, v.v.).
  * Đáng tin cậy và đã được sử dụng rộng rãi trong nhiều năm.
* **Cách tích hợp với Zimbra:** MailScanner thường được cài đặt trên cùng máy chủ hoặc một máy chủ riêng, và Postfix của Zimbra sẽ được cấu hình để gửi tất cả email đến MailScanner để quét trước khi đưa vào hàng đợi của Zimbra.

**Lưu ý quan trọng:** Việc tích hợp các giải pháp chống spam nâng cao như Rspamd hoặc MailScanner yêu cầu kiến thức chuyên sâu về Postfix và cấu hình hệ thống. Chúng thường được triển khai ở một lớp ngoài hoặc giữa MTA và Mailboxd của Zimbra để xử lý email trước khi chúng đến hộp thư người dùng.


---


# 4. TH: Cài đặt Roundcube với Zimbra
>Cài trên một máy chủ web riêng (ví dụ: Apache/Nginx) và cấu hình nó để trỏ đến máy chủ Zimbra

1. **Bước 1: Cài đặt Apache, PHP và các gói cần thiết**
```bash
sudo apt update && apt upgrade -y
sudo apt install apache2 php php-cli php-common php-mbstring php-xml php-intl php-mysql php-zip php-curl php-gd php-imagick unzip mariadb-server -y
```

2. **Bước 2: Tạo database cho Roundcube:**
```bash!
sudo mysql -u root -p
```
```sql!
CREATE DATABASE roundcubemail DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
CREATE USER 'user1'@'localhost' IDENTIFIED BY 'PassWordstrong!';
GRANT ALL PRIVILEGES ON roundcube.* TO 'user1'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```
> Thay tên DB, username, password tùy ý, nhưng phải nhớ để cấu hình ở bước sau

3. **Bước 3: Tải và cấu hình Roundcube**
```bash
## cd /var/www/

wget https://github.com/roundcube/roundcubemail/releases/download/1.6.6/roundcubemail-1.6.6-complete.tar.gz

tar -xvzf roundcubemail-1.6.6-complete.tar.gz

mv roundcubemail-1.6.6 roundcube

chown -R www-data:www-data /var/www/roundcube
```

4. **Bước 4: ấu hình Apache Tạo virtual host Rounecube**
`sudo nano /etc/apache2/sites-available/roundcube.conf`

```apacheconf
<VirtualHost *:80>
    ServerName webmail.antvpro.io.vn
    DocumentRoot /var/www/roundcube

    <Directory /var/www/roundcube/>
        Options -Indexes
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/roundcube_error.log
    CustomLog ${APACHE_LOG_DIR}/roundcube_access.log combined
</VirtualHost>
```

- **Kích hoạt site:**
```bash!
sudo a2dissite 000-default.conf
sudo a2ensite roundcube.conf
sudo a2enmod rewrite
sudo systemctl reload apache2
```

5. **Bước 5: Cài đặt Roundcube qua trình duyệt**
- Truy cập: và làm theo hướng dẫn sau:
http://webmail.antvpro.io.vn/installer

    - Trang đầu tiên cứ để mặc định:
        - G**eneral Configuration
Product name**: Roundcube Webmail
        - **Language**: Vietnamese (tuỳ chọn)
        
**--> Nesxt:** Cấu hình **SMTP/IMAP/DATABASE**
- Nhập đúng tên DB, username, passwd khi tạo ở trên

<img width="765" height="308" alt="image" src="https://github.com/user-attachments/assets/fc1fe4d6-b19b-40c0-9972-948a66aff824" />

- IMAP

<img width="655" height="275" alt="image" src="https://github.com/user-attachments/assets/f2fb4758-47cb-42d8-a03b-645099c08801" />

- SMTP

<img width="653" height="282" alt="image" src="https://github.com/user-attachments/assets/569f739c-17bd-487b-9d41-446125939d18" />


- **Xóa thư mục cài đặt khi đã cài đặt xong**
```bash
sudo rm -rf /var/www/roundcube/installerad
```

#####  Hoặc Cấu hình trong file `config.inc.php`
- Nếu có thay đổi (hoặc lỗi) có thể cấu hình trong

```
nano /var/www/roundcube/config/config.inc.php
```
- Cấu hình chuẩn sẽ như này:
```php!
<?php
$config = [];

// 1. Cấu hình database
$config['db_dsnw'] = 'mysql://user1:Passwdstrong!@localhost/roundcubemai';

// 2. Cấu hình IMAP
$config['imap_host'] = 'ssl://mail.antvpro.io.vn';
$config['imap_port'] = 993;

// 3. Cấu hình  SMTP 
$config['smtp_host'] = 'tls://mail.antvpro.io.vn';
$config['smtp_port'] = 587;
$config['smtp_user'] = '%u';
$config['smtp_pass'] = '%p';

// ➕ Gợi ý thêm dòng này để tránh lỗi xác thực một số hệ thống Zimbra
$config['imap_auth_type'] = 'PLAIN';

// 
$config['support_url'] = '';
$config['product_name'] = 'Roundcube Webmail';
$config['des_key'] = 'rcmail-!24ByteDESkey*Str';
$config['plugins'] = [
    'archive',
    'zipdownload',
];
$config['skin'] = 'elastic';
```

-  **Khởi động lại Apach để áp dụng cấu hình chỉnh sửa**
```bash!
sudo systemctl restart apache2
```

6. **Bước 6: Truy cập giao diện Webmail**
https://webmail.antvpro.io.vn/

<img width="921" height="647" alt="image" src="https://github.com/user-attachments/assets/2b431706-4aad-4b1e-a5c0-fb94bac1961f" />

> Lưu ý: Đừng quên cấu hình **DNS Record trỏ subdomain về IP của server Roundcube** nhé!! hẹ hẹ

<img width="1279" height="587" alt="image" src="https://github.com/user-attachments/assets/e96a346a-d818-40f1-8a57-ed2eb3a2cb46" />

---

# 5. Anti-spam nâng cao: Rspamd

