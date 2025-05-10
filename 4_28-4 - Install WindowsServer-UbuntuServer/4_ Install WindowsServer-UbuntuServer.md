# **NHỮNG VIỆC ĐƯỢC TRIỂN KHAI**
1. **Tìm hiểu về Domain name và một số nhà cung cấp tên miền tại Việt Nam và quốc tế**
   
2. **Cài đặt Windows Server trên VMWare và tìm hiểu các tập lệnh cơ bản**
   
3. **Cài đặt Ubuntu Server trên VMWare và tìm hiểu các tập lệnh cơ bản**
   
# **ĐÃ HOÀN THÀNH**
## 1. **Domain name?**
- ` `Domain Name (tên miền) là địa chỉ dùng để xác định vị trí của một website trên Internet.
- Nó thay thế cho địa chỉ IP khó nhớ (ví dụ: 172.217.194.100) thành một chuỗi dễ nhớ hơn (ví dụ: google.com).
- Ví dụ:Thay vì gõ 172.217.194.100 vào trình duyệt, bạn chỉ cần gõ google.com.
### 1.1. **Cấu trúc của một Domain Name**
- Domain name thường gồm 3 phần chính, phân tách bằng dấu chấm:

VD: [www.nhanhoa.com](http://www.nhanhoa.com) , trong đó: 

·  www: (không bắt buộc) thường đại diện cho World Wide Web.

·  nhanhoa: tên miền chính (Second-level domain - SLD).

·  com: phần mở rộng (Top-level domain – TLD: .net .org .vn .edu .jp .uk …)

### 1.2. **Quá trình hoạt động của Domain Name**
- Khi bạn nhập một domain vào trình duyệt, hệ thống DNS (Domain Name System) sẽ dịch tên miền thành địa chỉ IP tương ứng để máy tính có thể kết nối đúng máy chủ.
- Quy trình đơn giản:
  - Người dùng nhập google.com.
  - Trình duyệt hỏi hệ thống DNS để tìm địa chỉ IP của google.com.
  - DNS trả về IP (ví dụ: 172.217.194.100).

Trình duyệt dùng IP đó để kết nối tới server chứa website.

### 1.3. **Domain Name được quản lý như thế nào?**
- Tổ chức ICANN (Internet Corporation for Assigned Names and Numbers) chịu trách nhiệm điều phối tên miền toàn cầu.
- Các công ty gọi là nhà đăng ký tên miền (Registrar) được ICANN cấp quyền bán và quản lý tên miền, ví dụ: GoDaddy, Namecheap, Nhân Hòa, v.v.
### 1.4. **Một số lưu ý về Domain Name**
- Tên miền phải duy nhất trên toàn thế giới.
- Tên miền cần được gia hạn định kỳ (thường mỗi năm).
- Có thể mua bán, chuyển nhượng domain name.
### 1.5. **Các nhà cung cấp tên miền phổ biến**
#### a. **Tại Việt Nam**
- Ngoài Công ty **Nhân Hòa** là một trong những nhà đăng ký lớn nhất và đi đầu trong lĩnh vực Tên miền ở Việt Nam được **VNNIC và ICANN** cấp phép, thì bên cạnh đó còn có rất nhiều nhà cung cấp khác cũng có sức cạnh tranh không kém trong mảng này:
- Ví dụ 1 vài cái tên điển hình: PA Việt Nam, Mắt Bão, Viettel IDC, iNET, Tenten, Vietnix, TinoHost,…
#### b. **Quốc tế**

GoDaddy, Namecheap, Bluehost, Google Domain, Cloudflare

## 2. **Install Windows Server 2016 trên VMWare và tìm hiểu các lệnh PowerShell cơ bản**
- **Windows Server** là hệ điều hành do Microsoft phát triển dành riêng cho **máy chủ (server)** – nơi lưu trữ, xử lý dữ liệu và cung cấp dịch vụ mạng (như website, email, file sharing, domain, v.v.) cho nhiều máy tính khác.

### 2.1. **Cài đặt Windows Server trên VMWare**

### 2.2. **Những tính năng chuyên biệt trên Windows Server**

Khác với Windows 10/11 dùng cho máy cá nhân, Windows Server có thêm nhiều tính năng chuyên biệt như:

- **Active Directory (AD)** – quản lý tài khoản người dùng, máy tính trong mạng nội bộ (domain).
- **DNS, DHCP** – cấp phát IP, phân giải tên miền nội bộ.
- **File Server, Print Server** – chia sẻ tập tin và máy in trong mạng.
- **Web Server (IIS)** – chạy website.
- **Remote Desktop Services** – cho phép người dùng truy cập server từ xa.
- **Group Policy** – kiểm soát và cấu hình máy trạm tập trung.
### 2.3. **Một số tập lệnh PowerShell cơ bản**
- **Quản lý Active Directory:** 

  Get-ADUser, New-ADUser, Get-ADGroup, Add-ADGroupMember, ...

|**Lệnh**|**Giải thích**|**Ví dụ**|
| :-: | :-: | :-: |
|Get-ADUser|Lấy thông tin người dùng trong AD|Get-ADUser – “tên người dùng"|
|New-ADUser|Tạo người dùng mới trong domain|New-ADUser -Name "Truong An" -SamAccountName vutruongan -UserPrincipalName vutruongan@domain.local -AccountPassword (Read-Host -AsSecureString "mật\_khẩu") -Enabled $true|
|Get-ADGroup|Lấy thông tin nhóm người dùng trong AD|Get-ADGroup -Filter \*|
|Add-ADGroupMember|Thêm người dùng vào nhóm AD|Add-ADGroupMember -Identity "IT Support" -Members vutruongan|

- **Trong quản trị  Mạng:**

Test-Connection, Get-NetIPAddress, New-NetIPAddress, Get-NetAdapter

|**Lệnh**|**Giải thích**|**Ví dụ**|
| :-: | :-: | :-: |
|Test-Connection|Kiểm tra kết nối mạng (giống ping)|Test-Connection google.com|
|Get-NetIPAddress|Xem địa chỉ IP các card mạng|Get-NetIPAddress|
|New-NetIPAddress|Gán địa chỉ IP tĩnh cho adapter|New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 192.168.1.100 -PrefixLength 24 -DefaultGateway 192.168.1.1|
|Get-NetAdapter|Xem thông tin card mạng|Get-NetAdapter|

- **Trong quản lý dịch vụ:**

  Get-Service, Restart-Service, Set-Service

|**Lệnh**|**Giải thích**|**Ví dụ**|
| :-: | :-: | :-: |
|Get-Service|Xem tất cả dịch vụ trên máy|Get-Service hoặc Get-Service -Name w32time|
|Restart-Service|Khởi động lại dịch vụ|Restart-Service -Name w32time|
|Set-Service|Thay đổi trạng thái khởi động của dịch vụ|Set-Service -Name w32time -StartupType Automatic|

## 3. **Cài đặt Ubuntu Server trên VMWare và tìm hiểu các tập lệnh cơ bản (Đã cài đặt)**
- **Ubuntu Server** là phiên bản hệ điều hành **Ubuntu** được thiết kế tối ưu cho **máy chủ (server)**. Không giống như Ubuntu Desktop, nó **không có giao diện đồ họa (GUI)** mặc định, chỉ sử dụng dòng lệnh (Terminal), giúp nhẹ hơn, tiết kiệm tài nguyên và an toàn hơn cho môi trường server.
- **Những chức năng Ubuntu Server làm được:**
  - **Web server: Apache, Nginx, LAMP stack**
  - **Mail server: Postfix, Dovecot**
  - **File server: Samba, FTP, NFS**
  - **Cloud server: Cài Nextcloud, OpenStack, v.v.**
  - **Database server: MySQL, PostgreSQL, MongoDB**
  - **Ứng dụng doanh nghiệp: ERP, CRM, Docker, Kubernetes...**

### 3.1. **Lệnh về hệ thống**

|**Lệnh**|**Công dụng**|
| :-: | :-: |
|uname -a|Hiển thị thông tin hệ thống|
|hostname|Xem tên máy|
|uptime|Thời gian máy hoạt động|
|whoami|Xem bạn đang đăng nhập bằng user nào|
|date|Hiển thị ngày giờ hiện tại|

### 3.2. **Lệnh quản lý file và thư mục**

|**Lệnh**|**Công dụng**|
| :-: | :-: |
|ls|Liệt kê file/thư mục|
|ls -l|Liệt kê chi tiết|
|cd [thư\_mục]|Di chuyển thư mục|
|pwd|Xem đường dẫn thư mục hiện tại|
|mkdir [tên\_thư\_mục]|Tạo thư mục mới|
|rmdir [tên\_thư\_mục]|Xóa thư mục rỗng|
|touch [tên\_file]|Tạo file rỗng|
|rm [tên\_file]|Xóa file|
|cp [nguồn] [đích]|Sao chép file/thư mục|
|mv [nguồn] [đích]|Di chuyển/đổi tên file/thư mục|
|cat [file]|Xem nội dung file|
|less [file]|Xem file dài trang từng trang|
|head [file]|Xem 10 dòng đầu file|
|tail [file]|Xem 10 dòng cuối file|

### 3.3. **Lệnh về quyền và quyền sở hữu**

|**Lệnh**|**Công dụng**|
| :-: | :-: |
|chmod [quyền] [file]|Đổi quyền truy cập file/thư mục|
|chown [user]:[group] [file]|Đổi chủ sở hữu file/thư mục|

### 3.4. **Lệnh quản lý tiến trình**

|**Lệnh**|**Công dụng**|
| :-: | :-: |
|ps|Xem danh sách tiến trình|
|ps aux|Xem tất cả tiến trình|
|top|Xem tiến trình đang chạy theo thời gian thực|
|kill [PID]|Dừng tiến trình theo PID|
|killall [tên\_tiến\_trình]|Dừng tiến trình theo tên|

### 3.5. **Lệnh nén và giải nén**

|**Lệnh**|**Công dụng**|
| :-: | :-: |
|tar -czvf [tên\_file.tar.gz] [thư\_mục]|Nén thư mục thành file .tar.gz|
|tar -xzvf [tên\_file.tar.gz]|Giải nén file .tar.gz|
|zip [tên\_file.zip] [file/thư\_mục]|Nén file hoặc thư mục thành .zip|
|unzip [tên\_file.zip]|Giải nén file .zip|

### 3.6. **Lệnh quản lý người dùng (cần quyền root)**

|**Lệnh**|**Công dụng**|
| :- | :- |
|adduser [tên\_user]|Tạo user mới|
|passwd [tên\_user]|Đổi mật khẩu user|
|deluser [tên\_user]|Xóa user|

### 3.7. **Một số lệnh về mạng**

|**Lệnh**|**Công dụng**|
| :-: | :-: |
|ping [ip/host]|Kiểm tra kết nối mạng|
|ifconfig|Xem cấu hình mạng (ip a)|
|ip a|Xem địa chỉ IP|
|curl [url]|Gửi yêu cầu HTTP đơn giản|
|wget [url]|Tải file từ URL|

### 3.8. **Một số lệnh khác hay dùng**

|**Lệnh**|**Công dụng**|
| :- | :- |
|history|Xem lịch sử lệnh đã dùng|
|clear|Xóa màn hình Terminal|
|man [lệnh]|Xem tài liệu hướng dẫn của lệnh|
|sudo [lệnh]|Thực thi lệnh với quyền admin|

