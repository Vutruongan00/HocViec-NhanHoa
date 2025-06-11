> # Cấu hình Replica set - MongoDB
>- **Môi trường:** 3 máy chủ Ubuntu
>`35.240.156.172`	- mongod1 (primary)
>`34.92.129.179`	- mongod2 (slave)
>`34.124.212.78`	- mongod3 (slave)

# 1. Replica Set là gì?
Replica Set là cơ chế replication (sao lưu dữ liệu theo thời gian thực) của MongoDB. Nó đảm bảo tính sẵn sàng cao (high availability) và khả năng chịu lỗi (fault tolerance) bằng cách duy trì nhiều bản sao (copies) của dữ liệu trên nhiều node.

**Cấu trúc Replica Set**
- **Primary**: node chính, xử lý ghi (write).
- **Secondary**: đồng bộ dữ liệu từ Primary, xử lý đọc (read) nếu cho phép.
- **Arbiter** (tùy chọn): không chứa dữ liệu, chỉ tham gia bầu chọn để duy trì số lượng thành viên lẻ.

> Minimum cần 3 node để có thể bầu chọn tự động khi xảy ra lỗi.

**Cách thức hoạt động**
- Mọi ghi dữ liệu luôn thực hiện trên Primary.
- **Secondary** liên tục replicate dữ liệu từ Primary bằng cơ chế oplog (operation log).
- Nếu Primary gặp sự cố, một trong các Secondary sẽ tự động bầu lên làm Primary mới (quá trình này gọi là failover).

**Ưu điểm của Replica Set**
- High Availability: tự động chuyển đổi Primary khi gặp lỗi.
- Data Redundancy: dữ liệu luôn có bản sao dự phòng.
- Read Scaling (có giới hạn): có thể đọc dữ liệu từ Secondary.
- Disaster Recovery: giảm nguy cơ mất mát dữ liệu.

# 2. Cấu hình Replica Set trên mỗi node
- Mở file cấu hình: `sudo nano /etc/mongod.conf`
- Sửa các phần:
```yaml=
net:
  bindIp: 0.0.0.0  # cho phép mọi IP (hoặc chỉ IP cụ thể)

replication:
  replSetName: "rs0"
```
- Khởi động lại dịch vụ: `systemctl restart mongod` 

## Khởi tạo Replica Set từ node `mongo1`
-  Truy cập Mongo bằng người dùng quản trị trên `mongo1`:
```bash=
mongosh
```
- Khởi tạo:
```javascript=
rs.initiate({
  _id: "rs0",
  members: [
    { _id: 1, host: "35.240.156.172:27017" },
    { _id: 2, host: "34.92.129.179:27017" },
    { _id: 3, host: "34.124.212.78:27017" }
  ]
})
```
- Kiểm tra trạng thái:
```javascript=
rs.status()
```
![image](https://github.com/user-attachments/assets/54235c78-b71e-4901-92d3-b8804b1cdb4e)


## 2.3. Kiểm tra kết nối replication
- Từ mongo2 / mongo3:
```bash=
mongosh
rs.status()
```
![image](https://github.com/user-attachments/assets/cf9e7c8b-c741-4f02-89e7-af0e0e3af0d1)


## 2.4. Kiểm tra hoạt động
- Tạo database trên **mongod1**:

```javascript=
use replicationdb;

db.demo.insertOne({ name: "Replica Test" })
```
![image](https://github.com/user-attachments/assets/7e0c0435-f037-498b-a122-1437e0fd3f1f)


- Kiểm tra trên note 2 / 3: `show dbs`
![image](https://github.com/user-attachments/assets/d5fc2818-4fca-4d3c-b0de-d53942cc98dc)


