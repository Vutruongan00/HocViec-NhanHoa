> # PLESK CUSTOMER

<img width="943" height="446" alt="image" src="https://github.com/user-attachments/assets/859d7947-d801-4af4-a502-4cf5184db85c" />

Các chức năng chính trong giao diện Customer:
- **Websites & Domains:** Quản lý tên miền, subdomain, cấu hình DNS, SSL, và các thiết lập hosting khác.
- **Mail**: Tạo và quản lý tài khoản email, thiết lập spam filter, chuyển tiếp email, và các thiết lập liên quan.
* **Applications:** Cài đặt và quản lý các ứng dụng web như CMS (WordPress, Joomla...), công cụ bảo mật, và tiện ích mở rộng
* **Files:** Truy cập và quản lý file trên hosting thông qua trình quản lý file trực quan.
* **Databases:** Tạo, quản lý cơ sở dữ liệu MySQL hoặc PostgreSQL, truy cập phpMyAdmin...
* **Statistics:** Xem thống kê về dung lượng ổ đĩa, băng thông sử dụng, và các thông số hệ thống khác.
* **Users**: Quản lý người dùng phụ, phân quyền truy cập vào các phần khác nhau của hosting.
* **Account**: Thông tin tài khoản, gói dịch vụ, thiết lập bảo mật, và các thông tin liên quan đến người dùng.
* **WordPress :** Quản lý các website WordPress, cập nhật plugin, themes, bảo mật và sao lưu.
* **SEO:**  Công cụ hỗ trợ tối ưu hóa website cho công cụ tìm kiếm (Search Engine Optimization).
* **Laravel  :** Hỗ trợ triển khai và quản lý các ứng dụng Laravel trên hosting.


# 1. Website & Domain

Đây là nơi người dùng quản lý toàn bộ hệ thống tên miền và website đang hoạt động trên hosting. Các chức năng chính bao gồm:

- Hiển thị danh sách tên miền và subdomain đang được quản lý
- Các nút chức năng:
- <img width="1620" height="493" alt="image" src="https://github.com/user-attachments/assets/ce101962-e729-48ad-8522-ec3c53cdd0c6" />

    - **Add Domain:** Thêm một tên miền mới vào hệ thống hosting.
    - **Add Subdomain:** Tạo một subdomain cho tên miền đã có.
- Tìm kiếm và hiểu thị:

<img width="1613" height="395" alt="image" src="https://github.com/user-attachments/assets/69f6426c-b6c8-442f-a7a7-f2a8006b7319" />

- **Tác vụ cho từng tên miền:**

- <img width="1621" height="534" alt="image" src="https://github.com/user-attachments/assets/1464e383-fb5b-4853-927d-0ac01e46025e" />

    - Truy cập vào File Manager của tên miền.
    - Cấu hình Hosting Settings (PHP version, document root...).
    - Truy cập Mail Settings nếu tên miền có dịch vụ email.
    - Quản lý Databases
    - Cài đặt hoặc quản lý WordPress nếu có.
    
    
## Add Domain - Thêm tên miền mới
- Chọn các tùy chọn khởi tạo website

<img width="823" height="622" alt="image" src="https://github.com/user-attachments/assets/72af5ba1-4778-4539-a7f1-461e2b297791" />

- Điền thông tin tên miền:
    - Domain name: Tên miền bạn muốn tạo
    - **Webspace**: Cấu hình Subscription.
    - Hosting type: Chọn loại dịch vụ lưu trữ. Web Hosting/ Forwarding/ No Hosting
    - Activate the DNS service: Bật dịch vụ DNS để tên miền hoạt động.
    - Activate the mail service: Bật dịch vụ email cho tên miền.
    - Hosting Settings
        - Document root: Thư mục gốc của website trên máy chủ, ví dụ: /demo3.io.vn.
        - Preferred domain: Chọn định dạng tên miền ưu tiên để chuyển hướng người dùng hoặc không chuyển hướng (None)

- Chọn **Add Domain** để tiến hành tạo:

<img width="801" height="455" alt="image" src="https://github.com/user-attachments/assets/e65bdce1-afda-4d66-ad95-04e988e8a23d" />

## Add Subdomain

<img width="801" height="455" alt="image" src="https://github.com/user-attachments/assets/cf70864a-fdaf-400a-bd53-c9e008160ed2" />

- Subdomain name: Cấu hình subdomain, chọn domain cần tạo.
- Hosting Settings: Cấu hình thư mục lưu trữ

## Các phím biểu tượng đại diện cho các chức năng:

<img width="1905" height="597" alt="image" src="https://github.com/user-attachments/assets/e7799598-e616-4e85-9004-35dfbd8771af" />

### 1. **File Manager** - Quản lý tập tin của website.

<img width="743" height="359" alt="image" src="https://github.com/user-attachments/assets/a4ee4fcb-f4d0-4e6a-b642-b13ff4fea0ea" />

- Các chức nút chức năng chính:
    * **Copy / Move**: Sao chép hoặc di chuyển tập tin/thư mục.
    * **Archive**: Nén tập tin/thư mục thành file zip.
    * **More options**: Các thao tác nâng cao (ví dụ: phân quyền, chỉnh sửa).
    * **Remove**: Xóa tập tin/thư mục.
    * **Search bar**: Tìm kiếm nhanh tập tin theo tên.

### 2. Mail Account - Cấu hình tài khoản Mail
> Đây là nơi bạn có thể tạo và quản lý các tài khoản email gắn với tên miền

- **Các chức năng chính:**
    - **Email Addresses**: Danh sách các email đã tạo, cho phép chỉnh sửa, xóa, hoặc cấu hình thêm.
    * **Mail Settings**: Cấu hình chung cho dịch vụ email.
    * **Mailing Lists**: Tạo danh sách gửi thư nhóm.
    * **Outgoing Mail Control**: Kiểm soát số lượng email gửi ra, chống spam.

#### Create Email Address - Tạo tài khoản email mới cho tên miền

<img width="901" height="791" alt="image" src="https://github.com/user-attachments/assets/3aa545e8-82d3-435f-8454-8f1ce7bb5fcc" />

- **Email Address**\*: Tên tài khoản email (ví dụ: `info@antvpro.io.vn`).
- **External Email Address**: Địa chỉ email ngoài để khôi phục mật khẩu (tùy chọn).
- **Password**\* / **Confirm Password**\*: Mật khẩu đăng nhập email.

- **📦 Dung lượng hộp thư**

* **Mailbox Size**:

  * Mặc định: 100 MB.
  * Có thể chọn **Custom** để đặt dung lượng riêng.

- **🗒️ Description in Plesk**

* Ghi chú nội bộ về tài khoản email (không bắt buộc)

- **Ngoài ra còn có thể cấu hình mở rộng cho từng địa chỉ email**
    - **Forwarding:** Chuyển tiếp email đến một hoặc nhiều địa chỉ khác.
    - <img width="766" height="447" alt="image" src="https://github.com/user-attachments/assets/72a5374b-2627-4b62-96d5-bab741dee113" />
    - **Email Aliases:** Tạo các địa chỉ phụ trỏ về cùng một hộp thư (ví dụ: contact@ → info@).
    - <img width="769" height="281" alt="image" src="https://github.com/user-attachments/assets/228f2f34-79bf-43ef-a84c-a3d98403b963" />
    
    - **Auto-Reply**: Thiết lập trả lời tự động khi có email gửi đến.
    - <img width="882" height="821" alt="image" src="https://github.com/user-attachments/assets/966c907b-2cbf-45cc-bd38-2d08c0fce213" />
        - Switch on auto-reply: Bật tính năng trả lời tự động cho email. Khi có ai đó gửi email đến địa chỉ này, hệ thống sẽ tự động gửi lại một tin nhắn phản hồi.
        - Auto-reply message subject: Tiêu đề của email phản hồi. Mặc định là: Re: <request_subject> → phản hồi theo tiêu đề email gốc.
        - Định dạng nội dung phản hồi
            - Plain text: văn bản đơn giản, tương thích với mọi ứng dụng email.
            - HTML: cho phép định dạng đẹp hơn (font, màu, v.v.), nhưng có thể không hiển thị đúng ở một số ứng dụng email.
        - Auto-reply message text: Nội dung email phản hồi tự động.
        - Forward to: Khi gửi phản hồi tự động, email gốc cũng sẽ được chuyển tiếp đến địa chỉ bạn chỉ định.
        - Giới hạn số lần phản hồi: Tùy chọn giới hạn số lần gửi phản hồi tự động đến cùng một địa chỉ email trong một ngày.
        - Switch off auto-reply on: Chọn ngày để tự động tắt chức năng trả lời tự động. Hữu ích khi bạn chỉ muốn bật auto-reply trong thời gian nghỉ phép, công tác,...

    - **Antivirus**: Bật/tắt chức năng quét virus cho email.
    - <img width="880" height="354" alt="image" src="https://github.com/user-attachments/assets/065b37bd-510b-4ea7-963c-d6f5712f2aba" />

#### Mail Settings 
> Đây là nơi bạn cấu hình các thiết lập tổng thể cho dịch vụ email của một tên miền

<img width="753" height="810" alt="image" src="https://github.com/user-attachments/assets/75848dac-63f4-44bb-b2cb-88b07f7be6a8" />

- Mail service on this domain

    * **Enabled**: Cho phép gửi và nhận email.
    * **Disabled**: Tắt dịch vụ email (không thể gửi/nhận).
    * **Not configured**: Tắt hoàn toàn, xóa toàn bộ hộp thư và dữ liệu email.


-  **📬 Xử lý email gửi đến người dùng không tồn tại**
    * **Forward to address**: Chuyển tiếp đến địa chỉ cụ thể (ví dụ: `admin@tanpro.edu.vn`).
    * **Redirect to external mail server**: Chuyển tiếp đến máy chủ email ngoài (qua IP).
    * **Reject**: Từ chối email gửi đến địa chỉ không tồn tại.

- **🌐 Webmail:** Chọn giao diện webmail (ví dụ: MailEnable WebMail 10.50).

- **🔐 SSL/TLS Certificates**
    * **Webmail**: Chứng chỉ bảo mật cho giao diện webmail.
    * **Mail**: Chứng chỉ bảo mật cho dịch vụ gửi/nhận email (SMTP/IMAP/POP3).

- **📢 Mailing Lists**: Bật/tắt chức năng gửi thư nhóm (mailing list).

- **🛡️ DKIM (DomainKeys Identified Mail)**

    * Bật/tắt chức năng ký email bằng DKIM để chống spam.
    * **Active DKIM Selector**: Đang dùng selector mặc định (`default`).
    * Có thể tạo selector mới nếu cần.

- **🔍 Mail Autodiscover**: Cho phép tự động cấu hình email trên các ứng dụng như Outlook, Thunderbird...

---

### 3. Database - Quản lý Cơ sở dữ liệu
<img width="732" height="428" alt="image" src="https://github.com/user-attachments/assets/4d3c540a-cd8f-4e4d-9acf-a0bbdbe2b5c7" />

- Các chức năng trong quản lý Database:
    - 🔗 **phpMyAdmin**: Mở giao diện quản lý cơ sở dữ liệu qua trình duyệt.
    - 📤 **Export Dump**: Xuất bản sao lưu (dump) của database ra file .sql hoặc .zip.
    - 📥 **Import Dump**: Nhập dữ liệu từ file dump vào database hiện tại.
    - 🔄 **Move to Subscription:** Di chuyển database sang một gói dịch vụ (subscription) khác.
    - ℹ️ **Connection Info:** Hiển thị thông tin kết nối: host, port, username, password...
    - 🗑️ **Remove Database:** Xóa toàn bộ database khỏi hệ thống.
    - 📋 **Copy**: Tạo bản sao của database (rất hữu ích khi cần nhân bản cấu trúc/dữ liệu).
    - 🔍 **Check and Repair**: Kiểm tra và sửa lỗi trong database (ví dụ: bảng bị hỏng, index lỗi...).
  
#### **🔹 + Add Database**

<img width="731" height="720" alt="image" src="https://github.com/user-attachments/assets/479a74cf-14af-4cb0-915f-b5d2661e219f" />

* Tạo cơ sở dữ liệu mới cho website.
* Khi tạo, bạn sẽ cần:

  * Tên database.
  * Loại hệ quản trị (MySQL, PostgreSQL...).
  * Người dùng (username/password).
  * Gán database cho tên miền cụ thể.

#### **🔹 User Management**

* Quản lý người dùng database: tạo mới, đổi mật khẩu, phân quyền.

<img width="717" height="858" alt="image" src="https://github.com/user-attachments/assets/d1d022de-44d8-451c-b2dd-9d07a961905a" />

#### **🔹 Backup Manager**

<img width="721" height="398" alt="image" src="https://github.com/user-attachments/assets/9a9011c7-cfc8-4f43-b70d-051d2243306c" />

* Sao lưu hoặc khôi phục cơ sở dữ liệu.
* Có thể tải về bản sao lưu hoặc khôi phục từ bản sao trước đó.

---
### 4. Hosting Settings

<img width="810" height="917" alt="image" src="https://github.com/user-attachments/assets/fb7107ec-57ca-4294-8a3b-0163d0608f4e" />

-  **1. System User's Credentials**

    * Tài khoản hệ thống dùng để truy cập FTP hoặc Remote Desktop.
    * Bao gồm **username** và **password**.
    * Đây là thông tin quan trọng để quản trị website ở cấp độ hệ thống.

-  **🖥️ 2. Remote Desktop Access**

    * Cho phép truy cập máy chủ qua Remote Desktop (chỉ áp dụng với hosting Windows).
    * Cần bật nếu bạn muốn quản lý server trực tiếp.

-  **🌐 3. Domain Name & Hosting Type**

    * **Domain name**: Tên miền đang được cấu hình.
    * **Hosting type**: Loại dịch vụ hosting (Website, Forwarding, No Hosting...).

- **🔐 4. SSL/TLS Support**

    * Bật/tắt hỗ trợ HTTPS cho website.
    * Cho phép chọn chứng chỉ SSL để mã hóa kết nối.

- **📊 5. Web Statistics**

    * Công cụ thống kê truy cập website (ví dụ: Webalizer, AWStats).
    * Có thể bảo vệ bằng tài khoản FTP.

- **💻 6. Web Scripting**

    * Bật/tắt các công nghệ web như:

      * PHP, CGI, ASP, ASP.NET...
      * Chọn phiên bản cụ thể (ví dụ ASP.NET 4.8.0).

- **🌍 7. IP Addresses**

    * Gán địa chỉ IP cho tên miền (shared hoặc dedicated).
    * Quan trọng khi cấu hình SSL hoặc DNS.

- **💾 8. Disk Space Quota**

    * Giới hạn dung lượng lưu trữ cho tên miền.
    * Giúp kiểm soát tài nguyên và tránh vượt mức.

### 5. Các tùy chọn khác

<img width="269" height="219" alt="image" src="https://github.com/user-attachments/assets/8434375b-ff90-4e8b-8a95-2cd8e67152f9" />

- **Open**: Mở website
- **Preview**: Mở phần preview
- **Manage in Custom Panel / Manage in Reseller Panel**: Cho phép quản trị viên đăng nhập chuyển tới giao diện của khách hàng/reseller để quản lý, cấu hình.
- **Move Domain**: Chuyển tên miền tới **subscription** khác
- **Change domain name**: thay đổi tên tên miền

---

# 2. Mail - Quản lý Email 
<img width="946" height="473" alt="image" src="https://github.com/user-attachments/assets/1cc39fa7-94c8-4a40-8d37-ddefc24542a1" />

> Đây là nơi bạn có thể tạo và quản lý các tài khoản email gắn với tên miền

- **Các chức năng chính:**
    - **Email Addresses**: Danh sách các email đã tạo, cho phép chỉnh sửa, xóa, hoặc cấu hình thêm.
    * **Mail Settings**: Cấu hình chung cho dịch vụ email.
    * **Mailing Lists**: Tạo danh sách gửi thư nhóm.
    * **Outgoing Mail Control**: Kiểm soát số lượng email gửi ra, chống spam.

## Create Email Address - Tạo tài khoản email mới cho tên miền

<img width="901" height="791" alt="image" src="https://github.com/user-attachments/assets/3aa545e8-82d3-435f-8454-8f1ce7bb5fcc" />

- **Email Address**\*: Tên tài khoản email (ví dụ: `info@antvpro.io.vn`).
- **External Email Address**: Địa chỉ email ngoài để khôi phục mật khẩu (tùy chọn).
- **Password**\* / **Confirm Password**\*: Mật khẩu đăng nhập email.

- **📦 Dung lượng hộp thư**

* **Mailbox Size**:

  * Mặc định: 100 MB.
  * Có thể chọn **Custom** để đặt dung lượng riêng.

- **🗒️ Description in Plesk**

* Ghi chú nội bộ về tài khoản email (không bắt buộc)

- **Ngoài ra còn có thể cấu hình mở rộng cho từng địa chỉ email**
    - **Forwarding:** Chuyển tiếp email đến một hoặc nhiều địa chỉ khác.
    - <img width="766" height="447" alt="image" src="https://github.com/user-attachments/assets/72a5374b-2627-4b62-96d5-bab741dee113" />
    - **Email Aliases:** Tạo các địa chỉ phụ trỏ về cùng một hộp thư (ví dụ: contact@ → info@).
    - <img width="769" height="281" alt="image" src="https://github.com/user-attachments/assets/228f2f34-79bf-43ef-a84c-a3d98403b963" />
    - **Auto-Reply**: Thiết lập trả lời tự động khi có email gửi đến.
    - <img width="882" height="821" alt="image" src="https://github.com/user-attachments/assets/966c907b-2cbf-45cc-bd38-2d08c0fce213" />
        
        - Switch on auto-reply: Bật tính năng trả lời tự động cho email. Khi có ai đó gửi email đến địa chỉ này, hệ thống sẽ tự động gửi lại một tin nhắn phản hồi.
        - Auto-reply message subject: Tiêu đề của email phản hồi. Mặc định là: Re: <request_subject> → phản hồi theo tiêu đề email gốc.
        - Định dạng nội dung phản hồi
            - Plain text: văn bản đơn giản, tương thích với mọi ứng dụng email.
            - HTML: cho phép định dạng đẹp hơn (font, màu, v.v.), nhưng có thể không hiển thị đúng ở một số ứng dụng email.
        - Auto-reply message text: Nội dung email phản hồi tự động.
        - Forward to: Khi gửi phản hồi tự động, email gốc cũng sẽ được chuyển tiếp đến địa chỉ bạn chỉ định.
        - Giới hạn số lần phản hồi: Tùy chọn giới hạn số lần gửi phản hồi tự động đến cùng một địa chỉ email trong một ngày.
        - Switch off auto-reply on: Chọn ngày để tự động tắt chức năng trả lời tự động. Hữu ích khi bạn chỉ muốn bật auto-reply trong thời gian nghỉ phép, công tác,...

    - **Antivirus**: Bật/tắt chức năng quét virus cho email.

    - <img width="880" height="354" alt="image" src="https://github.com/user-attachments/assets/065b37bd-510b-4ea7-963c-d6f5712f2aba" />

#### Mail Settings 
> Đây là nơi bạn cấu hình các thiết lập tổng thể cho dịch vụ email của một tên miền

<img width="753" height="810" alt="image" src="https://github.com/user-attachments/assets/75848dac-63f4-44bb-b2cb-88b07f7be6a8" />

- Mail service on this domain

    * **Enabled**: Cho phép gửi và nhận email.
    * **Disabled**: Tắt dịch vụ email (không thể gửi/nhận).
    * **Not configured**: Tắt hoàn toàn, xóa toàn bộ hộp thư và dữ liệu email.


-  **📬 Xử lý email gửi đến người dùng không tồn tại**
    * **Forward to address**: Chuyển tiếp đến địa chỉ cụ thể (ví dụ: `admin@tanpro.edu.vn`).
    * **Redirect to external mail server**: Chuyển tiếp đến máy chủ email ngoài (qua IP).
    * **Reject**: Từ chối email gửi đến địa chỉ không tồn tại.

- **🌐 Webmail:** Chọn giao diện webmail (ví dụ: MailEnable WebMail 10.50).

- **🔐 SSL/TLS Certificates**
    * **Webmail**: Chứng chỉ bảo mật cho giao diện webmail.
    * **Mail**: Chứng chỉ bảo mật cho dịch vụ gửi/nhận email (SMTP/IMAP/POP3).

- **📢 Mailing Lists**: Bật/tắt chức năng gửi thư nhóm (mailing list).

- **🛡️ DKIM (DomainKeys Identified Mail)**

    * Bật/tắt chức năng ký email bằng DKIM để chống spam.
    * **Active DKIM Selector**: Đang dùng selector mặc định (`default`).
    * Có thể tạo selector mới nếu cần.

- **🔍 Mail Autodiscover**: Cho phép tự động cấu hình email trên các ứng dụng như Outlook, Thunderbird...

---

# 3. Application
- Cài đặt ứng dụng như WordPress, Joomla, v.v.

- Giao diện quản lý các ứng dụng được cài, cài đặt ứng dụng mới 
- Gồm các tab quản lý 
    - Manage My Applications: Hiển thị danh sách các ứng dụng được cài đặt 
  - <img width="1660" height="467" alt="image" src="https://github.com/user-attachments/assets/452680cb-2005-4d2d-9a87-c7fddb37215c" />

	- Click `Scan` để làm mới danh sách 
	- Name: Tên ứng dụng. CLick để mở giao diện cấu hình riêng của từng ứng dụng 
	- Installation Path: Đường dẫn cài đặt 
	- Remove: Gỡ cài đặt ứng dụng

- Featured Applications: Danh sách các ứng dụng được đề xuất và cài phổ biến. Có thể chọn cài đặt ứng dụng tại đây, custom phiên bản cần cài đặt. 

<img width="936" height="360" alt="image" src="https://github.com/user-attachments/assets/595cfb68-c4d1-4cb7-9acb-9389fb73f7c5" />

- All Available Applications 

<img width="936" height="360" alt="image" src="https://github.com/user-attachments/assets/53ee6caf-fdf3-4152-8772-f5df8e861100" />

- Hiển thị danh sách tất cả các ứng dụng có thể cài. Có thể cài đặt ứng dụng tại đây bằng nút thao tác `Install` cạnh tên ứng dụng. 

---

# 4. File - Trình quản lý tệp tin
Đây là nơi người dùng quản lý toàn bộ tệp tin của website trên hosting. Giao diện hiển thị nội dung thư mục gốc của tên miền

<img width="946" height="446" alt="image" src="https://github.com/user-attachments/assets/bc96f8db-394a-47ef-bb23-c5c36ec257dd" />

- Chức năng quản lý tệp
    - **Upload**: tải tệp từ máy tính lên hosting.
    - **Create File / Create Directory**: tạo tệp hoặc thư mục mới.
    - **Edit**: chỉnh sửa nội dung tệp trực tiếp trong trình soạn thảo.
    - **Rename**: đổi tên tệp hoặc thư mục.
    - **Delete**: xóa tệp hoặc thư mục.
    - **Move / Copy**: di chuyển hoặc sao chép tệp đến vị trí khác
    - **Permissions**: thiết lập quyền truy cập (đọc, ghi, thực thi).

---

# 5. Databases - Quản lý cơ sở dữ liệu
- Tạo và quản lý cơ sở dữ liệu (MySQL, PostgreSQL...). liên quan đến website
- Quản trị cấu hình cài đặt database với các domain các thao tác, cấu hình tương tự với phần quản trị từng domain.

<img width="1793" height="845" alt="image" src="https://github.com/user-attachments/assets/f9000632-0c7b-42b1-ac57-750732554ce0" />

- Mỗi CSDL hiển thị các thông tin:

    - **Host**: địa chỉ máy chủ CSDL (thường là localhost hoặc IP cụ thể).
    - **User**: tài khoản truy cập CSDL.
    - **Tables**: số lượng bảng trong CSDL.
    - **Size**: dung lượng CSDL đang sử dụng.

- Các chức năng quản lý CSDL:
- <img width="472" height="177" alt="image" src="https://github.com/user-attachments/assets/818b5b68-fc4f-4473-b763-62774dfc1a71" />

    - **Access phpMyAdmin:** Truy cập giao diện quản lý CSDL trực quan để thực hiện các thao tác như truy vấn SQL, chỉnh sửa bảng, xuất/nhập dữ liệu...
    -  **Export Dump :** Tạo bản sao lưu (dump) của CSDL để tải về hoặc lưu trữ.
    * **Import Dump**: Tải lên và khôi phục CSDL từ một file dump có sẵn.
    * **Connection Info**: Hiển thị thông tin kết nối như host, port, username, password (nếu có).
    * **Copy Database**: Tạo bản sao của CSDL hiện tại sang một tên khác.
    * **Check and Repair**: Kiểm tra và sửa lỗi trong các bảng của CSDL.
    * **Remove Database**: Xóa hoàn toàn CSDL khỏi hệ thống.

# 6. Statics - Thống kê tài nguyên sử dụng

Giao diện này cung cấp thông tin tổng quan về việc sử dụng **dung lượng ổ đĩa** và **băng thông** của dịch vụ hosting.

- **🧮 Disk Space Usage – Dung lượng ổ đĩa**

    * **Tổng dung lượng**: 500 GB
    * **Đã sử dụng**: 0.2 MB (≈ 0%)

    - **Phân bổ theo dịch vụ:**
        * **Web**: 191 KB – chiếm phần lớn dung lượng đã dùng.
        * **Logs**: 10.7 KB – nhật ký truy cập và lỗi.
        * **Mail**: 0 B – chưa có email nào sử dụng dung lượng.
        * **Databases**: 0 B – chưa có dữ liệu trong CSDL.
        * **Backups**: 0 B – chưa có bản sao lưu.
        * **Anonymous FTP directory**: 0 B – chưa sử dụng FTP ẩn danh.

- **Traffic Usage – Băng thông**

    * **Giới hạn hàng tháng**: 100 GB/tháng
    * **Đã sử dụng tháng này**: 0.3 MB (≈ 0%)

    - **Phân bổ theo dịch vụ:**

        * **HTTP**: 347 KB – lưu lượng truy cập web.
        * **FTP**: 0 B – chưa có truyền tải qua FTP.
        * **POP3/IMAP**: 0 B – chưa có truy cập email.
        * **SMTP**: 0 B – chưa có email gửi đi.


# 7. Users - Quản lý tài khoản người dùng
 
## User Accounts
<img width="1905" height="574" alt="image" src="https://github.com/user-attachments/assets/ba1646ca-09d7-40e7-82c5-7fe0263f918a" />

* **Add User**: tạo người dùng mới, thiết lập quyền truy cập và thông tin liên hệ.
* **Edit User**: chỉnh sửa thông tin người dùng hiện tại (email, tên, mật khẩu, quyền...).
* **Remove User**: xóa người dùng khỏi hệ thống.
* **Change Role**: thay đổi vai trò của người dùng (Application User ↔ Owner).

## User Roles - Vai trò người dùng

<img width="1912" height="620" alt="image" src="https://github.com/user-attachments/assets/a1334267-fba2-44d1-a10b-04130f27d319" />

Chức năng này cho phép bạn quản lý các **vai trò (roles)** được gán cho người dùng, từ đó kiểm soát quyền truy cập vào các dịch vụ và ứng dụng trên hosting.

-  **Ý nghĩa các vai trò**

    * **Owner**

      * Quyền cao nhất, toàn quyền quản lý hosting, tên miền, email, file, database, ứng dụng...
      * Thường là người sở hữu tài khoản Plesk.
    * **Application User**

      * Có quyền truy cập và quản lý các ứng dụng web (như WordPress, Joomla...), nhưng không có quyền thay đổi cấu hình hệ thống.
    * **WebMaster**

      * Có quyền quản lý nội dung website, file, và đôi khi cả email, nhưng không được truy cập vào các thiết lập tài khoản hoặc database.
    * **Accountant**

      * Chỉ có quyền truy cập vào các thông tin tài chính, hóa đơn, và thống kê – không được truy cập vào nội dung website hay email.

---

- **Chức năng quản lý vai trò**

    * **Create User Role**: tạo vai trò mới với quyền tùy chỉnh.
    * **Edit Role**: chỉnh sửa quyền của vai trò hiện tại.
    * **Remove Role**: xóa vai trò nếu không còn sử dụng.

---

# 8. Account – Thông tin tài khoản và gói dịch vụ

<img width="1920" height="852" alt="image" src="https://github.com/user-attachments/assets/741f17ed-a738-4ebb-853b-0fb35a55af1b" />

- Giao diện này cung cấp thông tin tổng quan về gói dịch vụ hosting mà bạn đang sử dụng, bao gồm tài nguyên được cấp và giới hạn

    - Thông tin gói dịch vụ
    - Tổng quan tài nguyên đã sử dụng
        - Disk space
        - Traffic
    - Tên miền & Subdomain
    - ...
