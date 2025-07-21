# Audit & Theo dõi truy cập

## 1. Theo dõi hoạt động Đăng nhập/Đăng xuất (SSH, Sudo, Web)

### 1.1 Ai đăng nhập, đăng xuất, từ đâu?
Để theo dõi hoạt động đăng nhập, đăng xuất, và các lệnh quan trọng, bạn cần tận dụng các file log có sẵn trên hệ thống Linux.
- **File log cần xem:**
    - `/opt/zimbra/log/mailbox.log`
    - `/var/log/zimbra.log`
    
#### Lọc thông tin đăng nhập:
```bash!
grep -i 'authentication' /opt/zimbra/log/mailbox.log | tail -n 50
```
![image](https://github.com/user-attachments/assets/3ad51105-6182-410a-8b48-e56836d991a9)

--> Kết quả cho thấy những lần **user1** và **user2** đăng nhập thành công vào **thời gian nào**, **từ địa chỉ IP nào**


## 2. Theo dõi Hoạt động Email (Ai gửi nhiều mail bất thường?)
- Việc theo dõi email bất thường chủ yếu dựa vào **log của Mail Transfer Agent (MTA)** (ví dụ:  Postfix, Sendmail, Exim )
- **File log chính:** `/var/log/mail.log` (Debian/Ubuntu) hoặc `/var/log/maillog` (CentOS/RHEL)

- Xem toàn bộ Log gửi/nhận:
```bash!
/opt/zimbra/libexec/zmmsgtrace
```

- Lọc Log theo người gửi (Sender): để tìm kiếm xem tài khoản nào đang gửi nhiều email bất thường:
```bash!
/opt/zimbra/libexec/zmmsgtrace -s user@yourdomain.com
```

-

## 3. Tích hợp giám sát qua Zabbix / Grafana / ELK

---

## 4. Tích hợp fail2ban để chặn IP brute-force
> 📌 Mục tiêu
> - Phát hiện hành vi đăng nhập sai liên tục vào Zimbra Webmail (`/zimbra`) và Admin Console (`/zimbraAdmin`).
> - Tự động chặn IP tấn công brute-force qua tường lửa (iptables).

- Nếu bạn đang dùng **single server** (cả mta và mailstore trên cùng 1 server), bạn cần thêm 127.0.0.1 và địa chỉ IP của server vào danh sách **`TrustedIP`**.
```bash=
# sudo -u zimbra - 
zmprov mcf +zimbraMailTrustedIP 127.0.0.1 +zimbraMailTrustedIP {IP of Server} 

zmcontrol restart
```

- Nếu bạn dùng **multiserver**, bạn cần thêm các ip của mailbox server và proxy server vào.
```bash=
# sudo -u zimbra - 
zmprov mcf +zimbraHttpThrottleSafeIPs {IP of Mailbox-1} 
zmprov mcf +zimbraHttpThrottleSafeIPs {IP of Mailbox-2} 
zmprov mcf +zimbraMailTrustedIP {IP of Proxy-1} 
zmprov mcf +zimbraMailTrustedIP {IP of Proxy-2} 
zmcontrol restart
```

### Bước 1: Cài đặt FAIL2BAN

- Trên redhat hoặc centos 7/8
```
yum install epel-release -y 
yum install fail2ban -y
```

- Trên Ubuntu 18/20
```
apt-get clean all ; apt-get update 
apt-get install fail2ban -y
```

### Bước 2:  Tạo file `jail.local` để thiết lập lại một số thông số mặc định của fail2ban
```bash=
# nano /etc/fail2ban/jail.local
---
[DEFAULT]
# "ignoreip" can be a list of IP addresses, CIDR masks or DNS hosts.
# Fail2ban will not ban a host which matches an address in this list.
# Several addresses can be defined using space (and/or comma) separator.
#ignoreip = 127.0.0.1/8 ::1 10.137.26.29/32
ignoreip = 127.0.0.1/8  IP-ADDRESS-OF-ZIMBRA-SERVER/32
banaction = route
```
> Điền các **IP của Zimbra servers** vào danh sách **`ignoreip`** trong file này. Bạn cũng có thể whitelist các IP khác nếu muốn.

### Bước 3: Tạo file jail cho các dịch vụ zimbra

```bash=
# nano /etc/fail2ban/jail.d/zimbra.local

[zimbra-webmail]
enabled = true
filter = zimbra-webmail
action = iptables[name=zimbra-webmail, port=https, protocol=tcp]
logpath = /opt/zimbra/log/mailbox.log
maxretry = 5
findtime = 600
bantime = 3600
backend = auto

[zimbra-admin]
enabled = true
filter = zimbra-admin
action = iptables[name=ZimbraAdmin, port=7071, protocol=tcp]
logpath = /opt/zimbra/log/mailbox.log
maxretry = 3
findtime = 600
bantime = 7200
backend = auto
```

<img width="766" height="334" alt="image" src="https://github.com/user-attachments/assets/028e693f-b4b6-43f5-88c5-8098dd40e82d" />

> Bạn tinh chỉnh các thông số: maxretry, findtime và bantime,... cho phù hợp với môi trường của bạn.


### Bước 4: Tạo các file filter ứng với các dịch vụ của zimbra ở trên.

> Tạo filter riêng: `zimbra-webmail.conf` và `zimbra-admin.conf` để bắt lỗi login thất bại trong mailbox.log.để bắt lỗi login thất bại trong mailbox.log

– **Tạo filter cho `web mail`:**
```ini=
# nano /etc/fail2ban/filter.d/zimbra-webmail.conf 

[Definition]
failregex = \[name=.*?;oip=<HOST>;.*?failed for \[.*?\], invalid password

ignoreregex =

```

– **Tạo filter cho `admin`:**
```ini=
# /etc/fail2ban/filter.d/zimbra-admin.conf

[Definition]
failregex = \[ip=<HOST>.*?\] .*authentication failed for \[.*?\]\. Reason: invalid password

ignoreregex =
```
<img width="885" height="122" alt="image" src="https://github.com/user-attachments/assets/32d6d4b8-9c65-4bfe-9031-b03172df919f" />

### Lưu ý: 
- Trong môi trường Zimbra sử dụng proxy nginx nội bộ (mặc định có trong Zimbra), khi người dùng kết nối từ webmail hoặc ứng dụng, Zimbra mailboxd (Java backend) không nhìn thấy IP thật của client, mà chỉ ghi nhận IP của proxy, thường là:
    - `127.0.0.1` (nginx cài cùng máy)
    - hoặc địa chỉ nội bộ như `10.128.x.x`
=> **Fail2ban sẽ không chặn được đúng IP người tấn công, mà chỉ thấy IP nội bộ.**

#### **Để mailboxd hiểu và ghi nhận IP thật của người dùng từ proxy, cần thực hiện:**

1. **Kiểm tra tên header mà Zimbra dùng để đọc IP gốc**
```bash=
# su - zimbra
zmlocalconfig zimbra_http_originating_ip_header
# Kết quả: X-Forwarded-For
```
**→ Zimbra sẽ lấy IP thật từ trường X-Forwarded-For trong header HTTP (nginx đã đẩy sẵn).**

2. **Thêm IP của proxy/nginx vào danh sách Trusted IP**
```bash!
zmprov mcf +zimbraMailTrustedIP 127.0.0.1 +zimbraMailTrustedIP 10.128.0.3
```
> Trong đó:
> - `127.0.0.1`: nginx và mailboxd cùng máy
> - `10.128.0.3`: IP nội bộ của nginx nếu chạy độc lập

3. Kiểm tra lại cấu hình
```bash!
zmprov gcf zimbraMailTrustedIP
```

### Bước 5: Khởi động lại dịch vụ Fail2ban
```bash=
systemctl restart fail2ban
systemctl status fail2ban
systemctl enable fail2ban
```
<img width="786" height="214" alt="image" src="https://github.com/user-attachments/assets/21ec0ef2-ba12-4d5e-9897-89f2e1812e1b" />

### Bước 6: Kiểm tra hoạt động

- Thử truy cập vào trang đăng nhập **admin** và **đăng nhập sai quá 3 lần**:
-  Trong khi đó hãy chạy lệnh sau để **kiểm tra Log:**
```bash
tail -f /opt/zimbra/log/mailbox.log
```

<img width="1124" height="391" alt="image" src="https://github.com/user-attachments/assets/ede6c572-2d69-4db5-a641-bebbc2b4e624" />

- **Đăng nhập sai lần thứ 4 --> Lỗi máy chủ**

<img width="827" height="399" alt="image" src="https://github.com/user-attachments/assets/fe152452-3edb-47aa-a33e-7e511ecef1c2" />

- **Kiểm tra trạng thái ban:** 

```
sudo fail2ban-client status zimbra-admin
```
<img width="623" height="185" alt="image" src="https://github.com/user-attachments/assets/14d30f77-8415-4092-bade-0d4423b89857" />

- Hoặc **Xem log fail2ban để xác nhận IP bị chặn do lý do đúng**
```bash=
sudo zgrep 'Ban' /var/log/fail2ban.log*
```

<img width="979" height="350" alt="image" src="https://github.com/user-attachments/assets/474c5371-5bfc-40c6-a1db-d68f646c2276" />

- Sau khi Test xong, có thể dùng lệnh sau để **Thêm whitelist** IP nội bộ của bạn để tránh bị tự chặn**

```bash=
sudo nano /etc/fail2ban/jail.local
# Thêm vào trong [DEFAULT] section
ignoreip = 127.0.0.1 192.168.1.0/24 <IP_CUA_BAN>
```

- **Reload lại:**
```bash
sudo fail2ban-client reload`
```


### BONUS:  `Ban` và `unban` 1 địa chỉ IP
- **`BAN`**: cú pháp
    ```bash=
    fail2ban-client set "Jail-Name" banip "IP-Address"
    ```
    - ví dụ: `sudo fail2ban-client set zimbra-webmail banip 116.106.97.171`

- **`UNBAN`** : cú pháp
    ```bash
    fail2ban-client set "Jail-Name" unbanip "Banned IP-Address"
    ```
    - ví dụ: fail2ban-client set sshd unbanip 10.137.26.29
    ```

- **Unban tất cả**: 
```
fail2ban-client unban --all
```
