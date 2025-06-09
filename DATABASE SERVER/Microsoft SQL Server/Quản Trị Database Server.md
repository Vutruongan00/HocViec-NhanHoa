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


# 4. Tối ưu hóa truy vấn (Query Optimization)
Tối ưu hóa truy vấn là quá trình cải thiện hiệu suất của các câu lệnh SQL để chúng thực thi nhanh hơn và sử dụng ít tài nguyên hơn.

### Các bước tối ưu hóa truy vấn
- **Xác định truy vấn chậm (Identify Slow Queries)**: Sử dụng các công cụ giám sát (DMVs, Activity Monitor, Extended Events) để tìm các truy vấn tiêu tốn nhiều tài nguyên nhất.
- **Phân tích Kế hoạch thực thi (Execution Plan):**
    - Trong SSMS, sau khi chạy một truy vấn, nhấp vào nút **Include Actual Execution Plan** (Ctrl+M) hoặc **Display Estimated Execution Plan** (Ctrl+L).
    - Kế hoạch thực thi cho thấy cách SQL Server dự định hoặc đã thực hiện truy vấn. Tìm kiếm các cảnh báo (màu vàng), các toán tử tốn kém (ví dụ: Table Scan, Index Scan lớn, Sort, Hash Match, Spool, Warning về Missing Index).
- **Tạo Index hiệu quả (Effective Indexing):**
- **Tối ưu hóa câu lệnh SQL (SQL Statement Optimization):**
    - Tránh `SELECT` *: Chỉ chọn các cột bạn thực sự cần.
    - Sử dụng `JOIN` thay vì `Subquery` (khi thích hợp): Đôi khi `JOIN` có hiệu suất tốt hơn.
    - Tránh các hàm trong mệnh đề `WHERE` trên các cột được đánh index
    - Tránh `LIKE '%value%'`: Sử dụng `LIKE 'value%'` nếu có thể để tận dụng index.
    - Sử dụng `UNION ALL` thay vì `UNION` nếu không cần loại bỏ các bản ghi trùng lặp (vì `UNION` yêu cầu sắp xếp và loại bỏ trùng lặp).

- **Tối ưu hóa cấu hình SQL Server**: ( MAXDOP, Cost Threshold for Parallelism, Memory).

## Ví dụ:
- Tạo CSDL và bảng mẫu:
```sql!
-- Tạo database
CREATE DATABASE SalesDB;
GO

USE SalesDB;
GO

-- Tạo bảng Customers
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    FullName NVARCHAR(100),
    Email NVARCHAR(100)
);

-- Tạo bảng Orders
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerID INT,
    OrderDate DATE,
    Amount DECIMAL(10,2),
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);
```
- Thêm Dữ liệu vào bảng đã tạo
```sql!
-- Thêm dữ liệu Customers
INSERT INTO Customers VALUES
(1, 'Nguyen Van A', 'a@example.com'),
(2, 'Tran Thi B', 'b@example.com'),
(3, 'Le Van C', 'c@example.com');

-- Thêm dữ liệu Orders
INSERT INTO Orders VALUES
(101, 1, '2024-01-10', 150.00),
(102, 1, '2024-01-15', 200.00),
(103, 2, '2024-02-20', 300.00),
(104, 3, '2024-03-05', 120.00),
(105, 2, '2024-03-10', 400.00);
```
- Thêm lượng dữ liệu lớn vào bảng đã tạo:(100000 khách hàng và 500.000 đơn hàng)

**Kiểm tra hiệu năng truy vấn:**
- Truy vấn ban đầu(chậm): `tổng số tiền đã chi của mỗi khách hàng trong năm 2024`
> Dùng **Execution Plan trong SSMS** (**Ctrl + M**) trước khi chạy truy vấn
- Thực hiện truy vấn: Truy vấn tổng số tiền đã chi của mỗi khách hàng trong năm 2024
```sql!
SELECT 
    c.FullName,
    SUM(o.Amount) AS TotalSpent
FROM 
    Customers c
JOIN 
    Orders o ON c.CustomerID = o.CustomerID
WHERE 
    o.OrderDate >= '2024-01-01' AND o.OrderDate < '2025-01-01'
GROUP BY 
    c.FullName;
```




### kỹ thuật tối ưu hóa truy vấn

-  Tạo chỉ mục Indexes phù hợp
```sql!
--Tạo chỉ mục trên OrderDate để tăng tốc filter theo thời gian
CREATE NONCLUSTERED INDEX idx_order_date ON Orders(OrderDate);
```
- Tránh SELECT *: Chỉ chọn các cột bạn thực sự cần.
> Giả sử có bảng `Orders` với nhiều cột như trong ví dụ lab của chúng ta, nhưng bạn chỉ cần `OrderID`, `OrderDate`, `Amount` và `CustomerID` để hiển thị trong báo cáo đơn hàng.
> - Truy vấn sau xử lý chậm:
> ```sql!
> SELECT * -- Lấy tất cả 7 cột
>FROM Orders WHERE OrderDate >= '2024-02-01' AND OrderDate < '2024-03-01';

- Xử lý nhanh hơn với: 
```sql!
SELECT OrderID, CustomerID, OrderDate, Amount -- Chỉ lấy 4 cột cần thiết
FROM Orders
WHERE OrderDate >= '2024-02-01' AND OrderDate < '2024-03-01';
```

- Sử dụng JOIN thay vì Subquery (khi thích hợp): Đôi khi JOIN có hiệu suất tốt hơn.

----
# 5. Quản lý Transaction và Lock

## 5.1 1. Transaction (Giao dịch):
Một transaction là một chuỗi các thao tác SQL được thực hiện như một đơn vị công việc logic duy nhất. Nó tuân thủ các thuộc tính ACID:
- **Atomicity** (Nguyên tử): Tất cả các thao tác trong transaction phải thành công, hoặc không có thao tác nào thành công. Nếu một phần thất bại, toàn bộ transaction sẽ được quay trở lại trạng thái ban đầu (ROLLBACK).
- **Consistency** (Nhất quán): Một transaction phải đưa database từ trạng thái nhất quán này sang trạng thái nhất quán khác.
- **Isolation** (Cô lập): Các transaction đồng thời phải được thực hiện một cách độc lập với nhau. Mỗi transaction nên "nhìn thấy" database như thể nó đang chạy một mình.
- **Durability** (Bền vững): Một khi transaction đã được COMMIT (hoàn thành), các thay đổi của nó phải là vĩnh viễn và tồn tại ngay cả khi hệ thống gặp sự cố

## 5.2 Lock (Khóa)
SQL Server sử dụng các lock để thực thi tính cô lập (Isolation) của các transaction. Khi một transaction truy cập hoặc sửa đổi dữ liệu, SQL Server sẽ đặt một hoặc nhiều lock lên dữ liệu đó để ngăn các transaction khác truy cập hoặc sửa đổi cùng một dữ liệu theo cách không tương thích cho đến khi transaction đầu tiên hoàn tất.

### **Các loại lock phổ biến:**

- **Shared Locks (S)**: Được đặt khi dữ liệu đang được đọc. Cho phép nhiều transaction đọc cùng một dữ liệu đồng thời.
- **Exclusive Locks (X)**: Được đặt khi dữ liệu đang được sửa đổi (INSERT, UPDATE, DELETE). Ngăn chặn tất cả các loại lock khác (bao gồm Shared) trên cùng dữ liệu đó. Chỉ một **Exclusive lock** được phép trên một tài nguyên tại một thời điểm.
- **Update Locks (U)**: Một loại lock trung gian. Được đặt khi một transaction đang tìm kiếm dữ liệu để có khả năng sửa đổi nó. Nó cho phép Shared locks tồn tại nhưng ngăn chặn các Update locks khác hoặc Exclusive locks. Khi transaction sẵn sàng sửa đổi, Update lock sẽ được chuyển thành Exclusive lock.
- **Intent Locks (IS, IX, IU)**: Được đặt ở cấp cao hơn (bảng, trang) để báo hiệu rằng một lock ở cấp thấp hơn (hàng) sẽ được đặt. Giúp SQL Server nhanh chóng xác định các xung đột lock mà không cần kiểm tra từng hàng.

### **Khóa hạt (Lock Granularity):** SQL Server có thể đặt lock ở nhiều mức độ khác nhau:

- **Row-level locks**: Khóa từng hàng. Chi phí cao nhưng độ chính xác cao, ít xung đột.
- **Page-level locks**: Khóa toàn bộ trang dữ liệu (8KB). Hiệu quả hơn về chi phí nhưng dễ gây xung đột hơn (nhiều hàng trên một trang).
- **Table-level locks**: Khóa toàn bộ bảng. Chi phí thấp nhất nhưng gây xung đột cao nhất (chặn toàn bộ bảng).

### **Vấn đề với Lock:**

- **Blocking** (Chặn): Một transaction giữ một lock trên một tài nguyên, ngăn chặn transaction khác truy cập cùng tài nguyên đó. Transaction thứ hai phải chờ transaction đầu tiên nhả lock.
- **Deadlock** (Tắc nghẽn): Một tình huống trong đó hai hoặc nhiều transaction đang chờ lẫn nhau để nhả lock, tạo thành một chu trình chờ đợi vĩnh viễn. SQL Server sẽ phát hiện deadlock và chọn một transaction làm "deadlock victim" (nạn nhân của deadlock) để tự động hủy bỏ (ROLLBACK) nó, cho phép các transaction khác tiếp tục.

## Ví dụ Thực hành
> **Môi trường**: mô phỏng mỗi transaction chạy trên 1 cửa sổ New Query trong SSMS(hoặc 2 kết nối khác nhau đến cùng database)

### **Bước 1: Chuẩn bị Database và Dữ liệu**

- **CHẠY TRONG SSMS - New Query 1 và Tạo database mới**
```sql!
USE master;

CREATE DATABASE LockDemoDB;
USE LockDemoDB;
GO
```
- Tạo bảng Products để demo
```sql!
CREATE TABLE Products (
    ProductID INT PRIMARY KEY,
    ProductName NVARCHAR(100),
    StockQuantity INT,
    Price DECIMAL(10,2)
);
GO
```
- Thêm dữ liệu mẫu:
```sql!
INSERT INTO Products (ProductID, ProductName, StockQuantity, Price) VALUES
(1, 'Laptop X', 100, 1200.00),
(2, 'Mouse Y', 500, 25.00),
(3, 'Keyboard Z', 200, 75.00);
GO

-- Kiểm tra dữ liệu
SELECT * FROM Products;
GO
```

### Bước 2: Thực hành Blocking (Chặn)
>**Tình huống**: Một **transaction A** cập nhật hàng, và **transaction B**  cố gắng đọc hoặc cập nhật cùng hàng đó trước khi transaction đầu tiên hoàn tất.

**Trong transaction A:**(Cửa sổ 1)
```sql!
USE LockDemoDB;
GO

BEGIN TRANSACTION; -- Bắt đầu Transaction A
```
- Transaction A cập nhật StockQuantity của ProductID = 1
```sql!
UPDATE Products
SET StockQuantity = StockQuantity - 10
WHERE ProductID = 1;
```
![image](https://github.com/user-attachments/assets/0be711b8-3023-40b1-a1a4-fc89b85cf88c)



**Transaction B - Bị Blocking** (Cửa sổ 2 
```sql!
USE LockDemoDB;
GO

SELECT * FROM Products WHERE ProductID = 1;

PRINT 'Transaction B: Trying to read ProductID 1. Blocked!';
```
>- Transaction B cố gắng ĐỌC cùng hàng
>- Lệnh này sẽ yêu cầu Shared Lock (S) trên ProductID = 1.
>- S Lock không tương thích với X Lock mà Transaction A đang giữ, nên nó sẽ bị BLOCK.

![image](https://github.com/user-attachments/assets/c6c58d08-0807-46fc-b108-f1e263b659d5)


**Nhận diện Blocking**
- Sử dụng công cụ **Activity Monitor**:
    - Trong process của Cửa sổ 2, tại đây ta thấy có cột `Blocked By` hiển thị SPID của Cửa sổ 1 (51)
    - Cột `Wait Type` sẽ hiển thị các kiểu chờ liên quan đến lock (ví dụ: LCK_M_S cho Shared lock, LCK_M_X cho Exclusive lock).
    
![image](https://github.com/user-attachments/assets/06b0ef4d-03b7-4b23-8285-708d287ccb56)


### Xử lý Blocking 
- Để giải quyết blocking, ta phải **kết thúc Transaction A:**

```sql!
COMMIT TRANSACTION; -- để lưu thay đổi và nhả lock

-- hoặc hủy thay đổi và nhả lock:
ROLLBACK TRANSACTION;
```

![image](https://github.com/user-attachments/assets/4aee3627-9e8d-4ff6-ac07-97546f7c778b)



## Bước 3: Thực hành Deadlock (Tắc nghẽn)

> **Tình huống**: Hai transaction mỗi bên giữ một lock trên một tài nguyên và cố gắng lấy lock trên tài nguyên mà bên kia đang giữ, tạo thành một vòng lặp chờ đợi.

**Trên Transaction A** - (Giữ Lock trên ProductID 1, cố lấy Lock trên ProductID 2):
```sql!
USE LockDemoDB;
GO

BEGIN TRANSACTION; -- Bắt đầu Transaction A

-- Bước 1: Transaction A cập nhật ProductID = 1 (đặt X Lock trên ProductID 1)
UPDATE Products
SET StockQuantity = StockQuantity - 5
WHERE ProductID = 1;

PRINT 'Transaction A: Updated ProductID 1. Waiting for ProductID 2...';

-- CHẠY DÒNG LỆNH NÀY. SAU ĐÓ NHANH CHÓNG CHUYỂN SANG CỬA SỔ 2 ĐỂ CHẠY CÁC LỆNH TƯƠNG ỨNG.
-- Cửa sổ 1 sẽ dừng lại ở đây.

```
```sql!
-- Bước 2: Transaction A cố gắng cập nhật ProductID = 2
-- Lệnh này sẽ yêu cầu X Lock trên ProductID = 2.
-- Nếu Transaction B đã giữ X Lock trên ProductID 2, Deadlock sẽ xảy ra.
UPDATE Products
SET StockQuantity = StockQuantity - 5
WHERE ProductID = 2;

-- Sau khi Deadlock xảy ra, một trong hai Transaction sẽ bị hủy bỏ (ROLLBACK tự động).
-- Lỗi xuất hiện "Deadlock victim" hoặc "Deadlock was detected"
```

**Transaction B - Giữ Lock trên ProductID 2, cố lấy Lock trên ProductID 1):**
```sql!
USE LockDemoDB;
GO

BEGIN TRANSACTION; -- Bắt đầu Transaction B

-- Bước 1: Transaction B cập nhật ProductID = 2 (đặt X Lock trên ProductID 2)
UPDATE Products
SET StockQuantity = StockQuantity - 5
WHERE ProductID = 2;

PRINT 'Transaction B: Updated ProductID 2. Waiting for ProductID 1...';
```
```sql!
-- Bước 2: Transaction B cố gắng cập nhật ProductID = 1
-- Lệnh này sẽ yêu cầu X Lock trên ProductID = 1.
-- Nếu Transaction A đã giữ X Lock trên ProductID 1, Deadlock sẽ xảy ra.
UPDATE Products
SET StockQuantity = StockQuantity - 5
WHERE ProductID = 1;
```

- Ngay khi chạy `bước 2` trên cửa sổ 1 sẽ xuất hiện lỗi Deadlock và bị Rollback ở một trong 2 cửa sổ(A/B). Cửa sổ còn lại sẽ thành công
![image](https://github.com/user-attachments/assets/d4a1f288-31d4-4aa7-9925-d3c034bf9d3e)


![image](https://github.com/user-attachments/assets/afc6554e-1f27-43f6-8350-924b73d9d775)


### Xử lý Blocking và Deadlock


