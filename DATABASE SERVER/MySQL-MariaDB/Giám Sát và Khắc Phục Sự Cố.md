
> # Giám Sát và Khắc Phục Sự Cố
> 
> Giám sát và khắc phục sự cố là các hoạt động liên tục nhằm đảm bảo hệ thống database hoạt động hiệu quả, ổn định và đáng tin cậy.

# 1. Các công cụ giám sát database

## 1.1. Zabbix

- **Tổng quan:** Zabbix là một giải pháp giám sát mã nguồn mở mạnh mẽ và linh hoạt, được thiết kế để giám sát mọi thứ từ mạng, máy chủ, ứng dụng đến cơ sở dữ liệu. Zabbix hỗ trợ giám sát đa dạng các loại cơ sở dữ liệu (SQL, NoSQL) thông qua các tác nhân (Agent), plugin Go, ODBC và các yêu cầu API.
- **Tính năng nổi bật** cho Database Monitoring:
    - Giám sát toàn diện: Theo dõi các chỉ số về CPU, bộ nhớ, dung lượng ổ đĩa, độ trễ đĩa, số lượng kết nối, truy vấn chậm, v.v.
    - Agent 2 và Plugin Go: Zabbix Agent 2 sử dụng các plugin được viết bằng Go để giao tiếp trực tiếp với cơ sở dữ liệu bằng API hoặc thư viện gốc, giúp việc thu thập dữ liệu chính xác và hiệu quả.
    - Template sẵn có: Cung cấp các template có sẵn cho nhiều cơ sở dữ liệu phổ biến như MySQL, PostgreSQL, MongoDB, Oracle, Microsoft SQL Server, giúp triển khai giám sát nhanh chóng.
    - Cấu hình linh hoạt: Cho phép giới hạn thời gian thực thi truy vấn, kiểm soát thời gian phiên, cấu hình mã hóa giữa Agent và database, kiểm soát chế độ cache.
    - Cảnh báo & Báo cáo: Hỗ trợ cảnh báo linh hoạt qua email, SMS và nhiều kênh khác. Cung cấp các báo cáo và biểu đồ trực quan về hiệu suất.
- **Ưu điểm**: Mã nguồn mở, miễn phí, khả năng mở rộng không giới hạn, giám sát phân tán, bảo mật cao, linh hoạt.
- **Nhược điểm**: Yêu cầu kiến thức kỹ thuật nhất định để cài đặt và cấu hình chuyên sâu.

## 1.2. Nagios
- **Nagios** là một trong những công cụ giám sát mã nguồn mở lâu đời và phổ biến nhất, cung cấp khả năng giám sát toàn diện cho hạ tầng IT, bao gồm máy chủ, mạng, ứng dụng và cơ sở dữ liệu.
- **Tính năng**: 
    - Kiến trúc Plugin mạnh mẽ: Nagios sử dụng kiến trúc plugin, cho phép người dùng mở rộng khả năng giám sát cho nhiều loại cơ sở dữ liệu khác nhau như MySQL, PostgreSQL, Oracle, v.v., bằng cách cài đặt các plugin tương ứng.
    - Giám sát hiệu suất và tình trạng: Theo dõi các thông số quan trọng như trạng thái dịch vụ database, thời gian phản hồi truy vấn, sử dụng tài nguyên (CPU, bộ nhớ, I/O).
    - Cảnh báo tùy chỉnh: Cung cấp hệ thống cảnh báo linh hoạt khi các ngưỡng hiệu suất bị vi phạm.
    - Giao diện web: Cung cấp giao diện web để quản lý và theo dõi các dịch vụ được giám sát.

- Ưu điểm: Mã nguồn mở, cộng đồng lớn, ổn định, hiệu suất cao, khả năng tùy biến cao với hàng nghìn plugin.
- Nhược điểm: Cấu hình có thể phức tạp, giao diện web có thể không hiện đại bằng các công cụ mới hơn.

## 1.3. Cacti
- **Cacti** là một framework giám sát mạng dựa trên web mã nguồn mở, chủ yếu tập trung vào việc thu thập dữ liệu time-series (dữ liệu theo thời gian) và trực quan hóa chúng dưới dạng biểu đồ. Cacti sử dụng công cụ RRDtool để lưu trữ và vẽ biểu đồ dữ liệu.
- **Tính năng nổi bật cho Database Monitoring**:
     - Trực quan hóa dữ liệu: Điểm mạnh nhất của Cacti là khả năng tạo ra các biểu đồ đẹp mắt và chi tiết về các thông số được giám sát
     - Thu thập dữ liệu linh hoạt: Mặc dù ban đầu được thiết kế để giám sát lưu lượng mạng qua SNMP, Cacti có thể mở rộng để thu thập dữ liệu từ hầu hết mọi phương pháp, bao gồm cả dữ liệu từ database thông qua các script PHP hoặc các plugin.
     - Quản lý thiết bị và Template: Cho phép người dùng tạo và quản lý các thiết bị, template để tự động hóa việc tạo biểu đồ và nguồn dữ liệu.

-**Ưu điểm**: Mã nguồn mở, miễn phí, mạnh về trực quan hóa dữ liệu lịch sử dưới dạng biểu đồ, có khả năng mở rộng qua plugin.
- **Nhược điểm**: Chủ yếu là công cụ tạo đồ thị, các tính năng cảnh báo và phân tích chuyên sâu cho database có thể bị hạn chế so với các công cụ giám sát tổng thể.


## 1.4. Grafana
- **Grafana** là một nền tảng hiển thị dữ liệu và giám sát mã nguồn mở hàng đầu, cho phép  tạo các bảng điều khiển (dashboard) tương tác và trực quan từ nhiều nguồn dữ liệu khác nhau. Grafana không tự thu thập dữ liệu mà tích hợp với các hệ thống thu thập dữ liệu khác.
- **Tính năng:**
    - Dashboard trực quan: Tạo các dashboard tùy chỉnh với nhiều loại biểu đồ, đồ thị để hiển thị các chỉ số hiệu suất database (CPU, bộ nhớ, I/O, truy vấn, trạng thái kết nối, v.v.).
    - Hỗ trợ đa dạng nguồn dữ liệu: Có thể kết nối với nhiều loại database trực tiếp (như MySQL, PostgreSQL, SQL Server, Prometheus, InfluxDB, Elasticsearch) hoặc thông qua các công cụ thu thập dữ liệu khác (như Telegraf).
    - Cảnh báo mạnh mẽ: Cung cấp khả năng thiết lập cảnh báo dựa trên các ngưỡng được xác định trước, có thể gửi thông báo qua nhiều kênh (email, Slack, PagerDuty, v.v.).
    - Khả năng mở rộng: Có một hệ sinh thái plugin phong phú để tích hợp với các công cụ và dịch vụ khác.

- **Ưu điểm**: Mã nguồn mở, miễn phí, giao diện đẹp, trực quan, khả năng tùy biến cao, hỗ trợ nhiều nguồn dữ liệu, hệ thống cảnh báo linh hoạt.
- **Nhược điểm**: Cần kết hợp với một công cụ thu thập dữ liệu (ví dụ: Prometheus, Telegraf) để có một giải pháp giám sát hoàn chỉnh.

## 1.5.  Percona Monitoring and Management (PMM)

- **Percona Monitoring and Management (PMM)** là một nền tảng giám sát và quản lý cơ sở dữ liệu mã nguồn mở, miễn phí, được tối ưu hóa cho MySQL, PostgreSQL và MongoDB.
- **Tính năng**: 
    - Giám sát toàn diện: Cung cấp khả năng hiển thị đầy đủ về hiệu suất của MySQL, PostgreSQL và MongoDB, từ toàn bộ cụm database đến từng truy vấn riêng lẻ.
    - Query Analytics (QAN): Phân tích hiệu suất truy vấn database, giúp xác định các truy vấn chậm, phân tích kế hoạch thực thi truy vấn và tối ưu hóa chúng.
    - Metrics Monitoring: Giám sát toàn diện các chỉ số hiệu suất chính như sử dụng CPU, bộ nhớ, I/O đĩa, lưu lượng mạng và các chỉ số cụ thể của database.
    - Dashboard thân thiện: Cung cấp giao diện đồ họa trực quan với các dashboard có thể tùy chỉnh để trực quan hóa hiệu suất database.
    - Cảnh báo: Cho phép thiết lập cảnh báo dựa trên các ngưỡng định trước, gửi thông báo qua email hoặc các kênh khác.
    - Percona Advisors: Các cố vấn tích hợp chạy kiểm tra định kỳ trên các database được kết nối để xác định các mối đe dọa bảo mật tiềm ẩn, suy giảm hiệu suất, mất dữ liệu và hỏng dữ liệu.

- **Ưu điểm**: Mã nguồn mở, miễn phí, tập trung vào các database phổ biến **(MySQL, PostgreSQL, MongoDB)**, khả năng phân tích truy vấn mạnh mẽ.
- **Nhược điểm**: Yêu cầu một mức độ kiến thức kỹ thuật để triển khai và cấu hình.

## 1.6. Site24x7
- **Site24x7** là một giải pháp giám sát tất cả trong một dựa trên đám mây, được cung cấp bởi Zoho Corp. Nó không chỉ giám sát cơ sở dữ liệu mà còn cung cấp khả năng giám sát toàn diện cho website, máy chủ, ứng dụng, đám mây, mạng và trải nghiệm người dùng thực.
- **Tính năng nổi bật của Site24x7**
    - **Hỗ trợ đa dạng Database**: Giám sát các hệ quản trị cơ sở dữ liệu quan hệ (RDBMS) và NoSQL phổ biến, bao gồm:
        - SQL: MySQL, PostgreSQL, Microsoft SQL Server, Oracle Database, MariaDB, IBM DB2.
        - NoSQL: MongoDB, Cassandra, Redis, Couchbase.
        - Cloud Databases: Amazon RDS, Google Cloud SQL, Azure Database Services, Oracle Autonomous Database.

    - **Giám sát hiệu suất chuyên sâu:**
        - Tình trạng và khả dụng: Theo dõi thời gian hoạt động, trạng thái kết nối, và các sự kiện down/up.
        - Chỉ số tài nguyên: Giám sát mức sử dụng CPU, bộ nhớ, I/O đĩa, dung lượng ổ đĩa, và lưu lượng mạng của máy chủ database.
        - Chỉ số cụ thể của Database: Theo dõi các chỉ số quan trọng như số lượng kết nối, tốc độ truy vấn, thời gian phản hồi truy vấn, hit ratios (ví dụ: buffer cache hit ratio), khóa (locks), deadlocks, số lượng hàng được đọc/ghi/cập nhật/xóa, và kích thước database.
        - Phân tích truy vấn (Query Analysis): Xác định các truy vấn chậm (slow queries), các truy vấn tiêu tốn nhiều tài nguyên nhất, và cung cấp thông tin chi tiết về thời gian thực thi, lỗi, và các thuộc tính khác của truy vấn để giúp tối ưu hóa.
        - Giám sát Replication: Theo dõi trạng thái và độ trễ của các cấu hình sao chép (replication) database để đảm bảo tính nhất quán của dữ liệu.

    - **Cảnh báo và tự động hóa**:
        - Cảnh báo tùy chỉnh: Thiết lập ngưỡng cảnh báo cho các chỉ số quan trọng và nhận thông báo qua email, SMS, cuộc gọi thoại, Slack, Microsoft Teams, PagerDuty, v.v.
        - Hành động khắc phục tự động (IT Automation): Tự động thực hiện các hành động khắc phục khi phát hiện sự cố, ví dụ như khởi động lại dịch vụ, chạy script.

- **Ưu điểm:** 
    - Dễ triển khai và sử dụng: Là dịch vụ dựa trên đám mây, việc cài đặt và cấu hình thường đơn giản hơn so với các giải pháp on-premises.
    - Giám sát đa dạng: Hỗ trợ nhiều loại database và môi trường (on-premises, cloud, hybrid).
    - Cảnh báo và tự động hóa mạnh mẽ.
- **Nhược điểm:** bản thương mại, chi phí cao

---
# 2. Nhận diện và xử lý bottleneck
>**Bottleneck** (nút thắt cổ chai) là điểm yếu trong hệ thống làm hạn chế hiệu suất tổng thể. Trong database, chúng thường liên quan đến CPU, RAM, Disk I/O, Network hoặc các truy vấn không tối ưu.

## 2.1. MySQL/MariaDB
**Nhận diện:**
- CPU tăng cao:`top`, `htop` (Linux), Task Manager (Windows). Kết hợp với `SHOW FULL PROCESSLIST` để xem truy vấn nào đang dùng nhiều CPU.
- Kiểm tra slow query log (`/etc/mysql/my.cnf`)
- Kiểm tra I/O (disk, RAM swap): `iostat`, `vmstat` (Linux). Có thể do thiếu index, bộ đệm không đủ.

**Xử lý:**
- **Tối ưu hóa truy vấn**: Sử dụng `EXPLAIN` để phân tích và thêm các chỉ mục (`indexes`) phù hợp. Tránh `SELECT *`, tránh hàm trên cột có index trong mệnh đề WHERE.
- Tối ưu hóa cấu hình:
    - Tăng `innodb_buffer_pool_size` (nếu dùng InnoDB và có đủ RAM). Khoảng 50-70% RAM của server thường được khuyến nghị.
    - Điều chỉnh `max_connections`, `innodb_flush_log_at_trx_commit`.
- **Tối ưu hóa I/O**: Đảm bảo ổ đĩa đủ nhanh (SSD). Phân chia dữ liệu...
- **Tăng tài nguyên phần cứng**: Nâng cấp CPU, RAM, SSD

## 2.2  Microsoft SQL Server
**Nhận diện**
- Dùng `Activity Monitor` để kiểm tra:%CPU, Blocking Session , Wait Type

**Xử lý:**
- Tối ưu hóa truy vấn và chỉ mục: Sử dụng Query Store để phân tích lịch sử, EXPLAIN (Execution Plan trong SSMS) để xem kế hoạch truy vấn. Thêm, sửa đổi hoặc xóa chỉ mục.
- Quản lý bộ nhớ: Cấu hình` max server memory` và `min server memory`. Tối ưu hóa bộ đệm (buffer pool).

## 2.3 PostgreSQL
- **Nhận diện:** CPU tăng cao, RAM/Cache miss, I/O Disk
- **Xử lý:** 
    - Tối ưu hóa truy vấn và chỉ mục
    - Phân vùng cho các bảng lớn để cải thiện hiệu suất truy vấn và quản lý dữ liệu
    - Cấu hình điều chỉnh bộ đệm chính của db `shared_buffers` 

## MongoDB
- Tối ưu hóa truy vấn sử dụng chỉ mục hiệu quả
- **Sharding**: Phân mảnh dữ liệu theo chiều ngang để phân tán tải và lưu trữ trên nhiều máy chủ
- Tăng tài nguyên...
## Redis
- **Clustering**: Sử dụng Redis Cluster để phân tán dữ liệu và tải trên nhiều node.
- **Giảm tải**: Tách các instance Redis cho các mục đích khác nhau (ví dụ: một cho cache, một cho queue)...

---

# 3. Xử lý deadlock và các vấn đề đồng thời

- **Deadlock** (bế tắc) xảy ra khi hai hoặc nhiều giao dịch (transaction) đang chờ nhau, mỗi giao dịch đang giữ tài nguyên mà giao dịch khác cần để tiếp tục. Để giải quyết deadlock và các vấn đề đồng thời trong database, cần sử dụng các phương pháp như phát hiện deadlock, ngăn chặn deadlock, hoặc xử lý deadlock khi nó xảy ra

## Xử lý Deadlock

- **MySQL (InnoDB Storage Engine):** MySQL (InnoDB) chủ yếu sử dụng **Deadlock Detection**
    - InnoDB tự động phát hiện deadlock bằng cách sử dụng biểu đồ chờ.
    - ...
    - ..
    - .
    - - 
