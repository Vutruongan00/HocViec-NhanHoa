> # High Availability và Scalability (MySQL / MariaDB)
> 
> **High Availability** (Tính khả dụng) và **Scalability** (Khả năng mở rộng) là hai khía cạnh quan trọng đối với các hệ thống database hiện đại, giúp đảm bảo ứng dụng luôn hoạt động ổn định và có thể xử lý lượng tải ngày càng tăng.

# 1. Replication (Đồng bộ hóa/ Sao chép)
**Replication** là quá trình sao chép dữ liệu từ một database server (master/primary) sang một hoặc nhiều database server khác (slave/replica). Mục tiêu chính là tăng cường khả năng sẵn sàng (HA), mở rộng khả năng đọc (read scalability) và cung cấp cơ sở cho các hoạt động báo cáo mà không ảnh hưởng đến primary server.

**Khái niệm cơ bản:**
- **Master (Primary)**: Server chính, nơi tất cả các thao tác ghi (INSERT, UPDATE, DELETE) diễn ra.
- **Slave (Replica)**: Server phụ, nhận bản sao của dữ liệu từ master. Các thao tác đọc có thể được thực hiện trên slave để phân tán tải.
- **Binary Log (Binlog)**: Trên master, tất cả các thay đổi dữ liệu được ghi vào Binary Log. Đây là "nhật ký" mà slave sẽ đọc và áp dụng.
- **I/O Thread (trên Slave)**: Kết nối với master, đọc các sự kiện từ binlog của master và lưu chúng vào một file Relay Log trên slave.
- **SQL Thread (trên Slave)**: Đọc các sự kiện từ Relay Log và áp dụng chúng vào database của slave.


**Các loại Replication**

- **Asynchronous Replication **(Bất đồng bộ - Mặc định và phổ biến nhất):
    - **Cơ chế**: Master ghi sự kiện vào binlog và gửi xác nhận thành công cho client ngay lập tức, mà không chờ slave nhận hoặc áp dụng các thay đổi.
    - **Ưu điểm**: Hiệu suất ghi cao trên master, ít ảnh hưởng đến độ trễ giao dịch.
    - **Nhược điểm:** Có khả năng mất một lượng nhỏ dữ liệu (thông tin trong binlog chưa được gửi đến slave) nếu master gặp sự cố trước khi các thay đổi được đồng bộ hóa hoàn toàn. Có độ trễ replication (replication lag).
- **Semi-Synchronous Replication(Bán đồng bộ**):
    - **Cơ chế**: Master chờ ít nhất một slave xác nhận rằng nó đã nhận được các sự kiện binlog (nhưng chưa cần áp dụng) trước khi gửi xác nhận thành công cho client.
    - **Ưu điểm**: Giảm đáng kể nguy cơ mất dữ liệu so với asynchronous replication, vẫn giữ được hiệu suất tương đối tốt.
    - **Nhược điểm**: Độ trễ giao dịch trên master tăng nhẹ do phải chờ xác nhận từ slave. Yêu cầu ít nhất một slave hoạt động.

- **Synchronous Replication**(Đồng bộ - Ít phổ biến hơn cho replication Master-Slave truyền thống, thường dùng trong các giải pháp Cluster/Group Replication):
    - **Cơ chế**: Master chờ tất cả các slave xác nhận rằng chúng đã nhận và áp dụng các thay đổi trước khi gửi xác nhận thành công cho client.
    - **Ưu điểm**: Không mất dữ liệu, đảm bảo tính nhất quán cao nhất.
    - **Nhược điểm**: Hiệu suất ghi trên master thấp hơn đáng kể, độ trễ cao, yêu cầu tất cả các slave phải hoạt động.

## Cấu Hình cơ bản Replication (Master-Slave)

> Yêu cầu: 2 máy chủ riêng biệt, 
>   - **Master**: 35.240.156.172
>   - **Slave**:  138.2.123.156
### Trên máy chủ Master: `35.240.156.172`

- Tạo user `repical` trên Master: user này sẽ được Slave sử dụng để kết nối và đọc binlog.
```sql!
CREATE USER 'repica'@'138.2.123.156' IDENTIFIED BY 'password'

GRANT ALL PRIVILEGES ON *.* TO 'repica'@'138.2.123.156';

FLUSH PRIVILEGES;
```
- Mở file cấu hình và thêm/sửa các dòng sau trong khối `[mysqld]`:
 `nano /etc/mysql/mysql.conf.d/mysqld.cnf`
 
```ini!
[mysqld]

#REPICA
server-id             = 1
log_bin               = /var/log/mysql/mysql-bin.log
binlog_do_db          = test1 #hoặc bỏ dòng này nếu muốn Slave Replica tất cả DB
general_log_file      = /var/log/mysql/mysql.log

bind-address          = 35.240.156.172  #(IP của master)
```

> 

- Kiểm tra trạng thái Master và vị trí Binlog hiện tại:
```sql!
SHOW MASTER STATUS;
```

![image](https://github.com/user-attachments/assets/335e1a58-736f-4104-976d-6e7c643ca494)




> Ghi lại 2 giá trị: `File` vs `Position` để giúp cho cấu hình replicate.


### Trên máy chủ Slave: `138.2.123.156`

- Mở file cấu hình: 
`nano /etc/mysql/mysql.conf.d/mysqld.cnf`

- Thêm vào: 
```ini!
server-id      = 2 #(số mấy cũng được khác id của master là được)
bind-address   = 138.2.123.156( IP máy slave)
log_bin        = /var/log/mysql/mysql-bin.log
```

- Khởi động lại MySQL: `systemctl restart mysql`

- Truy cập: `mysql -u root -p`
- Tắt Slave trước: 
```sql
STOP SLAVE;
```
- Cấu hình Slave để kết nối với Master:

```sql!
CHANGE MASTER TO
    MASTER_HOST='35.240.156.172',
    MASTER_USER='repica',
    MASTER_PASSWORD='Password',
    MASTER_LOG_FILE='mysql-bin.000097', -- Lấy từ SHOW MASTER STATUS của Master
    MASTER_LOG_POS=157;  -- Lấy từ SHOW MASTER STATUS của Master
```

```sql!
START SLAVE;

SHOW SLAVE STATUS\G;
```

![image](https://github.com/user-attachments/assets/cb9dd017-8e61-48a2-a270-dbf7ed6c5f73)


### Kiểm chứng kết quả
- Tạp thêm bảng mới trong database `test1` tên `demo` trên Master
![image](https://github.com/user-attachments/assets/9b7eadde-4c94-4a49-9542-851ccc8c1cd1)

- Kiểm tra trên Slave:
![image](https://github.com/user-attachments/assets/bb62d220-8969-4081-b8f8-084d6b52e916)
