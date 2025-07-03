># aaPanel

# HƯỚNG DẪN SỬ DỤNG

## aaPanel CLI
* Để hiển thị **MENU** quản lý của aaPanel, sử dụng lệnh:
  ```
  bt
  ```
  Sau khi chạy lệnh này, một bảng menu sẽ hiện ra với các tùy chọn được đánh số, bạn chỉ cần nhập số tương ứng để thực hiện chức năng. Các chức năng phổ biến bao gồm:

    ![image](https://github.com/user-attachments/assets/c34304a1-3049-4d31-a5d7-ccc23350c8b5)

    * `1`: Khởi động lại panel (Restart panel)
    * `2`: Dừng panel (Stop panel)
    * `3`: Mở panel (Start panel)
    * `4`: Tải lại panel (Reload panel)
    * `5`: Thay đổi mật khẩu đăng nhập panel (Change panel password)
    * `6`: Thay đổi tên người dùng đăng nhập panel (Change panel username)
    * `7`: Thay đổi mật khẩu root MySQL (Forcibly change MySQL root password)
    * `8`: Thay đổi cổng panel (Change panel port)
    * `9`: Xóa cache panel (Clear panel cache)
    * `10`: Xóa giới hạn đăng nhập (Clear login limit)
    * `11`: Bật/tắt xác thực IP + User-Agent (Turn on/off IP + User-Agent Authenticator)
    * `12`: Bỏ giới hạn liên kết tên miền với panel (Cancel domain binding limit)
    * `13`: Bỏ giới hạn truy cập theo IP (Cancel IP access limit)
    * `14`: Xem thông tin mặc định của panel (View panel default info)
    * `15`: Dọn rác hệ thống (Clear system rubbish)
    * `16`: Sửa lỗi panel, cập nhật bản mới nhất (Repair panel)
    * `17`: Bật/tắt log cutting và nén log (Set log cutting on/off compression)
    * `18`: Bật/tắt backup tự động cho panel (Set whether to back up the panel automatically)
    * `22`: Xem log lỗi của panel (Display panel error log)
    * `23`: Tắt xác thực BasicAuth (Turn off BasicAuth Authenticator)
    * `24`: Tắt xác thực Google Authenticator (Turn off Google Authenticator)
    * `25`: Lưu bản sao khi sửa file trong panel (Save copy when modify file in panel)
    * `26`: Giữ hoặc xóa backup cũ khi backup lên cloud (Keep/Remove local backup when backing up to cloud storage)
    * `27`: Bật/tắt SSL cho panel (Turn on/off panel SSL)
    * `28`: Đổi đường dẫn đăng nhập panel (Modify panel security entrance)
    * `33`: Gỡ giới hạn chống tấn công brute-force (lift the explosion-proof limit on the panel)
    * 0: Hủy bỏ, thoát menu (Cancel)

- **Các lệnh quản lý dịch vụ cơ bản:**
  * **Khởi động**: `service bt start`
  * **Dừng**: `service bt stop`
  * **Khởi động lại**: `service bt restart`

- **Gỡ cài đặt aaPanel:**
  ```bash!
  service bt stop && chkconfig --del bt && rm -f /etc/init.d/bt && rm -rf /www/server/panel
  ```

- **Các thư mục cấu hình và dữ liệu quan trọng:**

Để quản lý sâu hơn hoặc xử lý sự cố, bạn nên biết các thư mục chính mà aaPanel sử dụng:

* **Thư mục cài đặt Nginx:** `/www/server/nginx`
* **Thư mục cài đặt Apache:** `/www/server/apache`
* **Thư mục cài đặt MySQL:** `/www/server/mysql`
* **Thư mục gốc của các website:** Thường là `/www/wwwroot` (mặc định)
* **Thư mục cấu hình Virtual Host Nginx:** `/www/server/panel/vhost/nginx`
* **Thư mục cấu hình Virtual Host Apache:** `/www/server/panel/vhost/apache`
* **Thư mục sao lưu (backup):** `/www/backup/database` (cho database), `/www/backup/site` (cho website)

# HDSD GIAO DIỆN WEB AAPANEL

📋 **Tóm tắt tính năng của các mục trong aaPanel**

| 🧭 Mục menu     | 🎯 Chức năng chính                                                               |
| --------------- | -------------------------------------------------------------------------------- |
| **Home**        | Trang tổng quan: hiển thị trạng thái hệ thống, hiệu suất, tài nguyên server.     |
| **Website**     | Tạo, quản lý, cấu hình các website (PHP, NodeJS, Proxy).                         |
| **WP Toolkit**  | Công cụ quản lý WordPress: cài mới, sao lưu, cập nhật, bảo mật WP (plugin thêm). |
| **FTP**         | Quản lý tài khoản FTP: tạo user, phân quyền thư mục upload.                      |
| **Databases**   | Tạo và quản lý cơ sở dữ liệu (MySQL/MariaDB); tích hợp phpMyAdmin.               |
| **Docker**      | Triển khai và quản lý container Docker (plugin nâng cao).                        |
| **Monitor**     | Theo dõi tài nguyên hệ thống (CPU, RAM, mạng, I/O); cảnh báo hiệu suất.          |
| **Security**    | Thiết lập bảo mật cơ bản: tường lửa, giới hạn truy cập IP, chống quét port.      |
| **WAF**         | Web Application Firewall: ngăn chặn tấn công web như XSS, SQLi, RFI… *(plugin)*. |
| **Mail Server** | Cài đặt và cấu hình máy chủ Email (Postfix + Dovecot + Roundcube). *(plugin)*    |
| **Files**       | Trình quản lý file: upload, sửa file, phân quyền, unzip trực tiếp từ panel.      |
| **Logs**        | Xem nhật ký hoạt động: truy cập web, hệ thống, lỗi PHP…                          |
| **Domains**     | Quản lý tên miền đã khai báo (domain mapping, DNS nội bộ nếu cài thêm plugin).   |
| **Account**     | Quản lý tài khoản đăng nhập vào aaPanel, phân quyền người dùng.                  |
| **Terminal**    | Truy cập terminal trực tiếp từ trình duyệt (Web Shell).                          |
| **Cron**        | Thiết lập tác vụ định kỳ (backup, restart service, script).                      |
| **App Store**   | Cài đặt, cập nhật và quản lý plugin, phần mềm mở rộng từ aaPanel.                |
| **Settings**    | Cấu hình hệ thống aaPanel: đổi port, SSL panel, cấu hình IP whitelist…           |

## 1. Home overview (Dashboard – Trang Tổng Quan)
![image](https://github.com/user-attachments/assets/87a802ed-9be5-4b65-88aa-6955903b3f71)
#### **Thông tin hệ thống (Sys Status)**
  
![image](https://github.com/user-attachments/assets/a4189991-2efc-4332-8506-27c886b4e3a3)

  
| Mục             | Ý nghĩa                                                            |
| --------------- | ------------------------------------------------------------------ |
| **Load Status** | Tải hệ thống trung bình – càng thấp càng tốt (dưới 1 là lý tưởng). |
| **CPU Usage**   | Mức sử dụng CPU hiện tại (%).                                      |
| **RAM Usage**   | Mức sử dụng bộ nhớ RAM (% và MB đã dùng).                          |
| **/**           | Dung lượng đĩa sử dụng (root partition).                           |
 

-  **Overview**
![image](https://github.com/user-attachments/assets/43df4e53-6a8d-42bb-8ce4-0660db4f0191)


    * **Site**: số website đang được host.
    * **FTP**: số tài khoản FTP đã cấu hình.
    * **DB (Database)**: số cơ sở dữ liệu đang có.
    * **Security**: số plugin bảo mật đang hoạt động.
    ![image](https://github.com/user-attachments/assets/e76f2594-09d0-4e35-91d1-927b34c8f511)

        - `security risks`: Quét và hiển thị những rủi ro của hệ điều hành (Bản Pro)
        ![image](https://github.com/user-attachments/assets/305d1182-d6ec-4f89-94e8-b980a60d61d4)

        
        - Phát hiện những file độc hại:
        ![image](https://github.com/user-attachments/assets/e15899b4-0d32-4e44-9add-6ade43d16a56)


        - Rà quét lỗ hổng web (Pro):
        ![image](https://github.com/user-attachments/assets/c93f44f4-57a6-4c1b-af86-87557759b6b2)



    > Khi chưa cấu hình, các số liệu này là **0**.


- **Software – Phần mềm mở rộng (plugin)**
![image](https://github.com/user-attachments/assets/71261e56-5526-4108-bb6c-8c6e1a8f4d7e)

    * **WAF (Website Firewall)**: Tường lửa ứng dụng web (bản trả phí).
    * **Website statistics**: Thống kê truy cập chi tiết.
    * **Website Tamper-proof**: Bảo vệ nội dung website khỏi bị sửa đổi.
    * **Anti-intrusion**: Chống xâm nhập trái phép.
    >Những plugin này thường cần **nâng cấp bản Pro hoặc mua riêng**.

- **Network & Disk I/O Monitoring**

    - **Traffic**:
![image](https://github.com/user-attachments/assets/b3d6a12a-03aa-443e-8447-5997dfb0d51d)

        * Biểu đồ thể hiện dữ liệu **Upstream/Downstream** theo thời gian thực.
        * Hiển thị lượng **data đã gửi (sent)** và **đã nhận (received)**. **Disk I/O** *(tab bên cạnh)*:
        * Giám sát hoạt động đọc/ghi của ổ đĩa.

    - **Disk I/O** *(tab bên cạnh)*:
![image](https://github.com/user-attachments/assets/76808c48-0deb-4e8e-a27b-8b76704e6b2b)

        * Giám sát hoạt động đọc/ghi của ổ đĩa.
            - `Read`: Hiển thị tốc độ đọc của ổ cứng theo thời gian thực.
            - `Write`: Hiển thị tốc độ ghi của ổ cứng theo thời gian thực.
            - `TPS`: Hiển thị số lần ghi (giao dịch) của ổ cứng theo thời gian thực.
            - `IO Wait`: Hiển thị thời gian chờ IO của ổ cứng theo thời gian thực.
            - `Disk`: ALL: Chọn ổ cứng cụ thể để xem thông tin theo thời gian thực.

        > **Mục đích**: Giúp phát hiện sớm các vấn đề về mạng hoặc hiệu suất ổ đĩa.

---

## 2. Website

### PHP Project

![image](https://github.com/user-attachments/assets/085e5c84-57cd-4879-a822-74b47372a84a)

| **Chức năng**        | **Mô tả**                                                                                                                                                                                                       |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Add site**         | Tạo website mới. Có thể tạo hàng loạt nhiều website cùng lúc.                                                                                                                                                   |
| **Default Page**     | Hiển thị trang mặc định của site mới.                                                                                       |
| **Default Website**  | Thiết lập site mặc định: mọi domain/IP chưa được liên kết sẽ trỏ về site này. Giúp ngăn chặn việc trỏ domain độc hại.                                                                                           |
| **PHP CLI**          | Thiết lập phiên bản PHP được sử dụng khi chạy lệnh PHP qua dòng lệnh (CLI).                                                                                                                                     |
| **HTTPS Protection** | Khi bật tính năng này sẽ giúp khắc phục lỗi chéo HTTPS giữa các site.                                                                                                                                           |
| **Site name**        | Tên miền liên kết với website.                                                                                               |
| **Status**           | Hiển thị trạng thái hoạt động của site.                                                                                        |
| **Back up**          | Hiển thị trạng thái sao lưu website.       |
| **Document Root**    | Đường dẫn thư mục chứa mã nguồn website.                                                                                       |
| **Quota**            | Dung lượng giới hạn cho website.                                                                                            |
| **Expired date**     | Hiển thị thời gian hết hạn hoạt động của website.                                                                           |
| **Note**             | Ghi chú dùng để lưu thông tin mô tả website, ví dụ: mục đích sử dụng, ghi chú kỹ thuật…                                                                                                                         |
| **PHP**              | Hiển thị phiên bản PHP đang sử dụng của website.                                                                              |
| **SSL**              | Hiển thị trạng thái SSL của website.                                                                                       |
| **Attack**           | Hiển thị thông tin về các cuộc tấn công website (qua log quét).                                                                                                                            |
| **Stats**            | Dùng plugin “Website Statistics-v2” để theo dõi lưu lượng truy cập, số kết nối, v.v.                                                                                                                            |
| **WAF**              | Thiết lập cấu hình tường lửa ứng dụng web (WAF) cho website.                                                                                                                                                    |
| **Conf**             | Cấu hình chuyên sâu của website, bao gồm: Quản lý domain, thư mục web, giới hạn truy cập, giới hạn băng thông, rewrite URL, trang mặc định, redirect, reverse proxy, chống hotlink, log truy cập, log lỗi, v.v. |
| **Delete**           | Xóa website. Sau khi xóa, thư mục website sẽ được chuyển vào Thùng rác trong phần quản lý file.                                                                                                                 |
#### Add site - Tạo trang web

![image](https://github.com/user-attachments/assets/959aa070-1acb-4713-a3ef-6fc6d146db1b)

| **Chức năng**      | **Mô tả**                                                                                                                                                                                                                                         |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Resolve Domain** | Lựa chọn cách xử lý tên miền: **Tự thêm bản ghi (Manual)** hoặc **Tự động thêm bản ghi (Automatic)**. ⚠️ Khi chọn chế độ tự động, bạn cần thêm API DNS tương ứng trong mục **Domains**, và domain phải có chứng chỉ SSL hợp lệ để hoạt động đúng. |
| **Domain name**    | Nhập tên miền và cổng (port) sẽ gán cho website. ⚠️ Không tự động tạo bản ghi phụ `www`.                                                                                                                                                          |
| **Description**    | Mô tả mục đích hoặc vai trò của website. Giúp dễ dàng quản lý khi có nhiều website.                                                                                                                                                               |
| **Website Path**   | Đường dẫn đến thư mục chứa mã nguồn website. Có thể chọn đường dẫn tùy ý, nhưng khuyến nghị sử dụng mặc định `/www/wwwroot/` để thuận tiện quản lý.                                                                                               |
| **FTP**            | Lựa chọn tạo tài khoản FTP đi kèm website. Cho phép upload file dễ dàng thông qua FTP client.                                                                                                                                                     |
| **Database**       | Lựa chọn tạo cơ sở dữ liệu MySQL đồng thời khi tạo website. Bạn có thể chọn tên DB, user, mật khẩu…                                                                                                                                               |
| **PHP version**    | Chọn phiên bản PHP dùng để chạy website. Nếu cần thêm phiên bản khác, có thể cài trong **App Store** của aaPanel.                                                                                                                                 |
| **Site category**  | Phân loại website (ví dụ: blog, landing page, ecommerce…) giúp việc quản lý nhiều website thuận tiện hơn.                                                                                                                                         |

- Tạo site thành công:
![image](https://github.com/user-attachments/assets/f5c2a670-7798-4c2b-b9b2-f349e41ee931)

![image](https://github.com/user-attachments/assets/f69f1b8d-d41b-4fa3-be74-af8ca0f20a8d)

#### Add site -- Batch Create – Tạo Website hàng loạt

- Cho phép bạn tạo nhiều website cùng lúc bằng cách điền thông tin theo định dạng dòng. Phù hợp khi bạn quản lý nhiều subdomain, website test, hoặc clone site nhanh.
- Cú pháp định dạng mỗi dòng: 
```
Domain | Document Root | FTP | Database | PHP Version
```
| Trường            | Ý nghĩa                                                                 |
| ----------------- | ----------------------------------------------------------------------- |
| **Domain**        | Tên miền hoặc subdomain của website (có thể phân tách bằng `,`).        |
| **Document Root** | Đường dẫn thư mục chứa mã nguồn (VD: `/www/wwwroot/domain1`).           |
| **FTP**           | `1`: tạo tài khoản FTP tự động, `0`: không tạo.                         |
| **Database**      | `1`: tạo database MySQL tự động, `0`: không tạo.                        |
| **PHP Version**   | `0`: không đặt PHP, hoặc nhập phiên bản cụ thể (`56`, `74`, `82`, v.v.) |

- Ví dụ: 
![image](https://github.com/user-attachments/assets/47ad17a2-9151-4916-8140-4e7d01917eda)
--> 
![image](https://github.com/user-attachments/assets/b91c816b-971a-4cdb-aa0c-7e048fbd6ae8)

#### Default Page

![image](https://github.com/user-attachments/assets/c7bf68ee-a93b-4a4a-b151-a0791e987e01)

#### Default Website

![image](https://github.com/user-attachments/assets/e416081d-8669-4ecf-a5c9-4f2cff25215e)

- Khi bạn đã đặt một **"Default Website"**, tất cả các tên miền không được khai báo trong aaPanel nhưng lại trỏ về IP của bạn sẽ được tự động chuyển hướng đến trang web mặc định đó. Điều này giúp bạn kiểm soát được những gì sẽ hiển thị trong trường hợp này.
- Nếu không sử dụng tính năng này, Kẻ xấu có thể cố tình trỏ một tên miền của họ (ví dụ: tenmienrac.com) về địa chỉ IP của bạn. Nếu bạn không có thiết lập "Default Website", server của bạn có thể sẽ hiển thị nội dung của một trong các trang web bạn đang host, gây ra các vấn đề về trùng lặp nội dung cho SEO hoặc các rủi ro bảo mật khác.



#### Web program management

![image](https://github.com/user-attachments/assets/bc64e291-3fa1-4c70-96e8-561b1016a3c8)

#### Back up
![image](https://github.com/user-attachments/assets/3da6c6d7-9b7d-4800-9108-98d835eee8af)

#### Ví dụ sử dụng

- Add site 
![image](https://github.com/user-attachments/assets/f0b102b5-bf81-4281-9baa-52065abca17e)

- Up load source code lên tên miền vừa tạo:
    - **Bước 1**: Trong danh sách tên miền, bạn nhấp vào đường dẫn Document root
     ![image](https://github.com/user-attachments/assets/cd4e3676-b380-4b4a-94f8-87bc8fa98383)

    - **Bước 2:** Xóa file `index.html` mặc định và chọn `Upload` để tải source code lên:
    ![image](https://github.com/user-attachments/assets/272f4cda-9f7a-42e8-b1be-11ec8427a3a8)

    ![image](https://github.com/user-attachments/assets/6258947a-a243-466f-8d80-b5a9e7210b24)

- **Quản lý site**: Click chọn **Site name** hoặc **Conf**
    ![image](https://github.com/user-attachments/assets/2eb50d85-eae6-4ef2-bc92-7bed8d34c87d)

    - **Domain Manager:** 
        ![image](https://github.com/user-attachments/assets/69d77490-1414-41ec-98d4-4029225be94e)
        - Quản lý domain/subdomain của website.
        - Bạn có thể thêm nhiều domain (VD: www.domain.com, test.domain.com).
        - Hỗ trợ thêm port tùy chỉnh (VD: domain.com:88).
        - Có thể xóa domain khỏi site bằng checkbox và nút Del.
        
    - **Subdirectory Binding:**
    ![image](https://github.com/user-attachments/assets/cdc7f4d1-ee06-476c-8624-a1766ae87d9d)
        - Gán thư mục con cho subdomain.
        - Ví dụ: trỏ shops.antvpro.io.vn vào thư mục `/www/wwwroot/antvpro.io.vn/shops`.
        - Phù hợp khi chạy nhiều ứng dụng nhỏ trên cùng domain chính.

    - **Site Directory:**
    ![image](https://github.com/user-attachments/assets/15387cc2-fdb9-4d1f-9ae5-ebd0a90d97dd)

        - Quản lý đường dẫn thư mục chứa mã nguồn website.
        - Có thể thay đổi thư mục gốc nếu cần.
    
    - **Limit Access:** Hạn chế /chặn truy cập thư mục
    ![image](https://github.com/user-attachments/assets/6439a880-63c1-494e-a8a3-9b3c568353c0)

    - **Traffic Control:** Giới hạn băng thông và kết nối
    ![image](https://github.com/user-attachments/assets/9d7fb3b2-c16f-48c6-b3fa-fa0903c95b9a)
        - Giúp ngăn chặn tình trạng quá tải hoặc tấn công DDoS đơn giản.
        - `Turn on traffic control`: bật chức năng này
        - `Limit plan`: chọn mẫu có sẵn
        - `Limit of concurrency`: tổng số kết nối đồng thời tối đa
        - `Block single IP`: số kết nối đồng thời tối đa 1 IP được phép
        - `Traffic control`: giới hạn lưu lượng tối đa cho 1 request (KB)

    - **URL Rewrite:** Cấu hình rewrite rule (giống `.htaccess`).
    ![image](https://github.com/user-attachments/assets/c242726d-1db8-4e46-a247-e445239afc72)
        - Chọn loại rule sẵn có hoặc tự viết
        - Có hỗ trợ chuyển đổi rule từ Apache sang Nginx

    - **Default Document:** Thiết lập tên file mặc định khi truy cập thư mục (VD: `index.html`, `index.php`...có thể ưu tiên cho file nào chạy trước:
    ![image](https://github.com/user-attachments/assets/f169fe14-9e52-4b63-9456-bc1e2c2b0b5a)

    - **Config:** Truy cập và chỉnh sửa cấu hình trực tiếp của web server cho site này
    ![image](https://github.com/user-attachments/assets/2f6864d5-8803-400b-bc94-0829a9daae74)

    - **SSL** - Cấu hình chứng chỉ HTTPS cho website.
    ![image](https://github.com/user-attachments/assets/3621b2b6-f640-4f6e-bca2-4b8c8e3e1aec)

        - Tạo SSL miễn phí Let’s Encrypt.
        - Tải lên SSL tự ký hoặc mua từ nhà cung cấp
        - Bật **Force HTTPS**: ép redirect toàn bộ HTTP → HTTPS

    - **Redirect:** chuyển hướng URL
    ![image](https://github.com/user-attachments/assets/c4a476d1-d09a-48c5-8ab3-99fe4184c454)

        - Chuyển domain → domain khác.
        - Chuyển www → non-www hoặc ngược lại.
        - Chuyển sang HTTPS tùy điều kiện.
        - `301` (Permanent Redirect): Chuyển hướng vĩnh viễn (tốt cho SEO)
        - `302` (Temporary Redirect): Chuyển hướng tạm thời.
    - **Reverse Proxy:**
    ![image](https://github.com/user-attachments/assets/a4151842-9634-43d0-93d1-7c5ca64468bf)

        - Thiết lập website như reverse proxy (giống nginx proxy_pass).
        - Dùng để trỏ domain đến một server backend khác (VD: chạy NodeJS, Flask... trên port riêng).

    - **Response Log:**
    ![image](https://github.com/user-attachments/assets/480df125-f8ad-4021-b89e-31cd95da1cc9)


        - Ghi log phản hồi từ web server.
        - Hữu ích để gỡ lỗi khi gặp lỗi 404, 500, rewrite sai, hoặc tấn công từ client.
---

### Node Project
`Node Project`  là chức năng dùng để tạo và quản lý các ứng dụng web chạy bằng Node.js, tương tự như `PHP Project` để chạy trang web viết bằng PHP.

---
### Proxy Project 
- Tạo trang web đóng vai trò proxy để chuyển tiếp request đến máy chủ khác.
- Nói cách khác: domain bạn cấu hình sẽ trỏ đến backend khác, thay vì host nội dung tại máy chủ aaPanel.
![image](https://github.com/user-attachments/assets/a129ccae-d5db-4683-ab92-de4cc291c50d)

| **Chức năng===**      | **Mô tả**                                                                                                           |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------- |
| **Domain**        | Tên miền được gán cho site proxy. Nhấn vào tên site để cấu hình hoặc chỉnh sửa.                                           |
| **Proxy Address** | Địa chỉ đích (target) mà proxy sẽ chuyển tiếp request đến. Ví dụ: `http://127.0.0.1:3000`                                 |
| **SSL**           | Trạng thái SSL của site proxy. Nhấn vào để thêm hoặc cấu hình chứng chỉ SSL cho domain này (Let’s Encrypt hoặc thủ công). |
| **Note**          | Ghi chú nội dung liên quan đến site hiện tại, giúp quản trị viên dễ nhớ vai trò hoặc mục đích.                            |
| **Stats**         | Hiển thị thống kê lưu lượng, kết nối, v.v. thông qua plugin **Website statistics-v2** *(cần plugin Pro)*                  |
| **WAF**           | Thiết lập và quản lý cấu hình **Tường lửa ứng dụng web (Web Application Firewall)** cho site hiện tại.                    |
| **Modify**        | Mở giao diện chỉnh sửa cấu hình chi tiết của site: địa chỉ đích, SSL, rewrite rule, redirect…                             |
| **Delete**        | Xoá hoàn toàn site proxy này khỏi aaPanel (cấu hình sẽ mất,                                                               |

