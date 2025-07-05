> # MAIL SERVER 

# 1. Cấu hình Mail Domain và DNS

- Trước tiên cần thêm bản ghi A trỏ `mail.antvpro.io.vn` về IP máy chủ **aaPanel** như sau

![image](https://github.com/user-attachments/assets/fcbb649b-b6e4-4ef3-8db8-357fcb89f210)

### Bước 1: Add Domain - Thêm tên miền email

> - Chọn chế độ DNS: **Automatic** hoặc **Manual**:
>     - **Automatic**: sẽ tự động thêm các bản ghi DNS: A, MX, SPF, DKIM, DMARC (**yêu cầu cấu hình DNS API**).
>     - **Manual**: cần thêm các bản ghi DNS thủ công tại nhà cung cấp tên miền:

![image](https://github.com/user-attachments/assets/f435dcb4-0418-411b-89ab-078ac1a604dd)


### Bước 2: Cấu hình DNS 
Sau khi thêm tên miền vào máy chủ thư thành công, click --> `DNS Record`

- Làm theo hướng dẫn để thêm các bản ghi `MX`, `SPF`, `DKIM`, `DMARC`, và các bản ghi khác trong phần mềm quản lý tên miền 

![image](https://github.com/user-attachments/assets/5da5a5f9-3fee-4881-a506-f10ae7f7e2dd)

- Thêm các bản ghi:

![image](https://github.com/user-attachments/assets/4b719148-0cc2-4b71-b53d-eff1bcd2e991)

- Trạng thái thành công như sau:

![image](https://github.com/user-attachments/assets/a04f05c7-d271-4acb-879f-83b666d1329a)


### Bước 3: Thêm người dùng email
> **Mail Server** --> **Mailboxes** --> **Add Mailboxe**
- Tạo `user1` và `user2`

![image](https://github.com/user-attachments/assets/c7405861-81ec-4c78-99f3-9193251c3ca1)

### Bước 4: Cài đặt Webmail + SSL
- `**Mail Server`** --> **`Mail Doamin`** --> **`WebMail`**

![image](https://github.com/user-attachments/assets/142f8792-e158-45ec-a704-082499aa6e42)

- Nhập **Domain** và chọn **PHP version**

![image](https://github.com/user-attachments/assets/6e2b56a0-b34e-4f98-b09e-24f74bded28b)

#### Cài đặt **SSL** cho **WebMail**
- Chọn **`Website`** --> **`mail.antvpro.io.vn`** --> **`SSL`** --> (Tùy chọn) **`Lets Encrypt`** --> 

![image](https://github.com/user-attachments/assets/331ad521-de08-42ba-9b69-baa0dea4db30)


### Bước 5: Test gửi nhận Email

- Sau khi đã cài xong **Webmail+SSL** --> Đăng nhập vào **Webmail** bằng tên user và mật khẩu được tạo ở **Bước 3**

**--> Mail Server --> Maiboxes --> WebMail --> Public access**

![image](https://github.com/user-attachments/assets/45ab35f8-f8dd-46c3-a3fa-bd17540484b8)

- Đăng nhập bằng user được tạo:

![image](https://github.com/user-attachments/assets/9b3e159a-439e-4479-93d9-c958a28e8dd7)

- Kiểm tra gửi nhận email

![image](https://github.com/user-attachments/assets/d0cdd600-f4f8-4e5d-b97c-286e01e2b826)

---

# 2. Các tính năng và thông số cụ thể 

## Mail Domain

![image](https://github.com/user-attachments/assets/21faddf0-ef53-49c6-800e-020ce918354f)

| **Function**   | **Mô tả **   |
| -------------- | ------------ |
| **Add Domain**            | Thêm tên miền mới vào Mail Server.                                                |
| **Refresh domain record** | Làm mới bản ghi DNS của tên miền (thường mất vài phút để có hiệu lực).            |
| **Domain name**           | Hiển thị tên miền.                                                                |
| **Not in Spam List**      | Kiểm tra xem tên miền có nằm trong danh sách đen spam hay không (có thể tự động). |
| **Quota**                 | Dung lượng tối đa cho toàn bộ email của tên miền đó.                              |
| **Mailboxes**             | Số lượng tài khoản email đã tạo trên tên miền này.                                |
| **Default mailbox size**  | Dung lượng mặc định của mỗi tài khoản email thuộc tên miền này.                   |
| **Catch All**             | Bật để nhận email gửi đến địa chỉ không tồn tại trong tên miền.                   |
| **SSL**                   | Cấu hình chứng chỉ SSL cho tên miền, nên dùng wildcard như `*.domain.com`.        |
| **WebMail**               | Cài webmail Roundcube để truy cập email qua giao diện web.                        |
| **DNS Record**            | Xem và hướng dẫn thêm bản ghi DNS cần thiết cho email.                            |
| **Edit**                  | Chỉnh sửa thông tin tên miền.                                                     |
| **Delete**                | Xóa tên miền khỏi Mail Server (cẩn thận vì sẽ mất toàn bộ hộp thư và dữ liệu).    |


## Mailboxes

![image](https://github.com/user-attachments/assets/5a5427aa-9f9f-441b-b47f-008308263821)

| **Function**   | **Mô tả **    |
| -------------- | ------------- |
| **Add Mailbox**  | Thêm người dùng email mới.                                                     |
| **Batch Create** | Tạo nhiều tài khoản email cùng lúc (**PRO**)                                            |
| **Import**       | Nhập danh sách tài khoản email từ file.                                        |
| **Export**       | Xuất danh sách người dùng email ra file (.json).                                       |
| **Username**     | Hiển thị địa chỉ email của người dùng.                                         |
| **Password**     | Hiển thị, sao chép mật khẩu người dùng.                                        |
| **Login info**   | Sao chép thông tin đăng nhập của người dùng.                                   |
| **Quota**        | Dung lượng hộp thư của người dùng.                                             |
| **Type**         | Kiểu tài khoản: người dùng thường hoặc quản trị viên.                          |
| **Status**       | Trạng thái hoạt động: đã bật hoặc tắt.                                         |
| **WebMail**      | Đăng nhập WebMail (Roundcube) không cần mật khẩu hoặc qua thông tin tài khoản. |
| **Edit**         | Chỉnh sửa thông tin tài khoản email.                                           |
| **Delete**       | Xóa tài khoản email (mất toàn bộ dữ liệu, cần sao lưu trước).                  |


## Email

- **Inbox:** `View`, `Spam`, `DeleteEmail` được nhận bởi người dùng được chỉ định

![image](https://github.com/user-attachments/assets/7db27514-f809-4094-8e62-999aafc086cb)

- **Outbox**: `View`, `Deleteemail` mà người dùng được chỉ định đã gửi.
 
![image](https://github.com/user-attachments/assets/b08136d9-e4d7-42da-9459-fa97d79460e6)

- **Spam:** Thư rác
- **Sender:** gửi thư (Tính năng này chỉ được khuyến nghị để kiểm tra chức năng gửi. Để sử dụng)

![image](https://github.com/user-attachments/assets/20f588b2-fecf-484c-892f-b86f6a01d432)

## Other Settings

![image](https://github.com/user-attachments/assets/dedeba9a-a053-4efa-994a-d3409147c577)

- **Cài đặt chung - Common settings**
    - Cài đặt **WebMail** và quản lý thông qua Roundcube
    - **Mail save time**: Thiết lập thời gian lưu trữ của tất cả các email. Sau thời gian này, các email sẽ được `permanently deleted` (xóa vĩnh viễn)
    - Cài đặt Thông báo...
    - Tự động khởi động các dịch vụ máy chủ mail `postfix`, `dovecot`, `rspamd`

- **BCC:** BCC là "Blind Carbon Copy" cho phép gửi một bản sao email đến những người nhận được chỉ định, nhưng ẩn địa chỉ email của những người nhận đó để những người nhận khác không nhìn thấy.
    - **Thêm BCC:**
    - ![image](https://github.com/user-attachments/assets/882ce0cc-0f79-48af-8f96-3b28e684c126)


- **Mail forward:** chuyển tiếp thư

![image](https://github.com/user-attachments/assets/c39dffd9-6274-4a26-b32e-b870a918a3ba)

- **Auto Responder:** thiết lập trả lời tự động
    - Chỉ định người nhận `automatically reply` đến nội dung đã chỉ định sau khi nhận được email

- **Backup:** Thiết lập kế hoạch sao lưu 

![image](https://github.com/user-attachments/assets/4d5fbd54-13c9-4a71-9310-b615e78d1f7a)


## Mail Marketing 

- Chức năng là một công cụ có thể gửi email hàng loạt, quản lý chiến dịch tiếp thị, và theo dõi hiệu quả gửi email

![image](https://github.com/user-attachments/assets/25edb91c-e0e2-4453-912d-f688110f425f)

- **Mục đích của Mail Marketing**
    - Gửi email hàng loạt đến danh sách người đăng ký.
    - Tạo và quản lý mẫu email.
    - Theo dõi tỷ lệ mở, tỷ lệ nhấp, email bị trả lại (bounce).
    - Tự động hóa gửi email theo điều kiện.

## Marketing Task 

- **Add Send Tasks:**

    **--> Mail Server → Mail Marketing → Marketing Task → Add Send Tasks**

    - **From**: Chọn địa chỉ email gửi.
    - **Display Name**: Tên hiển thị người gửi.
    - **Subject**: Tiêu đề email.
    - **Recipients**: Chọn nhóm người nhận (tạo từ mục Subscribers).
    - **Email Template**: Chọn mẫu nội dung email.
    - **Unsubscribe Link**: Thêm liên kết hủy đăng ký.
    - **Threads num**: Số luồng gửi (tự động hoặc thủ công).
    - **Send time**: Chọn thời gian gửi.
    - **Send Test email to**: Gửi thử đến một email để kiểm tra.
    
![image](https://github.com/user-attachments/assets/0871e164-8e3c-4221-a154-43b186fbab42)

    
- **Templates (Mẫu email)**
    - Tạo nội dung email bằng HTML hoặc kéo thả.
    - Có thể xem trước, chỉnh sửa, nhân bản, xuất/nhập mẫu.  

![image](https://github.com/user-attachments/assets/09bd0210-46a0-41d8-8b63-713d8da8e66f)

    
- **Subscribers (Người đăng ký)**
    - Quản lý danh sách email:
        - Nhập từ file .txt hoặc .json (mỗi dòng là một email).
        - Gán vào nhóm cụ thể.
        - Theo dõi trạng thái: Đã đăng ký / Hủy đăng ký.
    
![image](https://github.com/user-attachments/assets/71f691d8-9cf8-4e53-9806-4efaf90b542a)

- **Groups (Nhóm người nhận)**
    - Tạo nhóm để phân loại người đăng ký.
    - Mỗi **Task** có thể gửi đến một hoặc nhiều nhóm.

![image](https://github.com/user-attachments/assets/412cfc3f-6c94-4442-9e5c-0cd1a2a60831)

- **Suspend List** (Danh sách tạm ngưng)
    - Tự động thêm email bị lỗi gửi (Deferred/Bounced).
    - Có thể quét email không hợp lệ và xóa khỏi danh sách.
    - Giúp tránh gửi lại đến email không tồn tại.

![image](https://github.com/user-attachments/assets/39b00022-03c0-456a-8b6c-56cd53d46e3a)

- **Automation - Tự động hóa (PRO)** 
    - Tạo quy trình gửi email tự động:
    - Ví dụ: Khi có người đăng ký mới → gửi email chào mừng.
    - Có thể thêm điều kiện: delay, webhook, nhóm cụ thể.
