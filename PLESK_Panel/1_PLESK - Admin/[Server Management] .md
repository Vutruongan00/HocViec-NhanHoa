> # PLESK ADMIN - SERVER MANAGEMENT
> Quản lý toàn bộ hệ thống máy chủ và các dịch vụ mở rộng trong Plesk. Bao gồm các thành phần:
> - **Tools & Setting**: Khu vực cấu hình hệ thống, bảo mật, email, DNS và các dịch vụ nền
> - **Statics**: Hiển thị thống kê tài nguyên hệ thống( CPU, RAM, traffic)
> - **Extensions:** quản lý việc cài đặt, cập nhật và cấu hình các tiện ích mở rộng
> - **WordPress / Laravel:** công cụ hỗ trợ triển khai và quản lý các ứng dụng web phổ biến.
> - **Monitoring**: giám sát hiệu suất và trạng thái hoạt động của máy chủ.


---

# 1. Tools Settings

<img width="1908" height="921" alt="image" src="https://github.com/user-attachments/assets/a2a01fa0-e8f4-46d0-bba2-4f9db908c59d" />

Giao diện trung tâm để quản trị viên cấu hình máy chủ, dịch vụ hệ thống và tài nguyên. Các thiết lập được phân chia thành các nhóm chức năng:
- Security: thiết lập bảo mật như tường lửa, chứng chỉ SSL, chính sách mật khẩu.
- Assistance and Troubleshooting: công cụ hỗ trợ kiểm tra, sửa lỗi và truy cập tài liệu trợ giúp.
- Tools & Resources: quản lý IP, sao lưu, email hàng loạt, tác vụ định kỳ và di chuyển dữ liệu.
- General Settings: thiết lập chung như thời gian hệ thống, DNS, FTP, PHP, giao diện truy cập.
- Server Management: quản lý dịch vụ hệ thống, thành phần máy chủ, khởi động và tắt máy chủ.
- Mail: cấu hình máy chủ email, bộ lọc spam, antivirus, webmail và SMTP ngoài.
- Applications & Databases: quản lý ứng dụng, cơ sở dữ liệu, ODBC, IIS và ASP.NET.
- Plesk: thông tin bản quyền, nhật ký hành động, cập nhật hệ thống và chính sách truy cập.
- Plesk Appearance: tùy chỉnh giao diện, ngôn ngữ, thương hiệu và nút chức năng.

## **🔐 Security**

### **Security Policy**: Thiết lập chính sách bảo mật cho người dùng.

- Security Policy trong Plesk cho phép quản trị viên thiết lập các chính sách bảo mật quan trọng cho hệ thống.

<img width="764" height="869" alt="image" src="https://github.com/user-attachments/assets/fb3b60af-ec83-4bc7-af15-87a3195ca8e9" />

- **Enhanced Security Mode:** Kích hoạt chế độ bảo mật nâng cao để bảo vệ dữ liệu nhạy cảm.
- **Secure FTP (FTPS Policy):** 
    - Chức năng: Quy định loại kết nối FTP được phép sử dụng.
    -  Các tùy chọn:
        - Chỉ cho phép kết nối FTPS bảo mật.
        - Cho phép cả FTPS và FTP thường.
        - Chỉ cho phép FTP không bảo mật.
        - Cấu hình chính sách riêng cho từng địa chỉ IP.

- **Password Strength**: Thiết lập mức độ mạnh tối thiểu cho mật khẩu người dùng.
    - **Các mức độ:** 
        - Rất yếu / Yếu (không khuyến nghị)
        - Trung bình / 
        - Mạnh (khuyến nghị) /
        - Rất mạnh

- **Custom Handlers Policy:** 
    - Chức năng: Ngăn người dùng thay đổi các thiết lập IIS handler qua file web.config.
    - Tác dụng: Bảo vệ cấu hình máy chủ khỏi bị ghi đè bởi người dùng.



### **Firewall / WAF ModSecurity**: Quản lý tường lửa và bảo vệ ứng dụng web

- **Firewall**: gồm các tab cấu hình:
- <img width="748" height="332" alt="image" src="https://github.com/user-attachments/assets/dbc6070f-bc01-49ab-a218-955226b9ef0c" />
    - **General**: Quản lý trạng thái tường lửa, interface mạng
    - **ICMP Protocol**: Quản lý các tuỳ chọn liên quan tới giao thức ICMP
    - **Firewall Rules**: Quản lý các quy tắc của tường lửa, sửa, thêm mới.
    - <img width="749" height="617" alt="image" src="https://github.com/user-attachments/assets/8a8e5d9a-4f1c-45fe-93e5-83807a0d7c99" />

- **WAF - ModSecurity**: cho phép cấu hình chế độ bảo vệ ứng dụng web khỏi các cuộc tấn công thông qua việc kiểm tra các yêu cầu HTTP. Gồm các chế độ
    - **Off :** tắt
    - **Detection Only** (Chế độ giám sát): 
        - Kiểm tra yêu cầu HTTP theo bộ quy tắc.
        - Nếu phát hiện bất thường, chỉ ghi log lại, không chặn.
        - Các dịch vụ khác như Fail2Ban vẫn có thể xử lý tiếp.
    - **On** (Chế độ bảo vệ đầy đủ): 
        - Kiểm tra yêu cầu HTTP theo bộ quy tắc.
        - Nếu phát hiện vi phạm, ghi log, gửi thông báo, và trả về mã lỗi HTTP để chặn truy cập





### **SSL/TLS Certificates / Let’s Encrypt**
Cài đặt và quản lý chứng chỉ bảo mật.


### **Restrict Creation of Subscriptions**
Giới hạn quyền tạo gói dịch vụ.


### **Additional Administrator Accounts**
Tạo thêm tài khoản quản trị.


### **Active Plesk Sessions**
Theo dõi phiên đăng nhập hiện tại.


### **Azure IPs Support**
Hỗ trợ IP từ Azure.
### ...

---

## **🛠️ Assistance & Troubleshooting**

- Các cấu hình tới dịch vụ hỗ trợ và khắc phục sự cố

    - **Advisor**: Công cụ tư vấn, cung cấp hướng dẫn và lời khuyên để cải thiện hiệu suất hoặc khắc phục sự cố kỹ thuật.
    - **Diagnose & Repair**: Chức năng chẩn đoán và sửa chữa các lỗi hệ thống hoặc phần mềm tự động.
    - **Process List:** Hiển thị danh sách các tiến trình đang chạy trên hệ thống, giúp theo dõi hoạt động và hiệu suất.
    - **Database Process List**: Hiển thị danh sách các tiến trình liên quan đến cơ sở dữ liệu đang hoạt động, hỗ trợ kiểm tra và xử lý các vấn đề liên quan đến truy vấn hoặc hiệu suất.
    - **Forum**: Diễn đàn cộng đồng, nơi người dùng có thể thảo luận, chia sẻ kinh nghiệm và giải pháp cho các vấn đề kỹ thuật.
    - **Help Center**: Trung tâm trợ giúp, cung cấp tài liệu hướng dẫn sử dụng, cấu hình và khắc phục sự cố.
    - **Support**: Dịch vụ hỗ trợ kỹ thuật từ đội ngũ chuyên gia, thường bao gồm các kênh liên hệ như email, chat hoặc ticket.

---
### **Advisor**

**Advisor** là công cụ đánh giá và đề xuất cải thiện bảo mật, hiệu suất và SEO cho máy chủ. Giao diện hiển thị điểm đánh giá tổng thể và các khuyến nghị theo từng nhóm:

<img width="1547" height="847" alt="image" src="https://github.com/user-attachments/assets/ca1c20c0-ff1a-4277-b71a-aca6ad28aa45" />

- **Các nhóm đề xuất:**
    - **Security**: Bật ModSecurity, cài SSL/TLS, dùng Let's Encrypt.
    * **SEO**: Tối ưu hóa website cho công cụ tìm kiếm.
    * **Performance**: Cải thiện tốc độ và tài nguyên hệ thống.
    * **Update**: Đảm bảo phần mềm luôn được cập nhật.
    * **Backup**: Khuyến nghị cấu hình sao lưu định kỳ.

---

### **Diagnose & Repair** 
Chẩn đoán và sửa lỗi hệ thống

<img width="1653" height="852" alt="image" src="https://github.com/user-attachments/assets/c5d1062a-8559-4882-a2e6-244e3ba978d4" />


### **Process List**
Xem danh sách tiến trình đang chạy.

<img width="1653" height="852" alt="image" src="https://github.com/user-attachments/assets/8ac87e3a-29a9-4474-8e29-c8c8ff5ca18e" />

---

## **📦 Tools & Resources**

<img width="398" height="303" alt="image" src="https://github.com/user-attachments/assets/c4c14d09-9504-47ba-aba6-e0f05120611e" />

* **IP Addresses**: Quản lý các địa chỉ IP được sử dụng trong hệ thống, bao gồm thêm, xóa hoặc gán IP cho các dịch vụ.
* **Virtual Host Template**: Mẫu cấu hình cho máy chủ ảo, cho phép thiết lập các thông số mặc định khi tạo website hoặc host mới.
* **Mass Email Messages:** Công cụ gửi email hàng loạt đến nhiều người nhận cùng lúc, thường dùng cho thông báo hệ thống hoặc marketing.
* **Backup Manager:** Quản lý các bản sao lưu dữ liệu, bao gồm tạo bản sao lưu mới, khôi phục từ bản sao lưu cũ và xóa bản sao lưu không cần thiết.
* **Scheduled Tasks (Cron jobs)**: Quản lý các tác vụ định kỳ, cho phép thiết lập các lệnh hoặc script tự động chạy vào thời gian xác định.
* **Task Manager:** Theo dõi và điều khiển các tác vụ đang chạy trên hệ thống, giúp kiểm tra hiệu suất và xử lý các tiến trình bị treo.
* **Event Manager**: Quản lý các sự kiện hệ thống như đăng nhập, thay đổi cấu hình, lỗi... bao gồm ghi nhật ký và xử lý sự kiện.
* **Migration & Transfer Manager**: Quản lý quá trình di chuyển dữ liệu hoặc chuyển giao dịch vụ giữa các hệ thống hoặc máy chủ khác nhau.


---

## **⚙️ General Settings**
<img width="410" height="281" alt="image" src="https://github.com/user-attachments/assets/52995f52-c737-4cb6-908b-c2cfc9843b7e" />

- Server Settings: Cấu hình chung cho máy chủ như hostname và thông tin liên hệ.
- System Time: Thiết lập múi giờ và thời gian hệ thống.
- DNS Settings: Cấu hình DNS mặc định cho các tên miền.
- FTP Settings: Quản lý chế độ hoạt động và cổng của FTP.
- Website Preview: Cho phép xem trước website khi chưa trỏ tên miền.
- PHP Settings: Chọn phiên bản PHP và thiết lập tham số toàn cục.
- Customize Plesk URL: Tùy chỉnh đường dẫn truy cập Plesk.

---
## Server Management

Các cài đặt quản lý máy chủ:
<img width="373" height="260" alt="image" src="https://github.com/user-attachments/assets/ed398378-0b33-4b82-9287-c60218d7ea46" />

- Info and Statistics: Hiển thị thông tin và thống kê hệ thống máy chủ.
- Server Components: Quản lý các thành phần phần mềm đã cài đặt trên máy chủ.
- Services Management: Bật/tắt và kiểm tra trạng thái các dịch vụ hệ thống.
- Restart Server: Khởi động lại toàn bộ máy chủ.
- Shut Down Server: Tắt máy chủ hoàn toàn.
Remote API (REST): Kích hoạt và cấu hình API từ xa để quản lý Plesk qua giao diện lập trình.


---
## **📧 Mail**
<img width="427" height="361" alt="image" src="https://github.com/user-attachments/assets/782e9937-c624-45f3-804e-7ea3946e0870" />

- **Mail Server Settings**: Cấu hình máy chủ gửi/nhận email.
- **Antivirus**: Quét và chặn email chứa mã độc.
- Spam Filter: Lọc thư rác tự động.
- Outgoing Mail Control: Giới hạn số lượng email gửi ra.
- Webmail: Kích hoạt giao diện email trên trình duyệt.
- Download SmarterMail: Tải phần mềm máy chủ email SmarterMail.
- Smarthost: Chuyển tiếp email qua máy chủ trung gian
- External SMTP Server: Cấu hình máy chủ SMTP bên ngoài để gửi email.

---

## **🗃️ Applications & Databases**

<img width="369" height="253" alt="image" src="https://github.com/user-attachments/assets/ab6a0e1c-4c8b-4580-8cf4-26869ec54436" />

* **Application Vault / Database Servers**: Quản lý ứng dụng và máy chủ cơ sở dữ liệu.

---

## Plesk 
Đây là nhóm chức năng liên quan đến thông tin hệ thống, nhật ký hoạt động, cập nhật phần mềm và quản lý giấy phép trong Plesk.

<img width="376" height="284" alt="image" src="https://github.com/user-attachments/assets/205e9781-53e0-436b-a0c3-de035e8bfb35" />

* **Restricted Mode Settings:** Giới hạn quyền và tính năng cho người dùng.
* **Notifications**: Quản lý thông báo hệ thống và cảnh báo.
* **Action Log**: Theo dõi lịch sử thao tác trong Plesk.
* **License Information:** Kiểm tra trạng thái và chi tiết giấy phép.
* **Updates**: Cập nhật phần mềm và thành phần hệ thống.
* **Update Settings**: Cấu hình cách thức cập nhật tự động.
* **About Plesk**: Hiển thị thông tin phiên bản và bản quyền.
* **Cookies in Plesk:** Quản lý cookie được sử dụng trong giao diện.

---

## **🎨 Plesk Appearance**

* **Branding / Languages**: Tùy chỉnh giao diện và ngôn ngữ của Plesk.


---


# 2. Statics

- Hiển thị thống kê về tài nguyên hệ thống như CPU, RAM, lưu lượng.
- Gồm các tab cung cấp
    - Thông tin về máy chủ, domain, traffic
    - Báo cáo chi tiết về hoạt động của máy chủ
- **Overview: Thông tin tổng quan**
- <img width="1903" height="901" alt="image" src="https://github.com/user-attachments/assets/60dcb7d4-0762-4c71-856b-de0342856fbe" />
    - Thông tin về máy chủ cài đặt Ple
    - Thống kê thông số sử dụng `CPU/RAM/Virtual Memory/ Hard Disk`
    - Thông tin về số lượng tên miền đang được thêm.

- **Domain: Thông tin chi tiết về các tên miền đã được cấu hình trong hệ thống**

- **Traffic usage: Thống kê băng thông sử dụng**
- <img width="1646" height="536" alt="image" src="https://github.com/user-attachments/assets/71f869c8-5b4d-450f-9625-a35f50689998" />

    - Có thể xem thông tin theo: tên miền, khách hàng, nhà phân phối
        - Tên miền: Gồm các trường tên miền, chủ sở hữu, băng thông đã dùng, giới hạn, băng thông khả dụng, % đã sử dụn
        - Khách hàng: Gồm các trường: Tên khách hàng, băng thông đã dùng, giới hạn băng thông, băng thông khả dụng, % đã sử dụng
        - Reseller: Gồm các trường: Tên reseller, băng thông đã dùng, giới hạn băng thông, băng thông khả dụng, % đã sử dụng


- **Reseller: Gồm các trường: Tên reseller, băng thông đã dùng, giới hạn băng thông, băng thông khả dụng, % đã sử dụng**

---

# 3. Extensions - Các tiện ích mở rộng

**Extensions** – nơi bạn có thể cài đặt, cập nhật và cấu hình các tiện ích mở rộng để mở rộng chức năng của Plesk.
- <img width="1899" height="901" alt="image" src="https://github.com/user-attachments/assets/398a4388-a00f-475f-bc87-190f79dcc362" />

- Gồm các tab:
    - **Extension Catalog**: Hiển thị tất cả các tiện ích
    - **My Extensions:** Hiển thị các tiện ích đã cài đặt
    - Updates: Hiển thị các bản cập nhật với các tiện ích đã cài, nút chức năng kiểm tra cập nhật/ cập nhật tự động
    - My purchases: Hiển thị các tiện ích đã mua.

---

# 4. WordPress
# 5. Laravel
