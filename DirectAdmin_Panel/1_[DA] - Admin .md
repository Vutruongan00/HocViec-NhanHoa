> # [DA] - Admin

<img width="422" height="506" alt="image" src="https://github.com/user-attachments/assets/8f3406e1-505c-45f0-8996-50bb8ede6b45" />

Các chức năng chính có thể thao tác trên giao diện người dùng quản trị viên **Admin**:

- **Account Manager – Quản lý tài khoản**
- **Server Manager – Quản lý máy chủ**
- **Admin Tools – Công cụ quản trị**
- **System Info & Files – Thông tin hệ thống & tập tin**
- **Extra Features – Tính năng bổ sung**
- **Support & Help – Hỗ trợ**

# 1. Account Manager - Quản lý tài khoản

## **🔹1.1. Add New User – Thêm người dùng mới**
<img width="1919" height="818" alt="image" src="https://github.com/user-attachments/assets/c02701aa-2e30-4e33-9419-22c2902acb2e" />

Giao diện cho phép tạo tài khoản người dùng mới với các trường:

- **Username**: Tên đăng nhập
- **Email**: Địa chỉ email người dùng
- **Password**: Mật khẩu
- **Domain**: Tên miền sẽ gán cho người dùng
- Tùy chọn gửi email thông báo khi tạo tài khoản
- **User Package**: Gói dịch vụ áp dụng

### Customize - thiết lập cấu hình tất cả các thông số
> Các thông số này tương tự như khi tạo Packages (Plan)
<img width="1324" height="819" alt="image" src="https://github.com/user-attachments/assets/11cf1d3f-6061-4805-898c-8b68bac90f26" />


---

## **🔹 1.2. Show All Users – Hiển thị tất cả người dùng**

<img width="1903" height="777" alt="image" src="https://github.com/user-attachments/assets/bfab2905-2758-4728-8f0b-5d57abf820b3" />

- **Thông tin hiển thị cho từng người dùng**
    * **Username**: Tên đăng nhập
    * **Creator**: Người tạo tài khoản (Admin hoặc Reseller)
    * **Bandwidth**: Băng thông đã sử dụng
    * **Disk Usage**: Dung lượng ổ đĩa đã dùng
    * **# of domains**: Số lượng tên miền
    * **Domain(s)**: Danh sách tên miền
    * **IP(s)**: Địa chỉ IP được gán
    * **Sent E-mails**: Số lượng email đã gửi

- **🔹 Thao tác hàng loạt (nút ở góc trên bên phải)**: Khi chọn nhiều người dùng bằng checkbox, bạn có thể:

    * **Send a Message**: Gửi tin nhắn đến người dùng đã chọn
    * **Suspend**: Khóa tài khoản (đình chỉ hoạt động)
    * **Unsuspend**: Mở khóa tài khoản
    * **Delete**: Xóa tài khoản

- **🔹 Thao tác riêng cho từng người dùng (menu thả xuống)**: Bên cạnh mỗi người dùng có menu với các tùy chọn:

    * **Send a Message**: Gửi tin nhắn riêng
    * **Suspend**: Khóa tài khoản
    * **Login as user!**: Đăng nhập vào tài khoản người dùng (dưới quyền admin)
    * **Change user's password**: Đổi mật khẩu
    * **Remove**: Xóa tài khoản

- **--> ✅ Tác dụng của giao diện này**

    * Quản lý tập trung tất cả người dùng trên hệ thống
    * Thực hiện thao tác nhanh chóng mà không cần truy cập từng tài khoản riêng lẻ
    * Hữu ích trong việc giám sát tài nguyên, xử lý sự cố hoặc hỗ trợ người dùng


---

## **🔹 1.3.  My Users – Người dùng của tôi**

Tại giao diện "My Users" trong cấp độ Admin của DirectAdmin, bạn có thể quản lý danh sách người dùng mà bạn trực tiếp tạo hoặc sở hữu.

<img width="1905" height="726" alt="image" src="https://github.com/user-attachments/assets/a1b4dca8-5544-4249-a914-592f2499159b" />

- Hiển thị thông tin cho từng người dùng:
    * **Username**: Tên đăng nhập
    * **Bandwidth**: Băng thông đã sử dụng
    * **Disk Usage**: Dung lượng ổ đĩa đã dùng
    * **# of Domains**: Số lượng tên miền
    * **Domains**: Danh sách tên miền
    * **IP(s)**: Địa chỉ IP được gán
    * **Package**: Gói dịch vụ đang sử dụng

<img width="1371" height="535" alt="image" src="https://github.com/user-attachments/assets/194ec655-2de8-4e28-882d-08c86f79efa3" />

- Các thao tác có thể thực hiện**
    - **Send a Message** – Gửi tin nhắn đến người dùn**Suspend** – Khóa tài khoản (đình chỉ hoạt động)
    - **Unsuspend** – Mở khóa tài khoản
    - **Change Package** – Thay đổi gói dịch vụ đang sử dụng
    - **Change IP** – Gán địa chỉ IP khác cho người dùng
    - **Delete** – Xóa tài khoản người dùng

### MODIFY YOUR OWN USER DATA

Nút này có thể dùng để chỉnh sửa thông tin hoặc cấu hình của chính tài khoản người dùng hiện tại đang đăng nhập.

<img width="1406" height="892" alt="image" src="https://github.com/user-attachments/assets/1d311f7a-6b80-4d37-93ec-c5245dc23689" />

- Tại đây có thể cấu hình:
    - **Change Package**: Thay đổi gói được cấu hình cho user hiện tại
    - **Change IP**: Thay đổi địa chỉ IP được gán
    - **Manual Change Settings**: Cấu hình các thông số khác (chi tiết trong phần **Manually Change Settings** dưới đây)


---

## **🔹 1.4.  Manage User Packages – Quản lý gói người dùng**

Cho phép tạo, chỉnh sửa, xóa các gói dịch vụ (package) dành cho người dùng. Mỗi gói có thể quy định:
* Dung lượng ổ đĩa
* Băng thông
* Số lượng email, database, subdomain...
* Quyền truy cập các tính năng

<img width="1919" height="800" alt="image" src="https://github.com/user-attachments/assets/81189b9e-1ea5-4b71-a8ce-42965a69eb7f" />

### Add Package – Thêm gói dịch vụ mới:
<img width="1463" height="909" alt="image" src="https://github.com/user-attachments/assets/43db24b4-0260-484e-9feb-5acaf3683709" />

- **Thông số tài nguyên:**
    - **Bandwidth (MB)**: → Giới hạn băng thông hàng tháng (tính bằng MB)
    - **Disk Space (MB)**: → Dung lượng ổ đĩa được cấp cho người dùng
    - **Inode**: → Số lượng file/tập tin tối đa 

- **Tên miền và dịch vụ liên quan:** số lượng **Domain/ Subdomain/ Domain Pointers**
- **Email:** 
    - **Email Accounts**: → Số lượng tài khoản email
    - **Email Forwarders**: → Số lượng email chuyển tiếp
    - **Mailing Lists**: → Danh sách gửi thư
    - **Autoresponders**: → Trả lời tự động
    - **Email Daily Limit**:  Số lượng email được gửi mỗi ngày 

- **Cơ sở dữ liệu và FTP:** số lượng CSDL MySQL/ tài khoản FTP/ bật tắt FTP ẩn danh
    
- **Cấu hình tính năng và quyền truy cập**

    <img width="1458" height="914" alt="image" src="https://github.com/user-attachments/assets/04cbaaa4-b130-4ef2-88fc-b4acedf76849" />
    > Có thể bật/tắt các tính năng sau cho gói dịch vụ:


    * **ClamAV** – Quét virus email
    * **PHP Access** – Cho phép sử dụng PHP
    * **SpamAssassin** – Lọc thư rác
    * **Catch-All E-mail** – Email bắt tất cả
    * **SSL Access** – Cho phép sử dụng SSL
    * **SSH Access** – Truy cập SSH
    * **Cron Jobs** – Tác vụ định kỳ
    * **Reseller** – Cho phép người dùng trở thành đại lý
    * **System Info** – Xem thông tin hệ thống
    * **Login Keys** – Quản lý khóa đăng nhập
    * **DNS Control** – Quản lý DNS
    * **Suspend at Limit** – Tự động khóa khi vượt giới hạn
    * **Automatic security updates (RPM/YUM)** – Cập nhật bảo mật tự động
    * **Jailed Shell Access** – Truy cập shell bị giới hạn

- **Giao diện và ngôn ngữ**
    - Skin – Chọn giao diện người dùng
        - **`Enhanced`** - **Giao diện Clasic** (cũ)
        - **`Evolution`** - **giao diện hiện đại** (mới)
    - Language – Chọn ngôn ngữ hiển thị

- **Feature Sets**: Cho phép tất cả lệnh hoặc chỉ 1 số tính năng cụ thể
- **Plugins Allow/Deny:** Quy định plugin nào được phép sử dụng hoặc bị chặn

<img width="784" height="399" alt="image" src="https://github.com/user-attachments/assets/fddb9283-2976-4878-b94c-c14ba490cd53" />


### Import / Export Package

<img width="1909" height="755" alt="image" src="https://github.com/user-attachments/assets/2ae537ab-f07a-488d-9451-794c7c4f192d" />

- **Export Package:** Tick chọn vào Package cần Export --> Chọn **Export** --> Lưu lại mã dữ liệu export  

- **Import Package**: click chọn **Import Package** và nhập lại nội dung gói đã Export


---

## **🔹 1.5.  Move Users between Resellers – Di chuyển người dùng giữa các đại lý**

Chức năng này cho phép chuyển tài khoản người dùng từ một đại lý (reseller) sang đại lý khác mà không mất dữ liệu.

<img width="1895" height="873" alt="image" src="https://github.com/user-attachments/assets/4ecb640a-13ae-4f19-b5df-8f9eb6454709" />

---

## **🔹 1.6. Edit User Message – Chỉnh sửa thông báo người dùng**

Cho phép thay đổi nội dung email hoặc thông báo hệ thống gửi đến người dùng khi tài khoản được tạo, bị khóa, hoặc có thay đổi.

<img width="1894" height="847" alt="image" src="https://github.com/user-attachments/assets/5f910e3a-0aa4-42b1-b9d1-96002ba7da89" />

---

## **🔹 1.7. Change Passwords – Đổi mật khẩu**

Giao diện để đổi mật khẩu cho người dùng bất kỳ. Có thể chọn người dùng từ danh sách và nhập mật khẩu mới.

<img width="1903" height="572" alt="image" src="https://github.com/user-attachments/assets/e8c3709f-d368-4fbd-bdd0-1b7d8146f6b9" />

---

## **🔹 1.8. Manage Reseller Packages – Quản lý gói đại lý**

Tạo và chỉnh sửa các gói dịch vụ dành cho đại lý **reseller**, quy định tài nguyên mà họ có thể phân phối cho người dùng cấp dưới.

<img width="1919" height="896" alt="image" src="https://github.com/user-attachments/assets/113a1986-86a8-4fb6-bc5c-e4da0c5cd3a4" />

### CREATE PACKAGE – Tạo gói reseller mới:
> Các thông số Tương tự như như khi tạo gói  cho User
- Khác biệt trong gói Reseller so với User
- <img width="1906" height="894" alt="image" src="https://github.com/user-attachments/assets/946a187e-0b69-413f-9612-a0b9c41c43c5" />
    - **User Accounts**: số lượng người dùng Reseller có thể tạo 
    - **IP(s)**: Số lượng địa chỉ IP được cấp.
    - **Overselling Allowance** → Cho phép reseller phân phối tài nguyên vượt quá giới hạn thực tế (nếu bật)
    - **DNS Control nâng cao** → Bao gồm:
        - Personal DNS Settings: Cho phép reseller cấu hình DNS riêng
        - Shared Server IP Settings: Cho phép dùng IP chia sẻ
        - Allow reseller to create shared IPs: Cho phép reseller tự tạo IP chia sẻ
    
<img width="907" height="199" alt="image" src="https://github.com/user-attachments/assets/93a16d1c-e551-4f5c-80f6-bf4cd46ca4ce" />


---

## **🔹 1.9. Create Reseller – Tạo đại lý**

Tạo tài khoản đại lý (reseller) mới, tương tự như "Add New User" nhưng có thêm quyền quản lý người dùng cấp dưới.

<img width="1916" height="851" alt="image" src="https://github.com/user-attachments/assets/de7a2cce-15b5-480c-b513-d65a29d38232" />

- **Các trường thông tin cần nhập**
    - **Username** – Tên đăng nhập của reselle
    - **Email** – Địa chỉ email liên hệ
    * **Password** – Mật khẩu đăng nhập
    * **Domain** – Tên miền chính của reseller
    * **Package Plan** – Gói dịch vụ reseller sẽ sử dụng

<img width="1407" height="205" alt="image" src="https://github.com/user-attachments/assets/702984b3-2967-4d48-b1d5-206270dc398a" />

- Tùy chọn cách thức cấp địa chỉ IP cho Reseller
    - **Shared – Reseller's IP** : Reseller sẽ dùng chung IP với các tài khoản người dùng mà họ tạo. IP này là IP đã được gán cho reseller.
    - **Shared – Server**: Reseller dùng chung IP với toàn bộ hệ thống (IP mặc định của máy chủ). 
    - **Assigned:** → Gán một IP riêng biệt cho reseller. IP này sẽ không dùng chung với ai khác, giúp tăng tính độc lập và bảo mật.


- Các nút phụ trợ (góc trên bên phải)
    - **Edit Reseller Message** – Chỉnh sửa nội dung thông báo gửi đến reseller
    - **Manage Reseller Packages** – Quản lý các gói dịch vụ dành cho reseller


---

## **🔹 1.10. List Resellers – Danh sách đại lý**

Hiển thị tất cả tài khoản reseller trên hệ thống, kèm theo số lượng người dùng họ quản lý, trạng thái tài khoản...



---

## **🔹 1.11. Create Administrator – Tạo quản trị viên**

Tạo tài khoản quản trị viên mới có toàn quyền trên hệ thống.

<img width="1919" height="790" alt="image" src="https://github.com/user-attachments/assets/f029497c-d90f-4b4c-81de-b9ad7207a68c" />

---

## **🔹 1.12. List Administrators – Danh sách quản trị viên**

Hiển thị tất cả tài khoản admin, cho phép chỉnh sửa, xóa hoặc khóa tài khoản.

<img width="1920" height="746" alt="image" src="https://github.com/user-attachments/assets/920ba91b-a7f0-4ec7-bd71-579769a0604a" />

---

## **🔹 1.13. Suspension Message – Tin nhắn khi bị khóa**

Cho phép chỉnh sửa nội dung thông báo hiển thị cho người dùng khi tài khoản của họ bị khóa hoặc đình chỉ.

<img width="1920" height="836" alt="image" src="https://github.com/user-attachments/assets/49310957-8718-479a-bc67-4c9cb59ecc1e" />

---
---

# 2. Server Manager - Quản lý máy chủ

Phần **Server Manager** trong giao diện Admin của DirectAdmin là nơi quản trị viên thực hiện các thiết lập và quản lý trực tiếp các dịch vụ cốt lõi chạy trên máy chủ (như web server, DNS server, PHP, v.v.), cấu hình mạng, và các thiết lập chung ảnh hưởng đến tất cả các tài khoản trên server. Đây là nơi bạn thực hiện các tác vụ như quản lý IP, cấu hình DNS, thiết lập chứng chỉ SSL cho server, hoặc điều chỉnh các thông số PHP.

<img width="1429" height="304" alt="image" src="https://github.com/user-attachments/assets/ab13f330-844b-45af-9a67-623422e77d01" />
* Server Manager trong DirectAdmin là nơi quản trị viên quản lý các cài đặt và dịch vụ cốt lõi của toàn bộ máy chủ hosting. Các mục chính:

  * Administrator Settings: Cấu hình chung của DirectAdmin và giới hạn server.
  * Custom HTTPD Configurations: Tùy chỉnh máy chủ web (Apache/Nginx).
  * DNS Administration: Quản lý các bản ghi DNS cho tên miền.
  * IP Management: Thêm, xóa, quản lý địa chỉ IP của server.
  * Name Servers: Thiết lập các máy chủ tên miền chính (ns1, ns2...).
  * Multi Server Setup: Cấu hình cho nhiều máy chủ DirectAdmin hoạt động cùng nhau.
  * PHP Configuration: Quản lý phiên bản và cài đặt PHP.
  * Server TLS Certificate: Cài đặt chứng chỉ SSL cho bảng điều khiển và dịch vụ server.
  * CustomBuild: Công cụ để cài đặt, cập nhật, biên dịch lại các phần mềm quan trọng của máy chủ (Apache, PHP, MySQL...).

## 2.1. Administrator Settings:

Trong phần **Administrator Settings** của DirectAdmin, giao diện được chia thành nhiều tab cấu hình để quản trị viên dễ dàng kiểm soát các khía cạnh khác nhau của hệ thống:

---

### **🔹  General Settings – Cài đặt chung**

<img width="1481" height="812" alt="image" src="https://github.com/user-attachments/assets/eba34e68-7e4e-4f7d-9474-3e8d00e7a184" />

Tập trung vào các thiết lập cơ bản và hành vi hệ thống như:

* Thông báo downtime
* Cho phép overselling
* Tự động khóa reseller khi vượt giới hạn
* Cho phép người dùng tự sao lưu
* Tự động cập nhật bộ lọc và bản vá

---

### **🔹  Server Settings – Cài đặt máy chủ**

<img width="1525" height="852" alt="image" src="https://github.com/user-attachments/assets/ba0de828-d082-4c9d-b425-a49693ea3d3c" />


* **Hostname & Port** – Cấu hình tên máy chủ và cổng truy cập DirectAdmin
* **Time & Locale** – Thiết lập múi giờ và định dạng ngôn ngữ hệ thống
* **Resource Limits** – Giới hạn tài nguyên hệ thống như RAM, CPU
* **Service Control** – Quản lý trạng thái các dịch vụ hệ thống
* **System Paths** – Đường dẫn đến các thư mục hệ thống quan trọng
* **Auto-clean & Logs** – Cài đặt tự động dọn dẹp và lưu nhật ký hệ thống

---

### **🔹  Security Settings – Cài đặt bảo mật**

<img width="1536" height="858" alt="image" src="https://github.com/user-attachments/assets/21b4c500-526d-40eb-974a-ae0be13f8f33" />

<img width="1482" height="808" alt="image" src="https://github.com/user-attachments/assets/4e36960d-6241-42a6-bde7-65da0d79807a" />

* **Brute Force Monitor** – Giám sát và ngăn chặn các cuộc tấn công dò mật khẩu
* **Login Attempts Threshold** – Giới hạn số lần đăng nhập sai trước khi bị khóa
* **Blacklisted IPs** – Chặn truy cập từ các địa chỉ IP cụ thể
* **Whitelisted IPs** – Cho phép truy cập từ các IP được tin cậy
* **Brute Force Log Expiry** – Thiết lập thời gian lưu nhật ký tấn công brute-force
* **Plugin Updates Schedule** – Lên lịch kiểm tra và cập nhật plugin tự động
* **Session Timeout Interval** – Tự động ngắt phiên làm việc sau thời gian không hoạt động
* **Reset Passwords Interval** – Yêu cầu người dùng đổi mật khẩu định kỳ
* ...


---

### 🔹 E-Mail Settings

<img width="1451" height="670" alt="image" src="https://github.com/user-attachments/assets/0eed117b-df79-4268-b774-e91c77fb42df" />

* **User Email Limits** – Cho phép người dùng tự đặt giới hạn email cho tài khoản của họ
* **Daily Email Limit per Mailbox** – Giới hạn số email gửi mỗi ngày cho từng hộp thư
* **Daily Email Limit per User** – Giới hạn tổng số email gửi mỗi ngày cho toàn bộ tài khoản
* **Purge Spam & Trash** – Tự động xóa email cũ trong thư mục Spam và Thùng rác
* **RBL Blocking in Exim** – Chặn email từ các IP nằm trong danh sách đen (RBL)

<img width="1450" height="406" alt="image" src="https://github.com/user-attachments/assets/99d5f800-59cd-4874-b3d9-720c7dedeb75" />

* **Blocked Users** – Chặn toàn bộ tài khoản người dùng khỏi việc gửi email
* **Blocked Mailboxes** – Chặn từng hộp thư cụ thể khỏi việc gửi email

---

### **🔹 Database Service**

* **General Feature Availability** – Bật/tắt dịch vụ quản lý cơ sở dữ liệu

<img width="1428" height="330" alt="image" src="https://github.com/user-attachments/assets/e0be1d8a-6e69-4fc7-84ce-ff95581b4207" />

* **Connection Configuration** – Cấu hình địa chỉ socket, tài khoản kết nối đến MySQL/MariaDB

<img width="1422" height="497" alt="image" src="https://github.com/user-attachments/assets/34b751ed-9105-4a5f-a0cc-6295032999c4" />

* **Default Server Roles** – Chọn máy chủ chính và phụ để thực hiện thao tác quản lý database
* **Access Hosts Limit** – Giới hạn số lượng host được phép truy cập vào mỗi tài khoản database
* **Extra Export Arguments** – Thêm tham số cho lệnh xuất dữ liệu (mysqldump/mariadb-dump)
* **Database Export Timeout** – Thiết lập thời gian tối đa cho quá trình xuất dữ liệu

<img width="1420" height="781" alt="image" src="https://github.com/user-attachments/assets/cd1b29be-2c26-4d4f-83ec-1eead766cbee" />

## 2.2. Custom HTTPD Configurations

Custom **HTTPD Configurations** trong **DirectAdmin** cho phép quản trị viên tùy chỉnh cấu hình web server (Apache) cho từng domain hoặc người dùng. 

<img width="1822" height="848" alt="image" src="https://github.com/user-attachments/assets/d9c54e91-3783-4360-a763-cd67ba430ff6" />

- **Chức năng chính**

    * **Quản lý file cấu hình HTTPD** – Hiển thị danh sách các domain, người dùng và file cấu hình liên quan như `httpd.conf`, `php-fpm.conf`
    * **Tùy chỉnh cấu hình riêng** – Cho phép chỉnh sửa cấu hình web server cho từng domain mà không ảnh hưởng đến toàn hệ thống
    * **DA BUILD REWRITE\_CONFS** – Nút để rebuild lại toàn bộ file cấu hình HTTPD nếu có thay đổi
    

---
## 2.3 DNS Administration

<img width="1917" height="871" alt="image" src="https://github.com/user-attachments/assets/42c7b27b-3301-48d5-af18-94c8ddc6961d" />

- **Chức năng chính**
    * **Danh sách tên miền** – Hiển thị tất cả các domain đang được quản lý DNS trên hệ thống
    * **Local Data** – Cho biết hệ thống có lưu bản ghi DNS cho domain đó hay không
    * **Local Mail** – Cho biết hệ thống có xử lý email cho domain đó hay không
    * **Bộ lọc tìm kiếm** – Lọc domain theo tên hoặc trạng thái Local Data / Local Mail
    * **Add DNS Zone** – Tạo mới vùng DNS cho một domain chưa có trên hệ thống
    * **Quản lý bản ghi DNS** – Truy cập để chỉnh sửa các bản ghi như A, MX, CNAME, TXT...

### Add DNS Zone

<img width="1382" height="549" alt="image" src="https://github.com/user-attachments/assets/33202df9-e2e4-4f6d-a132-1c512a2ba9c1" />

* Việc tạo DNS Zone là bước nền tảng để DirectAdmin có thể quản lý tất cả các bản ghi DNS (như A, CNAME, MX, TXT,...) cho tên miền đó.
* Các trường cần điền:

  * **Domain Name**: Tên miền mà bạn muốn quản lý DNS. Nhập tên miền của bạn mà không có [www](http://www/). hay dấu chấm cuối cùng.
  * **IP Address**: Địa chỉ IP chính mà tên miền này sẽ trỏ tới. Thường là địa chỉ IP của máy chủ hosting hoặc máy chủ VPS/Dedicated của bạn.
  * **Name Server 1**:Tên của máy chủ DNS chính (primary nameserver) sẽ chịu trách nhiệm phân giải tên miền của bạn.
  * **Name Server 2**: Tên của máy chủ DNS phụ (secondary nameserver) để dự phòng.
  * **Create Reverse IP Lookup**: Tùy chọn để tạo bản ghi PTR (Pointer Record) cho địa chỉ IP, dùng cho Reverse DNS. Điều này hữu ích cho việc gửi email để chống spam.
* Sau khi điền đủ thông tin: Nhấn nút CREATE để tạo DNS Zone.

---

### Edit DNS Records
- Click chọn vào domain cần chỉnh sửa, tại đây bạn có thể tạo bản ghi mới, Sửa, Xóa bản ghi

<img width="1368" height="774" alt="image" src="https://github.com/user-attachments/assets/f4e445c6-d855-417c-b207-8503848b56c2" />

### Các phím chức năng khác:
* **Reset Defaults** – Khôi phục cấu hình DNS mặc định
* **Override TTL** – Ghi đè thời gian TTL cho các bản ghi

#### **DNSSEC** – Quản lý bảo mật DNS nâng cao
> DNSSEC (Domain Name System Security Extensions) là một tính năng bảo mật nâng cao trong hệ thống DNS, giúp đảm bảo rằng các bản ghi DNS không bị giả mạo hoặc thay đổi trong quá trình truyền tải.

<img width="1383" height="656" alt="image" src="https://github.com/user-attachments/assets/8ccfb342-66cf-43f0-90f6-41a644ffb39d" />

<img width="1303" height="390" alt="image" src="https://github.com/user-attachments/assets/ec96d9b2-ce68-4c6a-9737-2d20a8b270f6" />

<img width="1372" height="749" alt="image" src="https://github.com/user-attachments/assets/4902760b-d7a6-46e0-bb28-4738937700b3" />

- Sau khi Kích hoạt DNSSEC như các bước trên, bạn sẽ thấy bản ghi DS được tạo. 

<img width="1374" height="849" alt="image" src="https://github.com/user-attachments/assets/4025aac1-956e-4979-b082-a6543ff16df2" />

- --> **Bạn cần copy bản ghi DS và cập nhật nó ở nơi quản lý tên miền gốc** (Ví dụ: Nhân Hòa,...)

- **Kiểm tra trạng thái DNSSEC**: Cái này các bạn tự google nha (gợi ý công cụ [Link](https://dnssec-analyzer.verisignlabs.com))



---

## 2.4. IP Management

Giao diện xem, thêm, và quản lý các địa chỉ IP được sử dụng trên máy chủ.

<img width="1813" height="737" alt="image" src="https://github.com/user-attachments/assets/b380a27b-aeb9-4b6d-bfb6-16cf00fb1750" />

* Các nút hành động 

  * Assign: Gán địa chỉ IP đã chọn cho một Reseller hoặc người dùng cụ thể.
  * Remove from reseller: Gỡ bỏ địa chỉ IP khỏi một Reseller.
  * Clear NS: Xóa cấu hình Nameserver liên quan đến IP (nếu có).
  * Un-Set Global: Bỏ trạng thái IP "Global" (nếu có), có thể liên quan đến việc nó được dùng chung hay riêng.
  * Share: Đặt IP ở chế độ chia sẻ (cho phép nhiều tài khoản sử dụng).
  * Free: Giải phóng IP, làm cho nó có sẵn để gán lại.
  * Delete: Xóa địa chỉ IP khỏi hệ thống. Cực kỳ cẩn thận khi sử dụng chức năng này, vì việc xóa một IP đang sử dụng có thể làm ngừng hoạt động các dịch vụ liên quan.


-  **Add IP**: Dùng để thêm một địa chỉ IP mới vào máy chủ. Khi nhấp vào, bạn sẽ được yêu cầu nhập địa chỉ IP, netmask.

<img width="574" height="418" alt="image" src="https://github.com/user-attachments/assets/c7a3d29f-eefe-4cbc-96f0-fbb0c8a22f8e" />

---

## 2.5. Name Servers 
Qn lý và thiết lập các máy chủ tên miền (name servers) dùng để phân giải DNS cho các domain trên hệ thống. Đây là nơi bạn cấu hình các name server mặc định sẽ được gán cho người dùng mới.

<img width="1889" height="858" alt="image" src="https://github.com/user-attachments/assets/5a547afa-d5a5-400b-bfd4-df84b9e7505f" />

Tại giao diện của **Name Server** sẽ hiển thị:
* **Danh sách IP** – Hiển thị các IP đang được dùng làm name server
* **Trạng thái IP** – Cho biết IP đang gán cho server hay user
* **Số lượng user** – Bao nhiêu người dùng đang sử dụng IP đó
* **Tạo name server mới** – Nút `CREATE NAME SERVERS` để thêm name server
* **Thiết lập mặc định cho user mới** – Nhập `Name Server 1` và `Name Server 2` để gán tự động cho tài khoản mới
* **Xóa name server** – Tùy chọn xóa các name server đã chọn

### CREATE NAME SERVERS

<img width="863" height="479" alt="image" src="https://github.com/user-attachments/assets/0fd00e80-d521-4cd3-8fd9-e0296254490f" />

- **Chức năng CREATE NAME SERVERS dùng để làm gì?**

    * Tạo các bản ghi A cho `ns1.yourdomain.com` và `ns2.yourdomain.com`
    * Gán các IP cụ thể cho các name server này
    * Cho phép domain của bạn **tự làm name server riêng**, thay vì dùng của nhà cung cấp

- **Khi nào cần dùng?**
    * Khi bạn muốn **tự quản lý DNS** cho các domain khác (reseller, hosting riêng)
    * Khi bạn muốn tạo **dịch vụ hosting** và cấp phát domain cho người dùng
    * Khi bạn cần **tách biệt hoàn toàn** khỏi name server của nhà cung cấp

---

## 2.6. Multi Server Setup

Tính năng Multi Server Setup trong DirectAdmin cho phép người quản trị (Admin) quản lý và điều khiển nhiều máy chủ DirectAdmin khác nhau từ một giao diện duy nhất. Điều này đặc biệt hữu ích cho các nhà cung cấp dịch vụ hosting hoặc người dùng có nhu cầu quản lý nhiều máy chủ một cách tập trung, giúp tiết kiệm thời gian và tối ưu hóa quy trình quản lý.

---
## 2.7. PHP Configuration
Quản lý các thiết lập liên quan đến PHP cho từng domain trên hệ thống, đặc biệt là cấu hình bảo mật như **Open BaseDir**.


<img width="1899" height="891" alt="image" src="https://github.com/user-attachments/assets/89951089-b1d4-4d2b-bfe8-905ce140fef2" />

* Open BaseDir:

  * Là một chỉ thị bảo mật trong PHP (cụ thể là trong php.ini) giới hạn các thư mục mà PHP có thể truy cập. Khi open\_basedir được bật và cấu hình, các tập lệnh PHP chỉ có thể tương tác (đọc, ghi, mở...) với các tệp trong thư mục được chỉ định hoặc thư mục con của nó.
  * Mục đích: Ngăn chặn các tập lệnh độc hại truy cập vào các tệp bên ngoài thư mục gốc của trang web (ví dụ: các tệp hệ thống quan trọng, tệp của người dùng khác), từ đó nâng cao bảo mật cho máy chủ shared hosting.
  * Trạng thái trong DirectAdmin:

    * "Open BaseDir" (với dấu tích màu xanh): Có nghĩa là tính năng open\_basedir đang được bật cho tên miền đó. Điều này giúp tăng cường bảo mật.
    * Không có dấu tích: Có nghĩa là open\_basedir đang tắt cho tên miền đó, cho phép tập lệnh PHP truy cập vào bất kỳ thư mục nào trên hệ thống (nếu không có các hạn chế bảo mật khác), điều này tiềm ẩn rủi ro bảo mật cao hơn.

---

## 2.8. Server TLS Certificate

Quản lý chứng chỉ bảo mật TLS/SSL cho máy chủ, giúp mã hóa kết nối HTTPS giữa người dùng và giao diện DirectAdmin hoặc các dịch vụ khác trên server.

<img width="1833" height="884" alt="image" src="https://github.com/user-attachments/assets/816809bf-5b47-41f1-933c-9f92a13e8363" />

-  **Các chức năng có trong giao diện này**

    * **Trạng thái chứng chỉ**

      * **Key File** & **Certificate File**: Kiểm tra tính hợp lệ của khóa và chứng chỉ
      * **DirectAdmin Panel Over HTTPS**: Cho biết giao diện quản trị có đang dùng HTTPS hay không
      * **Automatic Certificates**: Tự động gia hạn chứng chỉ (có nút `RENEW NOW` để làm thủ công)
    * **Thông tin chứng chỉ chính**

      * **Issuer**: Nhà phát hành chứng chỉ (ví dụ: Let's Encrypt)
      * **Ngày tạo & ngày hết hạn**: Theo dõi thời gian hiệu lực
      * **Tên miền/IP áp dụng**: Domain hoặc IP mà chứng chỉ bảo vệ
      * **Loại khóa**: Ví dụ `ECDSA P-256` – thuật toán mã hóa
      * **Serial Number**: Mã định danh duy nhất của chứng chỉ
    * **Chain Certificates**

      * Là các chứng chỉ trung gian giúp trình duyệt xác thực chứng chỉ chính thông qua một chuỗi tin cậy


---

# 3. Admin Tools
