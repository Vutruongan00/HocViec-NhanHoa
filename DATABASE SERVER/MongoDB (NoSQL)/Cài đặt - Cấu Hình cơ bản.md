
> # Cài đặt - Cấu hình MongoDB 

# 1. Yêu cầu hệ thống  
- Phần cứng
    - RAM: Tối thiểu 4GB, khuyến nghị 8GB+ với tập dữ liệu lớn.
	- Dung lượng ổ đĩa: Cần 10GB trống, cộng thêm dung lượng cho dữ liệu.
	- CPU: Hỗ trợ kiến trúc x86_64 (Intel Sandy Bridge trở lên).
- Phần mềm
	- Hệ điều hành:
		- Windows: Hỗ trợ Windows Server 2022, 2019 và các phiên bản 64-bit khác.
		- Linux: Hỗ trợ SuSE, Ubuntu, Red Hat Enterprise Linux, cùng nhiều bản phân phối khác.
	- Mạng: MongoDB cần quyền truy cập FQDN giữa các máy chủ.
	- Quyền hệ thống (Windows): Tài khoản chạy mongod, mongos phải thuộc nhóm "Performance Monitor Users" và "Local". 

# 2. Cài Đặt 
## 2.1 Windows
- Tải file cài đặt tại https://www.mongodb.com/try/download/community
![image](https://github.com/user-attachments/assets/fda61a46-dcfb-469c-a744-7ab70d317147)

 
- Chạy file cài đặt: `--> Next` -->Chọn Complete 
- Cấu hình **Mongod as a network service:**
    - Tích chọn **Run service as Network Service user** --> **Install Compass** (Mặc định)
- Install
- Cài đặt hoàn tất 

![image](https://github.com/user-attachments/assets/9b91ffec-b581-4cb6-a24c-8a8a354e2a0a)



## 2.2 Linux (Ubuntu)

- Cài các gói cần thiết và import public key 
```bash!
sudo apt-get install gnupg curl

curl -fsSL https://www.mongodb.org/static/pgp/server-8.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-8.0.gpg \
   --dearmor
```

- Tạo tệp danh sách cho kho lưu trữ MongoDB: 
```bash!
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-8.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/8.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-8.0.list
```
- Reload package database 
```bash!
sudo apt-get update
```

- Cài đặt gói MongoDB:
```
sudo apt-get install -y mongodb-org
```

- Khởi chạy và kiểm tra 
```
sudo systemctl start mongod
sudo systemctl status mongod
```
![image](https://github.com/user-attachments/assets/df8084d0-c75a-48c9-97ed-11b7d41968cb)


- Truy cập vào shell dùng lệnh 
```
mongosh 
```
- Tạo database : MongoDB chỉ tạo cơ sở dữ liệu khi được chèn dữ liệu vào. Lệnh sau sẽ chuyển sang cơ sở dữ liệu sampleDB. Nếu cơ sở dữ liệu chưa tồn tại, MongoDB sẽ tự động tạo khi dữ liệu được chèn:
```
use sampleDB
```
- Tạo Collection:  Collections tương tự như bảng trong cơ sở dữ liệu quan hệ. Để tạo một collection, sử dụng lệnh sau:
```
db.createCollection("users")
```
- Insert Data 
```
db.users.insertMany([
  { name: "Alice", age: 25, city: "New York" },
  { name: "Bob", age: 30, city: "San Francisco" }
])
```
- Kiểm tra Data
```
db.users.find()
```
![image](https://github.com/user-attachments/assets/ef1e83d5-b73a-42de-9632-432bd18bb3ce)

# 3. Cấu hình tối ưu hiệu năng

> File cấu hình `/etc/mongod.conf` trên **Linux** / (hoặc trên **Windows** `<thư mục cài đặt>/bin/mongod.cfg`)

**Tối ưu bộ nhớ (RAM) và cache size**
- WiredTiger tự động sử dụng 50% RAM khả dụng.
```yaml!
# /etc/mongod.conf
storage:
  wiredTiger:
    engineConfig:
      cacheSizeGB: 2  # Tùy RAM máy, ví dụ 2GB
```
**Tối ưu I/O bằng noatime**
```bash!
#sudo nano /etc/fstab
# Thêm noatime vào dòng /var/lib/mongodb
UUID=xxxx /var/lib/mongodb ext4 defaults,noatime 0 2
```
- Bật Compression (nén dữ liệu)
```yaml!
# /etc/mongod.conf
storage:
  wiredTiger:
    collectionConfig:
      blockCompressor: snappy  # Hoặc zlib nếu cần nén mạnh hơn
```

**Tối ưu chỉ mục (indexes)**
-  Tạo index cho các field thường truy vấn:
```js!
db.users.createIndex({email: 1})
```
- Sử dụng compound index nếu truy vấn nhiều trường cùng lúc:
```js!
db.orders.createIndex({user_id: 1, created_at: -1})
```

# 4. Cấu hình bảo mật cơ bản 

### Kích hoạt xác thực (authentication)
- Tạo user **admin**
```sql!
mongosh
> use admin
> db.createUser({
  user: "admin",
  pwd: "PassWd@123",
  roles: [ { role: "userAdminAnyDatabase", db: "admin" }, "readWriteAnyDatabase" ]
})
```
- Bật xác thực trong file cấu hình:
```yaml!
# /etc/mongod.conf
security:
  authorization: enabled
```
### Chỉ cho phép kết nối nội bộ hoặc IP cụ thể
```yaml!
# /etc/mongod.conf
net:
  bindIp: 127.0.0.1,192.168.1.100  #VD: chỉ cho phép IP này truy cập
```

### Không chạy MongoDB bằng quyền root
- Mặc định MongoDB chạy dưới user mongodb.
- `/var/lib/mongodb` và log file thuộc về user này:
```bash!
sudo chown -R mongodb:mongodb /var/lib/mongodb /var/log/mongodb
```
### Giới hạn quyền hệ thống
- Sử dụng `ufw` hoặc `firewalld` để giới hạn cổng 27017 với ip đáng tin cậy:
```bash!
sudo ufw allow from 192.168.1.100 to any port 27017
```
