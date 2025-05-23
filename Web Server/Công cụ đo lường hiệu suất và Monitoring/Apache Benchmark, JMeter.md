
# 1. Apache Benchmark (ab)
`ab` là một công cụ dòng lệnh đơn giản nhưng mạnh mẽ, được phát triển bởi Apache Software Foundation. Nó thường được sử dụng để kiểm tra hiệu suất của máy chủ HTTP (web server) bằng cách gửi nhiều yêu cầu đồng thời đến một URL nhất định và đo thời gian phản hồi.

**Đặc điểm nổi bật:**
- **Đơn giản, gọn nhẹ**: Là công cụ dòng lệnh, không yêu cầu cài đặt phức tạp (thường có sẵn nếu bạn cài đặt Apache HTTP Server hoặc có thể cài riêng).
- **Dễ sử dụng**: Cú pháp lệnh rất đơn giản.
- **Kết quả nhanh chóng**: Cung cấp các số liệu cơ bản như requests per second (số yêu cầu mỗi giây), thời gian hoàn thành, thời gian trung bình, median, percentile, v.v.
- **Phù hợp cho kiểm tra nhanh**: Lý tưởng để kiểm tra hiệu suất cơ bản của một URL đơn lẻ hoặc một API endpoint.

**Cách sử dụng cơ bản:**
```
ab -n <số_yêu_cầu> -c <số_kết_nối_đồng_thời> <URL>
```
![image](https://github.com/user-attachments/assets/51f5de59-41bf-4826-bcf1-0abc07c7ef33)



**Ví dụ:**
test luôn trên máy CentOS7 đã cài đặt cấu hình  **Nginx làm proxy và caching cho Apache** tù bài trước:
- Trên máy client chạy lệnh sau để gửi `10000 request` và 1000 kết nối đồng thời đến trang web
```
ab -n 100000 -c 1000 https://truongan12345.com/proxy.php
```
![image](https://github.com/user-attachments/assets/c40b769e-02f2-49fc-bbe5-cff970bffebb)


- Trong khi chạy Benchmark từ máy client, quay trở lại máy chủ Apache webserver kiểm tra tiến trình `top`
![image](https://github.com/user-attachments/assets/34830cf7-a2c6-4aad-bf58-98fcf8814681)


--> Apache ngồi chơi trong khi Benchmark đang chạy → Nginx đã gánh hết request từ cache thay vì proxy pass xuống backend. 

>**Đây cũng là minh chứng cho việc cấu hình NGINX Caching thành công cho Apache**


# 2. Apache JMeter

JMeter là một công cụ kiểm thử hiệu suất nguồn mở mạnh mẽ và đa năng, được phát triển hoàn toàn bằng Java. Nó không chỉ dùng để kiểm thử web server mà còn có thể kiểm thử nhiều loại dịch vụ và giao thức khác nhau.

**Đặc điểm nổi bật:**

-  Đa giao thức: Hỗ trợ HTTP/HTTPS, FTP, SOAP/REST webservices, JDBC, JMS, Mail (SMTP, POP3, IMAP), native commands, shell scripts, v.v.
-  Kiểm thử kịch bản phức tạp: Có thể mô phỏng các luồng người dùng phức tạp (login, logout, điều hướng qua nhiều trang, gửi form, xử lý session/cookie).
-  Giao diện đồ họa (GUI): Dễ dàng thiết kế test plan, xem kết quả dưới dạng đồ thị, bảng biểu.
-  Khả năng mở rộng: Hỗ trợ plugins và có thể viết script (Groovy, Beanshell) để tùy chỉnh.
-  Báo cáo chi tiết: Tạo báo cáo HTML đẹp mắt và dễ đọc.
-  Kiểm thử phân tán: Có thể chạy kiểm thử từ nhiều máy khác nhau để tạo ra tải lớn hơn.

**Các thành phần chính trong Test Plan của JMeter:**

Một Test Plan trong JMeter được cấu trúc thành nhiều thành phần:

- Test Plan: Container gốc cho toàn bộ bài kiểm thử.
- Thread Group (Nhóm Luồng): Đại diện cho một nhóm người dùng ảo. Bạn có thể cấu hình:
    - Number of Threads (users): Số lượng người dùng ảo đồng thời.
    - Ramp-up period: Thời gian để tất cả người dùng ảo được khởi tạo.
    - Loop Count: Số lần mỗi người dùng lặp lại các hành động trong Thread Group.
- Sampler: Đại diện cho một yêu cầu cụ thể mà người dùng ảo gửi đi (ví dụ: HTTP Request, JDBC Request).
- Listener (Trình lắng nghe): Thu thập và hiển thị kết quả kiểm thử (ví dụ: Graph Results, View Results Tree, Summary Report).
- Configuration Elements: Cấu hình chung cho Sampler (ví dụ: HTTP Request Defaults, HTTP Cookie Manager).
- Logic Controllers: Điều khiển luồng thực thi của các Sampler (ví dụ: If Controller, Loop Controller).
- Assertions: Xác minh phản hồi của server (ví dụ: Response Assertion để kiểm tra nội dung, Duration Assertion để kiểm tra thời gian phản hồi).


**Cách sử dụng cơ bản (GUI):**

- Cài đặt Java: JMeter yêu cầu Java Runtime Environment (JRE).
- Tải JMeter: Tải về từ trang web chính thức của Apache JMeter.
- Khởi chạy JMeter: Chạy file `jmeter.bat` (Windows) hoặc `jmeter.sh` (Linux/macOS) trong thư mục bin của JMeter.

**Ưu điểm của JMeter:**

- Linh hoạt và mạnh mẽ: Có thể mô phỏng hầu hết mọi loại tải và kịch bản người dùng.
- Hỗ trợ đa dạng: Kiểm thử hiệu suất cho web, database, API, FTP, mail server, v.v.
- Tạo báo cáo chi tiết: Các Listener cung cấp nhiều dạng báo cáo trực quan.
- Cộng đồng lớn: Có nhiều tài liệu, hướng dẫn và plugins hỗ trợ.

**Hạn chế của JMeter:**

- Yêu cầu tài nguyên: JMeter (đặc biệt là giao diện GUI) tiêu thụ khá nhiều RAM và CPU, đặc biệt khi chạy test với số lượng người dùng lớn trên cùng một máy. Nên chạy chế độ non-GUI cho kiểm thử tải lớn.
- Đường cong học tập: Phức tạp hơn ab, đòi hỏi thời gian để làm quen với các thành phần và cách thiết kế Test Plan.
- Không phải là trình duyệt thực: JMeter hoạt động ở cấp độ giao thức, không phải là một trình duyệt web hoàn chỉnh. Nó không render JavaScript hay CSS, điều này có thể ảnh hưởng đến độ chính xác khi kiểm thử các ứng dụng web phức tạp phụ thuộc nhiều vào frontend.
