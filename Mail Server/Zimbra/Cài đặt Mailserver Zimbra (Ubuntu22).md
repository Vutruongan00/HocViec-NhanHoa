
># CÀI ĐẶT - SỬ DỤNG MAIL SERVER ZIMBRA

# CÀI ĐẶT
> Cài đặt Zimbra trên localhost Ubuntu22.04
> IP: `192.168.137.140` 
> Hostname: `mail.antv.com` 

- Cài đặt các gói cần thiết:
```bash!
sudo apt-get update
sudo apt-get install nano wget bind9 bind9utils telnet perl ufw tar resolvconf net-tools -y
```
- Thiết lập múi giờ:
```bash!
timedatectl set-timezone Asia/Ho_Chi_Minh
```
- Đặt tên máy chủ (hostname):
```bash!
hostnamectl set-hostname mail.antv.com
```
- Tắt các dịch vụ mặc định gửi mail trên hệ thống(nếu không bật thì thôi):
```bash!
systemctl stop postfix
systemctl disable postfix
```
- Đổi Hostname: `nano /etc/hosts` thêm dòng sau:
```
192.168.137.140 mail.antv.com mail
```
- Cấu hình resolv.conf
```bash!
systemctl enable resolvconf
systemctl start resolvconf
cp /etc/resolv.conf /etc/resolv.conf.conf.backup
echo "search antv.vn" >> /etc/resolvconf/resolv.conf.d/head
echo "nameserver 192.168.137.140" >> /etc/resolvconf/resolv.conf.d/head
sudo resolvconf --enable-updates
sudo resolvconf -u
```

## 1. Tạo máy chủ DNS local
- **Tạo Zone DNS bind:**
```bash!
cp /etc/bind/named.conf.local /etc/bind/named.conf.local.backup
cp /etc/bind/named.conf.options /etc/bind/named.conf.options.backup
> /etc/bind/named.conf.local
sed -i '/directory*/a\        forwarders {8.8.8.8; 8.8.4.4;};' /etc/bind/named.conf.options;
```

- Tạo file `/etc/bind/named.conf.local` và thêm nội dung: 
```ini!
zone  antv.com {
        type master;
                file "/etc/bind/zones/db.antv.com";
        allow-transfer {
                127.0.0.1;
                localnets;
                };
        };
```
- Tạo file `/etc/bind/zones/db.antv.com` và thêm nội dung:
```
$ttl 3600
@       IN      SOA     ns1.antv.com. admin.antv.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL

@       IN      NS      ns1.antv.com.
ns1     IN      A       192.168.137.140

mail    IN      A       192.168.137.140
@       IN      MX 10   mail.antv.com.
```
- Restart dịch vụ **`bind9`**
```
sudo systemctl restart bind9
```
-  **Kiểm tra tra cứu:**
```
nslookup mail.antv.com
```
![image](https://github.com/user-attachments/assets/e1a63d7d-333b-49a0-9821-ede557a9e710)

## 2. Tải xuống và cài đặt Zimbra Mail Server
-  Download Paket zimbra mail server:
```bash=
cd /opt/
wget https://github.com/maldua/zimbra-foss-builder/releases/download/zimbra-foss-build-ubuntu-22.04/10.1.4/zcs 10.1.4_GA_4200000.UBUNTU22_64.20241224171952.tgz
```
- Giải nứn và chạy file cài đặt:
```bash=
tar -zxvf zcs-10.1.4_GA_4200000.UBUNTU22_64.20241224171952.tgz
cd zcs-10.1.4_GA_4200000.UBUNTU22_64.20241224171952
./install.sh
```
- Gõ y rồi enter
```ini=
----------------------------------------------------------------------
PLEASE READ THIS AGREEMENT CAREFULLY BEFORE 
...
----------------------------------------------------------------------
 
Do you agree with the terms of the software license agreement? [N] y
```
- tiếp tục: `y`
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
- Nhập `y`, sau đó nhập tên miền. (không có mail. ở phía trước)
```ini=
DNS ERROR resolving MX for mail.saad.my.id
It is suggested that the domain name have an MX record configured in DNS
Change domain name? [Yes] y
Create domain: [mail.example.com] antv.com
```
- Cấu hình mật khẩu admin : Chọn `6` và `4` để tới dòng cấu hình mật khẩu
```ini=
Password for admin@antv.com (min 6 characters): [EHO3AXUi7] Passwd@123
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

- Cài đặt hoàn tất:
```output=
Moving /tmp/zmsetup.20240823-143050.log to /opt/zimbra/log
 
Configuration complete - press return to exit
```

- Chạy tường lửa và mở port:
```bash=
sudo ufw enable
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tc
sudo ufw allow 25/tcp
sudo ufw allow 80/tcp
sudo ufw allow 110/tcp
sudo ufw allow 143/tcp
sudo ufw allow 443/tcp
sudo ufw allow 465/tcp
sudo ufw allow 587/tcp
sudo ufw allow 993/tcp
sudo ufw allow 995/tcp
sudo ufw allow 3443/tcp
sudo ufw allow 5222/tcp
sudo ufw allow 5223/tcp
sudo ufw allow 9071/tcp
sudo ufw allow 8443/tcp
sudo ufw allow 7071/tcp
sudo ufw allow 53/tcp
sudo ufw allow 53/udp
```
- Truy cập trang quản trị Zimbra admin và đăng nhập
`https://192.168.137.140:7071`
![image](https://github.com/user-attachments/assets/1e73ceb9-ad67-476d-9a69-ab7c1dd2c820)

----
---
