> # SETTINGS

# Các nhóm thiết lập chính**

## 1. Panel Settings (Cài đặt bảng điều khiển)**

![image](https://github.com/user-attachments/assets/044b34ba-8bdb-4e17-8fcd-ac9821248179)

* **Close Panel**: Tắt bảng điều khiển (không ảnh hưởng đến website, database…).
* **IPv6**: Cho phép truy cập panel qua địa chỉ IPv6.
* **Offline Mode**: Ngắt kết nối internet cho các dịch vụ cần mạng.
* **Developer Mode**: Dành cho nhà phát triển plugin.
* **API**: Bật giao diện API để dùng với ứng dụng như aaPanel Mobile.
* **Language**: Chọn ngôn ngữ hiển thị (Tiếng Việt chưa có, nhưng có English, 中文, Español…).
* **Alias**: Đặt tên riêng cho panel (hiển thị trên tiêu đề trình duyệt).
* **Timeout**: Tự động đăng xuất sau thời gian không hoạt động.
* **Default Site Folder**: Thư mục mặc định khi tạo website mới (`/www/wwwroot/`).
* **Default Backup Folder**: Thư mục lưu backup mặc định (`/www/backup/`).
* **Server IP**: Hiển thị IP máy chủ (dùng để kiểm tra bản ghi A).
* **Server Time**: Hiển thị và đồng bộ thời gian máy chủ.

---

## 2. Security (Bảo mật bảng điều khiển)

![image](https://github.com/user-attachments/assets/11fbc7e4-d0b0-45fb-9179-50038a7bffb1)

* **Panel SSL**: Bật HTTPS cho panel (dùng chứng chỉ tự ký hoặc Let's Encrypt).
* **BasicAuth**: Thêm lớp xác thực phụ (ngăn quét panel).
* **Google Authenticator**: Bật xác thực 2 lớp.
* **Strong Password**: Bắt buộc mật khẩu mạnh (ít nhất 8 ký tự, gồm chữ hoa, thường, số, ký tự đặc biệt).
* **Domain Binding**: Chỉ cho phép truy cập panel qua tên miền đã cấu hình.
* **Authorized IP**: Chỉ cho phép IP cụ thể truy cập panel.
* **Panel Port**: Thay đổi cổng truy cập panel (mặc định là 8888).

> Nếu bạn bị khóa truy cập do cấu hình sai, có thể dùng SSH và lệnh `bt` để hủy:

---

## 3.  Panel User (Người dùng quản trị)

![image](https://github.com/user-attachments/assets/f23c9242-0d48-449b-a0a4-46b566ac7993)

* **Panel User**: Hiển thị và chỉnh sửa tài khoản quản trị.
* **Panel Password**: Đổi mật khẩu đăng nhập.
* **Bind Account**: Liên kết tài khoản aaPanel (dùng để đồng bộ và nhận hỗ trợ).
* **Password Expire**: Thiết lập thời gian hết hạn mật khẩu.

---

## 4.  Giao diện và ẩn menu

![image](https://github.com/user-attachments/assets/9c3542cb-fb06-42c1-b222-f53341f419ea)

* **Menu Bar Hidden**: Ẩn thanh menu bên trái để giao diện gọn hơn.
* **Not Logged In Response**: Tùy chỉnh phản hồi khi truy cập sai hoặc chưa đăng nhập (ví dụ: hiển thị lỗi 404).
