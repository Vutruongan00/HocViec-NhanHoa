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

# 3. Failover (Chuyển đổi dự phòng)
**Failover** trong database là một cơ chế tự động chuyển đổi sang hệ thống dự phòng khi hệ thống chính gặp sự cố hoặc cần bảo trì. Điều này giúp duy trì tính liên tục của ứng dụng và hạn chế thời gian gián đoạn dịch vụ. 

- **Cơ chế dự phòng:**
Failover sử dụng một bản sao của hệ thống chính, được cấu hình để sẵn sàng tiếp quản khi hệ thống chính bị lỗi. 
- **Chuyển đổi tự động**:
Khi hệ thống chính gặp sự cố, failover sẽ tự động chuyển toàn bộ khối lượng công việc sang hệ thống dự phòng mà không cần sự can thiệp thủ công. 
- **Dữ liệu đồng bộ:**
Để đảm bảo dữ liệu được đồng bộ giữa hệ thống chính và hệ thống dự phòng, các hệ thống failover thường sử dụng các cơ chế sao chép dữ liệu. 
- **Lợi ích của failover:**
    - **Tính liên tục**: Giúp ứng dụng tiếp tục hoạt động khi hệ thống chính gặp vấn đề. 
    - **Giảm thiểu thời gian gián đoạn**: Chuyển đổi nhanh chóng giúp hạn chế tối đa thời gian ngừng hoạt động của ứng dụng. 
    - **Cải thiện độ tin cậy**: Failover giúp hệ thống hoạt động ổn định hơn và ít chịu ảnh hưởng của các sự cố. 

- **Ví dụ**: Nếu một máy chủ cơ sở dữ liệu gặp sự cố, failover sẽ tự động chuyển toàn bộ kết nối và yêu cầu của ứng dụng sang máy chủ dự phòng, cho phép ứng dụng tiếp tục hoạt động bình thường. 

# 4. Sharding (phân mảnh dữ liệu)
**Sharding** là một kỹ thuật phân mảnh dữ liệu theo chiều ngang, chia một database lớn thành nhiều database nhỏ hơn và phân tán chúng trên nhiều máy chủ khác nhau. Mỗi phần nhỏ của database được gọi là một "**shard**".

### Khái niệm cơ bản

- **Shard**: Một phần của dữ liệu toàn bộ database, được lưu trữ trên một máy chủ database riêng biệt (hoặc một cụm máy chủ).
- **Shard Key**: Một cột (hoặc nhiều cột) được sử dụng để xác định cách dữ liệu được phân phối giữa các shard. Việc lựa chọn shard key là cực kỳ quan trọng.
- **Router/Proxy**: Một lớp trung gian mà ứng dụng kết nối tới. Router biết shard key và chuyển tiếp truy vấn đến shard thích hợp.
- **Config Server**: Lưu trữ metadata về cấu hình sharding (ánh xạ dữ liệu đến các shard).

---

### Ưu điểm

-  **Scalability (Khả năng mở rộng)**: Tăng khả năng lưu trữ và xử lý bằng cách thêm nhiều shard khi dữ liệu tăng. Vượt qua giới hạn của một máy chủ duy nhất.
-  **Performance**: Phân tán tải truy vấn trên nhiều máy chủ, giảm gánh nặng I/O và CPU cho mỗi shard.
-  **High Availability (HA)**: Nếu một shard gặp sự cố, chỉ một phần nhỏ dữ liệu bị ảnh hưởng. Các shard có thể được cấu hình với replication để tăng tính sẵn sàng.

---

### Nhược điểm

-  **Phức tạp**: Triển khai và quản lý sharding cực kỳ phức tạp.
-  **Phân phối dữ liệu không đều**: Nếu lựa chọn shard key không phù hợp có thể dẫn đến hotspot.
-  **Cross-Shard Joins/Transactions**: Các truy vấn hoặc giao dịch cần dữ liệu từ nhiều shard trở nên phức tạp và chậm hơn.
-  **Resharding**: Việc thay đổi shard key hoặc phân phối lại dữ liệu là quá trình phức tạp và tốn kém.

---

### Cách thức Sharding

#### Horizontal Sharding (Phân mảnh theo chiều ngang)

Phổ biến nhất. Mỗi shard chứa một tập hợp con các **record** của một bảng.

- **Range-based Sharding**: Dựa trên khoảng giá trị của shard key.  
  _Ví dụ_: `user_id` từ `1–1M` trên Shard A, `1M–2M` trên Shard B.
- **Hash-based Sharding**: Dựa trên giá trị **băm** của shard key để phân phối dữ liệu ngẫu nhiên hơn.
- **List-based Sharding**: Dựa trên danh sách các giá trị cụ thể của shard key.

#### Vertical Sharding (Phân mảnh theo chiều dọc)

Chia một cơ sở dữ liệu thành nhiều database nhỏ hơn dựa trên **tính năng** hoặc **module** của ứng dụng.  
_Ví dụ_: database chứa thông tin người dùng (`users`), đơn hàng (`orders`), v.v.  
*Lưu ý*: Đây thường được gọi là **database splitting**, không phải sharding thực sự.

# 5. Load balancing cho database\
**Load balancing** (cân bằng tải) là quá trình phân phối các kết nối và truy vấn đến nhiều database server khác nhau để tối ưu hóa việc sử dụng tài nguyên, tối đa hóa thông lượng, giảm độ trễ và tránh quá tải cho một server đơn lẻ.

---

### Khi nào cần Load Balancing

- Khi có nhiều **replica (slave)** để xử lý các truy vấn **đọc**.
- Khi sử dụng các giải pháp **cluster** như **Galera**, **Group Replication** nơi tất cả các node đều có thể **đọc/ghi**.

---

### Các phương pháp Load Balancing

####  Application-Level Load Balancing

- **Cơ chế**: Ứng dụng tự động quản lý việc phân phối truy vấn. Ứng dụng biết về các master và slave, và tự mình gửi:
  - Truy vấn **ghi** đến **master**
  - Truy vấn **đọc** đến **slave**

- **Ưu điểm**:
  - Linh hoạt, có thể tùy chỉnh logic theo nhu cầu của ứng dụng

- **Nhược điểm**:
  - Tăng độ phức tạp cho code
  - Yêu cầu ứng dụng phải theo dõi trạng thái của các server (master/slave, hoạt động/lỗi)

---

####  Proxy-Level Load Balancing

- **Cơ chế**: Một proxy đặt giữa ứng dụng và database server. Ứng dụng chỉ kết nối với proxy.

- **Ưu điểm**:
  - Trong suốt đối với ứng dụng
  - Tự động phát hiện lỗi và failover
  - Định tuyến thông minh: phân chia đọc/ghi
  - Hỗ trợ thêm các tính năng như connection pooling, query rewriting, bảo mật...

- **Nhược điểm**:
  - Là một điểm lỗi tiềm ẩn nếu không cấu hình HA cho proxy
  - Tăng độ trễ nhỏ

#####  Các công cụ phổ biến:

| Công cụ            | Mô tả |
|--------------------|-------|
| **ProxySQL**       | Mã nguồn mở, mạnh mẽ, hỗ trợ regex, phân chia đọc/ghi, failover tự động. |
| **MariaDB MaxScale** | Proxy gateway cho MariaDB với routing, filtering, HA. |
| **MySQL Router**   | Tích hợp với MySQL Group Replication. |
| **HAProxy**        | Không chuyên cho database, nhưng vẫn có thể cấu hình TCP load balancing. |

---

####  Hardware Load Balancer

- **Cơ chế**: Dùng thiết bị phần cứng chuyên dụng.

- **Ưu điểm**: Hiệu suất rất cao, độ tin cậy lớn

- **Nhược điểm**: Giá cao, cấu hình phức tạp

---

##  Cấu hình ProxySQL (Ví dụ đơn giản: Read/Write Splitting)

**ProxySQL** là một trong các giải pháp phổ biến nhất để load balancing MySQL/MariaDB.

---

### 1. Cài đặt ProxySQL

**Ubuntu Linux**:
```bash
sudo apt install proxysql
```
### Kết nối vào admin interface (cổng 6032)

```bash!
mysql -u admin -padmin -h 127.0.0.1 -P 6032
```

### 3 Thêm các host database

```sql!
-- Master (ghi)
INSERT INTO mysql_servers (hostgroup_id, hostname, port) VALUES (10, 'master_ip', 3306);

-- Slaves (đọc)
INSERT INTO mysql_servers (hostgroup_id, hostname, port) VALUES (20, 'slave_ip1', 3306);
INSERT INTO mysql_servers (hostgroup_id, hostname, port) VALUES (20, 'slave_ip2', 3306);

LOAD MYSQL SERVERS TO RUNTIME;
SAVE MYSQL SERVERS TO DISK;
```
### 4. Thêm user kết nối từ ứng dụng
```sql!
INSERT INTO mysql_users (username, password, default_hostgroup) 
VALUES ('app_user', 'app_password', 20);

LOAD MYSQL USERS TO RUNTIME;
SAVE MYSQL USERS TO DISK;
```

### 5. Tạo rule để chia truy vấn đọc/ghi

```sql!
-- Ghi: INSERT, UPDATE, DELETE → master (hostgroup 10)
INSERT INTO mysql_query_rules (rule_id, active, match_digest, destination_hostgroup, apply) 
VALUES (100, 1, '^INSERT|UPDATE|DELETE', 10, 1);

-- Đọc: SELECT → slave (hostgroup 20)
INSERT INTO mysql_query_rules (rule_id, active, match_digest, destination_hostgroup, apply) 
VALUES (200, 1, '^SELECT', 20, 1);

LOAD MYSQL QUERY RULES TO RUNTIME;
SAVE MYSQL QUERY RULES TO DISK;
```


