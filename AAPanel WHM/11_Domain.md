
# DOMAINS - Quản lý tên miền
- Các chức năng chính:
    - Quản lý tên miền từ nhiều nhà cung cấp DNS khác nhau.
    - Thêm, sửa, xóa bản ghi DNS (A, CNAME, MX, TXT…).
    - Tích hợp API của nhà cung cấp DNS để đồng bộ tự động.
    - Quản lý chứng chỉ SSL cho từng tên miền

## 1. Tích hợp API của nhà cung cấp DNS - Integrate DNS Provider API 
- Bạn có thể thêm API của các nhà cung cấp như:
    - Cloudflare
    -  DNSPod
    -  Aliyu
    -  Google Domains…
    
![image](https://github.com/user-attachments/assets/7076c682-de33-4328-b9f1-427f83119e92)

- **Cách thêm**: Vào **Domains** → **Add DNS Provider** nhập:
    - **Tên nhà cung cấp**
    - **API Key / Token**
    - **Alias** (bí danh để phân biệt)
    - **Quyền truy cập API** (toàn quyền hoặc giới hạn)


## 2. Quản lý tên miền - Domain Management

- Tại đây:
    - Hiển thị danh sách tên miền đã kết nối.
    - Xem số lượng bản ghi DNS, trạng thái SSL, thời gian hết hạn.
    - **Quản lý bản ghi DNS**

## 3. Quản lý bản ghi DNS

- Bạn có thể thêm/sửa/xóa các bản ghi như:
    - **A** Record: trỏ tên miền về địa chỉ IPv4.
    - **AAAA** Record: trỏ về địa chỉ IPv6.
    - **CNAME**: trỏ tên miền phụ về tên miền chính.
    - **MX**: cấu hình máy chủ email.
    - **TXT**: dùng cho xác minh SPF, DKIM, v.v.

![image](https://github.com/user-attachments/assets/4e290f53-6eb5-4e6f-9e37-4c26e0466058)

## 4. Quản lý SSL
- Hiển thị thời gian hết hạn chứng chỉ SSL của từng tên miền.
- Cho phép:
    - **Đăng ký SSL mới** (Let's Encrypt)
    - **Gia hạn SSL**
    - **Tải lên chứng chỉ SSL thủ công**

![image](https://github.com/user-attachments/assets/5eb2cdb1-e112-4cfa-a564-cd43c4d9fee4)

- Bảng mô tả các chức năng quản lý SSL của Domain:

| **Chức năng**             | **Mô tả**                                                  |
| ------------------------- | ---------------------------------------------------------- |
| **Apply for SSL**         | Đăng ký chứng chỉ SSL mới (hỗ trợ wildcard & đa tên miền). |
| **Upload Certificate**    | Tải lên chứng chỉ SSL có sẵn.                              |
| **One-Click Renewal**     | Gia hạn chứng chỉ sắp hết hạn trong vòng 30 ngày.          |
| **Certificate Brand**     | Nhà cung cấp chứng chỉ SSL.                                |
| **Authenticated Domain**  | Danh sách domain đã xác thực.                              |
| **Validity Period**       | Thời hạn hiệu lực của chứng chỉ.                           |
| **Automatic Renewal**     | Bật/tắt tự động gia hạn SSL.                               |
| **Expiration Warning**    | Bật/tắt cảnh báo hết hạn.                                  |
| **Installation Location** | Vị trí đã triển khai chứng chỉ.                            |
| **Last Application Time** | Thời gian đăng ký SSL gần nhất.                            |
| **Last Application Log**  | Nhật ký đăng ký SSL lần gần nhất.                          |
| **Manage**                | Quản lý chứng chỉ: gán vào website, gia hạn...             |
| **View**                  | Xem chi tiết chứng chỉ.                                    |
| **Delete**                | Xóa chứng chỉ khỏi hệ thống.                               |


### Apply for SSL Let's Encrypt

![image](https://github.com/user-attachments/assets/99f5f299-361a-4d2b-8fc7-6efa741e9998)


- Phương thức xác minh:
    - **DNS Verification**: Xác minh bằng cách thêm bản ghi DNS.
    - **File Verification**: Xác minh bằng cách tải tệp lên thư mục website.
- **Manual Add Record**: Cho phép người dùng tự thêm bản ghi DNS thay vì để hệ thống tự động.
- Loại chứng chỉ:
    - **Single Domain**: chỉ 1 tên miền
    - **Multiple Domains**: Cho phép đăng ký chứng chỉ cho nhiều tên miền cùng lúc.
    - **Wildcard:** cho phép đăng ký tên miền chính và tất cả các tiền miền phụ của nó

### Upload Certificate - tải chứng chỉnh SSL thủ công

![image](https://github.com/user-attachments/assets/d946b30f-c97a-468e-81f7-d586b5f3d776)

### Business Certificate - Đăng ký SSL trả phí

![image](https://github.com/user-attachments/assets/22afc7a5-be3a-4376-9d4b-15e6a4beeb2b)

Các tùy chọn: 
- **Số lượng tên miền (Domain Count)**
- **Loại chứng chỉ (Cert Type)**
    - **DV (Domain Validation)**: Dành cho cá nhân, cấp nhanh, không cần xác minh thủ công.
    - **OV (Organization Validation)**: Dành cho doanh nghiệp, cần xác minh tổ chức.
    - **EV (Extended Validation)**: Dành cho tổ chức lớn hoặc chính phủ, độ bảo mật cao nhất.

- Nhà cung cấp chứng chỉ: **PositiveSSL**, **Sectigo**, **GoGetSSL**...
- **Loại tên miền (Certificate Type)**
    - **Single Domain**: Bảo vệ một tên miền hoặc subdomain.
    - **Wildcard**: Bảo vệ tất cả subdomain của một tên miền chính (ví dụ: *.example.com).

- Thời hạn mua (Purchase Period)
- Dịch vụ triển khai (Deploy Service)
