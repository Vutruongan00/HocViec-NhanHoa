
> # Cài đặt - Cấu hình PostgreSQL
> - Yêu cầu phần cứng:
>     - CPU: Tối thiểu 1 GHz.
>     - RAM: Ít nhất 1 GB.
>     - Dung lượng ổ cứng: >10GB 

# 1. Cài đặt cơ bản
## Windows
- Tải file cài đặt [PostgreSQL Download](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads)
- Chạy file cài đặt...
- Cấu hình admin và mật khẩu
- Port 5432 (mặc định)...
- --> Next...
![image](https://github.com/user-attachments/assets/3652e934-4db5-43b8-af84-373341b09ee8an)


## Linux (Ubuntu)
- Cài Đặt:
```bash!
sudo apt update && sudo apt upgrade -y

sudo apt install postgresql postgresql-contrib -y
```
- Kiểm tra hoạt động:
```bash=
sudo systemctl status postgresql
```
![image](https://github.com/user-attachments/assets/30b3ef28-2c37-4d98-97ea-be51219d14a2)


- Đăng nhập vào PostgreSQL
```bash=
sudo -u postgres psql
```
---
### Các lệnh và thao tác cơ bản:
- Đăng nhập: (như trên)

- **Quản lý Database:**

| Hành động          | Lệnh                      |
| ------------------ | ------------------------- |
| Hiển thị databases | `\l` hoặc `\list`         |
| Tạo database       | `CREATE DATABASE ten_db;` |
| Sử dụng database   | `\c ten_db`               |
| Xóa database       | `DROP DATABASE ten_db;`   |

- **Quản lý Bảng (Tables:**
    - Hiển thị bảng:  **`\dt`**
    - Tạo bảng: `bang1`
    ```sql!
    CREATE TABLE bang1 (	
    id SERIAL PRIMARY KEY,	
    name TEXT,	
    age INT	
    );
    ```
    
    - Xóa bảng: `DROP TABLE bang1;`
    - Xem cấu trúc bảng: `\d bang1`

- **Thao tác Dữ liệu (Data)**
    - Thêm dữ liệu: dùng `INSERT`
    ```sql
    INSERT INTO bang1 (name, age) VALUES ('An', 25);
    ```
    - Truy vấn dữ liệu: `SELECT`
    ```sql
    SELECT * FROM bang1;
    SELECT * FROM bang1 WHERE age > 20;
    ```
    - Cập nhạt dữ liệu: `UPDATE`
    ```sql
    UPDATE users SET age = 22 WHERE name = 'An';
    ```
    - Xóa: `DELETE`
    ```sql
    DELETE FROM users WHERE name = 'An';
    ```
    - Một số tiện ích:
    | Lệnh           | Tác dụng          |
| -------------- | ----------------- |
| `\l`           | Liệt kê database  |
| `\c dbname`    | Kết nối database  |
| `\dt`          | Liệt kê bảng      |
| `\d tablename` | Xem cấu trúc bảng |
| `\du`          | Liệt kê user/role |
| `\q`           | Thoát khỏi `psql` |

    
# 2. Cấu hình tối ưu hiệu năng cơ bản
- Tệp cấu hình:

**Windows:** `C:\Program Files\PostgreSQL\<version>\main\postgresql.conf (Windows)`

**Linux:** `/etc/postgresql/14/main/postgresql.conf` 
- Những tham số cần điều chỉnh(tùy theo RAM của máy) để tối ưu hiệu năng:

| Tham số                | Mục đích                        | Gợi ý (RAM 4GB) |
| ---------------------- | ------------------------------- | --------------- |
| `shared_buffers`       | Cache của PostgreSQL            | `1GB`           |
| `work_mem`             | RAM xử lý truy vấn trung gian   | `8MB`–`64MB`    |
| `maintenance_work_mem` | RAM cho VACUUM/CREATE INDEX     | `256MB`–`512MB` |
| `effective_cache_size` | Bộ nhớ hệ điều hành có thể dùng | `2GB`–`3GB`     |
| `max_connections`      | Số kết nối đồng thời            | `50–100`        |

- Cấu hình xong restart PostgreSQL:
```
sudo systemctl restart postgresql
```

---
# 3. Cấu hình bảo mật cơ bản
- Thay đổi phương thức xác thực:
`nano /etc/postgresql/14/main/pg_hba.conf`
    - Sửa các dòng `local`/`host` thành:
      ```css!
      local   all             all                                     md5
      host    all             all             127.0.0.1/32            md5
      ```

 **Chỉ cho phép truy cập từ localhost:**
- Mở tệp cấu hình PostgreSQL:
`nano /etc/postgresql/14/main/postgresql.conf`
- Tìm dòng `listen_addresses` và thay đổi thành: 
```
listen_addresses = 'localhost'
```
- Hoặc giới hạn IP truy cập (nếu truy cập từ xa) 
```
listen_addresses = 'localhost,192.168.1.10' 
```

Mở `nano /etc/postgresql/14/main/pg_hba.conf` và thêm: 
```css!
host    all             all             <IP_CLIENT>/32          md5
```
![image](https://github.com/user-attachments/assets/1f37b8f8-cd37-4ac6-b7f1-e832be4b497d)


**Đổi mật khẩu user postgres:**
```bash!
sudo -u postgres psql
\password postgres
```

**Tạp user và database riêng biệt** (phần sau)
