
># Bảo Mật Database Server

# 1. Các mối đe dọa bảo mật phổ biến



## 1.1. Tấn công Injection (SQL Injection)
**Mô tả:** Đây là một trong những mối đe dọa phổ biến và nguy hiểm nhất đối với các ứng dụng tương tác với cơ sở dữ liệu. Kẻ tấn công chèn các đoạn mã SQL độc hại vào các trường nhập liệu của ứng dụng (ví dụ: tên người dùng, mật khẩu, trường tìm kiếm) mà không được kiểm tra hoặc làm sạch đúng cách. Khi ứng dụng thực thi truy vấn SQL với đoạn mã độc hại này, kẻ tấn công có thể thao túng cơ sở dữ liệu.

**Ví dụ về hậu quả:**
- Truy cập, đọc hoặc sửa đổi dữ liệu trái phép.
- Xóa toàn bộ bảng hoặc cơ sở dữ liệu.
- Lấy quyền truy cập vào hệ thống file của máy chủ (nếu có đủ quyền).
- Thực thi các lệnh hệ điều hành thông qua các stored procedure mở rộng.

**Điểm yếu khai thác:** Lỗ hổng trong mã ứng dụng không sử dụng tham số hóa truy vấn (parameterized queries) hoặc không validate/escape dữ liệu nhập vào.

## 1.2. Tấn công Brute-Force và Credential Stuffing
**Mô tả:**
- **Brute-Force:** Kẻ tấn công cố gắng đăng nhập vào SQL Server bằng cách thử tất cả các kết hợp mật khẩu có thể có cho một tên người dùng cụ thể (ví dụ: sa).
- **Credential Stuffing:** Kẻ tấn công sử dụng danh sách các tên người dùng và mật khẩu bị rò rỉ từ các vụ vi phạm dữ liệu khác để thử đăng nhập vào SQL Server.

**Ví dụ về hậu quả:**
- Truy cập trái phép vào SQL Server và dữ liệu.
- Kiểm soát toàn bộ máy chủ SQL Server nếu tài khoản sa bị xâm nhập.

**Điểm yếu khai thác:** Mật khẩu yếu, tài khoản mặc định không được bảo vệ, thiếu cơ chế khóa tài khoản sau nhiều lần thử đăng nhập thất bại.

## 1.3. Lỗ hổng cấu hình sai (Misconfiguration)
**Mô tả:** Các lỗ hổng do cấu hình SQL Server không đúng cách hoặc không tuân thủ các thực tiễn bảo mật tốt nhất.

**Ví dụ:**
- Sử dụng tài khoản có quyền cao nhất cho ứng dụng hoặc người dùng thông thường.
- Bật các tính năng không cần thiết như SQL Server Browser, xp_cmdshell, CLR Integration.
- Không giới hạn quyền truy cập mạng.
- Không mã hóa kết nối.
- Không quản lý kích thước tệp nhật ký giao dịch.

**Hậu quả:** Tăng bề mặt tấn công, dễ bị khai thác bởi các tấn công khác, mất dữ liệu, DoS.

## 1.4. Kỹ thuật xã hội (Social Engineering)
**Mô tả:** Nhằm lừa đảo nhân viên để họ tiết lộ thông tin nhạy cảm hoặc thực hiện các hành động không an toàn.

**Ví dụ về hậu quả:**
- Tiết lộ thông tin đăng nhập SQL Server.
- Cung cấp quyền truy cập từ xa vào máy chủ.
- Cài đặt phần mềm độc hại.

## 1.5. Truy cập nội bộ trái phép hoặc lạm dụng quyền (Insider Threats)
**Mô tả:** Mối đe dọa từ người có quyền truy cập hợp pháp nhưng lạm dụng quyền đó.

**Ví dụ về hậu quả:**
- Đánh cắp dữ liệu khách hàng.
- Xóa hoặc sửa đổi dữ liệu.
- Cài đặt phần mềm độc hại.

**Điểm yếu khai thác:** Thiếu nguyên tắc đặc quyền tối thiểu, giám sát không đầy đủ, không vô hiệu hóa tài khoản của nhân viên đã nghỉ.

## 1.6. Các bản vá lỗi và cập nhật bị bỏ lỡ (Unpatched Vulnerabilities)
**Mô tả:** Lỗi do không cập nhật các bản vá bảo mật kịp thời.

**Ví dụ về hậu quả:**
- Thực thi mã từ xa.
- Leo thang đặc quyền.
- Tấn công từ chối dịch vụ (DoS).

## 1.7. Đánh cắp hoặc mất thiết bị vật lý (Physical Theft/Loss)
**Mô tả:** Nếu máy chủ hoặc ổ đĩa chứa dữ liệu bị đánh cắp, dữ liệu có thể bị truy cập nếu không mã hóa.

**Ví dụ về hậu quả:**
- Rò rỉ dữ liệu nhạy cảm.
- Mất toàn bộ cơ sở dữ liệu.

**Điểm yếu khai thác:** Thiếu bảo mật vật lý, không mã hóa dữ liệu.

## 1.8. Tấn công từ chối dịch vụ (Denial of Service - DoS)
**Mô tả:** Làm cho SQL Server hoặc ứng dụng không thể truy cập được bằng cách làm quá tải tài nguyên.

**Ví dụ về hậu quả:**
- Ứng dụng ngừng hoạt động.
- Mất doanh thu.
- Thiệt hại về danh tiếng.

**Điểm yếu khai thác:** Cấu hình tài nguyên không đủ, lỗ hổng phần mềm hoặc hệ điều hành, cấu hình mạng yếu.

## 1.9. Mã độc và Ransomware
**Mô tả:** SQL Server có thể bị mã độc hoặc ransomware tấn công thông qua lỗ hổng hệ điều hành, phần mềm khác hoặc lừa đảo.

**Ví dụ về hậu quả:**
- Cơ sở dữ liệu bị mã hóa và không thể truy cập.
- Mất dữ liệu vĩnh viễn nếu không có bản sao lưu.

**Điểm yếu khai thác:** Thiếu phần mềm bảo mật, lỗ hổng hệ điều hành, thiếu phân đoạn mạng.

# 2. Mã Hóa Dữ Liệu (Encryption)
Mã hóa là quá trình chuyển đổi thông tin (plaintext) thành một dạng không thể đọc được (ciphertext) mà không có khóa giải mã. Trong SQL Server, mã hóa có thể được áp dụng ở nhiều cấp độ khác nhau, mỗi cấp độ bảo vệ chống lại các mối đe dọa cụ thể.

## 2.1. Các cấp độ mã hóa trong SQL Server

SQL Server cung cấp một số tùy chọn mã hóa chính:

### Mã hóa ở cấp độ cơ sở dữ liệu (Database-level Encryption)

**Transparent Data Encryption (TDE)**: Mã hóa toàn bộ tệp cơ sở dữ liệu và nhật ký giao dịch ở cấp độ lưu trữ (data at rest). Dữ liệu được mã hóa trước khi ghi vào đĩa và giải mã tự động khi đọc vào bộ nhớ.

- **Ưu điểm**: Dễ triển khai, không yêu cầu thay đổi ứng dụng, bảo vệ dữ liệu nếu các tệp cơ sở dữ liệu bị đánh cắp.
- **Nhược điểm**: Không bảo vệ dữ liệu khi nó được truy cập bởi người dùng hợp lệ (đã được xác thực), chỉ mã hóa tệp dữ liệu vật lý chứ không phải từng cột cụ thể.

### Mã hóa ở cấp độ cột (Column-level Encryption)

**Always Encrypted**: Một tính năng mạnh mẽ cho phép mã hóa dữ liệu nhạy cảm trong các cột cơ sở dữ liệu cụ thể, và dữ liệu được mã hóa/giải mã ở phía ứng dụng (client-side).

- **Ưu điểm**: Dữ liệu nhạy cảm được bảo vệ ngay cả khỏi các quản trị viên cơ sở dữ liệu (DBAs) hoặc những người có quyền truy cập vào máy chủ SQL Server. SQL Server chỉ xử lý dữ liệu đã mã hóa.
- **Nhược điểm**: Yêu cầu thay đổi ứng dụng để hỗ trợ mã hóa/giải mã, có thể có một chút chi phí hiệu suất do quá trình mã hóa/giải mã ở phía client.
- **Quan trọng**: Khóa mã hóa nằm ở phía ứng dụng, không nằm trên máy chủ SQL Server.

### Mã hóa kết nối (Connection Encryption)

**SSL/TLS Encryption**: Mã hóa luồng dữ liệu giữa ứng dụng client và SQL Server trong quá trình truyền tải (data in transit). Ngăn chặn việc nghe trộm dữ liệu qua mạng.

- **Ưu điểm**: Bảo vệ dữ liệu nhạy cảm trong khi nó di chuyển qua mạng.
- **Nhược điểm**: Chỉ bảo vệ dữ liệu khi truyền tải, không bảo vệ dữ liệu khi nó được lưu trữ hoặc khi nó đã ở trong bộ nhớ của SQL Server.

### Mã hóa ở cấp độ hệ điều hành (Operating System-level Encryption)

**BitLocker (Windows)**: Mã hóa toàn bộ ổ đĩa mà SQL Server đang chạy trên đó.

- **Ưu điểm**: Bảo vệ mạnh mẽ nếu toàn bộ máy chủ hoặc ổ đĩa bị đánh cắp.
- **Nhược điểm**: Không cụ thể cho SQL Server, có thể ảnh hưởng đến hiệu suất hệ thống tổng thể.

## 2.2. Transparent Data Encryption (TDE)

TDE mã hóa dữ liệu ở cấp độ tệp vật lý. Nó trong suốt đối với ứng dụng, nghĩa là ứng dụng không cần biết dữ liệu có đang được mã hóa hay không.

### Kiến trúc TDE

- **Database Encryption Key (DEK)**: Là một khóa đối xứng được sử dụng để mã hóa cơ sở dữ liệu. DEK được lưu trữ trong cơ sở dữ liệu được mã hóa.
- **Certificate (hoặc Asymmetric Key) được bảo vệ bởi Master Key**: DEK được mã hóa bằng một Certificate (hoặc một Asymmetric Key) được lưu trữ trong cơ sở dữ liệu master.
- **Service Master Key (SMK)**: Certificate (hoặc Asymmetric Key) được bảo vệ bởi Service Master Key. SMK được SQL Server tự động tạo và mã hóa bằng Data Protection API (DPAPI) của Windows.

## Cấu Hình Mã Hóa TDE (Transparent Data Encryption) 
>Transparent Data Encryption (TDE) chỉ hỗ trợ trong phiên bản: 
>- **SQL Server 2022 Developer Edition**
>- **SQL Server 2022 Enterprise Edition**


**Bước 1: Tạo Database Master Key (DMK) trong cơ sở dữ liệu master (nếu chưa có)**
>DMK là khóa mã hóa đầu tiên trong chuỗi khóa bảo mật của SQL Server.
```sql!
USE master;
GO
CREATE MASTER KEY ENCRYPTION BY PASSWORD = N'Your_Strong_Password_For_DMK!';
GO
-- Nên sao lưu DMK này và cất giữ ở nơi an toàn.
BACKUP MASTER KEY TO FILE = 'D:\SQLBackup\MasterKey.bak'
ENCRYPTION BY PASSWORD = N'Another_Strong_Password_For_Backup_DMK!';
GO
```

**Bước 2: Tạo hoặc sử dụng Certificate trong cơ sở dữ liệu master**
>Certificate này sẽ được sử dụng để mã hóa Database Encryption Key (DEK).

```sql!
USE master;
GO
CREATE CERTIFICATE TDE_Cert_YourDatabase
WITH SUBJECT = 'TDE Certificate for YourDatabaseName';
GO

-- Sao lưu Certificate này và khóa riêng của nó. RẤT QUAN TRỌNG cho Disaster Recovery!
BACKUP CERTIFICATE TDE_Cert_YourDatabase
TO FILE = 'D:\SQLBackup\TDE_Cert_YourDatabase.cer'
WITH PRIVATE KEY (
    FILE = 'D:\SQLBackup\TDE_Cert_YourDatabase_PrivateKey.pvk',
    ENCRYPTION BY PASSWORD = N'Yet_Another_Strong_Password_For_Cert_Key!'
);
GO
```

**Bước 3: Tạo Database Encryption Key (DEK) trong cơ sở dữ liệu muốn mã hóa**
> DEK là khóa đối xứng sẽ mã hóa dữ liệu thực tế.

```sql!
USE YourDatabaseName; -- Thay thế bằng tên cơ sở dữ liệu your
GO
CREATE DATABASE ENCRYPTION KEY
WITH ALGORITHM = AES_256 -- Chọn thuật toán mã hóa (AES_128, AES_192, AES_256)
ENCRYPTION BY SERVER CERTIFICATE TDE_Cert_YourDatabase;
GO
```

**Bước 4: Bật mã hóa cho cơ sở dữ liệu**
```sql!
USE master;
GO
ALTER DATABASE YourDatabaseName
SET ENCRYPTION ON;
GO
```
- Kiểm tra trạng thái TDE
    ```sql!
    SELECT
        db.name,
        db.is_encrypted,
        dm.encryption_state,
        dm.percent_complete,
        dm.key_algorithm,
        dm.key_length
    FROM sys.databases db
    LEFT JOIN sys.dm_database_encryption_keys dm
    ON db.database_id = dm.database_id
    WHERE db.name = 'YourDatabaseName';
    GO
    ```
- `is_encrypted = 1`: Cơ sở dữ liệu đã được bật TDE.
- `encryption_state = 3`: Cơ sở dữ liệu đã được mã hóa hoàn toàn. (0: không mã hóa, 1: đang mã hóa, 2: đang giải mã, 3: đã mã hóa, 4: khóa không có sẵn)

- Tắt TDE để giải mã
```sql!
USE master;
GO
ALTER DATABASE YourDatabaseName
SET ENCRYPTION OFF;
GO
```
## 2.3. Mã hóa từng cột dữ liệu - CLE (Column-Level Encryption)

- Tạo Database mới:
```sql!
CREATE DATABASE Demo_CLE;
GO

-- 2. Chuyển vào database này
USE Demo_CLE;
GO

-- 3. Tạo bảng NhanVien với cột CMND (kiểu varbinary để chứa dữ liệu mã hóa)
CREATE TABLE NhanVien (
    ID INT IDENTITY PRIMARY KEY,
    HoTen NVARCHAR(100),
    CMND VARBINARY(256) -- Lưu dữ liệu mã hóa
);
GO

-- 4. Tạo Master Key trong database này
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'StrongKey@123';
GO

-- 5. Tạo Certificate
CREATE CERTIFICATE NhanVienCert
WITH SUBJECT = 'Encrypt CMND column';
GO

-- 6. Tạo Symmetric Key - Khóa đối xứng
CREATE SYMMETRIC KEY CMND_Key
WITH ALGORITHM = AES_256
ENCRYPTION BY CERTIFICATE NhanVienCert;
GO
```
- Chèn dữ liệu đã mã hóa:
```sql!
-- Mở khóa đối xứng
OPEN SYMMETRIC KEY CMND_Key
DECRYPTION BY CERTIFICATE NhanVienCert;
GO

-- Chèn dữ liệu đã mã hóa vào bảng
INSERT INTO NhanVien (HoTen, CMND)
VALUES 
    ('Nguyen Van A', EncryptByKey(Key_GUID('CMND_Key'), '123456789')),
    ('Tran Thi B', EncryptByKey(Key_GUID('CMND_Key'), '987654321'));
GO

-- Đóng khóa
CLOSE SYMMETRIC KEY CMND_Key;
GO
```

-  Truy vấn dữ liệu đã giải mã:
```sql!
-- Mở khóa
OPEN SYMMETRIC KEY CMND_Key
DECRYPTION BY CERTIFICATE NhanVienCert;
GO

-- Truy vấn dữ liệu giải mã
SELECT 
    ID,
    HoTen,
    CONVERT(VARCHAR, DecryptByKey(CMND)) AS CMND
FROM NhanVien;
GO

-- Đóng khóa
CLOSE SYMMETRIC KEY CMND_Key
```
### Thử đăng nhập bằng user khác để xem dữ liệu đã được mã hóa:
- Dùng `user1` thử đọc dữ liệu mã hóa:
```sql!
USE Demo_CLE;
SELECT HoTen, CMND
FROM dbo.NhanVien;
```
![image](https://github.com/user-attachments/assets/f2d854c8-67ec-4774-9c4b-11feee728ea6)

>Cột CMND đã được mã hóa 

- Sau đó thử giải mã:
```sql!
USE Demo_CLE;

SELECT 
    ID,
    HoTen,
    CONVERT(VARCHAR, DecryptByKey(CMND)) AS CMND
FROM NhanVien;

CLOSE SYMMETRIC KEY CMND_Key;

```

![image](https://github.com/user-attachments/assets/bc1d0c46-7e31-4cb7-847b-b2eef358a39f)
>  Lúc này user1 sẽ KHÔNG giải mã được, trả về NULL.


- Cấp quyền CONTROL trên certificate và key cho `user1` - (Thực hiện trên tab `master`)
```sql!
USE Demo_CLE;
GO

GRANT CONTROL ON CERTIFICATE::NhanVienCert TO user1;
GRANT VIEW DEFINITION ON SYMMETRIC KEY::CMND_Key TO user1;
GRANT CONTROL ON SYMMETRIC KEY::CMND_Key TO user1;
```
- Quay lại trên tab đăng nhập bằng `user1` → thử lại câu truy vấn giải mã:
```sql!
USE Demo_CLE;

OPEN SYMMETRIC KEY CMND_Key  
DECRYPTION BY CERTIFICATE NhanVienCert;

SELECT 
    ID,
    HoTen,
    CONVERT(VARCHAR, DecryptByKey(CMND)) AS CMND
FROM NhanVien;

CLOSE SYMMETRIC KEY CMND_Key;
```
![image](https://github.com/user-attachments/assets/2e6b73ec-b1bb-4630-bd63-32c6c1c4fcdc)


## 2.4. Mã hóa kết nối (SSL/TLS Encryption) 

# 3. Audit và theo dõi truy cập
**Kiểm toán (Auditing)** là quá trình ghi lại các sự kiện diễn ra trên SQL Server. Mục tiêu chính của việc kiểm toán là:

- **Tuân thủ quy định**: Đáp ứng các yêu cầu về bảo mật và tuân thủ của các tiêu chuẩn như GDPR, HIPAA, PCI DSS, SOX, v.v.
- **Phát hiện hành vi đáng ngờ**: Nhận diện các hoạt động truy cập hoặc sửa đổi dữ liệu trái phép, cố gắng leo thang đặc quyền, hoặc các hành vi độc hại khác.
- **Điều tra sự cố**: Cung cấp thông tin chi tiết để điều tra các vụ vi phạm bảo mật hoặc các vấn đề liên quan đến dữ liệu.
- **Giám sát hoạt động người dùng**: Theo dõi hành vi của người dùng và ứng dụng để đảm bảo họ tuân thủ các chính sách bảo mật.

**Các cơ chế theo dõi trong SQL Server**
| Cơ chế | Mô tả   |
| -------- | ------- |
| **SQL Server Audit**       | Ghi log chi tiết truy cập vào DB, table, user actions... |
| **SQL Server Profiler**    | Ghi trace real-time các truy vấn (để debug)              |
| **Extended Events (XE)**   | Hệ thống log chi tiết hơn, thay thế Profiler             |
| **CDC / CT (Change Data)** | Ghi lại thay đổi dữ liệu                                 |
| **Trigger Audit**          | Tạo trigger để log INSERT/UPDATE/DELETE                  |

### Ví dụ:
#### Audit hành vi SELECT và DELETE trên bảng `NhanVien`

- Bật tính năng Audit:

```sql!
USE master;

CREATE SERVER AUDIT Audit_NhanVien
TO FILE (
    FILEPATH = 'C:\SQLAuditLogs\', --tạp thư mục này trước
    MAXSIZE = 10 MB,
    MAX_FILES = 50,
    RESERVE_DISK_SPACE = OFF
)
WITH (ON_FAILURE = CONTINUE);

ALTER SERVER AUDIT Audit_NhanVien
WITH (STATE = ON);
```
> Lệnh này tạo 1 audit ghi log vài file tại C\AuditLogs

- Tạo Database Audit Specification
```sql!
USE Demo_CLE;

CREATE DATABASE AUDIT SPECIFICATION AuditSpec_NhanVien
FOR SERVER AUDIT Audit_NhanVien
ADD (SELECT ON dbo.NhanVien BY PUBLIC),
ADD (DELETE ON dbo.NhanVien BY PUBLIC)
WITH (STATE = ON);
GO
```
>`BY PUBLIC` nghĩa là ghi lại hành động của mọi người dùng
>Có thể thay bằng `BY [tên_user]` nếu muốn theo dõi từng  cụ thể

- Kiểm tra lịch sử đăng nhập:
```sql!
SELECT login_time, host_name, program_name, login_name FROM sys.dm_exec_sessions;
```
![image](https://github.com/user-attachments/assets/f67c8ce0-11fb-4f56-ab04-1ef6ce3442eb)

- Theo dõi những lần đăng nhập thất bại:
```sql!
SELECT * FROM sys.dm_exec_sessions WHERE is_user_process = 0;
```
![image](https://github.com/user-attachments/assets/ac68f92a-13a4-412d-b17c-56df0502efe0)

#### Sử dụng Trigger để theo dõi thay đổi dữ liệu
> Trigger giúp ghi lại các thay đổi trên bảng dữ liệu

- Tạo Trigger theo dõi INSERT< UPDATE< DELETE
```sql!
CREATE TABLE Audit_Log (
    EventTime DATETIME DEFAULT GETDATE(),
    UserName NVARCHAR(100),
    EventType NVARCHAR(50),
    SQLCommand NVARCHAR(2000)
);
```
```sql!
CREATE TRIGGER trg_Audit
ON NhanVien
AFTER INSERT, UPDATE, DELETE
AS
BEGIN
    DECLARE @EventType NVARCHAR(50) = CASE 
        WHEN EXISTS (SELECT * FROM inserted) AND EXISTS (SELECT * FROM deleted) THEN 'UPDATE'
        WHEN EXISTS (SELECT * FROM inserted) THEN 'INSERT'
        WHEN EXISTS (SELECT * FROM deleted) THEN 'DELETE'
    END;

    INSERT INTO Audit_Log (UserName, EventType, SQLCommand)
    VALUES (SUSER_NAME(), @EventType, EVENTDATA().value('(/EVENT_INSTANCE/TSQLCommand)[1]', 'NVARCHAR(2000)'));
END;
```
> Lệnh này giúp ghi lại các thao tác INSERT, UPDATE, DELETE trên bảng `NhanVien`

- Kiểm tra và dữ liệu đã ghi:
```sql!
SELECT * FROM sys.fn_get_audit_file('C:\SQLAuditLog\*.sqlaudit', DEFAULT, DEFAULT);
```
- Xem lại thay đổi dữ liệu:
```sql!
SELECT * FROM Audit_Log ORDER BY EventTime DESC;
```

---
># .


