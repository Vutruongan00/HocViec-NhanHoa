# 1. Định nghĩa
### a. Web server
- Web server là hệ thống phần mềm hoặc phần cứng xử lý các yêu cầu HTTP từ người dùng và trả về các phản hồi như HTML, video, hình ảnh hoặc chuyển hướng đến URI khác. Vai trò chính của nó là phân tích cú pháp yêu cầu và cung cấp phản hồi nhanh chóng, với mục tiêu tối ưu hóa hiệu suất, sử dụng tài nguyên tối thiểu
- Web Server thường phục vụ các tệp tĩnh từ hệ thống tệp hoặc có thể chuyển yêu cầu đến một ứng dụng để tạo phản hồi động, ví dụ như hiển thị ngày và giờ. Các máy chủ web có thể dễ cấu hình và được thiết kế để phản hồi nhanh chóng các yêu cầu về tệp tĩnh. Khi sử dụng cùng App Server, Web Server đảm nhiệm việc xử lý các yêu cầu HTTP nhanh chóng, trong khi máy chủ ứng dụng tạo nội dung động, tối ưu hóa hiệu suất hệ thống.

 ![image](https://github.com/user-attachments/assets/b7e93b6a-b28d-49fa-85f8-a4919f25f62d)

 
 ### b. App Server 
 - Máy chủ ứng dụng là nền tảng cho các ứng dụng doanh nghiệp, xử lý logic kinh doanh phức tạp và cung cấp dữ liệu cho các máy chủ web hoặc ứng dụng khách hàng. App Server không chỉ phân phối nội dung mà còn xử lý các logic nghiệp vụ phức tạp, giúp hỗ trợ các giao dịch, quản lý kết nối cơ sở dữ liệu, và cho phép ứng dụng động tương tác với người dùng.
 - Máy chủ ứng dụng cần nhiều tài nguyên CPU và bộ nhớ hơn do khối lượng công việc phức tạp. Chúng thường được cấu hình cùng máy chủ web ở phía trước để hỗ trợ cân bằng tải và nâng cao hiệu suất. Các chức năng bổ sung như kết nối cơ sở dữ liệu, hàng đợi công việc và khả năng chịu lỗi giúp máy chủ ứng dụng đáp ứng linh hoạt nhu cầu người dùng, duy trì hiệu suất cao khi lưu lượng truy cập tăng.
 # 2. So sánh Web Server và Application Server
 
  ![image](https://github.com/user-attachments/assets/1bc6509c-c6eb-408c-a09e-e259e973a25b)

| **Tiêu chí**             | **Web Server (Máy chủ Web)**                                                                 | **Application Server (Máy chủ Ứng dụng)**                                                                 |
|--------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| **Mục đích chính**       | Phục vụ nội dung tĩnh và quản lý các yêu cầu HTTP/HTTPS.                                     | Cung cấp môi trường chạy cho ứng dụng web động, xử lý logic nghiệp vụ phức tạp.                          |
| **Nội dung phục vụ**     | HTML, CSS, JavaScript, hình ảnh, video, file PDF.                                            | Nội dung động được tạo ra bởi mã nguồn như JSP, Servlets, ASP.NET, Node.js, Python scripts,...            |
| **Cách thức hoạt động**  | Nhận yêu cầu HTTP, tìm file tĩnh trên ổ đĩa, gửi phản hồi. Có thể chuyển tiếp đến app server. | Nhận yêu cầu (từ web server hoặc trực tiếp), xử lý logic, tương tác DB, tạo nội dung động, trả phản hồi. |
| **Giao thức**            | Chủ yếu là HTTP/HTTPS.                                                                       | HTTP/HTTPS, và các giao thức cao cấp hơn như RMI, RPC, JMS, WebSockets.                                   |
| **Tính năng chính**      | Caching, Load Balancing, SSL termination, Virtual Hosting, Reverse Proxy.                   | Session Management, Connection Pooling, Transaction Management, Threading, Security, MQ, DI.             |
| **Khả năng mở rộng**     | Dễ mở rộng ngang để phục vụ nhiều kết nối tĩnh.                                              | Mở rộng tốt nhưng phức tạp hơn do quản lý trạng thái và đồng bộ hóa.                                      |
| **Ví dụ**                | Apache HTTP Server, Nginx, Microsoft IIS, Caddy.                                              | Apache Tomcat, JBoss/WildFly, WebLogic, WebSphere, Node.js, Gunicorn, uWSGI, Puma, Kestrel.              |
| **Mối quan hệ**          | Hoạt động ở "frontend", tiếp nhận request từ client.                                         | Hoạt động ở "backend", xử lý logic và gửi phản hồi cho web server.                                       |

# 3. Khi nào dùng Web Server (phục vụ file tĩnh)?

Sử dụng **Web Server** hoặc để nó đảm nhận vai trò chính trong các trường hợp sau:

####  Trang web hoàn toàn tĩnh (Static Websites)

- Các trang web giới thiệu doanh nghiệp, blog cá nhân được xây dựng bằng các công cụ tạo trang tĩnh như:
  - **Jekyll**, **Hugo**, **Gatsby**.
- Website chỉ bao gồm các file **HTML**, **CSS**, **JavaScript**, **hình ảnh**, **video**.
- **Ví dụ:** Một trang portfolio đơn giản, một landing page.

---

#### Phục vụ tài nguyên tĩnh cho ứng dụng động

- Ngay cả với ứng dụng web động (Node.js, Spring Boot, Django...), bạn nên dùng Web Server (Nginx/Apache) để:
  - Phục vụ CSS, JS, ảnh, font, video.
- **Lý do:**
  - Web Server tối ưu hơn Application Server khi xử lý tài nguyên tĩnh.
  - Hỗ trợ cache, gzip, xử lý đồng thời hiệu quả, giảm tải backend.

---

#### Làm Reverse Proxy và Load Balancer

- Trong kiến trúc phức tạp:
  - Web Server là điểm vào chính cho client.
  - Chuyển tiếp các yêu cầu động đến App Server (proxy).
  - Phân phối đều yêu cầu (load balancing).
- **Lý do:**
  - Tăng khả năng chịu tải, độ sẵn sàng.
  - Giải mã HTTPS (SSL termination).
  - Dễ cấu hình & quản lý luồng truy cập.

---

#### Bảo mật lớp đầu tiên

- Web Server có thể:
  - Chặn IP độc hại.
  - Giới hạn tốc độ truy cập.
  - Ngăn chặn các tấn công đơn giản (brute-force, scan...).
- **Lý do:**
  - Là tuyến phòng thủ đầu tiên bảo vệ App Server.
  - Giảm thiểu rủi ro và gánh nặng bảo mật cho hệ thống phía sau.

# 4. Khi nào cần Application Server (xử lý logic như Node.js, Tomcat)?
---

#### Ứng dụng web động (Dynamic Web Applications)

- Khi nội dung trang web được tạo hoặc thay đổi dựa trên:
  - Tương tác người dùng
  - Dữ liệu từ cơ sở dữ liệu
  - Logic nghiệp vụ

- **Ví dụ:**
  - Website thương mại điện tử: Thêm sản phẩm vào giỏ, xử lý thanh toán
  - Mạng xã hội: Tạo bài viết, tương tác với bạn bè
  - CMS động như WordPress (trừ khi dùng static site generator)

---

####  Xử lý logic nghiệp vụ phức tạp

- Khi ứng dụng cần:
  - Thực hiện tính toán
  - Xử lý dữ liệu
  - Tương tác với hệ thống backend khác (DB, dịch vụ bên thứ ba, message queue)

- **Ví dụ:**
  - Tính giá đơn hàng, xử lý đơn hàng
  - Gửi email xác nhận
  - Phân tích dữ liệu theo thời gian thực

---

#### Quản lý dữ liệu người dùng và phiên (User Data & Session Management)

- Khi ứng dụng:
  - Yêu cầu người dùng đăng nhập
  - Lưu trạng thái người dùng (session)
  - Kiểm soát quyền truy cập (authentication, authorization)

---

#### Cung cấp API (Application Programming Interface)

- Khi bạn xây dựng các API phục vụ:
  - Ứng dụng di động
  - Ứng dụng desktop
  - Frontend dùng React, Angular, Vue.js

- **Ví dụ phổ biến:** Node.js kết hợp Express.js cho REST API hoặc GraphQL API

---

#### Tích hợp với cơ sở dữ liệu

- Application Server là nơi:
  - Code kết nối với DB
  - Thực hiện CRUD (Create, Read, Update, Delete)
  - Quản lý connection pooling (nhóm kết nối) để tối ưu hiệu suất DB

---

>  Tóm lại: Nếu cần **xử lý động**, **tương tác với dữ liệu**, hoặc **cung cấp API**, thì chắc chắn cần đến một **Application Server**.
