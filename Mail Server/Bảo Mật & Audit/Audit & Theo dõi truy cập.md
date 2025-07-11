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

## 4. Tích hợp fail2ban để chặn IP brute-force.

