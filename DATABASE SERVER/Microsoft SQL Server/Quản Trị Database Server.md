> # Quản Trị Database Server
>- User và quản lý quyền truy cập
>- Sao lưu và phục hồi (Backup & Recovery)
>- Theo dõi hiệu năng (Monitoring)
>- Tối ưu hóa truy vấn (Query Optimization)
>- Quản lý transaction và lock
>---

# 1. User và quản lý quyền truy cập
- Trong SQL Server, việc quản lý người dùng và quyền truy cập là nền tảng để bảo mật dữ liệu. SQL Server sử dụng mô hình xác thực dựa trên **Login** (cấp máy chủ) và **User** (cấp cơ sở dữ liệu).
- **Khái niệm**
    - **Login** (Đăng nhập): Là một tài khoản ở cấp độ máy chủ SQL Server. Login có thể là tài khoản Windows (người dùng hoặc nhóm) hoặc tài khoản SQL Server riêng. Login cho phép kết nối đến **Instance SQL Server**.
    - **User** (Người dùng): Là một tài khoản ở cấp độ cơ sở dữ liệu, được ánh xạ tới một Login. Một Login có thể có User trong nhiều cơ sở dữ liệu khác nhau. User là đối tượng mà gán các quyền cụ thể trên các đối tượng cơ sở dữ liệu (bảng, view, stored procedure, v.v.).
    - **Role** (Vai trò): Là một tập hợp các quyền được định nghĩa trước. Có thể gán các Login/User vào các Role để quản lý quyền dễ dàng hơn, thay vì gán quyền riêng lẻ cho từng người dùng. Có các Server Roles (vai trò máy chủ) và Database Roles (vai trò cơ sở dữ liệu).


## 1.1. Tạo Login (Đăng nhập vào SQL Server)
- Login là tài khoản cấp hệ thống, cho phép người dùng truy cập vào SQL Server nhưng chưa có quyền trên bất kỳ database nào.
- **Tạo Login mới:** với tên `test1login`
```sql!
CREATE LOGIN tes1login WITH PASSWORD = 'PasswordStrong123@';
```
![image](https://github.com/user-attachments/assets/425f5968-f135-4e76-be10-73002b8140cc)


- Hoặc thao tác trên giao diện:  
Click chuột phải vào phần `Security` > `Logins` > `New Login`: điền tên và mật khẩu mạnh...
![image](https://github.com/user-attachments/assets/232d6478-5043-4a38-a85e-aa5871bd20f6)


- Kiểm tra danh sách Login:
```sql!
SELECT name FROM sys.server_principals WHERE type = 'S';
```
![image](https://github.com/user-attachments/assets/5a024828-af1a-43fc-a3d6-85f1c702255e)


## 1.2. Tạo User trong Database
User là tài khoản cấp database, giúp kiểm soát quyền truy cập vào từng cơ sở dữ liệu cụ thể.
- Tại Database `MyDB1`, tạo user `userDB1` và liên kết với user login `test1login` để có quyền truy cập vào dữ liệu
 
```sql!
use MyDB1;
CREATE USER userDB1 FOR LOGIN test1login;
```
> Lệnh này tạo một user trong `MyDB1` từ login `test1login`

![image](https://github.com/user-attachments/assets/6a8229d3-6897-48b7-8a39-ffec063f0a7e)


- Kiểm tra danh sách User trong Database:
```sql!
SELECT name FROM sys.database_principals WHERE type = 'S';
```

## 1.3. Gán quyền cho User
> Có 2 loại quyền chính:
> - **Server Role**: Quyền cấp hệ thống (ví dụ: sysadmin, securityadmin).
> - **Database Role**: Quyền cấp database (ví dụ: db_datareader, db_datawriter)

- **Gán quyền Database Role:**
```sql!
USE MyDB1;
ALTER ROLE db_datareader ADD MEMBER userDB1; -- Quyền đọc dữ liệu
ALTER ROLE db_datawriter ADD MEMBER userDB1; -- Quyền ghi dữ liệu
```
>- Lệnh này cấp quyền đọc và ghi dữ liệu cho `userDB1` trong `MyDB1`.
>- Các quyền phổ biến:
> `db_owner` : Toàn quyền trên database
> `db_datareader` :	Chỉ đọc dữ liệu
> `db_datawriter`: Chỉ ghi dữ liệu
> `db_ddladmin`	: Thay đổi cấu trúc bảng, schema..

- **Test quyền `userDB1`:**
    - `userDB1` có thể đọc được dữ liệu trong MyDB1
![image](https://github.com/user-attachments/assets/bb730c7f-44c6-496d-b1f1-c17f32b3af08)

    - Nhưng không đọc được dữ liệu trong MyDB2:
![image](https://github.com/user-attachments/assets/082bc670-4597-4950-80b7-1e4dd965c420)



# 2. Sao lưu và Khôi phục (Backup & Recovery)
- **Backup database `MyDB1`:**
```sql!
BACKUP DATABASE MyDB1
TO DISK = 'D:\Backups\MyDB1_full.bak'
WITH FORMAT, INIT;
```

- **Restore database `MyDB1`**
```sql!
RESTORE DATABASE TestDB_Recovery
FROM DISK = 'D:\Backups\MyDB1_full.bak'
WITH MOVE 'MyDB1' TO 'D:\Data\MyDB1_Recovery.mdf',
     MOVE 'MyDB1_log' TO 'D:\Data\MyDB1_Recovery.ldf';
```

**// Hoặc sử dụng giao diện SSMS**

![image](https://github.com/user-attachments/assets/b64419db-99e7-43b2-b21e-08fe94a0671e)


![image](https://github.com/user-attachments/assets/7b9ec753-3068-4b80-8661-cdc027822b59)


---

# 3. Theo dõi hiệu năng

- **Kiểm tra mức sử dụng CPU và RAM**
```sql!
SELECT * FROM sys.dm_os_memory_clerks;
SELECT * FROM sys.dm_exec_requests;
```
>Lệnh này giúp theo dõi mức sử dụng tài nguyên.

- **Kiểm tra truy vấn đang chạy**
```sql!
SELECT session_id, status, blocking_session_id, wait_type, wait_time FROM sys.dm_exec_requests;
```
>Giúp xác định các truy vấn đang chạy và có thể gây chậm hệ thống.

- Top 5 truy vấn tiêu tốn CPU:
```sql!
SELECT TOP 5
    total_worker_time/1000.0 AS CPU_ms,
    execution_count,
    query_text = SUBSTRING(st.text, (qs.statement_start_offset/2)+1,
    ((CASE statement_end_offset
      WHEN -1 THEN DATALENGTH(st.text)
      ELSE qs.statement_end_offset END - qs.statement_start_offset)/2)+1)
FROM sys.dm_exec_query_stats AS qs
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) AS st
ORDER BY total_worker_time DESC;
```

// **Hoặc Sử dụng Activity Monitor trong SSMS**
![image](https://github.com/user-attachments/assets/6f8aba96-0a45-445b-a712-1e3c61179f42)


- Tại đây có thể Xem: Processes, Resource Waits, Recent Expensive Queries

![image](https://github.com/user-attachments/assets/10cb1911-ed76-4d24-8ae1-ca463bde8e64)


// **Report SSMS**

![image](https://github.com/user-attachments/assets/2a3f0bf3-8f23-4bdf-bd18-4d8b9341f8bd)


# 4. Tối ưu hóa truy vấn 


