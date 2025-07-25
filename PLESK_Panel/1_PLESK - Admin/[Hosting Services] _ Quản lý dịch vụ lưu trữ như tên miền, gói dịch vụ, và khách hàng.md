> # PLESK ADMIN - HOSTING SERVICES
> <img width="697" height="378" alt="image" src="https://github.com/user-attachments/assets/f869eced-a361-4f71-8dd0-26aeff956eab" />
>
> - **Customers**: Quản lý tài khoản khách hàng sử dụng dịch vụ hosting.
> 
> - **Resellers**: Quản lý đại lý phân phối dịch vụ (reseller).
>   
> - **Domains**: Quản lý tên miền được cấp phát cho khách hàng.
>
> - **Subscriptions**: Quản lý gói dịch vụ cụ thể của từng khách hàng.
> 
> - **Service Plans**: Tạo và chỉnh sửa các gói dịch vụ hosting tiêu chuẩn.
---

# 1. Customers - Quản lý khách hàng
- Các chức năng chính trong giao diện **Customers:**
- <img width="928" height="301" alt="image" src="https://github.com/user-attachments/assets/be68de81-1e24-436e-bdd8-97e03dd05993" />
    - **Add Customer**: Tạo mới một tài khoản khách hàng.
    - **Convert to Reseller**: Chuyển đổi khách hàng thành đại lý phân phối dịch vụ (**Reseller**).
    - **Move to**: Cho phép bạn chuyển quyền sở hữu khách hàng từ một người dùng (hoặc reseller) sang một người dùng khác. Đây là cách để tái tổ chức hoặc phân quyền lại hệ thống hosting.
    - **Change Status**: Kích hoạt hoặc vô hiệu hóa tài khoản khách hàng.
    - **Remove**: Xóa tài khoản khách hàng khỏi hệ thống.

## Add Customer - Thêm Tài khoản cho khách hàng mới

1. Đăng nhập vào **Plesk Administrator.**
2. Từ menu điều hướng bên trái, mở rộng  **Hosting Services**, sau đó nhấp vào  tab **Customers**

<img width="202" height="286" alt="image" src="https://github.com/user-attachments/assets/c0105b88-1c6c-47d7-933d-26fc7bf93856" />

- chọn **Add Customer**

<img width="798" height="247" alt="image" src="https://github.com/user-attachments/assets/388155ea-6efc-4179-b1a1-bfe20fd0ebb7" />

- Trong phần **Contact Information**(thông tin liên hệ) điền đầy đủ các thông tin (các trường có dấu * là bắt buộc)
    - Tên liên lạc
    - Địa chỉ email
    
<img width="597" height="343" alt="image" src="https://github.com/user-attachments/assets/618f51d4-8047-4079-8a40-1aa140620c02" />

- Trong phần **Access to Plesk** nhập **Username** và **Password** để truy cập Plesk
    - Tick chọn vào ô **Activate account by e-mail**

<img width="508" height="233" alt="image" src="https://github.com/user-attachments/assets/602d8ce7-dfd6-41dd-a6a5-35e18bd7a2fd" />


- Trong phần **Subscription**:
    - Nhập tên miền.
    - Chọn **Service plan - gói dịch vụ** từ menu

<img width="505" height="299" alt="image" src="https://github.com/user-attachments/assets/9bbe5d57-2472-4625-8999-70b6efb3c81b" />

- **Giao diện danh sách khách hàng:**
> Tại đây quản trị viên có thể quản lý các tài khoản khách hàng sử dụng dịch vụ hosting. 
> **Mỗi khách hàng có thể sở hữu một hoặc nhiều Subscription (gói dịch vụ)**, và được cấp quyền truy cập để quản lý website, email, cơ sở dữ liệu, v.v.

<img width="1546" height="429" alt="image" src="https://github.com/user-attachments/assets/53a1ecd5-b29b-489b-bf83-fa34cad30195" />


## Giao diện quản trị khách hàng
- Click chọn vào tên 1 khách hàng cụ thể để có thể tùy chỉnh **Domain/Subcription**, **Ví dụ:**

<img width="694" height="365" alt="image" src="https://github.com/user-attachments/assets/f21135fa-a731-4af0-80bf-be491edc860f" />


### Tab Domain - Quản lý tên miền

<img width="1898" height="652" alt="image" src="https://github.com/user-attachments/assets/e2345fba-2340-4aa0-a1e1-05a6f07bafa4" />


- **Các phím chức năng:** 
    - **Add Domain / Subdomain / Domain Alias**: Thêm tên miền chính/ tên miền phụ hoặc alias.
    - **Set Status / Remove**: Đặt trạng thái hoạt động hoặc xóa tên miền
    - Các tùy chọn: **Open, Preview, Manage in Customer Panel**.

- **Thông tin khách hàng:** xem thông tin khách hàng và các tùy chọn chỉnh sửa:
    - **Edit Contact Info**: Chỉnh sửa thông tin liên hệ
    - **Convert to Reseller**: Chuyển thành đại lý.
    - **Change Login Info / Login as Customer**: Đổi thông tin đăng nhập hoặc đăng nhập với tư cách khách hàng.
    - **View Status**: Trạng thái tài khoản (Active/Suspend).
    - **Provider**: Người quản lý (Administrator).
    - **Username**: user1.
    - **Owner's Description**: Thêm mô tả về khách hàng.

- Các tác vụ hệ thống:
    - **IIS Application Pool**: Quản lý pool ứng dụng IIS cho khách hàng 
    - **Remove Customer**: Xóa khách hàng khỏi hệ thống
### Tab Subcriptions - Quản lý gói dịch vụ của khách hàng

<img width="1367" height="413" alt="image" src="https://github.com/user-attachments/assets/aa15851c-f014-4e49-9bd2-634b92a13ca4" />

- Hiển thị các gói dịch vụ đã tạo cho khách hàng:
    - **Tên Subscription**: Ví dụ antvpro.io.vn (Default Domain).
    - **Setup Date**: Ngày thiết lập (24/07/2025).
    - **Tùy chọn**: Quản lý gói dịch vụ này trong Customer Panel.
    
- Các chức năng chính:
    - **Add Subcription:** Thêm 1 gói dịch vụ mới cho khách hàng (bao gồm domain, hosting, email...)
    - **Change Plan:** Thay đổi gói dịch vụ hiện tại (ví dụ: đổi gói từ cơ bản sang gói nâng cao)
    - **Change Subcriber:** Chuyển gói dịch vụ này sang 1 khách hàng khác
    - **Set status:** Bật/tắt hoặc tạm ngưng hoạt động của gói dịch vụ
    - **Remove:** Xóa gói dịch vụ


---
---

# 2. Resellers - Quản lý các Đại lý
- Giao diện quản lý Resellers:

<img width="929" height="321" alt="image" src="https://github.com/user-attachments/assets/931131ef-f8a9-4216-a3c7-0462f2b542e6" />

- **Các chức năng chính:**
    - **Add Reseller**: Tạo tài khoản đại lý mới, kèm theo gói dịch vụ.
    - **Convert to Customers**: Chuyển tài khoản đại lý thành tài khoản khách hàng thông thường.
    - **Change Status**: Thay đổi trạng thái hoạt động (Active/Suspended).
    - **Remove**: Xóa tài khoản đại lý khỏi hệ thống.

## Add Reseller - Thêm tài khoản Đại lý mới
- Truy cập: **Resellers → Add Reseller**

<img width="924" height="931" alt="image" src="https://github.com/user-attachments/assets/f82538d2-5df2-4e95-be10-ffc7d0bb6e3e" />

- **Nhập thông tin liên hệ**
    - **Contact name***: Tên người đại diện.
    - **Email address***: Email liên hệ chính.
    - Company name: Tên công ty (nếu có).
    - Phone number: Số điện thoại.
    - Address / City / State / Postal code /
    - Country: Địa chỉ đầy đủ của reseller.
> **Các trường có dấu * là bắt buộc phải điền.**

<img width="729" height="313" alt="image" src="https://github.com/user-attachments/assets/a8ca7d10-c68c-4d64-93a3-74f3b73da42d" />

- **Thông tin Truy cập Plesk (Access to Plesk):**
    - **Username**\*: Tên đăng nhập của reseller.
    - **Password**\* / **Repeat password**\*: Mật khẩu và xác nhận lại mật khẩu.
    - **Generate / Show**: Tạo mật khẩu ngẫu nhiên hoặc hiển thị mật khẩu đã nhập.
    - **Activate account by e-mail**: Nếu chọn, tài khoản sẽ được kích hoạt qua email. Nếu không, quản trị viên phải kích hoạt thủ công.

<img width="725" height="252" alt="image" src="https://github.com/user-attachments/assets/f91325b1-0860-46bd-9a8c-af39bd347efb" />

- **Gán gói dịch vụ (Subscription)**
    - **Service plan**: Chọn gói dịch vụ dành cho reseller (ví dụ: **Default Reseller**).
    - **Proceed to customizing the subscription parameters...**: Nếu chọn, bạn sẽ được chuyển đến bước tùy chỉnh chi tiết gói dịch vụ sau khi tạo tài khoản. 
        > Lưu ý: việc này sẽ khóa khả năng đồng bộ gói dịch vụ sau này.
 

- Giao diện danh sách các Đại lý (Reseller):

<img width="1904" height="621" alt="image" src="https://github.com/user-attachments/assets/391a950e-05c5-49d6-86b7-cbc34d2a9734" />

## Giao diện quản lý Reseller
- Click chọn vào tên 1 **Reseller** cụ thể để có thể tùy chỉnh **Domain/Subcription/Customer**, Ví dụ:

<img width="1119" height="492" alt="image" src="https://github.com/user-attachments/assets/081f355c-c2af-4793-a100-304dff244fcb" />

### Quản lý Domains
* **Add Domain / Subdomain / Domain Alias**: Thêm tên miền chính, phụ hoặc alias cho reseller.

<img width="1680" height="908" alt="image" src="https://github.com/user-attachments/assets/70ca4a79-f3b2-4c6d-a2e0-1470ed50530f" />

* **Danh sách tên miền**:

  * Hiển thị các tên miền mà reseller đang quản lý.
  * Thông tin gồm:
    * **Domain Name**
    * **Hosting Type**
    * **Setup Date**
    * **Disk Usage**
    * **Traffic**
    * **Rank Tracker**: Theo dõi thứ hạng website.


- **👤 Thông tin reseller**

    * **Tên công ty**: ANTV Group.
    * **Email**: `admin@antvgroups.com`.
    * **Ngày tạo tài khoản**: 24/07/2015.
    * **Máy chủ**: Anttv CP Shared.


- **⚙️ Tùy chọn quản lý**

    * **Manage in Reseller Panel**: Truy cập bảng điều khiển riêng của reseller.
    * **View More Statistics**: Xem thêm thống kê về hoạt động của reseller.
    * **Transfer Panel**: Chuyển quyền quản lý bảng điều khiển.
    * **Change Login Info**: Thay đổi thông tin đăng nhập của reseller.

### Quản lý Subcriptionss/Customer
> Thao tác tương tự [Phần 1: Customer](https://github.com/Vutruongan00/HocViec-NhanHoa/edit/main/PLESK_Panel/1_PLESK%20-%20Admin/%5BHosting%20Services%5D%20_%20Qu%E1%BA%A3n%20l%C3%BD%20d%E1%BB%8Bch%20v%E1%BB%A5%20l%C6%B0u%20tr%E1%BB%AF%20nh%C6%B0%20t%C3%AAn%20mi%E1%BB%81n%2C%20g%C3%B3i%20d%E1%BB%8Bch%20v%E1%BB%A5%2C%20v%C3%A0%20kh%C3%A1ch%20h%C3%A0ng.md#1-customers---qu%E1%BA%A3n-l%C3%BD-kh%C3%A1ch-h%C3%A0ng)



---

# 3. Domain - Quản lý tên miền 
> Trong giao diện quản lý Domains của Plesk , bạn có thể thực hiện các thao tác quản lý tên miền trên toàn hệ thống.

<img width="1920" height="501" alt="image" src="https://github.com/user-attachments/assets/3ac3a56a-148a-439f-b659-feddb13a6b77" />

- **🌐 Các chức năng chính**

    * **Add Domain**: Thêm tên miền mới vào hệ thống.
    * **Add Subdomain**: Thêm tên miền phụ (subdomain).
    * **Add Domain Alias**: Thêm alias cho tên miền (tên miền phụ trỏ về tên miền chính).
    * **Set Status**: Thay đổi trạng thái hoạt động của tên miền (Active/Suspended).
    * **Remove**: Xóa tên miền khỏi hệ thống.

- **📋 Danh sách tên miền**
> Hiển thị thông tin chi tiết của từng tên miền:
- 
    * **Domain Name**: Tên miền như `antv.net`, `antvpro.lo.vn`, `shop.antvpro.lo.vn`.
    * **Subscriber**: Người đăng ký tên miền (ví dụ: ANTV Group, Vũ Trường An).
    * **Disk Usage**: Dung lượng đã sử dụng (hiện tại là 0 MB).
    * **Traffic**: Lưu lượng truy cập hàng tháng (0 MB/tháng).
    * **Status**: Trạng thái hoạt động (Active).

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

## Giao diện Quản lý 1 Domain cụ thể:
- Click chọn vào một **Domain Name** mà bạn muốn quản lý:

<img width="1894" height="661" alt="image" src="https://github.com/user-attachments/assets/f0034323-79cb-4a81-9672-0ca20ad99f7b" />

> Khi **click vào một domain cụ thể trong phần "Domains"**, Plesk sẽ chuyển sang giao diện **Subscription** tương ứng với domain đó. Đây là hành vi mặc định vì mỗi domain nằm trong một **subscription**, và toàn bộ cấu hình, tài nguyên, dịch vụ liên quan đều được quản lý theo subscription.

<img width="1656" height="866" alt="image" src="https://github.com/user-attachments/assets/c2098bc9-fe59-4564-ac50-32b3efb2e1e9" />

- **Các tab chức năng**

    * **Dashboard**: Tổng quan dịch vụ đang dùng.
    * **Hosting & DNS**: Cấu hình hosting, DNS, SSL...
    * **Mail**: Quản lý email.
    * **Get Started**: Hướng dẫn khởi tạo nhanh.

### **🛠️ Dashboard**

#### **Cấu hình & Kết nối**

1. **Connection Info** – Thông tin kết nối FTP và database.
2. **ODBC Data Sources** – Quản lý nguồn dữ liệu ODBC.
3. **Virtual Directories** – Quản lý thư mục ảo (chỉ dùng cho hosting Windows).
4. **PHP** – Cấu hình phiên bản và thiết lập PHP.
5. **ASP.NET Settings** – Cấu hình ASP.NET (nếu dùng công nghệ .NET).
6. **Monitoring** – Theo dõi tình trạng server (chưa kết nối).
7. **Advisor** – Gợi ý tối ưu bảo mật và hiệu suất.

---

#### **🌐 Website & Bảo mật**

8. **Git** – Kết nối và triển khai mã nguồn từ Git.
9. **Website Importing** – Nhập website từ nơi khác về.
10. **Website Copying** – Sao chép website sang domain khác.
11. **SSL/TLS Certificates** – Quản lý chứng chỉ bảo mật HTTPS.
12. **Password-Protected Directories** – Bảo vệ thư mục bằng mật khẩu.
13. **Hotlink Protection** – Ngăn chặn website khác dùng hình ảnh/tài nguyên của bạn.

---

#### **📁 Tập tin & Dữ liệu**

14. **Files** – Truy cập và quản lý tập tin website.
15. **FTP** – Quản lý tài khoản FTP.
16. **Databases** – Tạo và quản lý cơ sở dữ liệu.
17. **PHP Composer** – Quản lý thư viện PHP qua Composer.

---

#### **🕒 Tác vụ & Sao lưu**

18. **Scheduled Tasks** – Tạo cron job (tác vụ định kỳ).
19. **Backup & Restore** – Sao lưu và khôi phục dữ liệu.
20. **Logs** – Xem nhật ký hoạt động và lỗi.
21. **Failed Request Tracing** – Ghi lại các yêu cầu lỗi để debug.

---

#### **📈 SEO & Tối ưu**

22. **SEO** – Công cụ hỗ trợ tối ưu hóa công cụ tìm kiếm.

---

### Hosting & DNS

![image](https://hackmd.io/_uploads/SyUY7dlvlg.png)

#### Hosting 

- [Xem ở đây](https://github.com/Vutruongan00/HocViec-NhanHoa/blob/main/PLESK_Panel/1_PLESK%20-%20Admin/%5BHosting%20Services%5D%20_%20Qu%E1%BA%A3n%20l%C3%BD%20d%E1%BB%8Bch%20v%E1%BB%A5%20l%C6%B0u%20tr%E1%BB%AF%20nh%C6%B0%20t%C3%AAn%20mi%E1%BB%81n,%20g%C3%B3i%20d%E1%BB%8Bch%20v%E1%BB%A5,%20v%C3%A0%20kh%C3%A1ch%20h%C3%A0ng.md#4-hosting-settings)

#### DNS - Quản lý các bản ghi DNS cho tên miền

<img width="1411" height="507" alt="image" src="https://github.com/user-attachments/assets/2afc2121-cf25-4b36-8e2b-db3deb262ad1" />

- **Các chức năng chính:**
    - **Add Record**: Thêm bản ghi mới
    - **Disable**: Tắt dịch vụ DNS local của tên miền
    - **Switch to Secondary**: Chuyển qua lại giữa vai trò là server DNS chính, phụ của tên miền đang cấu hình
    - **Reset to Default**: Đặt lại cấu hình bản ghi mặc định, tất cả các bản ghi tuỳ chỉnh sẽ bị xoá.
    - **Remove**: Xoá bản ghi đang được chọn

##### **Add Record**: Thêm bản ghi mới

<img width="730" height="427" alt="image" src="https://github.com/user-attachments/assets/4af87a6f-3ba8-41b1-8e98-883722c95d26" />

##### Settings - Cài đặt các thông số khác

<img width="750" height="648" alt="image" src="https://github.com/user-attachments/assets/674a745f-a4e9-45f2-b1fa-36810d45a51c" />

- Primary Name Server: Là máy chủ DNS chính chịu trách nhiệm quản lý vùng DNS này. Có thể chọn chế độ Autoselect, tức là hệ thống sẽ tự động chọn máy chủ chính. Hoặc lựa chọn thủ công.
- Zone Defaults: TTL (Time To Live): Mặc định là 1 ngày. Đây là thời gian mà bản ghi DNS được lưu trong bộ nhớ cache của các máy chủ DNS khác trước khi được cập nhật lại.
- SOA Record (Start of Authority): SOA là bản ghi đầu tiên trong một vùng DNS, chứa thông tin quản lý vùng. Các thông số:
    - Refresh (3 giờ):Khoảng thời gian máy chủ phụ chờ trước khi kiểm tra lại bản ghi từ máy chủ chính.
    - Retry (1 giờ): Nếu lần kiểm tra trước thất bại, máy chủ phụ sẽ thử lại sau thời gian này.
    - Expire (2 tuần): Nếu không thể liên lạc với máy chủ chính trong thời gian này, máy chủ phụ sẽ ngừng phục vụ bản ghi.
    - Minimum (3 giờ): Thời gian tối thiểu mà các máy chủ DNS khác nên lưu bản ghi trong cache.

- Advanced DNS Features: Tính năng nâng cao
    - Use the serial number format recommended by IETF and RIPE: Tùy chọn này bật định dạng số hiệu bản ghi (serial number) theo chuẩn quốc tế, giúp dễ quản lý và đồng bộ hóa bản ghi DNS.


##### Zone Tranfers: Cấu hình chuyển zone DNS

- Chức năng **Zone Transfers** trong DNS là để cho phép **máy chủ DNS phụ (secondary DNS)** sao chép toàn bộ vùng dữ liệu DNS từ **máy chủ DNS chính (primary DNS)**

---


#### IIS Settings: Cấu hình máy chủ web IIS (dành cho Windows Server).

#### Dedicated IIS Application Pool: Tạo môi trường riêng cho website chạy độc lập.

#### Bandwidth Limiting: Giới hạn băng thông sử dụng.


---

### Mail - Quản lý Mail theo tên miền

- [Click xem tại đây](https://github.com/Vutruongan00/HocViec-NhanHoa/blob/main/PLESK_Panel/1_PLESK%20-%20Admin/%5BHosting%20Services%5D%20_%20Qu%E1%BA%A3n%20l%C3%BD%20d%E1%BB%8Bch%20v%E1%BB%A5%20l%C6%B0u%20tr%E1%BB%AF%20nh%C6%B0%20t%C3%AAn%20mi%E1%BB%81n,%20g%C3%B3i%20d%E1%BB%8Bch%20v%E1%BB%A5,%20v%C3%A0%20kh%C3%A1ch%20h%C3%A0ng.md#2-mail-account---c%E1%BA%A5u-h%C3%ACnh-t%C3%A0i-kho%E1%BA%A3n-mail)

<img width="685" height="224" alt="image" src="https://github.com/user-attachments/assets/ab86fca9-6207-49c1-ad70-12b93021b932" />


---
---



# 4. Subcriptions - Quản lý các gói đăng ký
> Trong giao diện Subscriptions của Plesk, bạn có thể quản lý các gói dịch vụ hosting đã được đăng ký bởi khách hàng hoặc reseller.

![image](https://hackmd.io/_uploads/Bk2KYwgwxg.png)

- **Các chức năng chính**
    * **Add Subscription**: Tạo gói dịch vụ mới cho khách hàng hoặc reseller.
    * **Change Plan**: Thay đổi gói dịch vụ hiện tại (nâng cấp, hạ cấp...).
    * **Change Subscriber**: Chuyển gói dịch vụ sang người dùng khác.
    * **Set Status**: Bật/tắt hoặc tạm ngưng hoạt động của subscription.
    * **Remove**: Xóa gói dịch vụ khỏi hệ thống.

## 


























---
# 5. Service Plans







