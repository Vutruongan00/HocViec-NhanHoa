> # Quản Trị Database Server
### Các lệnh và thao tác cơ bản trong MongoDB

- **Quản lý Cơ sở dữ liệu (Database):**
| Hành động                | Câu lệnh            |
| ------------------------ | ------------------- |
| Hiển thị tất cả database | `show dbs`          |
| Sử dụng database         | `use mydb`          |
| Xóa database hiện tại    | `db.dropDatabase()` |


- **Thêm dữ liệu:**
```javascript=
//thêm 1 hoặc nhiều dữ liệu//
db.students.insertOne({ name: "An", age: 20 }) 

db.students.insertMany([
  { name: "Bình", age: 21 },
  { name: "Chi", age: 19 }
])
```
- Cập nhật dữ liệu:
```javascript=
db.students.updateOne(
  { name: "An" },
  { $set: { age: 22 } }
)

db.students.updateMany(
  { age: { $lt: 20 } },
  { $inc: { age: 1 } }
)
```

- Xóa dữ liệu:
```javascript=
db.students.deleteOne({ name: "Chi" })
db.students.deleteMany({ age: { $gt: 25 } })
```
---
# 1. User và quản lý quyền truy cập (Users and Access Control)
### 1.1 Tạo User `root`
- Để quản lý người dùng và quyền truy cập, trước hết cần **Kích hoạt xác thực Authentication** trong file cấu hình và **tạo user quản trị (root)** trong MongoDB `/etc/mongod.conf`.
```yaml!
security:
  authorization: enabled
```
--> Khởi động lại dịch vụ MongoDB
- Truy cập MongoDB ở **chế độ không bảo mật tạm thời** (để tạo user root ban đầu)
```bash!
sudo systemctl stop mongod
```
- Chạy lại Mongod không bật xác thực:
```bash=
sudo mongod --dbpath /var/lib/mongodb --bind_ip 127.0.0.1 --port 27017 --fork --logpath /var/log/mongodb/disable-auth.log --noauth
```
- Truy cập vào mongo: 
 ```
 mongosh
 ```
- Tạo User `root` đầu tiên:
```javascript!
use admin

db.createUser({
  user: "root",
  pwd: "yourStrongPassword123",
  roles: [ { role: "root", db: "admin" } ]
});
```

>**Các role phổ biến:**
>- `readWrite`: đọc/ghi trong 1 DB.
>- `dbAdmin`: quản trị DB (index, thống kê, validate).
>- `userAdmin`: tạo/sửa người dùng DB hiện tại.
>- `clusterAdmin`: quản trị toàn bộ cluster.


- Tắt chế độ không bảo mật:
```bash!
sudo pkill mongod
```
- Khởi động lại Mongod: `sudo systemctl start mongod`


### 2.1 Tạo User
- Kết nối lại bằng tài khoản root:
```bash!
mongosh -u root -p --authenticationDatabase admin
```
-->nhập Password vừa tạo để login root

- Tạo user: `user1`

```sql!
use admin
db.createUser({
   user: "user1",
   pwd: "123456",
   roles: []
 });
```
> `user`: "user1" – Xác định tên đăng nhập của người dùng sẽ được tạo.
> `pwd`: "123456" – Đặt mật khẩu cho người dùng.
> `roles`: [...] – Danh sách các quyền mà người dùng này sẽ có

- Gán quyền cho `user1`
```
db.grantRolesToUser(
 	"user2",
 	[
 		{ role: "readWrite", db: "test"}
 	]
 )
```
> user1 có quyền đọc/ghi trong db `test`

- Kiểm tra: ` db.getUsers();`

![image](https://github.com/user-attachments/assets/0a9f19c6-2aeb-4769-9fe2-9b0aea67aafa)



---
# 2. Sao lưu và Phục hồi  (Backup & Recovery)

## 2.1 Sao lưu (Backup)
**Backup bằng `mongodump`**
```bash=
mongodump --out /backups/mongo-backup-$(date +%F)
```
- Hoặc chỉ backup 1 databas cụ thể:
```bash=
mongodump -d test -o /backups/test
```
> `--out` hoặc `-o`: Chỉ định đường dẫn tới thư mục nơi muốn lưu bản sao lưu
> `--db` hoặc `-d` CHỉ định tên database cần sao lưu

- **Nếu lỗi xác thực** MongoDB khi dùng mongodump  mà không cung cấp thông tin đăng nhập:
```bash!
mongodump --db MyNewMongoDB --out /path/to/backup/directory --username <your_username> --password <your_password> --authenticationDatabase <auth_db>
```
> - `--authenticationDatabase <auth_db>`:  Tên của database nơi tài khoản người dùng của bạn được tạo.
- Ví dụ: 
```bash 
mongodump --db test --out /path/to/backup/directory --username admin --password PassWd@123 --authenticationDatabase admin
```


## 2.2 Phục hồi
**Phục hồi bằng `mongorestore`**
```bash!
mongorestore --db <target_database_name> --authenticationDatabase <auth_db> --username <your_username> --password <your_password> /path/to/backup/directory/<original_database_name>
```
---

# 3. Theo dõi hiệu năng (Monitoring)

**Công cụ nội bộ:**
- `db.serverStatus()`

- `db.currentOp()` – hiển thị các operation đang chạy

- `db.stats()` – thống kê DB
>Các thông số quan trọng trong `db.stats()`
>      - `storageSize`: Dung lượng ổ đĩa đã sử dụng.
>     - `dataSize`: Kích thước dữ liệu được lưu trữ.
>     - `objects`: Số lượng tài liệu trong cơ sở dữ liệu.
    
    
- `db.collection.stats()` – thống kê từng collection

**mongostat, mongotop (CLI)**

```bash!
mongotop -u <your_username> -p --authenticationDatabase <auth_db>
```

# 4 Tối ưu hóa truy vấn Tối ưu hóa truy vấn (Query Optimization)
 **Sử dụng `explain()` để phân tích truy vấn**
-  `explain()` là công cụ mạnh mẽ nhất để hiểu cách MongoDB thực thi một truy vấn và xác định các nút thắt cổ chai.
-  Cách sử dụng: Đặt `.explain("executionStats")` sau một truy vấn để phân tích Ví dụ:
```javascript=
db.products.find({ category: "Electronics", price: { $lt: 500 } }).sort({ name: 1 }).explain("executionStats")
```

>Các thông tin quan trọng từ explain():
> - `queryPlanner.winningPlan`: Kế hoạch thực thi mà MongoDB chọn.
> - `stage`: Bước thực thi (ví dụ: IXSCAN - quét chỉ mục, COLLSCAN - quét collection).
> - `indexName`: Chỉ mục được sử dụng (nếu có).
> - `executionStats.executionSuccess`: Truy vấn có thành công không.
> - `executionStats.nReturned`: Số lượng tài liệu trả về.
> - `executionStats.totalKeysExamined`: Số lượng khóa chỉ mục được kiểm tra.
> - `executionStats.totalDocsExamined`: Số lượng tài liệu được kiểm tra.
> - `executionStats.executionTimeMillis`: Thời gian thực thi.

-  Câu truy vấn này yêu cầu Tìm tất cả các tài liệu trong collection products nơi trường category là "Electronics" VÀ trường price nhỏ hơn `500`. Thay vì trả về dữ liệu, Mông chạy truy vấn và cung cấp số liệu thống kê chi tiết về cách MongoDB thực hiện các bước tìm kiếm và sắp xếp đó (ví dụ: liệu nó có sử dụng index không, cần quét bao nhiêu tài liệu, mất bao nhiêu thời gian).

![image](https://github.com/user-attachments/assets/0c2976b3-e582-41e6-886f-f59ad03fd655)

> - `winningPlan`: Đây là kế hoạch mà MongoDB đã chọn để thực thi truy vấn
> - `inputStage`: Đây là giai đoạn đầu vào cho giai đoạn SORT
> - `stage: 'COLLSCAN'`: **Đây là điểm mấu chốt!** COLLSCAN (Collection Scan) nghĩa là MongoDB đã phải **quét toàn bộ collection products** từ đầu đến cuối để tìm các tài liệu phù hợp với bộ lọc
> --> Truy vấn **không sử dụng bất kỳ index nào** để lọc dữ liệu. Nó phải quét toàn bộ collection, sau đó mới áp dụng bộ lọc và cuối cùng là sắp xếp các kết quả đã lọc.
> 
> - `executionSuccess: true`: Truy vấn đã **chạy thành công**. Nhưng **không hiệu quả** 


## --> Cách tối ưu:
**Tạo chỉ mục (Indexes)**
```
db.products.createIndex({ category: 1, price: 1, name: 1 });
```
> - `category: 1`: Sẽ giúp lọc nhanh category: "Electronics".
> - `price: 1`: Tiếp tục giúp lọc nhanh price: { $lt: 500 }.
> - `name: 1`: Sẽ giúp sắp xếp nhanh name: 1 và có thể tránh được giai đoạn `SORT` riêng biệt, vì dữ liệu đã được sắp xếp theo thứ tự mong muốn ngay từ index. (Đảm bảo thứ tự của `price` và `name` phù hợp với truy vấn

![image](https://github.com/user-attachments/assets/3482f9e0-8de4-40e1-bf28-04b0e9aaefa0)

- `stage: 'IXSCAN'`: Đây là dấu hiệu của sự tối ưu hóa! MongoDB đã sử dụng một Index Scan để tìm kiếm dữ liệu. Điều này có nghĩa là nó không còn phải quét toàn bộ collection nữa mà sử dụng index để định vị các tài liệu nhanh chóng.
- `stage: 'SORT'`: Giai đoạn này vẫn còn, nhưng quan trọng là nó nằm sau IXSCAN và nó đang sắp xếp một tập hợp dữ liệu nhỏ hơn (đã được lọc qua index).
- `stage: 'FETCH'`: Giai đoạn cuối cùng sau khi đã tìm và sắp xếp dữ liệu từ index. Nó lấy các tài liệu đầy đủ từ ổ đĩa dựa trên các khóa tìm thấy trong index.
- `totalKeysExamined` > 0, -->  có **index** được sử dụng
- trong khi `totalDocsExamined` = `nReturned` (hoặc thấp hơn đáng kể so với tổng số tài liệu trong collection).

# 5. Quản lý Transaction và Lock
**a. Transaction (Giao dịch đa tài liệu)**
**Transactions** trong MongoDB đảm bảo tính nguyên tử (Atomicity), nhất quán (Consistency), cô lập (Isolation) và bền vững (Durability) (ACID) cho các hoạt động ghi liên quan đến nhiều tài liệu, hoặc nhiều collection, hoặc nhiều database.

- **Khi nào sử dụng Transaction:**
    - Cần đảm bảo rằng một nhóm các thao tác đọc/ghi (ví dụ: chuyển tiền từ tài khoản A sang tài khoản B) phải thành công hoặc thất bại cùng nhau
    - Khi cần tính nhất quán dữ liệu mạnh mẽ giữa các tài liệu.

**b. Lock (Khóa)**
MongoDB sử dụng các **khóa (locks)** để đảm bảo **tính nhất quán dữ liệu** và **tránh xung đột** khi nhiều thao tác truy cập hoặc chỉnh sửa cùng một dữ liệu đồng thời.

- Các loại khóa trong MongoDB

    - **Global Lock (Khóa toàn cục)**: Khóa cấp cao nhất, ít được sử dụng trong các phiên bản hiện đại.
    - **Database Lock**: Khóa ở cấp cơ sở dữ liệu.
    - **Collection Lock**: Khóa ở cấp bộ sưu tập (collection).
    - **Document Lock (Storage Engine Lock)**: Khóa ở cấp tài liệu hoặc chi tiết hơn, tuỳ theo Storage Engine.

- Cơ chế khóa trong **WiredTiger** (Storage Engine mặc định) WiredTiger sử dụng kết hợp:
    - **Optimistic Concurrency Control** (Kiểm soát đồng thời lạc quan)
    - **Document-level Lock** hoặc **granular lock** tại cấp độ B-tree

- **Đặc điểm:**
    - Các thao tác ghi (insert, update, delete) vẫn cần một mức khóa để đảm bảo **tính nguyên tử (atomicity)**.
    - Các thao tác đọc **không chặn** ghi và ngược lại, **trừ khi**:
      - Có xung đột trực tiếp trên cùng một tài liệu
      - Hoặc có các thao tác quản trị yêu cầu khóa toàn cục

- Theo dõi khóa

    - Sử dụng:
  ```js
    db.serverStatus().locks
    // hoặc
   db.serverStatus().locks 
    ```
    để xem thông tin về các khóa đang hoạt động.
