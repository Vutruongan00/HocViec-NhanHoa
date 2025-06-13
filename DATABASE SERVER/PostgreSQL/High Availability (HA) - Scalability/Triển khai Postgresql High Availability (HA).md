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

---

## Cluster-Failover with `Patroni` + `Etcd`
> - Mở Firewall trước cho chắc đỡ quên:
> ```bash!
> sudo ufw status
> sudo ufw allow 2379/tcp
> sudo ufw allow 2380/tcp
> sudo ufw reload
> ```

### Triển khai Patroni + Etcd trên từng node

- **Cài đặt Patroni và Etcd**
```bash=
sudo apt update
sudo apt install -y python3-pip python3 psycopg2 python3-yaml python3-click python3 etcd postgresql
sudo pip3 install patroni[etcd]
sudo apt install etcd-server
```

- **Cấu hình Etcd (trên mỗi node)**
    - Tạo file cấu hình `nano /etc/etcd/etcd.conf.yml` như sau:
```yaml
name: node1  # thay bằng node2 hoặc node3 tương ứng
data-dir: /var/lib/etcd
listen-peer-urls: http://<local_ip>:2380
listen-client-urls: http://<local_ip>:2379
initial-advertise-peer-urls: http://<local_ip>:2380
advertise-client-urls: http://<local_ip>:2379
initial-cluster: node1=http://IP1:2380,node2=http://IP2:2380,node3=http://IP3:2380
initial-cluster-state: new
initial-cluster-token: patroni-cluster
```
node1:
![image](https://github.com/user-attachments/assets/1c7d86a9-5fc7-4e04-8d28-28f4ebc43291)

node2
![image](https://github.com/user-attachments/assets/b5c34f41-b986-49c8-9830-3c0e82f9d6dd)
node3
![image](https://github.com/user-attachments/assets/21312270-a649-4977-b3e0-1a5f02cdc32a)

- **Chạy etcd bằng systemd hoặc screen:**
```bash!
etcd --config-file /etc/etcd/etcd.conf.yml
```

### Cấu hình Patroni trên mỗi node

- **Tạo file `nano /etc/patroni.yml` trên mỗi node:**

```yaml!
scope: postgres_cluster
namespace: /db/
name: node1   	#đổi node2/node3 cho phù hợp

restapi:
  listen: 0.0.0.0:8008
  connect_address: 10.170.0.3:8008

etcd:
  hosts:
    - 10.170.0.3:2379
    - 10.170.0.2:2379
    - 10.148.0.7:2379


bootstrap:
  dcs:
    ttl: 30
    loop_wait: 10
    retry_timeout: 10
    maximum_lag_on_failover: 1048576
    postgresql:
      use_pg_rewind: true
      parameters:
        wal_level: replica
        hot_standby: "on"
        max_wal_senders: 10
        max_replication_slots: 10
        wal_keep_size: 64

  initdb:
    - encoding: UTF8
    - data-checksums

  users:
    replicator:
      password: Qwerty123@
      options:
        - replication

postgresql:
  listen: 0.0.0.0:5432
  connect_address: 10.148.0.7:5432
  data_dir: /var/lib/postgresql/14/main
  config_dir: /etc/postgresql/14/main       # <-- thêm dòng này để tránh lỗi
  bin_dir: /usr/lib/postgresql/14/bin
  authentication:
    replication:
      username: replicator
      password: Qwerty123@
    superuser:
      username: postgres
      password: Qwerty123@
  parameters:
    unix_socket_directories: '/var/run/postgresql'

tags:
  nofailover: false
  noloadbalance: false
  clonefrom: false
```

### Cấu hình Patroni kết nối đến PostgreSQL  

- Mở file cấu hình: 
```bash!
nano /etc/postgresql/14/main/pg_hba.conf
```

- Thêm(/sửa) những dòng sau vào cuối file:
```conf!
# Cho phép postgres kết nối từ localhost
host    all             postgres        127.0.0.1/32            trust
# Cho phép các kết nối replication từ các node standby.
host    replication     replicator      10.170.0.2/32           md5     
host    replication     replicator      10.148.0.7/32           md5
```
- Lưu file và tải lại cấu hình PostgreSQL trên Primary (node1)
```bash!
sudo systemctl reload postgresql
```

- **Khởi động Patroni (chạy trên mỗi node):**
    - Đăng nhập user postgres: `sudo -u postgres`
    ```
    patroni /etc/patroni.yml
    ```
- **// Hoặc chạy patroni dưới dạng một dịch vụ Systemd:**
    - Tạo hoặc chỉnh file unit dịch vụ Systemd cho Patroni: `nano /etc/systemd/system/patroni.service`
    ```ini
    [Unit]
    Description=Patroni PostgreSQL high-availability
    After=network.target etcd.service 

    [Service]
    Type=simple
    User=postgres     
    Group=postgres    
    ExecStart=/usr/local/bin/patroni /etc/patroni.yml
    Restart=on-failure
    LimitNOFILE=100000
    LimitNPROC=100000

    [Install]
    WantedBy=multi-user.target
    ```
    
- Tải lại cấu hình Systemd: 
`sudo systemctl daemon-reload`
- Khởi động dịch vụ Patroni:
`sudo systemctl start patroni`  
    
### **Kiểm tra trạng thái cluster Patroni từ node1 (Primary):**
```bash!
patronictl -c /etc/patroni.yml list postgres_cluster
```
![image](https://github.com/user-attachments/assets/e5e09c64-674f-4b8a-b7e2-4944c0d5d742)


### Kiểm tra hoạt động của FAILOVER

- Thử dừng patroni trên node1 (Primary) - điều này sẽ khiến PostgreSQL trên node1 cũng dừng lại:
```bash!
sudo systemctl stop patroni
```
- Lúc này `node2` tự động trở thành `Leader`, 
`node1` trở thành `replica`
![image](https://github.com/user-attachments/assets/90f052e3-a421-42c1-b121-b3e9b9958465)

