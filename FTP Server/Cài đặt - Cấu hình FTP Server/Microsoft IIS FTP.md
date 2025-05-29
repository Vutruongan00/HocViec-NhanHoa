# Microsoft IIS FTP

>Yêu cầu hệ thống:
>- Phần cứng chạy Windows Server: 2012 trở lên
>    - CPU: Tối thiểu 1.4 GHz (64-bit)
>    - RAM: Tối thiểu 512 MB (recoment 2 GB+)
>   - Ổ cứng: Tối thiểu 32 GB

## Cài đặt
- Mở “**Server Manager**” và chọn “**Add roles and features**”

![image](https://github.com/user-attachments/assets/e6b60018-53b3-4151-9d35-324132d83f44)


- Tích chọn Web Server IIS,rồi cứ nhấn Next tiếp thôi...
![image](https://github.com/user-attachments/assets/84071594-e63c-48e0-a477-dab1f1b6bd39)


- Tiếp đến mục Role Services, tích chọn **FTP Server** --> Sau đó cứ Next rồi nhấn Install thôi:

[![image](https://hackmd.io/_uploads/B1x4JBBMgx.png)](https://hackmd.io/_uploads/B1x4JBBMgx.png)

 --> Sau đó cứ Next rồi nhấn Install thôi:
![image](https://github.com/user-attachments/assets/3c96d0ee-c69f-4fd1-b902-e0f066cc16de)



 ### Tạo FTP site trên FTP server
 
 - Mở Internet Information Services (IIS) Manager
 - Nhấp chuột phải vào “**Sites**” và chọn “**Add FTP Site**”

![image](https://github.com/user-attachments/assets/88db20d0-5c5b-49ce-9025-da95db9b5a29)


- Đặt tên cho FTP site và trỏ đường dẫn đến thư mục chỉ định làm FTP site đó:

![image](https://github.com/user-attachments/assets/fc9fc92a-9a94-4dc7-bd2a-38ae85d9aac9)




- Gán tên miền nếu có(tích chọn Enable Virtual Host name). Nếu có SSL cert thì import còn không thì chọn No SSL

![image](https://github.com/user-attachments/assets/0e5de57e-ead5-44c6-95d2-1566135ed3d7)


- Đến mục **Authentication and Authorization Information**. Chọn **Specified roles or user groups** và đặt tên nhóm người dùng được phép sử dụng FTP server, gán quyền cho nhóm người dùng này, ví dụ cho phép cả đọc và ghi thì tích vào **Read + Write**

![image](https://github.com/user-attachments/assets/b193daf8-2e46-41fa-9498-847ab410fee6)


## Cấu hình
### Thiết lập người dùng, thư mục, quyền truy cập.

#### Tạo nhóm người dùng

- Mở **Computer Management**: chọn **Local Users and Group** từ menu bên trái, chọn **Groups**. Click chuột phải chọn **New Group**

![image](https://github.com/user-attachments/assets/4e370a09-100a-4f94-8335-7bddaaaf1d24)

- Create -->OK
![image](https://github.com/user-attachments/assets/801227bb-e503-471d-bbb9-22bd8692625b)


#### Tạo Users
- Tương tự tạo group, click chuột phải vào Users chọn New User:
- Đặt tên và mật khẩu() cho user

![image](https://github.com/user-attachments/assets/241c0288-6612-4a2e-af82-dbe3d549ee85)


- Sau khi tạo User, tiến hành thêm user vào group:
![image](https://github.com/user-attachments/assets/6fadb94d-2246-48c0-9a4a-5f30491c3f94)


- Nhập tên user và click vào Check Names-->Ok
  
![image](https://github.com/user-attachments/assets/1b8160a2-370a-4c18-b63f-ed6ebf0ffa41)


#### Cách ly, phân vùng người dùng
Để mỗi người dùng có được thư mục riêng của mình và không có quyền truy cập vào các tệp khác sau khi kết nối với máy chủ, cần phải thiết lập Isolation.

Chọn FTP site cần cấu hình trong IIS, chọn **FTP User Isolation**

![image](https://github.com/user-attachments/assets/bd9c61f2-973a-4c13-9092-755921c9bb84)


- Chọn User name directory và Apply

![image](https://github.com/user-attachments/assets/8c8e31bf-5518-40e3-9352-d64c7ff87154)


- Tiến hành phân quyền cho nhóm người dùng chung vào FTP site chính

![image](https://github.com/user-attachments/assets/a53f28c3-28c7-4879-a2e2-270da6e3c224)

- Chuyển sang tab Security,chon Edit
![image](https://github.com/user-attachments/assets/f376aa91-6824-4491-8b7a-15ecd580e6d9)


- Chọn Add để thêm nhóm người dùng chung FTP site

![image](https://github.com/user-attachments/assets/438c9dc7-f880-4370-8ae2-da06fcd0dd71)


- Nhập tên nhóm `FTP group` đã tạo vào click Check Names

![image](https://github.com/user-attachments/assets/d4c83ca0-d426-4368-a4fd-af6038e6eb45)

--> Sau đó chọn Apply để áp dụng thay đổi. Chọn OK để hoàn tất

- Tiếp tục nhấn chuột phải lên FTP site, chọn **Add Virtual Directory**

![image](https://github.com/user-attachments/assets/e9c388d6-d069-45b3-a92c-d35e65b418a8)


- Mục `Alias`, đặt tên theo biệt danh hoặc tên người dùng. Phía dưới mục đường dẫn, ta trỏ về thư mục dành riêng cho user này (Thư mục phải là thư mục con, nằm trong thư mục chính cấu hình để chạy FTP site). Ví dụ folder rành diêng cho `file_user1`

![image](https://github.com/user-attachments/assets/36d52433-01f3-4095-b834-70c7a8174651)



- Cài đặt **quyền truy cập** cho đường dẫn ảo này. Tại site của bạn và chọn **Edit Permission** cho thư mục của `user1`, chuột phải chọn **Edit Permissions**.
 
![image](https://github.com/user-attachments/assets/3fcc30db-3db0-448c-b28e-bd7c8e09d107)


- Sang tab Security, chọn Advanced

![image](https://github.com/user-attachments/assets/902e68de-a3a5-4c65-bf58-4ff54d8956d1)

- Ở bảng Advanced Security Setting, chọn  **Disable Inheritance** để vô hiệu tính kế thừa phân quyền từ thư mục mẹ bên ngoài

![image](https://github.com/user-attachments/assets/4cdd8189-a40f-43ed-8a50-2e8f9290f75d)

![image](https://github.com/user-attachments/assets/82e843a9-93f5-4428-9756-0cdc4787fe8f)

- Trở lại tab Security, chọn **Edit**

- Xóa những Users, Group không liên quan, để đảm bảo chỉ chủ sở hữu thư mục này mới có quyền truy cập và chỉnh sửa.
- Sau đó tiến hành thêm người dùng `user1` được sở hữu thư mục này

- Gán quyền cho user sử dụng thư mục. Ví dụ, ta cấp toàn quyền cho user01 sử dụng folder tên user01

![image](https://github.com/user-attachments/assets/4fd22c62-34c7-4d0b-8ca6-69a73fdba960)


--> chọn Apply -> OK

- **Tương tự với các user khác cũng vậy**

### Tùy chỉnh cổng, giới hạn băng thông, và nhật ký (log)

#### Thiết lập Rule trong Firewall cho phép dịch vụ FTP đi qua

- Mở **Windows Firewall with Advanced Security**
(chạy `wd.msc` trong RUN hoặc thanh tìm kiếm)

- Chọn Inbound Rules, rồi chọn New Rule ở menu bên phải

![image](https://github.com/user-attachments/assets/3a0820cb-50ce-4c44-8901-202113f3e0ae)


- Chọn **Predefined**, chọn **FTP Server** và nhấn **Next**

![image](https://github.com/user-attachments/assets/af4fbeb2-ac6d-4ffa-ae4b-f0b39df9d630)


![image](https://github.com/user-attachments/assets/bee47579-d0f1-445a-95f6-b9fdb1c6ce3b)


- Chọn **Allow the connection** --> **Finish**

![image](https://github.com/user-attachments/assets/125479d5-2227-49f1-b7a2-cbee48230ae8)



### Thiết lập chế độ Passive Mode

Mở IIS Manager → Chọn server chính (tên máy chủ ở bên trái) →  Bấm vào "**FTP Firewall Support**"

![image](https://github.com/user-attachments/assets/9a327640-c5cc-4a07-8f66-0b5193f53c16)



>- Data Channel Port Range: Ví dụ: 5501-5600
>- External IP Address:

![image](https://github.com/user-attachments/assets/9bf863de-b2eb-4908-bf5b-7665aa9c99e3)


- Mở Firewall cho dải cổng Passive:

Chạy lệnh PowerShell hoặc Command Prompt dưới quyền admin:
```powershell!
netsh advfirewall firewall add rule name="FTP Passive Ports" dir=in action=allow protocol=TCP localport=5501-5600
```

### RESTART lại dịch vụ FTP 
 - Chạy `Services.msc` trong run hoặc khung tìm kiếm
   
![image](https://github.com/user-attachments/assets/238fae8b-8704-426e-ae15-689bdc25a341)

- Tìm dịch vụ **Microsoft FTP Service** --> Chuột phải → Restart
![image](https://github.com/user-attachments/assets/f1947887-c02b-4eb1-aa45-492a9edb032b)



**Hoặc dùng PowerShell**

```
Restart-Service ftpsvc
```

### Kết nối đến FTP Server từ FileZilla client
![image](https://github.com/user-attachments/assets/d6526c29-ed88-4515-b4e6-d849e0fb8e08)


