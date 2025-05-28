# FileZilla 

## 1. Giới thiệu chung về FileZilla
**FileZilla** là một phần mềm hỗ trợ truyền các dữ liệu trên máy tính cá nhân với máy chủ web thông qua việc kết nối internet bằng giao thức có tên là FTP. FileZilla gồm có hai thành phần là FileZilla Server và FileZilla Client.

### Các tính năng của FileZilla
- Giao diện người dùng đồ họa (GUI) trực quan, dễ sử dụng cho việc cấu hình và quản lý.
- Hỗ trợ FTP và FTPS (FTP Secure qua SSL/TLS) cho cả kênh điều khiển và kênh dữ liệu.
- Quản lý người dùng và nhóm với các quyền hạn truy cập chi tiết.
- Hỗ trợ giới hạn băng thông (bandwidth throttling) và số lượng kết nối tối đa.
- Lọc địa chỉ IP để chặn hoặc cho phép truy cập.
- Ghi nhật ký (logging) chi tiết các hoạt động.
### Ưu điểm:
- Dễ cài đặt và cấu hình: Giao diện GUI rất thân thiện với người dùng không chuyên về dòng lệnh.
- Miễn phí và mã nguồn mở: Tiết kiệm chi phí.
- Nhẹ và hiệu quả: Tiêu thụ ít tài nguyên hệ thống.
### Nhược điểm 
- Tính năng hạn chế: Phiên bản miễn phí có thể thiếu một số tính năng nâng cao hơn có trong bản Pro, chẳng hạn như quản lý tệp từ đám mây này sang đám mây khác.
- Lo ngại về bảo mật: Mặc dù phiên bản miễn phí có thể được bảo mật, nhưng nó có thể không cung cấp mức độ bảo mật tương tự như các tính năng nâng cao của bản Pro.

## 2. Cài đặt - Cấu hình 

### 2.1 Trên Windows
> - Yêu cầu hệ thống:
    - Cần có ít nhất 25 MB dung lượng trống trên ổ đĩa và quyền quản trị trên máy tính.
    - Hệ điều hành: Windows

**Bước 1: Cài đặt:**

-  Tải phần mềm FileZilla Server cho Windows. ([Tải Filezilla server](https://filezilla-project.org/download.php?type=server))
![image](https://github.com/user-attachments/assets/1ad0f83c-e172-4716-8046-13ba884cd310)


- Khi cài đặt kết thúc, chạy ứng dụng FileZilla đã cài đặt , nhấn Connect để bắt đầu thiết lập cấu hình cho FPT Server mới :
![image](https://github.com/user-attachments/assets/8b122219-341a-4054-bad8-1883520cad02)


**Bước 2 Cấu Hình:**
- Thiết lập người dùng, thư mục, quyền truy cập:
![image](https://github.com/user-attachments/assets/ec4f87d5-3257-4d17-a41f-eec38cc7b6d2)


![image](https://github.com/user-attachments/assets/28a47ab5-c6f1-4bf6-a314-5388c0939f1b)


> 1,2,3,4,5: Thiết lập User
> 6,8: Thiết lập thư mục và phân quyền

![image](https://github.com/user-attachments/assets/79ba5b20-2d4d-43bb-b5d1-799989be19b1)


**- Mở port FTP trong firewall**
    - Chạy lệnh `wf.msc` trong `run` để mở Windows Defender Firewall để cấu hình mở port `21`
    
![image](https://github.com/user-attachments/assets/7ceb6b20-6360-48e9-83f7-13474e6c1b34)

- Thêm FTP vào danh sách ngoại lệ của Firewall:
Trên giao diện **Windows Firewall with Advanced Security**, click chọn `Inbound Rules` ở cột bên trái rồi chọn `New Rule`… ở cột bên phải. Cọn `Program` >> Chọn đúng đường dẫn chưa file cài đặt filezila Server (ổ C\ProgramFil\ FileZilla Server\filezilla-server.exe).

![image](https://github.com/user-attachments/assets/6f8107ff-3392-4ac4-97f2-26b7c33ffaa5)


- Cấu hình giới hạn băng thông:
Vẫn ở Menu chính của FileZilla Server: chọn  `Server -> Configure -> Users(antv) -> Chọn tab Limits` để cấu hình giới hạn băng thông cho user
![image](https://github.com/user-attachments/assets/c7306757-a394-47ce-a879-0de30fedc283)


- Cấu hình ghi Nhật ký (log)
Server -> Configurations -> Logging

![image](https://github.com/user-attachments/assets/68132752-81fa-4ded-84f6-e6983ba7ae0d)


---
**Bước 3: Cài đặt Client**

![image](https://github.com/user-attachments/assets/aadef58b-a394-4f35-9765-0f498489e93c)

- Thử kết nối với user vừa tạo:

![image](https://github.com/user-attachments/assets/9ffa12e2-f9ea-4757-a41c-e8ec2792eb8d)



### 2.2 Trên Linux

#### Bước 1: Cài đặt:
- Truy cập tới trang [Download](https://filezilla-project.org/download.php?platform=linux64&type=server) của Filezilla để tải xuống file cài đặt
- 
![image](https://github.com/user-attachments/assets/03b56ad0-ce24-4745-8714-303758db8074)

- Truy cập vào thư mục chứa file cài đặt dạng `.deb` để tiến hành cài đặt:
```bash!
dpkg -i FileZilla_Server_1.10.3_x86_64-linux-gnu.deb
```
![image](https://github.com/user-attachments/assets/4df152d8-b569-429d-80e8-c52747c7d29e)


#### Bước 2: Cấu hình

- Cấu hình người dùng, thư mục, quyền truy cập:
    - Tạo user mới (antv):
```
sudo adduser antv
```
- 
    - Tạo thư mục: ` sudo mkdir /home/antv/ftpshare`
    - Set quyền: ` sudo chown nobody:nogroup /home/antv/ftpshare`
    - Xóa quyền ghi: ` sudo chmod a-w /home/antv`
    - Kiểm tra lại quyền: ` sudo ls -la /home/antv/ftpshare`
![image](https://github.com/user-attachments/assets/e1f7a0b4-5744-465a-9009-20aa8cb95d96)


- Tùy chỉnh cổng, giới hạn băng thông và cấu hình ghi nhật ký (log)
    - Cấu hình port tại `/opt/filezilla-server/etc/settings.xml`
      
    ![image](https://github.com/user-attachments/assets/3fcf9535-fc70-4fcf-a471-3d17cae4de15)

    - Để giới hạn băng thông chỉnh sửa các cấu hình tại `/opt/filezilla-server/etc/users.xml`

        ![image](https://github.com/user-attachments/assets/a30267e1-907b-4e95-871c-8d53bdfcf1ff)


> - Sửa  `<user name="&lt;system user>" enabled="false">` -> **"true"**
> - Các thuộc tính:
> 
>| Thuộc tính         | Ý nghĩa                                   |
>| ------------------ | ----------------------------------------- |
>| `inbound`          | Tổng tốc độ tải xuống (download) cho user |
>| `outbound`         | Tổng tốc độ tải lên (upload) cho user     |
>| `session_inbound`  | Giới hạn download mỗi session             |
>| `session_outbound` | Giới hạn upload mỗi session               |

- Các cấu hình liên quan tới log, rotate log tại phần logger `/opt/filezilla-server/etc/settings.xml`

    ![image](https://github.com/user-attachments/assets/23e030ef-8685-45a2-9df2-b59bc36ae1b0)



- Cấu hình xong khởi chạy lại FileZilla Server để áp dụng cấu hình
```
sudo systemctl restart filezilla-server
```



