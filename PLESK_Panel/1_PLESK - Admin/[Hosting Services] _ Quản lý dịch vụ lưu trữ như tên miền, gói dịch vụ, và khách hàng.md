> # PLESK ADMIN - HOSTING SERVICES
> <img width="697" height="378" alt="image" src="https://github.com/user-attachments/assets/f869eced-a361-4f71-8dd0-26aeff956eab" />
> - **Customers**: Quản lý tài khoản khách hàng sử dụng dịch vụ hosting.
> - **Resellers**: Quản lý đại lý phân phối dịch vụ (reseller).
> - **Domains**: Quản lý tên miền được cấp phát cho khách hàng.
> - **Subscriptions**: Quản lý gói dịch vụ cụ thể của từng khách hàng.
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





























----
# 3. Domain

----
# 4. Subcriptions


---
# 5. Service Plans


