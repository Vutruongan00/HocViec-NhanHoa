# Tối ưu DATABASE kết hợp WEB Server

## I. Tối ưu hóa Phía Cơ sở dữ liệu (Database Optimization)

Một cơ sở dữ liệu được tối ưu hóa tốt là nền tảng cho hiệu suất của ứng dụng web.

### 1. Tối ưu hóa cấu trúc cơ sở dữ liệu

#### Thiết kế Schema hợp lý

- **Chuẩn hóa dữ liệu (Normalization):**  
  Giảm sự dư thừa dữ liệu và cải thiện tính toàn vẹn của dữ liệu bằng cách tổ chức bảng và mối quan hệ một cách hợp lý.

- **Phân vùng bảng (Partitioning):**  
  Đối với các bảng lớn, chia nhỏ bảng thành các phần nhỏ hơn dựa trên một tiêu chí (ví dụ: ngày, ID) giúp cải thiện hiệu suất truy vấn và quản lý dữ liệu.

#### Sử dụng chỉ mục (Indexes) hiệu quả

- **Tạo chỉ mục trên các cột thường xuyên được truy vấn:**  
  Chỉ mục giúp tăng tốc độ tìm kiếm, sắp xếp và lọc dữ liệu. Tuy nhiên, quá nhiều chỉ mục có thể làm chậm các thao tác ghi (INSERT, UPDATE, DELETE).

- **Chọn loại chỉ mục phù hợp:**  
  Sử dụng chỉ mục B-tree cho các truy vấn phạm vi, chỉ mục hash cho các truy vấn so sánh bằng.

- **Kiểm tra và loại bỏ chỉ mục không cần thiết:**  
  Định kỳ phân tích và loại bỏ các chỉ mục không được sử dụng để tránh lãng phí tài nguyên.

#### Chọn kiểu dữ liệu phù hợp

- Sử dụng kiểu dữ liệu nhỏ nhất có thể để lưu trữ dữ liệu mà không làm mất thông tin (ví dụ: `INT` thay vì `BIGINT` nếu giá trị không vượt quá giới hạn).
- Tránh sử dụng `BLOB`/`TEXT` cho các trường có thể lưu trữ dữ liệu nhỏ hơn bằng `VARCHAR`.

### 2. Tối ưu hóa truy vấn (Query Optimization)

#### Viết truy vấn hiệu quả

- Tránh `SELECT *`: Chỉ chọn các cột cần thiết.
- Hạn chế `JOIN` không cần thiết: Giảm số lượng bảng tham gia trong một truy vấn.
- Sử dụng `WHERE` clause hiệu quả: Lọc dữ liệu càng sớm càng tốt để giảm tập dữ liệu cần xử lý.
- Tránh các hàm trong `WHERE` clause: Các hàm (như `DATE()`, `SUBSTRING()`) áp dụng trên cột có chỉ mục sẽ làm vô hiệu hóa việc sử dụng chỉ mục.

#### Phân tích truy vấn chậm (Slow Query Log)

- Kích hoạt và phân tích nhật ký truy vấn chậm để xác định các truy vấn gây tắc nghẽn hiệu suất.
- Sử dụng các công cụ như `EXPLAIN` (MySQL, PostgreSQL) để hiểu cách cơ sở dữ liệu thực thi truy vấn và tối ưu hóa chúng.

#### Tránh N+1 Query Problem

- Trong các ứng dụng ORM (Object-Relational Mapping), tránh việc thực hiện nhiều truy vấn nhỏ trong một vòng lặp để lấy dữ liệu liên quan.
- Thay vào đó, sử dụng các phương pháp *eager loading* hoặc *join* để lấy tất cả dữ liệu cần thiết trong một hoặc ít truy vấn.

### 3. Cấu hình Database Server

#### Cấu hình bộ nhớ đệm (Cache)

- **Buffer Pool Size (MySQL InnoDB):** Tăng kích thước buffer pool để lưu trữ nhiều dữ liệu và chỉ mục hơn trong RAM.
- **Query Cache (MySQL - phiên bản cũ):** Nếu đang sử dụng phiên bản cũ, cần cấu hình phù hợp.
- **Shared Buffers (PostgreSQL):** Tăng kích thước shared buffers để lưu trữ các khối dữ liệu được truy cập thường xuyên.

#### Giới hạn kết nối (Connection Limits)

- Cấu hình số lượng kết nối tối đa phù hợp để tránh quá tải cơ sở dữ liệu.

#### Cấu hình I/O đĩa

- Đảm bảo hệ thống lưu trữ có hiệu suất cao (SSD/NVMe).
- Cấu hình I/O tối ưu để giảm độ trễ truy xuất dữ liệu.

### 4. Quản lý kết nối (Connection Management)

- **Sử dụng Connection Pooling:**  
  Thay vì mở và đóng kết nối DB cho mỗi yêu cầu, ứng dụng nên sử dụng connection pool để tái sử dụng kết nối, giảm chi phí.

- **Giữ kết nối ngắn gọn:**  
  Đóng kết nối cơ sở dữ liệu ngay sau khi hoàn thành thao tác để giải phóng tài nguyên
  
## II. Tối ưu hóa Phía Web Server (Web Server Optimization)

Web Server là giao diện trực tiếp với người dùng và có vai trò quan trọng trong việc chuyển tiếp các yêu cầu đến cơ sở dữ liệu một cách hiệu quả.

### 1. Tối ưu hóa cấu hình Web Server

#### Cấu hình Worker Processes/Threads

- **Apache (MPM Event/Worker):**  
  Cấu hình số lượng tiến trình và luồng con phù hợp với tài nguyên server và mô hình tải.

- **Nginx (Worker Processes/Connections):**  
  Tăng `worker_processes` và `worker_connections` để xử lý nhiều kết nối đồng thời hơn.

#### Kích hoạt bộ nhớ đệm (Caching) trên Web Server

- **Caching nội dung tĩnh:**  
  Cấu hình Web Server để phục vụ các tệp tĩnh (CSS, JS, hình ảnh) từ bộ nhớ đệm hoặc từ mạng phân phối nội dung (CDN) để giảm tải cho máy chủ gốc và cơ sở dữ liệu.

- **Reverse Proxy Caching:**  
  Sử dụng Nginx làm reverse proxy với bộ nhớ đệm cho các phản hồi động từ ứng dụng, giảm số lượng yêu cầu đến máy chủ ứng dụng và cơ sở dữ liệu.

#### Nén dữ liệu (Gzip/Brotli)

- Kích hoạt nén HTTP (Gzip hoặc Brotli) cho các phản hồi văn bản để giảm kích thước dữ liệu truyền qua mạng, giúp tải trang nhanh hơn.

#### Giữ kết nối (Keep-Alive)

- Bật Keep-Alive để cho phép nhiều yêu cầu được gửi qua một kết nối TCP duy nhất, giảm độ trễ và chi phí thiết lập kết nối.

#### Tắt các module/dịch vụ không cần thiết

- Giảm thiểu tài nguyên mà Web Server tiêu thụ bằng cách tắt các module Apache hoặc Nginx không được sử dụng.

### 2. Tối ưu hóa kết nối giữa Web Server và Database

#### Đặt Database Server trên một máy chủ riêng biệt

- Đối với các ứng dụng có lưu lượng truy cập cao, tách rời Web Server và Database Server ra các máy chủ riêng giúp phân bổ tài nguyên hiệu quả và giảm tranh chấp.

#### Sử dụng mạng nội bộ tốc độ cao

- Đảm bảo kết nối mạng giữa Web Server và Database Server là nhanh và ổn định (ví dụ: sử dụng mạng LAN ảo, kết nối 10Gbps).

#### Cấu hình Firewall phù hợp

- Đảm bảo firewall cho phép kết nối giữa Web Server và Database Server trên các cổng cần thiết và không gây ra độ trễ.

#### Load Balancer (Bộ cân bằng tải)

- Sử dụng bộ cân bằng tải để phân phối lưu lượng truy cập đến nhiều Web Server, giúp mở rộng ứng dụng theo chiều ngang.
- Kết hợp với các giải pháp Database Replication (Master-Slave, Master-Master) hoặc Clustering để phân phối tải truy vấn.
