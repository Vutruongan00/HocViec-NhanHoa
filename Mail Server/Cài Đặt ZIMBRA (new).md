# Cài Đặt ZIMBRA (Ubuntu 22.04)

> ### **Yêu cầu cài đặt:**
> 
> **OS**: Ubuntu 22.04 x64 (fresh cài đặt)
> 
> **RAM**: ≥ 4 GB (tốt nhất 8 GB nếu đầy đủ dịch vụ)
> 
> **Disk**: ≥ 40 GB
> 
> **Tên miền**: đã trỏ record A về IP (ví dụ: mail.antvpro.io.vn)
> 
> **Port**: 22, 25, 80, 443, 587, 465, 993 


## **BƯỚC 1: Chuẩn bị hệ thống**
- **Đặt hostname:**

```bash!
sudo hostnamectl set-hostname mail.antvpro.io.vn
```

- Sửa file `nano /etc/hosts`, Thêm dòng:

```lua!
127.0.0.1 localhost 
35.185.137.50 mail.antvpro.io.vn mail   -- IP Public
10.140.0.7   mail.antvpro.io.vn mail    -- IP local của máy
```

- Cập nhật hệ thống & cài gói phụ thuộc

```bash!
sudo apt update && sudo apt upgrade -y 
sudo apt install -y curl rsync net-tools libsasl2-2 libsasl2-modules libsasl2-modules-ldap
```

-  **Cấu hình DNS Records**
    - **Bản ghi A (Address Record)**
    - **Bản ghi MX (Mail Exchanger Record)**

- **Vô hiệu hóa và gỡ cài đặt Postfix/Sendmail/Apache (nếu có):**
```bash!
sudo systemctl stop postfix sendmail apache2 nginx
sudo systemctl disable postfix sendmail apache2 nginx
sudo apt remove --purge postfix sendmail apache2 nginx -y
sudo apt autoremove -y
```

- **Cấu hình tường lửa (Firewall):**
```bash!
sudo ufw allow ssh 
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp 
sudo ufw allow 25/tcp 
sudo ufw allow 587/tcp     # SMTP Submission (cho các ứng dụng client gửi thư)
sudo ufw allow 993/tcp     # IMAPS (IMAP qua SSL/TLS)
sudo ufw allow 995/tcp     # POP3S (POP3 qua SSL/TLS)
sudo ufw allow 7071/tcp     # Zimbra Admin Console
sudo ufw enable
sudo ufw status
```

---

## Bước 2: Tạo máy chủ DNS cục bộ 
>### Áp dụng khi chưa trỏ DNS hoặc sử dụng local
- **Tạo Zone DNS bind:**
```bash!
cp /etc/bind/named.conf.local /etc/bind/named.conf.local.backup
cp /etc/bind/named.conf.options /etc/bind/named.conf.options.backup
> /etc/bind/named.conf.local
sed -i '/directory*/a\        forwarders {8.8.8.8; 8.8.4.4;};' /etc/bind/named.conf.options;
```

- Tạo file `nano /etc/bind/named.conf.local` và thêm nội dung: 
```ini!
zone  antv.com {
        type master;
                file "/etc/bind/zones/db.antvpro.io.vn";
        allow-transfer {
                127.0.0.1;
                localnets;
                };
        };
```
- Tạo file `nano /etc/bind/zones/db.antvpro.io.vn` và thêm nội dung:
```
$ttl 3600
@       IN      SOA     mail.antvpro.io.vn. admin.antvpro.io.vn. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL

@       IN      NS      mail.antvpro.io.vn.
        IN      MX 10   mail.antvpro.io.vn.
mail    IN      A       35.185.137.50
@       IN      A       35.185.137.50
```

- Restart dịch vụ **`bind9`**
```
sudo systemctl restart bind9
```
-  **Kiểm tra tra cứu:**
```
nslookup mail.antv.com
```


--- 

##  **BƯỚC 3: Tải và giải nén Zimbra 
- Di chuyển vào thư mục tạm thời và tải gói cài đặt, `cd /tmp`

```bash!
cd /tmp

wget https://github.com/maldua/zimbra-foss-builder/releases/download/zimbra-foss-build-ubuntu-22.04/10.1.4/zcs-10.1.4_GA_4200000.UBUNTU22_64.20241224171952.tgz
```

- Giải nén:

```bash!
tar -zxvf zcs-10.1.4_GA_4200000.UBUNTU22_64.20241224171952.tgz
```

- Di chuyển vào thư mục vừa giải nén và chạy script cài đặt:
```bash!
cd zcs-10.1.4_GA_4200000.UBUNTU22_64.20241224171952
sudo ./install.sh
```

- Quá trình cài:
    - Gõ y rồi enter
```ini=
----------------------------------------------------------------------
PLEASE READ THIS AGREEMENT CAREFULLY BEFORE 
...
----------------------------------------------------------------------
 
Do you agree with the terms of the software license agreement? [N] y
```

```
Use Zimbra's package repository [Y] y
```

- Nhập `y` theo hướng mẫu dưới đây:

```ini=
Select the packages to install
 
Install zimbra-ldap [Y] y
 
Install zimbra-logger [Y] y
 
Install zimbra-mta [Y] y
 
Install zimbra-dnscache [Y] n
 
Install zimbra-snmp [Y] y
 
Install zimbra-store [Y] y
 
Install zimbra-apache [Y] y
 
Install zimbra-spell [Y] y
 
Install zimbra-memcached [Y] y
 
Install zimbra-proxy [Y] y
```


- Cấu hình mật khẩu admin : Chọn `6` và `4` để tới dòng cấu hình mật khẩu
```ini=
Password for admin@antv.com (min 6 characters): [EHO3AXUi7] Qwerty123@
```

- Nhập `r` 
```ini=
Select, or 'r' for previous menu [r] r
```

- Nhập  `a` --> `y` --> `y`
```ini=
*** CONFIGURATION COMPLETE - press 'a' to apply
Select from menu, or press 'a' to apply config (? - help) a
Save configuration data to a file? [Yes] y
Save config in file: [/opt/zimbra/config.11662] tekan enter
Saving config in /opt/zimbra/config.11662...done.
The system will be modified - continue? [No] y
```
- Nhập `n` --> ENTER
```ini!
Notify Zimbra of your installation? [Yes] n
```

- Cài đặt hoàn tất:
```output=
Moving /tmp/zmsetup.20240823-143050.log to /opt/zimbra/log
 
Configuration complete - press return to exit
```
* Chọn `r` để review cấu hình
* Gõ `a` để apply → quá trình cài sẽ bắt đầu


---

## **BƯỚC 4: Kiểm tra sau cài đặt**
- Khởi động lại các dịch vụ Zimbra
```bash!
sudo su - zimbra
zmcontrol restart
exit
```
- Truy cập webmail:

```
https://mail.antvpro.io.vn
```

* User: `admin@antvpro.io.vn`
* Mật khẩu: đã đặt lúc cài

- Truy cập admin panel:

https://mail.antvpro.io.vn:7071

![image](https://github.com/user-attachments/assets/2c1564bf-9c39-44bc-8766-12146a9e1609)
