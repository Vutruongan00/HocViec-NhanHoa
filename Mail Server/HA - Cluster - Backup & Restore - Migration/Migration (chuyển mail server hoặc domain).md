> # Migration (chuyển mail server hoặc domain)

# 1.  Migration đổi domain
> Giữ nguyên Zimbra server, chuyển user1@olddomain.com → user1@newdomain.com

### BƯỚC 1: Chuẩn bị
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

### BƯỚC 2: Export mailbox từ domain cũ

```bash
zmmailbox -z -m user1@antvpro.io.vn getRestURL "//?fmt=tgz" > /tmp/user1_migrate.tgz
```

### BƯỚC 3: Import mailbox sang domain mới
```bash!
zmmailbox -z -m user.migrate@newdomain.io.vn postRestURL "//?fmt=tgz" /tmp/user1_migrate.tgz
```

### BƯỚC 4:  Kiểm tra kết quả
- Truy cập Webmail: `https://mail.antvpro.io.vn`

- Đăng nhập bằng tài khoản: `user.migrate@newdomain.io.vn`

<img width="925" height="502" alt="image" src="https://github.com/user-attachments/assets/2fdf98a7-bc6c-4bf0-97c8-d532b77d1658" />

--- 

# 2. Migration Mailbox giữa 2 server Zimbra 
> - Di chuyển toàn bộ dữ liệu của user từ server **Zimbra cũ (A)** → **Zimbra mới (B)**
>-  Cả hai máy phải cùng version Zimbra OSE 
>- Backup Server trước khi di chuyển 
>- Cấu hình DNS Records trỏ về Server mới

### BƯỚC 1: Lấy giá trị `ldap_userdn` và `ldap_password` trên Zimbra cũ (A)

```bash!
# su - zimbra

zmlocalconfig -s zimbra_ldap_userdn
zmlocalconfig -s zimbra_ldap_password
```
<img width="634" height="255" alt="image" src="https://github.com/user-attachments/assets/c40340e0-8c04-408d-8771-2180c3ca4930" />

### BƯỚC 2: Thực hiện Migrate mail Zimbra cũ sang Zimbra mới
> Đăng nhập và thực hiện trên máy chủ **Zimbra mới (B)**

- Chọn **Công cụ và di trú (Tools & Migration)**

<img width="923" height="566" alt="image" src="https://github.com/user-attachments/assets/db8f6be0-d929-4be5-90c7-09b8a1d171ed" />

- Chọn tiếp **Di chuyển tài khoản (Account Migration) →**  Click chọn **Thuật sĩ di trú(Migration Wizard)** 

<img width="1909" height="702" alt="image" src="https://github.com/user-attachments/assets/d9cd5e1a-a76b-4a3c-9e32-ffd91b1d9b98" />

- **Ở Tab Giới thiệu này bạn chọn như sau**
    - Chọn máy chủ thư: **Bộ công tác Zimbra(Zimbra Collaboration Suite)**
   
<img width="1517" height="748" alt="image" src="https://github.com/user-attachments/assets/500f4eba-dd2b-4da5-973a-34d673e68a96" />

- **Tab Tổng quan -->** chọn **Nhập từ một thư mục LDAP Zimbra khác(Import from antoher Zimbra LDAP directory)**

<img width="1517" height="732" alt="image" src="https://github.com/user-attachments/assets/c463e0e4-0288-4be0-b5b8-763fa678670a" />

- **Tab Tùy chọn dữ liệu số lượng lớn:**
    - Không tạo mật khẩu ngẫu nhiên
    - Sử dụng cùng một mật khẩu cho tất cả tài khoản mới: Đặt mật khẩu vào
    - Yêu cầu người dùng cần thay đổi mật khẩu sau lần đăng nhập đầu tiên: Tick vào
    - Máy chủ SMTP: Nhập vào địa chỉ **IP của Zimbra cũ**/ Hoặc **SMTP Relay**
    - Cổng SMTP: **25** hoặc **587**
    
<img width="1109" height="543" alt="image" src="https://github.com/user-attachments/assets/b03ba1d9-409c-4309-9c60-5e238b2884ea" />

> Ở đây mình chọn `port 587` vì mình đã sử dụng **SMTP Relay qua Sendgrid**  --> [Tìm hiểu thêm cách cấu hình SMTP Relay](https://github.com/Vutruongan00/HocViec-NhanHoa/blob/e2e93139dae76a05eee0edc6aeeb141d4935e521/Mail%20Server/V%E1%BA%ADn%20h%C3%A0nh%20%26%20Qu%E1%BA%A3n%20tr%E1%BB%8B/EX_C%E1%BA%A5u%20h%C3%ACnh%20D%E1%BB%8Bch%20v%E1%BB%A5%20SMTP%20Relay.md)


- **Tab kết nối thư mục:** 
    > Đây là phần giao tiếp với máy **Zimbra cũ**

    - **URL của LDAP**: Tên máy chủ: Nhập vào IP Zimbra cũ
    - **Mật khẩu kết nối**: Mật khẩu đã lấy ở **Bước 1**
    - **Bộ lọc LDAP**: Để mặc định
    - **Cở sở tìm kiếm LDAP**: Thêm tên miền hoặc các tên miền mà bạn muốn di chuyển
        - ví dụ: (antvpro.io.vn) thì: **`dc=antvpro,dc=io,dc=vn`** 

<img width="1276" height="613" alt="image" src="https://github.com/user-attachments/assets/118785e0-19b5-462b-b444-07e2266a955c" />

- Next tiếp để kiểm tra lại xemm có đúng không nhé:

<img width="1106" height="535" alt="image" src="https://github.com/user-attachments/assets/99c7cfca-6777-494a-abdc-21fe28f76ab7" />

- Chờ **Tiến trình** lên **100%** thì --> **Next tiếp** nhé!

<img width="531" height="239" alt="image" src="https://github.com/user-attachments/assets/5d97ed07-17b3-4b97-b958-3b2826790903" />

- Tab **Tùy chọn nhập**: tick **"Chọn tài khoản để nhập"**:

<img width="1112" height="542" alt="image" src="https://github.com/user-attachments/assets/2e12b7a7-88c7-460c-a03a-69c645f4c04f" />

- **Chọn Tài khoản**: 
    - **Nhập tiên miền** và chọn --> **Tìm kiếm** 
    - Hệ thống sẽ list danh sách các tên miền sau đó bạn chọn vào **Thêm** hoặc T**hêm tất cả**.
    - <img width="1319" height="661" alt="image" src="https://github.com/user-attachments/assets/7851f689-53af-49d6-9545-843fd8aa6867" />

- Tab **Chi tiết kết nối IMAP**
    - **Máy chủ IMAP**: Nhập vào IP server Zimbra cũ
    - **Cổng IMAP**: 993
    - **Đăng nhập quản trị IMAP:** admin
    - **Mật khẩu quản trị IMAP**: Mật khẩu đăng nhập vào Zimbra cũ
    - **Xác nhận mật khẩu**: Nhập lại lần nữa
    - <img width="1271" height="617" alt="image" src="https://github.com/user-attachments/assets/145423cf-15c3-42d1-866e-afb4109d7c39" />

- Kiểm tra lại thông tin rồi --> **Next stiep tục**

<img width="1259" height="610" alt="image" src="https://github.com/user-attachments/assets/ac998618-44a5-4b56-bf1c-a6f824a51dfb" />

### BƯỚC 3: Kiểm tra thông tin

<img width="1259" height="498" alt="image" src="https://github.com/user-attachments/assets/e48d3682-64c8-47a2-bf35-4f5f11e9b696" />



