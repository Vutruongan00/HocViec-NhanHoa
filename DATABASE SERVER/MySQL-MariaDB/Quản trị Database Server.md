# Quản Trị Database Server (MySQL / MariaDB)

**Trước khi tạo User và phân quyền truy cập, cần tạo database:**
- Tạo DB: 
```sql!
create database test1
```
- Tạo 1 bảng: `nguoidung1`
```sql
CREATE TABLE nguoidung1 (
    id INT AUTO_INCREMENT PRIMARY KEY,
    ten VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    ngay_dang_ky DATE DEFAULT (CURDATE())
);
```

![image](https://github.com/user-attachments/assets/c475d8f7-fcac-45ed-aade-f5cec41c71c4)


## 1.  User và quản lý quyền truy cập
 
 -  **Tạo người dùng**: 
 **Ví dụ**
```sql!
CREATE USER 'user1'@'localhost' IDENTIFIED BY '123456a@';
```

- **Phân quyền**:

```sql!
--Quyền toàn bộ DB:
GRANT ALL PRIVILEGES ON test1 TO 'user1'@'localhost';

-- Chỉ SELECT, INSERT, UPDATE bảng cụ thể
GRANT SELECT, INSERT, UPDATE ON test1.nguoidung1 TO 'user1'@'localhost';
```

- **Xem quyền**:
```sql!
SHOW GRANTS FOR 'user1'@'localhost';
```
![image](https://github.com/user-attachments/assets/079f9283-c917-44b4-bd91-a17b360ea3e2)


- Thu hồi quyền:
```sql!
REVOKE INSERT ON test1.nguoidung1 FROM 'user1'@'localhost';
```

## 2. Sao Lưu và Khôi Phục (Backup & Recovery)

- **Sao lưu toàn bộ database**
```sql!
mysqldump -u root -p --all-databases > all_backup.sql
```
-  **Sao lưu database cụ thể**, ví dụ:
```sql!
mysqldump -u root -p test1 > test1_backup.sql
```

- Hoặc chỉ sao lưu 1 bảng:
```sql!
mysqldump -u root -p test1 nguoidung1 > nguoidung1_backup.sql
```
> Khi chạy lệnh sao lưu dữ liệu từ DB, file sao lưu sẽ được lưu sẽ được lưu tại thư mục khi ta chạy lệnh 

- **Khôi phục:**
```sql!
mysql -u root -p < all_backup.sql

mysql -u root -p test1 < test1_backup.sql
```

## 3. Theo dõi hiệu năng (Monitoring)
- Các câu lệnh SQL:
```sql!
SHOW PROCESSLIST;      -- Truy vấn đang chạy
SHOW GLOBAL STATUS;    -- Trạng thái hệ thống
SHOW VARIABLES;        -- Biến cấu hình
```

- Bật slow query log:
Sửa file:` nano /etc/mysql/mysql.conf.d/mysqld.cnf`
```sql!
[mysqld]
slow_query_log = 1
slow_query_log_file = /var/log/mysql/slow.log
long_query_time = 1
```

## 4. Tối ưu hóa truy vấn
