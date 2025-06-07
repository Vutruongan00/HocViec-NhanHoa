
# Cài Đặt và Cấu Hình Microsoft SQL Server
## 1. Yêu cầu hệ thống
### Trên Windows

- **Hệ điều hành**:
    - **Desktop**: Windows 10/11 (Pro/Enterprise) hoặc các phiên bản Windows 8.1/7 SP1 cũ hơn (tùy SQL Server version).
    - **Server**: Windows Server 2016 trở lên (khuyến nghị 2019/2022).
    - Lưu ý: Phải là hệ điều hành 64-bit.
- **CPU**: Bộ xử lý x64, 1.4 GHz trở lên.
- **RAM**: Tối thiểu 1 GB (512 MB cho bản Express). Khuyến nghị 4 GB trở lên cho môi trường thực tế.
- **Disk**: Tối thiểu 6 GB dung lượng trống.
- **.NET Framework**: Phiên bản phù hợp (ví dụ: 4.6.2 trở lên cho SQL Server 2019/2022). Trình cài đặt thường tự động bổ sung.

### Trên Linux
- **Hệ điều hành**:
    - Ubuntu (20.04 LTS, 22.04 LTS)
    - Red Hat Enterprise Linux (RHEL 8, 9)
    - SUSE Linux Enterprise Server (SLES 12 SP5, 15 SP4)
    - Lưu ý: Phải là hệ điều hành 64-bit.
- CPU: Bộ xử lý x64, 2 lõi trở lên.
- RAM: Tối thiểu 2 GB. Khuyến nghị 4 GB trở lên.
- Ổ đĩa: Tối thiểu 6 GB dung lượng trống.
- Hệ thống tệp: XFS hoặc EXT4.

## 2. Cài đặt
### 2.1 Trên Windows
- Truy cập trang chủ để tải: Chọn phiên bản phù hợp (Express)

![image](https://github.com/user-attachments/assets/db0f140c-dd75-45ef-be2b-144e0eb7b8fc)


- Mở file vừa Download:

![image](https://github.com/user-attachments/assets/b8de46d2-d581-4283-9d29-8a40acfe9fb9)


- Tải file cài đặt: 
![image](https://github.com/user-attachments/assets/891c79ef-d845-4484-8f78-4ca1a37f75cc)


- Sau khi Download thành công bạn có thể chọn chế độ Customize để tùy chỉnh:

![image](https://github.com/user-attachments/assets/5d1fe83f-81e3-43dd-bea7-d43956c41ca0)


-  Click Next . . .
- Chọn **Perform a new installation of SQL Server 2022** --> Next:

![image](https://github.com/user-attachments/assets/8ac5258f-ca2f-4421-b9bc-d69883b69397)


- Chọn **Default instance** --> Next:

![image](https://github.com/user-attachments/assets/ca9234f9-3672-4519-bc44-dd0606c92091)

- Tiếp tục Next. . .
- Điền thông tin passwd:
![image](https://github.com/user-attachments/assets/372272ad-2f4b-4d4a-b9f2-053561cc03ab)

- Cài đặt thành công --> **Close**
![image](https://github.com/user-attachments/assets/81f36b5a-c812-412e-b528-8f202a2d03a6)


- Cài đặt **SQL Server Management Studio (SSMS):**
![image](https://github.com/user-attachments/assets/8f66757e-70ff-42c4-8026-0933b8b05a2a)


![image](https://github.com/user-attachments/assets/b7ce6d9c-2f77-4313-8fbd-791f513eb423)


- Mở file vừa download về và cài đặt

![image](https://github.com/user-attachments/assets/9f620bb4-1e78-45e9-b899-bafb369eb49a)

- Done --> Mở giao diện launch

![image](https://github.com/user-attachments/assets/b44f2aab-f33b-4585-9dc1-821811d18906)


- Thử kết nối 

![image](https://github.com/user-attachments/assets/b9f2e167-8562-47a8-a6fa-4a552c1242ed)



## 3. Cầu hình tối ưu hiệu năng

- **Cấu hình RAM tối đa sử dụng**: 
>Tùy vào cấu hình phần cứng của máy chủ mà có thể set 50%-80% dung lượng RAM

![image](https://github.com/user-attachments/assets/37ec696c-45a9-4138-a4e4-a9abec688597)


![image](https://github.com/user-attachments/assets/fde6f138-b5b2-4be3-8688-fbd893b3e791)


- hoặc cấu hình thông qua query:

```sql!
EXEC sp_configure 'min server memory', 512; -- Đặt bộ nhớ tối thiểu là 512MB
EXEC sp_configure 'max server memory', 1536; -- Đặt bộ nhớ tối đa là 1.5GB
RECONFIGURE;
```

![image](https://github.com/user-attachments/assets/260faa02-bdf6-434c-8515-58f30c061c40)


- **Tối ưu hóa TempDB:**
```sql!
ALTER DATABASE tempdb MODIFY FILE (NAME = 'tempdev', SIZE = 512MB, FILEGROWTH = 64MB);
ALTER DATABASE tempdb MODIFY FILE (NAME = 'templog', SIZE = 256MB, FILEGROWTH = 32MB);
```
> - Đặt kích thước ban đầu của file dữ liệu chính `tempdev` =512MB, khi đầy, file sẽ tự động mở rộng thêm 64MB mỗi lần
> - lệnh sau tương tự với file `templog`
> --> Giúp tránh tình trạng phân mảnh dữ liệu, tối ưu hiệu suất của `tempdb`

- **Giới hạn số lượng kết nối**
```sql!
EXEC sp_configure 'user connections', 50;
RECONFIGURE;
```

# 4. Cấu hình bảo mật 

- Bật chế độ kiểm tra đăng nhập thất bại:

```sql!
EXEC sp_configure 'user connections', 50;
RECONFIGURE;
```
- Vô hiệu hóa các tính năng không cần thiết:
> Tắt xp_cmdshell giúp giảm nguy cơ bị tấn công thông qua lệnh hệ thống.

```sql!
EXEC sp_configure 'audit level', 2;
RECONFIGURE;
```
- **Cấu hình tính toán song song (Parallelism)**
    - **MAXDOP** là cấu hình cho phép SQLServer sử dụng bao nhiêu Processor của CPU để thực hiện các query (plan execution)
        - Mặc định SQLServer để MAXDOP = 0 là cho phép dùng tất cả các processor có thể của máy chủ và max = 64.

    - **Cost threshold for parallelism**:  là giá trị cấu hình ngưỡng chi phí của 1 query mà ở đó SQLServer bắt đầu xem xét thực thi query đó bằng multiple thread.
        - Thông thường các query đơn giản có cost < 50
        - Trong khi đó default của SQLServer là 5, quá bé, ta set ngưỡng này là 50 (tức là dưới 50 thì không cần chạy song song nhiều thread)

![image](https://github.com/user-attachments/assets/42bd8ac2-9c46-4a21-8741-efb8349e7f47)

