># WordPress - WP Toolkit
**Các chức năng trong WordPress Site**
| **Function**  | **Description**  |
| ------------- | ---------------- |
| **Add WordPress**                                  | Thêm hoặc import một website WordPress mới. Bạn có thể cài mới hoàn toàn hoặc import mã nguồn và database từ site cũ.                     |
| **Site Name**                                      | Tên miền đã gán với website WordPress. Nhấn vào tên để mở cấu hình chi tiết của site.                                                     |
| **Status**                                         | Trạng thái hoạt động của website (running / stopped). Nhấn vào để bật/tắt website này.                                                    |
| **Back up**                                        | Quản lý trạng thái sao lưu của site: tạo mới backup, khôi phục, tải về hoặc xem danh sách các bản sao lưu. Gồm cả mã nguồn và database.   |
| **Document Root**                                  | Đường dẫn thư mục chứa mã nguồn website. Nhấn vào để mở thư mục tương ứng trong trình quản lý file của aaPanel.                           |
| **Cache**                                          | Bật/tắt chức năng cache của website. Chỉ hoạt động khi dùng Nginx. Nếu không dùng Nginx, nên cài plugin WordPress như **WP Super Cache**. |
| **Vulnerability Scan**                             | Tự động kiểm tra lỗ hổng bảo mật trong mã nguồn WordPress của site.                                                                       |
| **WP Setting**                                     | Hiển thị phiên bản WordPress đang dùng. Cho phép thao tác: **Clone site** , **Quản lý Plugin/Themes** , **Kiểm tra toàn vẹn mã nguồn (Integrity check)**, **Cài lại WordPress (Reinstall)** |
| **PHP**  | Hiển thị phiên bản PHP của site. Nhấn vào để thay đổi version PHP dùng cho website.                                                       |
| **SSL**                                            | Trạng thái SSL (HTTPS). Nhấn vào để cài chứng chỉ SSL (miễn phí từ Let’s Encrypt hoặc thủ công).                                          |
| **Login**                                          | Đăng nhập nhanh vào trang quản trị WordPress (`/wp-admin`). Không cần nhập user/pass nếu đã thiết lập.                                    |
| **Modify**                                         | Mở giao diện cấu hình nâng cao: quản lý domain, bảo mật, URL rewrite, kiểm soát traffic, log lỗi...                                       |
| **Delete**                                         | Xoá hoàn toàn website khỏi aaPanel, bao gồm thư mục, database và tất cả cấu hình liên quan.                                               |

---

# Local Manage - Quản lý cục bộ

## Add WordPress - Tạo website mới

![image](https://github.com/user-attachments/assets/6cbf132b-9d98-4838-8976-38bc80799da3)

| **Chức năng**  | **Mô tả**  |
| -------------- | ---------- |
| **Domain**        | Nhập **tên miền chính** cho website WordPress (ví dụ: `myblog.com`). Đây là domain bạn đã trỏ về IP server.  |
| **Website Title** | Tiêu đề website WordPress. Sẽ hiển thị trong tiêu đề trang và header của site. Bạn có thể đổi lại sau trong WordPress admin. |
| **Language**      | Chọn **ngôn ngữ giao diện chính** cho trang web. Có thể chọn tiếng Việt, tiếng Anh, tiếng Trung…                                                                                               |
| **PHP version**   | Chọn phiên bản PHP sẽ dùng cho website. Nếu chưa có version phù hợp, bạn cần **cài đặt từ App Store** trước. Khuyến nghị dùng PHP 8.0+ cho bảo mật và hiệu năng.                               |
| **WP version**    | Chọn phiên bản WordPress muốn cài. ⚠️ Lưu ý: từ bản 5.6.0 trở lên, WordPress sẽ **tự động cập nhật phiên bản mới** trừ khi tắt bằng plugin hoặc cấu hình.                                      |
| **User name**     | Tên đăng nhập của tài khoản **quản trị viên WordPress**. Dùng để vào `/wp-admin`.                                                                                                              |
| **Password**      | Mật khẩu của tài khoản quản trị viên WordPress. Nên dùng mật khẩu mạnh. |
| **Email**         | Email của quản trị viên. Dùng để khôi phục mật khẩu, nhận thông báo hệ thống...                                                                                                                |
| **Prefix**        | Tiền tố bảng trong database MySQL. Mặc định là `wp_`, bạn có thể đổi để tăng bảo mật. Ví dụ: `abc_` → bảng sẽ là `abc_posts`, `abc_users`...                                                   |
| **Cache**         | Bật/tắt tính năng cache giúp tăng tốc độ tải trang bằng cách lưu tạm thời nội dung tĩnh. ⚠️ Chỉ hoạt động khi Web Server là **Nginx**. Nếu dùng Apache, nên cài plugin như **WP Super Cache**. |

![image](https://github.com/user-attachments/assets/e586e627-58ef-4363-b13a-2fcd05bfcbe0)

## Add WordPress - Create from backup (Tạo từ bản sao lưu)

![image](https://github.com/user-attachments/assets/bd6c4d40-f06f-42bf-a702-e700f3e152e2)

## WP Sets
Sử dụng WP Sets để nhanh chóng chỉ định các plugin và chủ đề để cài đặt trên trang web

### Add Plugins:
![image](https://github.com/user-attachments/assets/51b802ba-f2e2-4112-9fcd-58fb7399257d)

### Add Themes:
- thêm tương tự Plugin
![image](https://github.com/user-attachments/assets/99d902e1-3d72-4929-bdfd-c0638cfacf3d)

--> **Install**
![image](https://github.com/user-attachments/assets/8fba9521-17c8-4262-a078-7af212094acc)

- chọn site cần cài đặt
![image](https://github.com/user-attachments/assets/10015074-eff0-466b-83a7-375023e2ba47)

### Các cấu hình chỉnh sửa trang web
- Khi nhấp vào **Sitename** hoặc **Modify** có thể sửa đổi cấu hình trang web, Sercurity, SSL, log,... tương tự như trong hướng dẫn cấu hình **Website**

![image](https://github.com/user-attachments/assets/273b3675-f397-4251-a9d9-c85725e2107e)


--- 
## Ví dụ sử dụng
- Cấu hình bản ghi DNS trỏ domain về IP của server aaPanel

![image](https://github.com/user-attachments/assets/a42b54df-728f-4315-b764-a72b4c6cd8d5)

- Thêm một trang web WordPress mới bằng tên miền `antvpro.io.vn` ()

![image](https://github.com/user-attachments/assets/898b6bd3-e7b3-4c38-9df6-3a9f9c3adfbe)

- Cấu hình SSL cho tên miền vừa tạo: 
Chọn Sitename `antvpro.io.vn` --> `SSL` và cấu hình tương tự như khi cấu hình **Website**

![image](https://github.com/user-attachments/assets/2f6d2cb0-95bd-4c15-81b6-84dc5007fd19)

- Nhấp Login để vào trang quản trị của trang web và thực hiện một số thiết lập khác
![image](https://github.com/user-attachments/assets/1b77fdde-d7f6-4154-8e92-e894d1ce76e3)

![image](https://github.com/user-attachments/assets/ed154d24-878e-45b8-89ba-e3921acbd1d0)


---
# Remote Manage - Quản lý WordPress từ xa

![image](https://github.com/user-attachments/assets/05c267f3-874c-497f-a6dd-3596f815428d)

- Các chức năng:

| **Chức năng** | **Mô tả**  |
| ------------- | -----------|
| **Site name**      | Tên miền của website WordPress đã được thêm vào từ xa.                             |
| **WP version**     | Phiên bản WordPress đang được sử dụng trên site từ xa. Giúp bạn theo dõi site nào cần cập nhật.                           |
| **Plugin version** | Phiên bản của **aaPanel WP Toolkit plugin** được cài trên site từ xa. Phải đồng bộ phiên bản plugin để quản lý chính xác. |
| **PHP version**    | Phiên bản PHP đang chạy trên site từ xa. Nếu có sự khác biệt lớn (ví dụ: PHP 5.x), nên cân nhắc nâng cấp.                 |
| **Create time**    | Thời gian bạn đã thêm site vào hệ thống quản lý từ xa (thường là ngày tháng cụ thể).                                      |
| **Login**          | Nút đăng nhập nhanh vào admin (`/wp-admin`) của site từ xa. Hệ thống sẽ sử dụng token hoặc thông tin xác thực đã lưu.     |
| **Delete**  | Gỡ bỏ site WordPress ra khỏi danh sách quản lý từ xa (không xoá site thực tế). |


## Add WordPress

### Credential
> Sử dụng tài khoản quản trị viên và mật khẩu để đăng nhập vào trang web từ xa

![image](https://github.com/user-attachments/assets/2ca1bb76-9158-438a-9e39-a02bec061bdb)

### Secret key:
> Sử dụng `aapanel WP Toolkit` plug-in quản lý từ xa do aaPanel phát triển để xác thực đăng nhập

![image](https://github.com/user-attachments/assets/861eee24-f276-412d-8f06-f59e3135de67)


