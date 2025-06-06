# Bảo Mật Database Server (MySQL / MariaDB
## 1. Các mối đe dọa bảo mật phổ biến đối với Cơ sở dữ liệu

Hiểu rõ các mối đe dọa giúp chúng ta xây dựng các biện pháp phòng ngừa hiệu quả hơn.

### 1.1 Truy cập trái phép (Unauthorized Access)

- **Tài khoản yếu/mặc định**: Sử dụng mật khẩu mặc định, yếu hoặc không thay đổi mật khẩu root.
- **Quyền truy cập quá mức**: Cấp quyền `ALL PRIVILEGES` cho người dùng không cần thiết.
- **Broken Authentication**: Lỗ hổng trong xác thực cho phép kẻ tấn công vượt qua hệ thống login.

### 1.2 SQL Injection

- Kẻ tấn công chèn mã SQL độc hại qua các input (username, password, tìm kiếm).
- Có thể dẫn đến: lấy cắp dữ liệu, xóa bảng, vượt quyền, điều khiển hệ thống.

### 1.3 Lộ lọt dữ liệu (Data Leakage)

- **Truy cập vật lý**: Kẻ tấn công có quyền truy cập trực tiếp máy chủ.
- **Truy cập mạng**: Không mã hóa dữ liệu truyền → dễ bị nghe lén (eavesdropping).
- **Cấu hình sai**: Ví dụ: mở cổng cơ sở dữ liệu ra Internet mà không giới hạn truy cập.

### 1.4 Tấn công từ chối dịch vụ (DoS - Denial of Service)

- Gửi hàng loạt truy vấn phức tạp/đồng thời khiến hệ thống quá tải.
- Hậu quả: Dịch vụ bị gián đoạn, người dùng hợp pháp không thể truy cập.

### 1.5 Mất mát dữ liệu (Data Loss)

- **Lỗi phần cứng/phần mềm**.
- **Lỗi con người**: Vô tình xóa dữ liệu.
- **Phần mềm độc hại**: Ví dụ như ransomware.

### 1.6 Thiếu cập nhật (Unpatched Software)

- Sử dụng phiên bản DBMS cũ, chứa lỗ hổng đã biết nhưng chưa vá.
- Tạo điều kiện cho kẻ tấn công khai thác.

### 1.7 Thiếu giám sát và kiểm tra (Inadequate Monitoring)

- Không phát hiện kịp thời các hành vi bất thường hoặc tấn công.
- Không đủ log để điều tra sự cố sau này.

-----
-----



## 2. Mã hóa dữ liệu (Encryption) SSL/TLS

Mã hóa là biện pháp bảo vệ dữ liệu khi **lưu trữ (at rest)** và **truyền tải (in transit)**.

>- Cài openssl (nếu chưa có):
> ```bash
> sudo apt update
> sudo apt install openssl
> ```

### 2.1 Mã hóa khi truyền tải (Encryption In Transit - SSL/TLS)

#### Mục tiêu:
- Bảo vệ dữ liệu truyền giữa **client** và **database server**.

#### Bước 1: Tạo chứng chỉ tự ký bằng OpenSSL:

```bash!
sudo mkdir /etc/mysql/ssl
cd /etc/mysql/ssl

# Tạo CA Key
openssl genrsa 2048 > ca-key.pem

# Tạo CA Certificate (tự ký)
openssl req -new -x509 -nodes -days 3650 -key ca-key.pem -out ca-cert.pem

# Tạo Server Key và Certificate Request
openssl req -newkey rsa:2048 -days 3650 -nodes -keyout server-key.pem -out server-req.pem

# Ký Server Certificate bằng CA
openssl x509 -req -in server-req.pem -days 3650 -CA ca-cert.pem -CAkey ca-key.pem -set_serial 01 -out server-cert.pem

# Tạo Client Key và Certificate Request
openssl req -newkey rsa:2048 -days 3650 -nodes -keyout client-key.pem -out client-req.pem

# Ký Client Certificate bằng CA
openssl x509 -req -in client-req.pem -days 3650 -CA ca-cert.pem -CAkey ca-key.pem -set_serial 02 -out client-cert.pem
```

#### Bước 2: **Cấu hình MySQL để dùng SSL**
- Sửa file cấu hình MySQL:  
`nano /etc/mysql/mysql.conf.d/mysqld.cnf`
```ini!
[mysqld]
ssl-ca = /etc/mysql/ssl/ca-cert.pem
ssl-cert = /etc/mysql/ssl/server-cert.pem
ssl-key = /etc/mysql/ssl/server-key.pem
require_secure_transport = ON
```
- Hoặc Cấu hình trên Client (khác máy)
    - Trong file cấu hình `my.cnf` (Linux) hoặc `my.ini` (Windows):
```ini!
[mysqld]
ssl_ca = /etc/mysql/ssl/ca-cert.pem
ssl_cert = /etc/mysql/ssl/server-cert.pem
ssl_key = /etc/mysql/ssl/server-key.pem
require_secure_transport = ON
```

- **Khởi động lại** : `sudo systemctl restart mysql
`
#### Bước 3: Kiểm tra MySQL đã bật SSL chưa

- Truy cập mysql với quyền root:
```
mysql -u root -p
```
- Chạy lệnh sau để kiểm tra:
```sql!
SHOW VARIABLES LIKE '%ssl%';
```

![image](https://github.com/user-attachments/assets/a460d7d6-8e4d-457b-8a97-2f1e130245df)



#### Bước 4:  Tạo user bắt buộc dùng SSL

```sql!
CREATE USER 'ssluser'@'%' IDENTIFIED BY 'matkhaumanh' REQUIRE SSL;

GRANT ALL PRIVILEGES ON *.* TO 'ssluser'@'%';

ALTER USER 'ssluser'@'%' REQUIRE X509;

FLUSH PRIVILEGES;
```

> - Dùng lệnh sau để kiểm tra những bản ghi của user `ssluser`
> ```
> SELECT user, host, ssl_type FROM mysql.user WHERE user = 'ssluser';

![image](https://github.com/user-attachments/assets/f32c0228-0746-48b6-a924-7a3560d86070)

> ```
> Nếu giá trị `ssl_type = SPECIFIED` = 
> - `ANY` : thì user có đó có thể đăng nhập mà không yêu cầu gì
>- `SPECIFIED` : có yêu cầu SSL, nhưng không bắt buộc Client phải có Cert hợp lệ -chỉ cần sử dụng kết nối có SSL là được
> - `X509` : bắt buộc Client phải cung cấp `--ssl-ca` , `--ssl-cert`, `--ssl-key`. - Bảo mật hơn


---

#### Bước 5: Kết nối bằng SSL từ client
- Nếu kết nối trên cùng máy:
```bash!
mysql -u ssluser -p \
--ssl-ca=/etc/mysql/ssl/ca-cert.pem \
--ssl-cert=/etc/mysql/ssl/client-cert.pem \
--ssl-key=/etc/mysql/ssl/client-key.pem
```
- Nếu Client trên máy khác:
```bash=
mysql -h your_server_ip -u ssluser -p \
--ssl-ca=/etc/mysql/ssl/ca-cert.pem \
--ssl-cert=/etc/mysql/ssl/client-cert.pem \
--ssl-key=/etc/mysql/ssl/client-key.pem
```

- Kết nối thành công, chạy `STATUS` để kiểm tra:
```sql!
STATUS;
```
Sẽ thấy dòng SSL: Cipher in use is ... xác nhận kết nối đang được mã hóa.


![image](https://github.com/user-attachments/assets/efa8af51-7062-4285-95fa-9168efb5fc6b)



- Nếu Client trên máy khác (mình dùng Windows Server)
```bash=
mysql -h 35.240.156.172 -u ssluser -p ^
--ssl-ca="C:\mysql\ssl\ca-cert.pem" ^
--ssl-cert="C:\mysql\ssl\client-cert.pem" ^
--ssl-key="C:\mysql\ssl\client-key.pem"
```


![image](https://github.com/user-attachments/assets/1cc70f26-c974-4fbf-a6d8-280395b2cb8e)



### 2.2 Mã hóa dữ liệu ở mức lưu trữ (TDE – Transparent Data Encryption)
----
## 3. Audit và theo dõi truy cập

### Nhật ký truy cập (Audit Log)
#### MySQL (ubuntu): 
- Bật log: 
```ini!
# /etc/mysql/mysql.conf.d/mysqld.cnf

[mysqld]
general_log = 1
general_log_file = /var/log/mysql/general.log
log_output = FILE
```


#### MariaDB (CentOS)
```ini!
# /etc/my.cnf.d/mariadb-server.cnf

[mysqld]
plugin-load-add=server_audit
server_audit_logging=ON
server_audit_events=CONNECT,QUERY
```

>Dùng cho audit chi tiết: user nào login, chạy câu truy vấn gì, vào DB nào...


- Restart: `sudo systemctl restart mysql`

--> Kết quả ví dụ check log: phát hiện user nào kết nối, từ ip nào, sử dụng câu truy vấn gì~

![image](https://github.com/user-attachments/assets/98f018b7-632c-47c3-a326-e4dd65f9c303)



---
## 4. Cập nhật bản vá Bảo mật (Patch Management)

- Kiểm tra phiên bản hiện tại:
```bash=
mysql --version
```

- Cập nhật MySQL (Ubuntu)
```bash=
sudo apt update
sudo apt install mysql-server -y
```
> Nên **Backup** trước khi cập nhật: cấu hình , db, user,...(Đã hướng dẫn trong folder **[Quản Trị Database Server (MySQL / MariaDB](https://github.com/Vutruongan00/HocViec-NhanHoa/blob/9ba79fd02144d6b72c68501568f92a46f3fc09ad/DATABASE%20SERVER/MySQL-MariaDB/Qu%E1%BA%A3n%20tr%E1%BB%8B%20Database%20Server.md)**

