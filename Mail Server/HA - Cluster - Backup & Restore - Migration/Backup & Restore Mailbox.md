># Backup & Restore

# 1. Backup - Sao lưu 
-  **Backup Mailbox** với **`zmmailbox`**
- Liệt kê các tài khoản tồn tại:

```bash!
#su - zimbra
zmprov -l gaa
```
## 1.1 Sao lưu một Mailbox cụ thể (**`user1`**)
```bash!
zmmailbox -z -m user1@antvpro.io.vn getRestURL "/?fmt=tgz" > /tmp/user1_backup.tgz
```
> Thay `user1@antvpro.io.vn` bằng địa chỉ email của người dùng bạn muốn sao lưu và bằng đường dẫn <path/to/backup.tgz> tới nơi bạn muốn lưu tệp tin sao lưu.

## 1.2 Sao lưu tất cả mail trên Zimbra

```bash!
zmprov -l gaa | while read -r account; do zmmailbox -z -m "$account" getRestURL "/?fmt=tgz" > ""; done
```

- **Ví dụ:** sao lưu tất cả mail với định dạng nén `.tgz` và lưu trong thư mục `/opt/zimbra/backup/`

```bash!
zmprov -l gaa | while read -r account; do zmmailbox -z -m "$account" getRestURL "/?fmt=tgz" > "/opt/zimbra/backup/backup-$account.tgz"; done
```

# 2. Restore Mailbox - Khôi phục

## 2.1 Restore một Mail cụ thể

```bash!
zmmailbox -z -m user1@antvpro.io.vn postRestURL "/?fmt=tgz&resolve=reset" /tmp/user1_backup.tgz
```

## 2.2 Restore sang user2 (mô phỏng migration nội bộ)
```bahs!
zmmailbox -z -m user2@antvpro.io.vn postRestURL "/?fmt=tgz&resolve=reset" /tmp/user1_backup.tgz
```

## 2.3 Restore nhiều Mail
- Nếu bạn muốn khôi phục email cho nhiều người dùng, bạn có thể sử dụng một vòng lặp và lệnh sau:

```bash!
while read -r account; do zmmailbox -z -m "$account" postRestURL "/?fmt=tgz&resolve=reset" ""; done < <(zmprov -l gaa)
```


# 3. Tạo Cron BACKUP tự động
- Chạy dưới quyền user **zimbra**:

```bash!
su - zimbra
crontab -e
```
- Ví dụ thêm đoạn Cron sau, Cron này sẽ chạy vào `18h 0 phút` hằng ngày:
```ini
0 18 * * * /opt/zimbra/backup-zimbra/auto-backup.sh >> /opt/zimbra/backup-zimbra/logs/cron.log2>&1
```

---
