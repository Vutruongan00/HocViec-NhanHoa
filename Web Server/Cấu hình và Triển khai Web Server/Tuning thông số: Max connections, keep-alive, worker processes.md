# 1. Max Connections
- Max Connections (hay Max Clients, Max Workers, Worker Connections, v.v., tùy theo Web Server) là giới hạn số lượng kết nối đồng thời mà một Web Server được phép mở và duy trì. Mỗi khi một trình duyệt kết nối đến máy chủ để tải một trang web hoặc tài nguyên, một "kết nối" được thiết lập. Nếu số lượng kết nối vượt quá giới hạn này, máy chủ sẽ bắt đầu từ chối các kết nối mới, dẫn đến lỗi "Connection refused" hoặc "503 Service Unavailable" đối với người dùng.

## **1.1. Yếu tố cần xem xét khi điều chỉnh**
Khi điều chỉnh Max Connections, bạn cần cân nhắc các yếu tố sau:

- Tài nguyên phần cứng:

    - RAM (Bộ nhớ): Mỗi kết nối tiêu tốn một lượng RAM nhất định. Đây là yếu tố quan trọng nhất. Xác định lượng RAM trung bình mà mỗi tiến trình/worker tiêu tốn.
    - CPU: Mức độ sử dụng CPU cho mỗi kết nối (phụ thuộc vào độ phức tạp của ứng dụng web).
    - File Descriptors (FDs): Mỗi kết nối sử dụng ít nhất một file descriptor. Hệ điều hành cũng có giới hạn về số lượng FDs mà một tiến trình hoặc toàn hệ thống có thể mở.
    
- Loại ứng dụng web:

    - Ứng dụng tĩnh (Static content): Tốn ít tài nguyên hơn cho mỗi kết nối.
    - Ứng dụng động (Dynamic content - PHP, Python, Node.js, Ruby): Tốn nhiều tài nguyên hơn, đặc biệt nếu có tương tác cơ sở dữ liệu hoặc tính toán phức tạp.
    - Kết nối "long-polling" hoặc WebSockets: Duy trì kết nối trong thời gian dài, cần quản lý cẩn thận.

- Lưu lượng truy cập:
    - Lượng truy cập đồng thời trung bình và đỉnh điểm.
- Thời gian trung bình mà một kết nối được duy trì.

- Hệ điều hành: Giới hạn mặc định về số lượng file descriptors của hệ điều hành (thường là 1024). Bạn có thể cần tăng giới hạn này bằng cách sửa file /etc/security/limits.conf hoặc /etc/sysctl.conf.

## **1.2. Cấu hình Max Connections trên các Web Server phổ biến**
**a. Nginx**
Nginx hoạt động theo mô hình bất đồng bộ, sử dụng ít tiến trình hơn nhưng mỗi tiến trình có thể xử lý hàng ngàn kết nối. Thông số chính cần quan tâm là `worker_connections`.
- File cấu hình: Thường là `/etc/nginx/nginx.conf` hoặc các file trong `/etc/nginx/conf.d/` hoặc `/etc/nginx/sites-available/.`
- Thông số:
    - `worker_processes`: Số lượng tiến trình worker. Lý tưởng nhất là bằng số lõi CPU của bạn
    - `worker_connections`: Số lượng kết nối tối đa mà MỘT tiến trình worker có thể xử lý.

- **Ví dụ: tuning max connections** 
    - Chỉnh sửa file cấu hình 
		```
		sudo nano /etc/nginx/nginx.conf
		```
	- Tuning thông số worker_connections đây chính là thông số cấu hình số kết nối đồng thời trong Nginx
		```
		worker_connections 10240;
		```
	- Reload Nginx để apply 
		```
		sudo systemctl reload nginx.service
		```
**b. Apache**
- Chỉnh sửa file cấu hình `/etc/apache2/mods-available/mpm-prefolk.conf`
		```
		nano /etc/apache2/mods-available/mpm-prefolk.conf
		```
		- Cấu hình các thông số 
			```
			<IfModule mpm_prefork_module>
					StartServers               5
					MinSpareServers            5
					MaxSpareServers           10
					MaxRequestWorkers        150
					MaxConnectionsPerChild     0
			</IfModule>
			```
			- StartServers 5: Khởi động 5 tiến trình Apache khi server bật.
			- MinSpareServers 5: Duy trì ít nhất 5 tiến trình "rảnh rỗi" để xử lý yêu cầu mới nhanh chóng.
			- MaxSpareServers 10: Giới hạn tối đa 10 tiến trình "rảnh rỗi".
			- MaxRequestWorkers 150: Tổng số tiến trình tối đa mà Apache có thể tạo để xử lý yêu cầu đồng thời (giới hạn quan trọng nhất).
			- MaxConnectionsPerChild 0: Mỗi tiến trình con xử lý không giới hạn số yêu cầu trước khi được khởi tạo lại.
		- Restart để apply cấu hình 
		```
		systemctl restart apache2 
		```

# 2. HTTP Keep-Alive
## 2.1. HTTP Keep-Alive là gì?
HTTP Keep-Alive là một cơ chế trong giao thức HTTP cho phép một kết nối TCP duy nhất được sử dụng để gửi và nhận nhiều yêu cầu và phản hồi HTTP liên tiếp. Thay vì đóng kết nối sau mỗi cặp yêu cầu/phản hồi, kết nối được giữ "sống" (keep-alive) trong một khoảng thời gian nhất định, sẵn sàng để phục vụ các yêu cầu tiếp theo từ cùng một client.
## 2.2. Cách thức hoạt động của Keep-Alive
- **Thiết lập kết nối ban đầu**: Trình duyệt gửi yêu cầu đầu tiên đến máy chủ. Một kết nối TCP được thiết lập thông qua quá trình bắt tay 3 bước (3-way handshake).
- **Trao đổi dữ liệu**: Máy chủ xử lý yêu cầu và gửi phản hồi.
- **Giữ kết nối mở:** Thay vì đóng kết nối, cả trình duyệt và máy chủ đều đồng ý giữ kết nối mở trong một khoảng thời gian chờ (timeout) nhất định (ví dụ: 5-15 giây).
- **Tái sử dụng kết nối:** Nếu trình duyệt có yêu cầu tiếp theo trong thời gian chờ đó (ví dụ: tải các file CSS, JavaScript, hình ảnh trên cùng một trang), nó sẽ sử dụng lại kết nối TCP hiện có thay vì thiết lập một kết nối mới.
- **Đóng kết nối**: Nếu không có yêu cầu nào được gửi trong khoảng thời gian chờ, hoặc nếu số lượng yêu cầu tối đa trên một kết nối đã đạt đến, kết nối sẽ được đóng.

Để bật Keep-Alive, HTTP/1.1 yêu cầu client gửi header Connection: keep-alive trong yêu cầu, và server cũng sẽ gửi lại header tương tự trong phản hồi.

## 2.3. Lợi ích của HTTP Keep-Alive trong việc tối ưu tốc độ
- Giảm độ trễ (Latency)
  
- Giảm mức sử dụng CPU/Bộ nhớ: Cả client và server đều tốn tài nguyên để thiết lập và đóng các kết nối. Với Keep-Alive, số lượng kết nối tổng thể được giảm thiểu, giúp tiết kiệm tài nguyên cho cả hai bên.
- Hiệu suất mạng tốt hơn: Giảm số lượng gói tin truyền qua mạng, giúp tối ưu hóa việc sử dụng băng thông.
- Cải thiện tốc độ tải trang: Đặc biệt đối với các trang web có nhiều tài nguyên nhỏ (như hình ảnh, icon, font, file CSS/JS riêng lẻ), việc tái sử dụng kết nối giúp tải tất cả các tài nguyên này nhanh chóng hơn.

## 2.4. Cấu hình trên Nginx
- File cấu hình: Thường nằm trong khối `http {}` hoặc `server {}` trong `/etc/nginx/nginx.conf` hoặc các file include.
- Thông số:
    - keepalive_timeout: Thời gian tối đa (tính bằng giây) mà một kết nối keep-alive sẽ được giữ mở trên máy chủ.
    - keepalive_requests: Số lượng yêu cầu tối đa có thể được phục vụ trên một kết nối keep-alive duy nhất.
- Ví dụ: 
```cat=
http {
    # Thời gian (giây) mà một kết nối keep-alive sẽ được giữ mở.
    # Phần thứ hai (tùy chọn) là giá trị của header Keep-Alive: timeout=time
    keepalive_timeout 65s;

    # Số lượng yêu cầu tối đa có thể được phục vụ trên một kết nối keep-alive duy nhất.
    # Sau khi đạt đến giới hạn này, kết nối sẽ tự động đóng lại.
    # Điều này giúp ngăn chặn việc một client độc chiếm một kết nối quá lâu.
    keepalive_requests 1000;

    # ... các cấu hình http khác ...

    server {
        # ... cấu hình server block của bạn ...
    }
}
```

## 2.5. Cấu hình trên Apache 
- File cấu hình: Thường là httpd.conf hoặc các file cấu hình liên quan đến MPM
- Thông số:
    - `KeepAlive`: Bật hoặc tắt Keep-Alive.
    - `MaxKeepAliveRequests`: Số lượng yêu cầu tối đa trên một kết nối.
    - `KeepAliveTimeout`: Thời gian chờ cho kết nối.

-Ví dụ:
```cat=
# Trong httpd.conf hoặc file cấu hình liên quan

# Bật Keep-Alive
KeepAlive On

# Số lượng yêu cầu tối đa trên một kết nối Keep-Alive.
# Giá trị 0 nghĩa là không giới hạn (không khuyến nghị).
MaxKeepAliveRequests 100

# Thời gian (giây) mà Apache sẽ đợi cho yêu cầu tiếp theo trên một kết nối Keep-Alive.
KeepAliveTimeout 5
```

# 3. Ưorker processes
**Worker Process** là một tiến trình con (child process) được tạo ra bởi tiến trình chính (master process) của một Web Server.
- **Master Process (Tiến trình chính)**: Chịu trách nhiệm quản lý tổng thể. Nó khởi tạo các worker processes, giám sát trạng thái của chúng, và nếu một worker process bị lỗi, nó có thể khởi động lại một worker mới. Master process thường không xử lý các yêu cầu client trực tiếp.
- **Worker Processes (Tiến trình worker)**: Các tiến trình này thực sự là những "người làm việc" nhận các yêu cầu kết nối đến server, đọc yêu cầu, xử lý nó (bằng cách phục vụ file tĩnh, chuyển tiếp yêu cầu đến ứng dụng backend như PHP-FPM, Node.js, v.v.), và gửi phản hồi lại cho client.

