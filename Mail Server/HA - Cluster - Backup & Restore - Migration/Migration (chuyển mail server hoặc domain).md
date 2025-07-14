> # Migration (chuyển mail server hoặc domain)

# 1.  Migration đổi domain
> Giữ nguyên Zimbra server, chuyển user1@olddomain.com → user1@newdomain.com

## BƯỚC 1: Chuẩn bị
- Kiểm tra domain hiện tại:
```bash
su - zimbra
zmprov gad
```
--> Bạn sẽ thấy domain hiện tại, VD của tôi: `antvpro.io.vn`

-  **Tạo domain mới**
```bash!
zmprov cd newdomain.io.vn
```
- Kiểm tra lại:
```bash
zmprov gad
```
<img width="500" height="154" alt="image" src="https://github.com/user-attachments/assets/dd75a0ef-aef2-470f-8148-9a5bdb4dbeb2" />

- **Tạo tài khoản mới trên domain mới:**
```bash!
zmprov ca user.migrate@newdomain.io.vn 123456
```

## BƯỚC 2: Export mailbox từ domain cũ

```bash
zmmailbox -z -m user1@antvpro.io.vn getRestURL "//?fmt=tgz" > /tmp/user1_migrate.tgz
```

## BƯỚC 3: Import mailbox sang domain mới
```bash!
zmmailbox -z -m user.migrate@newdomain.io.vn postRestURL "//?fmt=tgz" /tmp/user1_migrate.tgz
```

## BƯỚC 4:  Kiểm tra kết quả
- Truy cập Webmail: `https://mail.antvpro.io.vn`

- Đăng nhập bằng tài khoản: `user.migrate@newdomain.io.vn`

<img width="925" height="502" alt="image" src="https://github.com/user-attachments/assets/2fdf98a7-bc6c-4bf0-97c8-d532b77d1658" />

--- 

# 2. Migration Mailbox giữa 2 server Zimbra 
> - Di chuyển toàn bộ dữ liệu của user từ server **Zimbra cũ (A)** → **Zimbra mới (B)**
>-  Cả hai máy phải cùng version Zimbra OSE 
>- Backup Server trước khi di chuyển 


