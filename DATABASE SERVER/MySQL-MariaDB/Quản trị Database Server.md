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

> - Tối ưu hóa truy vấn là quá trình cải thiện hiệu suất của các câu lệnh SQL để:
>     - Trả kết quả nhanh hơn
>     - Tiết kiệm tài nguyên CPU, RAM, I/O
>     - Giảm độ trễ, đặc biệt khi dữ liệu lớn


- Giả sử bảng `nguoidung1` của tôi có các cột giá trị như sau:
```sql!
CREATE TABLE nguoidung1 (
    id INT AUTO_INCREMENT PRIMARY KEY,
    ten VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    ngay_dang_ky DATE DEFAULT (CURDATE())
    trang_thai TINYINT
);
```

- T**ạo chỉ mục(Index) hợp lý:**
    - Nếu thường tìm kiếm người dùng theo `email`:
    ```sql!
    CREATE INDEX idx_email ON nguoidung1(email);
    ```
    - Nếu tìm kiếm theo `ngay_dang_ky` hoặc `id`:
    ```sql!
    CREATE INDEX idx_ngay_dang_ky ON nguoidung1(ngay_dang_ky);
    CREATE INDEX idx_id ON nguoidung1(id);
    ```
    
- **Dùng `EXPLAIN` để phân tích truy vấn**:

```sql!
EXPLAIN SELECT * FROM nguoidung1 WHERE email = 'abc@example.com';
```
> Nó sẽ cho thấy MySQL có dùng chỉ mục không, hay phải quét toàn bảng (Full Table Scan)

- **Tránh dùng `SELECT *` nếu không cần tất cả**
    - Viết rõ cột cần truy vấn:
    ```sql!
    SELECT ten, email FROM nguoidung1 WHERE trang_thai = 1;
    ```
    
## 5. Quản lý Transaction và Lock

>- **Transaction** và **Lock** là các chơ chế cốt lõi để đảm bảo tính toàn vẹn và nhất quá của dữ liệu trong môi trường nhiều người dùng đồng thời

### 5.1 Transaction (Giao dịch)
- **Transaction** là một nhóm các thao tác SQL (INSERT, UPDATE, DELETE...) mà ta muốn thực hiện như một khối, nghĩa là:
    - Thành công hết thì mới lưu lại
    - Thất bại ở đâu thì rollback (quay lui) như chưa từng thực hiện
- Một đơn vị công việc hợp lý phải thể hiện bốn thuộc tính để đủ điều kiện là một **transaction**:

    - **Tính nguyên tử**: Nếu transaction có nhiều phép tính thì tất cả phải được thực hiện. Nếu bất kỳ phép tính nào trong nhóm thất bại thì nó nên được rollback (trở lại trạng thái trước khi transaction bắt đầu)
    - **Tính nhất quán**: Chuỗi các phép tính phải nhất quán
    - **Cô lập**: Những phép tính được thực hiện phải được phân lập từ những phép tính khác trên cùng một máy chủ hoặc trên cùng một cơ sở dữ liệu.
    - **Độ bền**: Những phép tính được thực hiện trên cơ sở dữ liệu phải được lưu lại và lưu trữ trong cơ sở dữ liệu vĩnh viễn (commit data)
-  **Ví dụ:** trong kịch bản chuyển tiền trong hệ thống ngân hàng. Đầu tiên, người dùng phải tăng số dư tài khoản đích và sau đó giảm số dư của tài khoản nguồn. Người dùng phải xem xét xem các giao dịch có được triển khai không và có cùng những thay đổi được thực hiện cho tài khoản nguồn và tài khoản đích. Lúc này chúng ta cần sử dụng Transaction, để chắc chắn rằng tất cả xử lý đều phải thực hiện thành công, nếu có lỗi thì sẽ thực hiện rollback lại dữ liệu trước khi thực hiện giao dịch.

```sql!
START TRANSACTION;
UPDATE account SET balance = balance - 100 WHERE user_id = 1;
UPDATE account SET balance = balance + 100 WHERE user_id = 2;
COMMIT;
```
> Nếu một lệnh bị lỗi(tài khoản nguồn không đủ tiền) thì: `ROLLBACK;`

--> Cả 2 thao tác sẽ không có gì thay đổi → giữ tính toàn vẹn dữ liệu

### 5.2 Lock (Khóa dữ liệu)
**Lock** là cách cơ sở dữ liệu ngăn chặn xung đột dữ liệu khi nhiều người cùng đọc/ghi vào cùng một bản ghi hay bảng.
