> # Triển khai Postgresql High Availability (HA)
>
> note1: `34.92.221.58`  -	**master**
>
>  note2: `34.92.129.179` -	**slave_1**
>
>  note3: `34.124.212.78` -	**slave_2**

# Cấu Hình
## Bước 1: **Thực hiện cấu hình trên cả 3 note:**

- Truy cập file cấu hình Postgresql: 
```bash!
nano /etc/postgresql/14/main/postgresql.conf
```
- Tìm và sửa dòng dưới đây để có thể kết nối từ các server khác:
```
listen_addresses = '*'
```
- Truy cập file chứa các quy tắc xác thực, phân quyền rồi xóa đi và điền như sau lưu và đóng lại:
```css!
# TYPE  DATABASE        USER            ADDRESS                 METHOD
local   all             all                                     peer
# IPv4 local connections:
host    all             all             0.0.0.0/0               scram-sha-256
# IPv6 local connections:
host    all             all             ::1/128                 scram-sha-256
# Allow replication connections from localhost, by a user with the
# replication privilege.
local   replication     all                                     peer
host    replication     all             127.0.0.1/32            scram-sha-256
host    replication     all             ::1/128                 scram-sha-256
```
- Mở Firewall
```bash!
ufw enable
ufw allow 5432/tcp
```
- Khởi động lại Postgresql để áp dụng cấu hình: `systemctl restart postgresql`

-  Kiểm tra trạng thái `systemctl status postgresql`

## Bước 2: Thiết lập cấu hình trên Master

- Tạo một user **replica** để phục vụ cho việc thiết lập Postgresql high availability Master-Slave
```bash!
sudo su - postgres

createuser --replication -P -e replicator

exit
```
> Nhập mật khẩu người dùng mới!

- Mở file chứa các quy tắc xác thực và phân quyền cho các kết nối đến cơ sở dữ liệu PostgreSQL
```bash!
nano /etc/postgresql/14/main/pg_hba.conf
```
- Thêm 2 dòng dưới đây vào cuối file (2 IP server Slave 1 và Slave 2)
```css!
host    replication     replicator      34.92.129.179/24       md5
host    replication     replicator      34.124.212.78/24       md5
```
![image](https://github.com/user-attachments/assets/5eacaa58-aaa8-4fdc-b818-d59444b41b0a)

- khởi động lại Postgresql: 
```
systemctl restart postgresql
```
## Bước 3: Thiết lập cấu hình trên Slave

### Truy cập sang server slave_1:
- Dừng PostgreSQL: `systemctl stop postgresql`
- xóa dữ liệu cũ: `rm -rf /var/lib/postgresql/14/main/*`
- Chuyển đổi người dùng hiện tại sang người dùng **postgres**: `sudo su - postgres`

- Sao chép cơ sở dữ liệu PostgreSQL từ một máy chủ chính (primary) sang một máy chủ sao chép (standby) mới:
```sql!
pg_basebackup -h 34.92.221.58 -D /var/lib/postgresql/14/main -U replicator -P -v -R -X stream -C -S slave_1 -- đổi thành slave_2 khi chạy ở note3
```
![image](https://github.com/user-attachments/assets/5f042a55-a56b-42c4-87da-0452032477bc)

> `pg_basebackup`:  công cụ để sao chép cơ sở dữ liệu từ máy chủ chính sang máy chủ sao chép.
> `34.92.221.58`: IP của **master**
> `-D /var/lib/postgresql/14/main`: Chỉ định thư mục lưu trữ dữ liệu cho máy chủ sao chép (standby).
> `-U replicator`: Sử dụng người dùng replicator để kết nối và sao chép từ máy chủ chính (primary).
> `-P`: Yêu cầu nhập mật khẩu cho người dùng replicator.
> `-v`: Hiển thị tiến trình sao chép chi tiết.
> `-R`: Cấu hình máy chủ đích thành máy chủ sao chép (standby), sẵn sàng nhận cập nhật.
> `-X` stream: Sử dụng phương thức streaming replication để truyền dữ liệu.
> `-C`: Sao chép cả tệp pg_control, chứa thông tin cấu hình cơ sở dữ liệu.
> `-S slave_1`: Tạo và sử dụng **replication slot** có tên `slave_1` trên máy chủ chính để đảm bảo không mất dữ liệu sao chép. -- đổi thành `slave_2` khi thực hiện trên **note3**

### Truy cập sang Server slave_2
(Làm tương tự như trên slave_1)


## Bước 4: Kiểm tra hoạt động Master-Slave

- Quay lại Server master, truy vấn thông tin về các máy ghi (replication slots) đang tồn tại trong Postgresql 
```bash!
sudo -i -u postgres psql postgres
```
```sql!
SELECT * FROM pg_replication_slots;
```
![image](https://github.com/user-attachments/assets/e43f2bec-a378-4d40-90bc-51f7bcf4c398)

> có 2 Slave trạng thái là t (true) và một số thông tin khác 
**--> Kết nối thành công Master-Slave**

- Tạo thử 1 bảng dữ liệu `users`:
```sql!
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100)
);
```
- Thêm dữ liệu sample vào table `users`:
```sql!
INSERT INTO users (name) VALUES ('administrator'); INSERT INTO users (name) VALUES ('elroy');
```
![image](https://github.com/user-attachments/assets/acd71775-9fef-4785-bb24-713feed26a7f)

- **Kiểm tra sự đồng bộ dữ liệu trên `salve_1/2`**
```
sudo -i -u postgres psql postgres
```
```sql!
select * from users;
```

![image](https://github.com/user-attachments/assets/169bd18e-45c3-4489-a9e4-f78c97d7d0ed)

> Mô hình trên: **Slave** chỉ có thể `read only`  (là chỉ có thể `select` dữ liệu) và **không thể** `insert

## Bước 5: Cấu hình FAIL OVER 

>-  Nếu như **Master** gặp vấn đề gì đó và bị shutdown, lúc đó ta sẽ phải cấu hình để cho **Slave lên làm Master** để có thể đảm bảo việc ghi và đọc --> đó cũng là cơ chế của **FAIL OVER**
>- Tuy nhiên PostgreSQL không tự động xử lý **failover**, ta phải kích hoạt **failover** **thủ công** bằng cách dùng lệnh `pg_ctl promote` hoặc gọi hàm `pg_promote()`
>- Một cách khác để tự động xử lý **failover** là cần dùng thêm các công cụ hoặc **script** ngoài như **repmgr**, **Patroni**, hay **Pacemaker**.

- Giả lập tình huống khi **Master** gặp sự cố: `stop psql`
```bash!
 sudo systemctl stop postgresql
```
- Truy cập postgresql trên máy của slave_1  (hoặc slave_2):
```sql!
sudo -i -u postgres psql postgres

SELECT pg_promote();
```
![image](https://github.com/user-attachments/assets/5ac31bc8-cae4-4896-907b-a716b0947ad9)

- Lúc này trên Server của Slave_1 có thể ghi độc lập:
```sql!
CREATE TABLE employees (
 emp_id INT,
 emp_name TEXT
 );
 INSERT INTO employees VALUES (101, 'Alice');
 INSERT INTO employees VALUES (102, 'Bob');
 INSERT INTO employees VALUES (103, 'Charlie');
 INSERT INTO employees VALUES (104, 'Diana');
 SELECT * FROM employees;
```

![image](https://github.com/user-attachments/assets/1fc6fc68-5166-42d7-80f5-e09e611453af)

>Đây là cách kích hoạt thủ công Failover

