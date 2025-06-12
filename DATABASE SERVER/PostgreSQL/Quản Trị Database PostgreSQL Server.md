> # Quản Trị Database PostgreSQL Server

- Đăng nhập PostgreSQL: `sudo -u postgres psql`
- Tạo User: `user1`
```sql!
CREATE USER user1 WITH PASSWORD '123456';
```
- Tạo Database:
```sql!
CREATE DATABASE testdb OWNER user1
```
![image](https://github.com/user-attachments/assets/3baaf4a7-6dd7-43e4-a901-02e2ee55d49c)

- Truy cập và database `testdb` và Tạo bảng mẫu:
```sql!
CREATE TABLE nhanvien (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50),
    position VARCHAR(50),
    salary NUMERIC
);
```
- Thêm dữ liệu vào bảng:
```sql!
INSERT INTO nhanvien (name, position, salary)
VALUES
('Nguyen Van A', 'Developer', 1200),
('Tran Thi B', 'Tester', 1000),
('Le Van C', 'Manager', 2000);
```

# 1. User và quản lý quyền truy cập

- Phân quyền cho `user1` chỉ có quyền SELECT trên bảng `nhanvien`:
```sql!
GRANT CONNECT ON DATABASE testdb TO user1;
GRANT USAGE ON SCHEMA public TO user1;
GRANT SELECT ON nhanvien TO user1;
```
- Kiểm tra quyền: `\du user1`

- Thu hồi quyền: 
```sql!
REVOKE SELECT ON employees FROM readonly_user;
```

- **Đăng nhập bằng `user1` để thử `SELECT` và `INSERT`:**
```
psql -u user1 -d testdb -h 127.0.0.1
```
![image](https://github.com/user-attachments/assets/1f6652e6-1ae6-4813-9b07-96e41dcd4145)


---
# 3. Theo dõi hiệu năng (Monitoring)

-  Các công cụ tích hợp sẵn:

| View / Lệnh                 | Mục đích                |
| --------------------------- | ----------------------- |
| `pg_stat_activity`          | Theo dõi các kết nối    |
| `pg_stat_user_tables`       | Thống kê hoạt động bảng |
| `pg_stat_database`          | Hiệu suất tổng thể DB   |
| `EXPLAIN / EXPLAIN ANALYZE` | Phân tích truy vấn      |


# 4. Tối ưu hóa truy vấn (Query Optimization)
> Giả sử ta có 1 Database bán hàng `sales_db`, với 2 bảng là `products`  và `orders`. Tạo DB với những dữ liệu sau:
```sql!
> postgres=# \c sales_db
> You are now connected to database "sales_db" as user "postgres".
> sales_db=# CREATE TABLE products (
>     product_id SERIAL PRIMARY KEY,
>     product_name VARCHAR(100) NOT NULL,
>     category VARCHAR(50),
>     price DECIMAL(10, 2) NOT NULL,
>     stock_quantity INT NOT NULL DEFAULT 0
> );
> CREATE TABLE
> sales_db=# CREATE TABLE orders (
>     order_id SERIAL PRIMARY KEY,
>     product_id INT NOT NULL,
>     order_date DATE DEFAULT CURRENT_DATE,
>     quantity INT NOT NULL,
>     total_amount DECIMAL(10, 2) NOT NULL,
>     customer_email VARCHAR(100),
>     status VARCHAR(20) DEFAULT 'Pending',
>     CONSTRAINT fk_product
>         FOREIGN KEY (product_id)
>         REFERENCES products (product_id)
>         ON DELETE CASCADE
> );
> CREATE TABLE
> sales_db=# INSERT INTO products (product_name, category, price, stock_quantity) VALUES
> ('Laptop Pro', 'Electronics', 1200.00, 50),
> ('Mechanical Keyboard', 'Accessories', 150.00, 100),
> ('Gaming Mouse', 'Accessories', 75.00, 200),
> ('External SSD 1TB', 'Storage', 100.00, 75),
> ('Monitor 27 inch', 'Electronics', 300.00, 60),
> ('USB Hub 4-port', 'Accessories', 25.00, 150),
> ('Webcam HD', 'Electronics', 50.00, 90),
> ('Noise Cancelling Headphones', 'Audio', 200.00, 40),
> ('Smartphone X', 'Electronics', 800.00, 30),
> ('Smartwatch Y', 'Wearables', 250.00, 20);
> INSERT 0 10
> sales_db=#
```


- **Dùng `EXPLAIN` và `EXPLAIN ANALYZE`** để phân tích truy vấn trước khi có chỉ mục 
```sql!
EXPLAIN ANALYZE SELECT * FROM products WHERE category = 'Electronics' AND price < 500;
```
>  `EXPLAIN ANALYZE`:Thực thi truy vấn và hiển thị kế hoạch thực thi thực tế cùng với thời gian chạy và số hàng được xử lý ở mỗi bước. Điều này hữu ích hơn để xác định các nút thắt cổ chai, nhưng nó cũng tốn tài nguyên vì truy vấn được chạy.

--> **Tạo chỉ mục (index)**
```sql!
CREATE INDEX idx_products_category_price ON products (category, price);
```
- Kiểm tra và chạy `EXPLAIN ANALYZE` lại


---
# 5. Quản lý Transaction và Lock 

### a. Transaction (Giao dịch)
Trong PostgreSQL, mọi câu lệnh SQL đều chạy trong một giao dịch. Nếu bạn không bắt đầu một giao dịch rõ ràng, mỗi câu lệnh sẽ chạy trong một "autocommit transaction" của riêng nó.

- **Bắt đầu một giao dịch**: `BEGIN;` hoặc `START TRANSACTION;`

- Các thao tác trong transaction:
```sql!
UPDATE products SET stock_quantity = stock_quantity - 1 WHERE product_id = 1;
INSERT INTO orders (product_id, quantity, total_amount, customer_email, status)
VALUES (1, 1, 1200.00, 'new_customer@example.com', 'Completed');
```
- Xác nhận giao dịch: `COMMIT;`

- Hủy bỏ giao dịch (quay lại trạng thái ban đầu): `ROLLBACK;`

### b. Lock
