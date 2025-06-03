
# Cài Đặt và Cấu Hình MySQL/MariaDB

## 1. Yêu cầu hệ thống phần cứng
- Tối thiểu (Dev/Test)
    - **CPU**: 1-2 core (x64)
    - **RAM**: 2-4 GB
    - **Disk**: 10-20 GB trống

-**Hệ điều hành**: Windows Server 2016+, Ubuntu 20.04+, CentOS 7+/8.

## 2. Cài đặt 

### 2.1 Trên WINDOWS

#### a. MySQL Server
---

- **Tải xuống MySQL Install (MSI)**
    - Truy cập trang tải xuống của MySQL: https://dev.mysql.com/downloads/mysql/
    - Chọn phiên bản **MSI Installer**
    ![image](https://github.com/user-attachments/assets/b04f36ff-270a-406d-905d-b636b9b7f3ec)


- Chạy tệp cài đặt  MSI...
    - Tại các bảng cài đặt, chỉnh theo ý muốn hoặc giữ nguyên như cài đặt chuẩn của nhà sản xuất và chọn Next. 
   ![image](https://github.com/user-attachments/assets/ea01ffc1-1203-45d5-858f-7dc92d1fdeba)


    - Đặt mật khẩu và xác nhận lại mật khẩu của bạn, tiếp đến chọn Add user để thêm tài khoản sử dụng.
    ![image](https://github.com/user-attachments/assets/3953ff93-9048-420b-a80f-8cb70f8b93dc)

    - Đặt tên tài khoản và nhập mật khẩu. Kế tiếp chọn Next
    ![image](https://github.com/user-attachments/assets/53f4895c-9ad7-463e-b072-8b978d302962)


--> Sau khi quá trình cài đặt hoàn tất 

- Mở MySQL Command Line Client hoặc MySQL Shell nhập mật khẩu vừa tạo để kiểm tra cài đặt: 

    ![image](https://github.com/user-attachments/assets/47e2208c-c8e1-40e6-bcc8-a42045cfe067)


#### b. MariaDB

- Truy cập trang web chính thức: [mariadb.com/downloads/](https://mariadb.org/download/.). Tải xuống MariaDB MSI package.

![image](https://github.com/user-attachments/assets/ae99a857-b12a-4390-b89f-59da6f23f5cd)



### 2.2 Linux (Ubuntu - dùng MySQL/ CentOS - dùng MariaDB)
---

#### a. Cài đặt MySQL (Ubuntu):

- Cập nhật hệ thống
```bash!
sudo apt update
sudo apt upgrade -y
```
- Cài đặt MySQL Server:
```bash!
sudo apt install mysql-server -y
```

- Kiểm tra lại trạng thái dịch vụ:
```bash!
sudo systemctl status mysql
```

![image](https://github.com/user-attachments/assets/1ac0d9dd-11ed-43e9-b280-f1b650458b1b)


---
#### b. Cài đặt MariaDB (CentOS)

- Cài đặt MariaDB Server:
```
sudo yum install mariadb-server mariadb -y
```
- Khởi động và bật dịch vụ:

```bash!
sudo systemctl start mariadb
sudo systemctl enable mariadb 
```
![image](https://github.com/user-attachments/assets/1b6ba6c3-d17e-453e-8a94-d2bf2aa74d0b)


## 3. Cấu hình tối ưu hiệu năng

- File cấu hình:
    - **MySQL**: `/etc/mysql/my.cnf`
    - **MariaDB**: `/etc/my.cnf`
#### Linux:
- Thêm vào trong file cấu hình:

```ini!
[mysqld]
max_connections         = 200
tmp_table_size          = 64M
max_heap_table_size     = 64M
innodb_buffer_pool_size = 512M
innodb_log_file_size    = 128M
innodb_flush_log_at_trx_commit = 2
slow_query_log          = 1
slow_query_log_file     = /var/log/mysql/slow.log
```

> Các tham số:
> - `max_connections`: số lượng kết nối đồng thời (Mặc định 151)
> - `tmp_table_size` : Tăng kích thước tối đa cho bảng tạm (trong RAM). Nếu vượt quá, MySQL sẽ ghi ra đĩa → chậm.
> - `max_heap_table_size`: Giới hạn kích thước tối đa của bảng kiểu MEMORY/HEAP. Nên bằng với tmp_table_size.
> - `innodb_buffer_pool_size`: Đây là bộ đệm quan trọng nhất, lưu trữ dữ liệu và index trong RAM. Đặt giá trị khoảng 50-80% tổng RAM khả dụng nếu server chỉ dành riêng cho database.
> - `slow_query_log`: giúp phát hiện truy vấn chậm để tối ưu
> - `innodb_log_file_size`: Kích thước mỗi file log InnoDB. Tác động đến hiệu suất ghi và phục hồi.
> - `innodb_flush_log_at_trx_commit`: Hiệu suất ghi log:
1 = an toàn (ghi mỗi transaction);
2 = nhanh hơn (ghi ra RAM, flush định kỳ).
>...

#### Windows

## Cấu hình bảo mật cơ bản
- Chạy câu lệnh sau:
```bash!
sudo mysql_secure_installation
```
>- Khi được hỏi, chọn tùy chọn để sử dụng phương thức xác thực auth_socket hoặc caching_sha2_password (khuyên dùng caching_sha2_password cho tính tương thích).
>- Đặt mật khẩu mạnh cho người dùng `root`: 
>- Trả lời `Y` (Yes) cho các câu hỏi:
>    - Remove anonymous users? (Xóa người dùng ẩn danh)
>    - Disallow root login remotely? (Cấm đăng nhập root từ xa - RẤT QUAN TRỌNG CHO BẢO MẬT)
>    - Remove test database and access to it? (Xóa database test)
>    - Reload privilege tables now? (Tải lại bảng quyền)

- Tạo user riêng theo nguyên tắc đặc quyền tối thiểu:
**Ví dụ:** Tạo user chỉ cho phép truy cập từ địa chỉ IP cố định:
```sql!
CREATE USER 'readonly'@'192.168.1.100' IDENTIFIED BY 'StrongPass!';
GRANT SELECT ON mydb.* TO 'readonly'@'192.168.1.100';
```

- Tường lửa:
    - Chỉ cho phép truy cập cổng 3306 từ IP cụ thể:
```bash!
sudo ufw allow from [your_IP_add] to any port 3306
```


