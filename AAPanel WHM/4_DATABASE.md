# Quản lý DATABASE trên aaPanel
aaPanel Hỗ trợ quản lý 5 loại cơ sở dữ liệu:
- MysSQL
- SQLServer
- MongoDB
- Redis
- PgSQL
 
## 1. MySQL

![image](https://github.com/user-attachments/assets/960e4de5-8fe5-4be0-b35d-01b67283821d)

- Các chức năng có trên MySQL:

| **Chức năng**  | **Mô tả**   |
| -------------- | ------------|
| **Add DB**     | Tạo một cơ sở dữ liệu mới trong MySQL nội bộ hoặc từ xa (remote). Bạn cần cung cấp tên DB, user và password.     |
| **Root password**    | Xem hoặc thay đổi mật khẩu của tài khoản **root MySQL** cục bộ (nội bộ).                                  |
| **Remote DB**  | Quản lý và thêm cơ sở dữ liệu trên máy chủ MySQL từ xa (qua IP khác).  |
| **Sync all**    | Đồng bộ tất cả cơ sở dữ liệu nội bộ với danh sách hiển thị trên aaPanel.                                            |
| **Get DB from server**                                              | Lấy danh sách cơ sở dữ liệu từ MySQL nội bộ hoặc từ xa và đồng bộ về giao diện aaPanel.                             |
| **Database name**                                                   | Hiển thị tên của cơ sở dữ liệu hiện có.                                                                             |
| **Username**                                                        | Hiển thị tên người dùng quản lý tương ứng với cơ sở dữ liệu đó.                                                     |
| **Password**                                                        | Xem và **sao chép** mật khẩu đăng nhập vào DB.                                                                      |
| **Quota**                                                           | Hiển thị và thiết lập giới hạn dung lượng cho database đó (*chỉ hoạt động với ổ đĩa định dạng XFS*).                |
| **Backup**                                                          | Hiển thị số bản sao lưu hiện có. Nhấn vào để thao tác: **Backup, Download, Restore, Delete** file backup tương ứng. |
| **Import**                                                          | Tải lên file `.sql` để **import vào database hiện tại**. Hữu ích khi di chuyển hoặc phục hồi dữ liệu.               |
| **Location**                                                        | Hiển thị đường dẫn lưu trữ vật lý của tệp database trên hệ thống (thường là `/www/server/data/`).                   |
| **Note**                                                            | Ghi chú dành riêng cho database để dễ quản lý hoặc phân loại.                                                       |
| **phpMyAdmin**                                                      | Mở **phpMyAdmin** để quản lý database bằng giao diện web trực quan.                                                 |
| **Permission**                                                      | Hiển thị và thiết lập quyền đăng nhập, truy cập cho user database (host, IP, quyền SELECT, INSERT...).              |
| **Tools**   | Cung cấp công cụ quản trị DB nâng cao: →  **Repair**: sửa lỗi bảng; 
||-> **Optimize**: tối ưu hóa bảng;         
||**Convert**: chuyển đổi engine bảng giữa **MyISAM** và **InnoDB** |
| **CHG PW**                                                          | Xem hoặc thay đổi mật khẩu của user quản lý database hiện tại.                                                      |
| **Delete**                                                          | Xóa cơ sở dữ liệu. ⚠️ *Nên sao lưu trước khi xóa để tránh mất dữ liệu vĩnh viễn.*  |


### Add DB - Thêm Database
- Thêm thủ công hoặc thêm trong quá trình add website:

    ![image](https://github.com/user-attachments/assets/9d318615-0273-4fae-a61b-0669d8489b7f)

    - Nhập tên DB, username, passw, và chọn quyền truy cập từ Local /hoặc IP cụ thể

### Xem / sửa Password root của MySQL

![image](https://github.com/user-attachments/assets/539c19d6-fa18-46a5-850c-7cc423609f98)

### Cấu hình phpMyAdmin
- Tại cột **Operate** --> nhấp chọn **phpMyAdmin**
![image](https://github.com/user-attachments/assets/8b9285af-92b0-4ee8-b589-0ef5a903e4bc)

- **Service**
![image](https://github.com/user-attachments/assets/753a5da9-6645-49c6-a7df-77ceedb45e68)
    - **Enable public access:** Cho phép truy cập công cộng
    - **Password-free access:** `Password-free access` yêu cầu đăng nhập vào phpMyAdmin
    - **Public access:** Mở **phpMyAdmin login page**
    
- **PHP version**
- **Cấu hình bảo mật** (Sercurity configuration): tùy chọn
    -  cổng truy cập, 
    -  Tắt mở SSL hoặc 
    -  Truy cập bằng mật khẩu
![image](https://github.com/user-attachments/assets/eea16746-bd3e-40ce-bbed-5fadbddb53e3)

### Thêm và quản lý DB từ xa - Remote DB
![image](https://github.com/user-attachments/assets/64277f0c-8a9a-47d6-8f60-041cf48f8e1b)

- Hỗ trợ MySQL 5.5, MariaDB 10.1 trở lên
- Hỗ trợ cơ sở dữ liệu đám mây từ các nhà cung cấp đám mây
- Đảm bảo máy chủ chạy aaPanel có quyền truy cập vào CSDL
- ...

### Trình quản lý chung của MySQL 

- **Service:** Hiển thị trạng thái dịch vụ MySQL hiện tại (`Start`, `Stop`, `Restart`, `Reload`).

![image](https://github.com/user-attachments/assets/fecd0351-5846-4978-900b-29fbc80bfa73)

- **Config file**: Truy cập và chỉnh sửa file cấu hình my.cnf của MySQL trực tiếp từ giao diện.

- **Storage location**	Xem vị trí lưu trữ dữ liệu MySQL (thư mục chứa DB thực tế).
- **Port**	Hiển thị và chỉnh sửa cổng mà MySQL đang sử dụng (mặc định: `3306`)
- **SSL**	Cấu hình SSL cho MySQL để tăng bảo mật kết nối giữa ứng dụng và CSDL.
- **Current status:** Hiển thị trạng thái và hiệu suất của MySQL:
![image](https://github.com/user-attachments/assets/aed2605a-6a95-4584-bb21-ad929a821102)

    - Tổng kết nối, số truy vấn/giây.
    - Các chỉ số như:
    - Active/Max connections
    - Thread cache hit rate
    - Innodb index hit rate
    - Query cache hit rate, v.v.
    > Mỗi mục đều có gợi ý để tối ưu nếu giá trị không đạt.

- **Optimization**: Cấu hình các thông số tối ưu hiệu suất (RAM, cache, buffer, thread...).
![image](https://github.com/user-attachments/assets/4855db19-4c36-4d65-a276-85d34857a38e)

    - Cho phép chọn cấu hình sẵn hoặc tự điều chỉnh thủ công.
    - Nút `Restart MySQL` cần thiết để áp dụng thay đổi.
    
- **Error log**	Xem log lỗi MySQL để hỗ trợ việc khắc phục sự cố.
- **Slow log**	Xem các truy vấn SQL thực thi chậm – hỗ trợ cho việc tối ưu câu lệnh
- **Binary log**	Bật hoặc xem các binary log, phục vụ sao lưu, replication.
- **Memory protection**	Tùy chọn để hạn chế MySQL sử dụng quá nhiều RAM (bảo vệ bộ nhớ hệ thống).

### Quota 
- Hiển thị và thiết lập giới hạn dung lượng cho database

![image](https://github.com/user-attachments/assets/fd00c947-8800-45cd-ba5a-a1e20de997c9)


---
## 2. SQLServer
![image](https://github.com/user-attachments/assets/77911774-28c0-47e9-9135-7b3c31c46a29)

Các chức năng quản lý SQL Server trong aaPanel:

| **Function** | **Mô tả** |
| ------------ | --------- |
| **Add DB**             | Thêm mới một cơ sở dữ liệu vào máy chủ SQL Server từ xa.          |
| **Remote DB**          | Thêm hoặc quản lý các kết nối đến máy chủ SQL Server từ xa.       |
| **Sync all**           | Đồng bộ danh sách cơ sở dữ liệu từ máy chủ SQL Server về aaPanel. |
| **Get DB from server** | Truy xuất danh sách CSDL từ máy chủ và hiển thị trong aaPanel.    |
| **Database name**      | Tên của cơ sở dữ liệu hiện tại.                                   |
| **Username**           | Tên người dùng liên kết với CSDL.                                 |
| **Password**           | Xem hoặc sao chép mật khẩu tài khoản kết nối.                     |
| **Location**           | Vị trí lưu trữ vật lý của CSDL trên máy chủ từ xa.                |
| **Note**               | Ghi chú mô tả về CSDL.                                            |
| **CHG PW**             | Thay đổi mật khẩu cho user kết nối cơ sở dữ liệu.                 |
| **Delete**             | Xóa CSDL hiện tại (khuyến nghị sao lưu trước khi thực hiện).      |

> Thao tác sử dụng các tính năng tương tự trên **MySQL**
---

## MongoDB
![image](https://github.com/user-attachments/assets/6d3c77ef-f43f-40ca-8a43-11bade6df7c0)

- Các chức năng quản lý DB:

| **Chức năng** | **Mô tả**  |
| ------------- | -----------|
| **Add DB**    | Thêm cơ sở dữ liệu mới vào MongoDB cục bộ hoặc máy chủ từ xa.                                        |
| **Root Password**   | Thay đổi mật khẩu **root** cho MongoDB cục bộ.                                              |
| **Security authentication** | Bật/Tắt cơ chế xác thực bảo mật (authentication mode).                                            |
| **Remote DB**               | Thêm hoặc quản lý các kết nối MongoDB từ xa.                                                      |
| **Sync all**                | Đồng bộ danh sách cơ sở dữ liệu từ MongoDB vào aaPanel.                                           |
| **Get DB from server**      | Truy xuất và đồng bộ danh sách DB từ MongoDB cục bộ hoặc từ xa vào aaPanel.                       |
| **Database name**           | Hiển thị tên cơ sở dữ liệu hiện tại.                                                              |
| **Username**                | Tên người dùng được cấp quyền trên CSDL.                                                          |
| **Password**                | Xem hoặc sao chép mật khẩu kết nối.                                                               |
| **Backup**                  | Hiển thị số lượng bản sao lưu, có thể thực hiện các thao tác: sao lưu, tải xuống, khôi phục, xóa. |
| **Import**                  | Tải file lên và import dữ liệu vào cơ sở dữ liệu hiện tại.                                        |
| **Position**                | Vị trí lưu trữ vật lý của dữ liệu MongoDB.                                                        |
| **Note**                    | Ghi chú mô tả cho cơ sở dữ liệu.                                                                  |
| **CHG PW**                  | Thay đổi mật khẩu của user trên MongoDB.                                                          |
| **Delete**                  | Xóa cơ sở dữ liệu hiện tại *(cần sao lưu trước khi thao tác).*                                    |

> Thao tác sử dụng các tính năng tương tự trên **MySQL**

---

## Redis 

![image](https://github.com/user-attachments/assets/0c848273-bee7-4cbc-b40b-da2594b8c3df)

- Các chức năng quản lý RedisDB trong aaPanel:

| **Chức năng**  | **Mô tả**    |
| -------------- | ------------ |
| **Add Key**          | Thêm dữ liệu mới (Key-Value) vào Redis.                                         |
| **Remote DB**        | Quản lý cơ sở dữ liệu Redis từ xa (remote Redis server).                        |
| **Backup list**      | Sao lưu dữ liệu Redis hiện tại, xem và quản lý danh sách backup.                |
| **Clear DB**         | Xóa toàn bộ dữ liệu trong Redis database đã chọn. *(Cẩn trọng!)*                |
| **DB0 - DB15**       | Redis mặc định có 16 database (DB0 → DB15), chọn database để xem hoặc thao tác. |
| **Key**              | Tên của Redis Key (khóa dữ liệu).                                               |
| **Value**            | Giá trị của Redis Key.                                                          |
| **Data type**        | Kiểu dữ liệu của Key, ví dụ: `string`, `list`, `set`, `hash`, `zset`.           |
| **Data length**      | Chiều dài hoặc số phần tử của giá trị Redis Key.                                |
| **Term of validity** | Thời gian sống (TTL - Time to live) của Redis Key (nếu có).                     |
| **Edit**             | Chỉnh sửa nội dung của Redis Key.                                               |
| **Delete**           | Xóa Redis Key khỏi database.                                                    |
> Thao tác sử dụng các tính năng tương tự các DB trên

---
## PgSQL 

![image](https://github.com/user-attachments/assets/f93c76ec-7705-4e55-abb7-11830db1d94e)

> Các tính năng và sử dụng tương tự trên các DB trước
