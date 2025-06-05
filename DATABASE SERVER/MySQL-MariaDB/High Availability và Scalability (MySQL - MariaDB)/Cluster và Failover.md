> # Cluster và Failover
> **Cluster** và **Failover** là các giải pháp nâng cao HA, vượt xa replication Master-Slave cơ bản bằng cách cung cấp khả năng tự động chuyển đổi sang node dự phòng khi có sự cố.

# 1. MySQL Group Replication (MySQL 8.0+)
- **Mô hình**: Multi-Master Update Anywhere (cho phép ghi trên nhiều node), nhưng đảm bảo tính nhất quán mạnh mẽ thông qua cơ chế Paxos. Các thành viên trong nhóm đồng ý về thứ tự của các giao dịch.
- **Ưu điểm:**
    - **High Availability**: Tự động phát hiện lỗi và loại bỏ node bị lỗi.
    - Fault Tolerance: Hệ thống vẫn hoạt động ngay cả khi một số node gặp sự cố.
    - Scalability: Có thể ghi trên bất kỳ node nào (hoặc chỉ một node và dùng các node khác cho đọc).
    - Strong Consistency: Dữ liệu nhất quán trên tất cả các node.

- **Cách hoạt động **:
    - Cài đặt các plugin cần thiết trên mỗi node.
    - Cấu hình mỗi node để tham gia một nhóm (Group Replication).
    - Khi một giao dịch được commit, nó được gửi đến tất cả các thành viên trong nhóm.
    - Các thành viên sử dụng thuật toán Paxos để đảm bảo tất cả các giao dịch được cam kết theo cùng một thứ tự. Nếu có xung đột, một giao dịch sẽ bị rollback.

- Khi dùng: Yêu cầu HA cao, tính toàn vẹn dữ liệu mạnh mẽ, và khả năng ghi/đọc phân tán. Phức tạp hơn để cấu hình và quản lý so với replication truyền thống.
- Công cụ đi kèm: MySQL Shell có các tiện ích để triển khai và quản lý Group Replication dễ dàng hơn.

## Cấu hình
> **Mô hình:** 
> - 3 note MySQL (Ubuntu 22.04)
> - Sử dụng Group Replication
> - Quản lý thông qua MySQL Shell

- **Bước 1: Cài đặt trên cả 3 note**
```bash!
sudo apt install mysql-server mysql-shell
```

- **Bước 2: Cấu hình MySQL để bật Group Replication (trên cả 3 máy)
`nano /etc/mysql/mysql.conf.d/mysqld.cnf`**
```ini!
[mysqld]
server-id=1  # mỗi node khác nhau: 2,3...
log_bin=mysql-bin
gtid_mode=ON
enforce_gtid_consistency=ON
binlog_checksum=NONE
log_slave_updates=ON
plugin-load=group_replication.so
group_replication_group_name="aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee"
group_replication_start_on_boot=OFF
group_replication_local_address="IP1:33061"
group_replication_group_seeds="IP1:33061,IP2:33061,IP3:33061"
group_replication_bootstrap_group=OFF
```

- Restart MySQL

- **Bước 3: Tạo user và cấu hình bảo mật trên note1**

```sql!
CREATE USER 'rep1'@'%' IDENTIFIED BY 'StrongPass123!';
GRANT REPLICATION SLAVE ON *.* TO 'rep1'@'%';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

- **Bước 4: Cấu hình InnoDB Cluster bằng MySQL Shell**
    ```bash
    mysqlsh
    ```
    - Kết nối tới instance đầu tiên:
    ```
    shell.connect("root@localhost")
    ```
    - Cấu hình node để tham gia InnoDB Cluster:
    ```
    dba.configure_instance("root@localhost:3306")
    ```
    
    - Tạo InnoDB Cluster mới:
    ```
    cluster = dba.create_cluster("myCluster")
    ```
    
    - Thêm node 2 & 3 vào cluster:
    ```
    cluster.add_instance("root@138.2.123.156:3306", {'password': "Password123@"})

    cluster.add_instance("root@158.179.164.117:3306", {'password': "Password123@"})
    ```
    
- Kiểm tra: `cluster.status()`



# 2. MariaDB Galera Cluster
- **Mô hình**: Synchronous Multi-Master Cluster. Tất cả các node đều là master, có thể ghi vào bất kỳ node nào. Dữ liệu được đồng bộ hóa tức thì trên tất cả các node.
- **Ưu điểm:**
    - **High Availability**: Tự động chuyển đổi dự phòng và phục hồi.
    - **Zero Data Loss**: Không mất dữ liệu khi có sự cố node.
    - **Scalability**: Tăng khả năng ghi bằng cách thêm node (tối ưu nhất khi tải ghi phân tán đều).
    - **Read Scalability**: Mọi node đều có bản sao dữ liệu đầy đủ.
    - **No Slave Lag**: Không có độ trễ đồng bộ hóa.

- **Nhược điểm:**  Hiệu suất ghi có thể bị ảnh hưởng nếu có nhiều xung đột giao dịch. Yêu cầu ít nhất 3 node để đạt được quorum an toàn.

- **Cách hoạt động**
    - Cài đặt Galera Cluster trên mỗi node.
    - Cấu hình các node để kết nối với nhau.
    - Khi một giao dịch được thực hiện trên một node, nó được gửi đến tất cả các node khác trong nhóm.
    - Tất cả các node cùng commit giao dịch theo một thứ tự xác định. Nếu có xung đột (write-set conflict), một giao dịch sẽ bị hủy bỏ.

# 3. 
