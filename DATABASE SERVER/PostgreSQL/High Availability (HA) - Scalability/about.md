# I. High Availability (HA) - Tính Sẵn Sàng Cao trên PostgreSQL

**High Availability (HA)** là khả năng của một hệ thống, ứng dụng hoặc dịch vụ duy trì hoạt động liên tục và không bị gián đoạn trong một khoảng thời gian đáng kể. Mục tiêu chính của HA là **giảm thiểu thời gian ngừng hoạt động (downtime)** do các sự cố phần cứng, phần mềm, lỗi mạng hoặc lỗi con người. Một hệ thống HA thường được thiết kế để đạt mục tiêu **uptime rất cao**, như 99.99% ("four nines") hoặc 99.999% ("five nines").

## Nguyên tắc hoạt động của HA

- **Dự phòng (Redundancy)**: Triển khai các thành phần dự phòng (máy chủ, mạng, lưu trữ) để khi một thành phần bị lỗi, thành phần dự phòng có thể tiếp quản.
- **Phát hiện lỗi (Failure Detection)**: Hệ thống phải có khả năng phát hiện lỗi nhanh chóng và tự động.
- **Chuyển đổi dự phòng (Failover)**: Tự động chuyển đổi sang tài nguyên dự phòng khi phát hiện lỗi, đảm bảo dịch vụ không bị gián đoạn.
- **Phục hồi (Recovery)**: Khả năng khôi phục thành phần bị lỗi và đưa nó trở lại trạng thái sẵn sàng dự phòng.

## Các phương pháp và công nghệ HA phổ biến trong PostgreSQL

PostgreSQL cung cấp nhiều cơ chế để xây dựng các giải pháp HA, chủ yếu dựa trên replication (sao chép dữ liệu).

### Streaming Replication (Sao chép luồng)

- **Cách hoạt động**: Là phương pháp HA chính của PostgreSQL. Máy chủ chính (Primary/Master) liên tục gửi các bản ghi Write-Ahead Log (WAL) đến một hoặc nhiều máy chủ dự phòng (Standby/Replica). Các máy chủ standby áp dụng các bản ghi WAL này để giữ dữ liệu của chúng đồng bộ với primary.
- **Ưu điểm**: Tích hợp sẵn trong PostgreSQL, hiệu quả và đáng tin cậy.

#### Các chế độ:

- **Asynchronous Replication (Bất đồng bộ)**: Primary không chờ xác nhận từ standby trước khi commit giao dịch. Tối đa hóa hiệu suất ghi (write performance) nhưng có thể mất một lượng nhỏ dữ liệu (RPO > 0) nếu primary gặp sự cố trước khi các bản ghi WAL được standby ghi nhận.
- **Synchronous Replication (Đồng bộ)**: Primary chờ xác nhận từ ít nhất một standby rằng các bản ghi WAL đã được ghi vào đĩa của standby trước khi commit giao dịch. Đảm bảo không mất dữ liệu (RPO = 0) trong trường hợp primary thất bại, nhưng có thể ảnh hưởng đến hiệu suất ghi.
- **Hot Standby**: Các standby có thể được cấu hình ở chế độ Hot Standby, cho phép chúng phục vụ các truy vấn đọc (read queries) ngay cả khi đang trong quá trình khôi phục hoặc chờ failover. Điều này cũng giúp tăng khả năng đọc (read scalability).

### Logical Replication (Sao chép logic)

- **Cách hoạt động**: Sao chép các thay đổi dữ liệu ở cấp độ đối tượng (ví dụ: các bảng cụ thể) thay vì toàn bộ cluster vật lý. Nó dựa trên cơ chế xuất bản/đăng ký (publish/subscribe).
- **Ưu điểm**: Linh hoạt hơn Streaming Replication, cho phép sao chép có chọn lọc, sao chép giữa các phiên bản PostgreSQL khác nhau, và thậm chí giữa các cơ sở dữ liệu khác nhau (ví dụ: PostgreSQL sang một hệ thống khác). Hỗ trợ các kiến trúc phức tạp hơn như multi-master (với công cụ bổ sung).
- **Nhược điểm**: Thường chậm hơn Streaming Replication và có thể phức tạp hơn để quản lý trong các kịch bản HA cơ bản.

## Công cụ Quản lý Cluster (Cluster Management Tools)

Các công cụ này tự động hóa việc phát hiện lỗi, chuyển đổi dự phòng và quản lý cluster.

- **Patroni**: Một giải pháp quản lý HA phổ biến và mạnh mẽ. Nó cung cấp một "template" để triển khai các cluster PostgreSQL có tính sẵn sàng cao, sử dụng Streaming Replication và tích hợp với các hệ thống phân tán key-value store (như etcd, Consul, ZooKeeper) để quản lý trạng thái cluster và bầu chọn primary. Patroni tự động thực hiện failover và có thể tự động khôi phục các node bị lỗi.
- **repmgr**: Một công cụ khác hỗ trợ quản lý replication và failover.
- **pg_auto_failover**: Một công cụ được phát triển bởi Citus Data, tập trung vào việc tự động hóa hoàn toàn việc thiết lập và quản lý HA cho PostgreSQL.

## Shared Storage Failover (Không khuyến khích cho môi trường Cloud)

- **Cách hoạt động**: Cả primary và standby đều truy cập vào cùng một bộ lưu trữ dùng chung. Nếu primary gặp sự cố, standby sẽ tiếp quản và gắn bộ lưu trữ dùng chung.
- **Nhược điểm**: Điểm yếu duy nhất (Single Point of Failure - SPoF) là bộ lưu trữ dùng chung, và thường phức tạp để quản lý, không phù hợp với các nền tảng đám mây hiện đại.

## Connection Poolers & Load Balancers (HAProxy, PgBouncer, Pgpool-II)

- **HAProxy**: Một bộ cân bằng tải TCP/HTTP, được sử dụng để định tuyến kết nối đến primary node cho các thao tác ghi và đến các standby node (hot standby) cho các thao tác đọc, đồng thời xử lý việc chuyển đổi tự động khi có failover.
- **PgBouncer**: Một connection pooler giúp quản lý hiệu quả các kết nối đến PostgreSQL, giảm overhead khi tạo kết nối mới và cải thiện hiệu suất. Nó có thể được sử dụng để tự động chuyển hướng kết nối đến primary mới sau failover.
- **Pgpool-II**: Cung cấp cả pooling kết nối, cân bằng tải đọc/ghi, và tính năng HA cơ bản (bao gồm phát hiện lỗi và failover).

---

# II. Scalability - Khả Năng Mở Rộng trên PostgreSQL

**Scalability (Khả năng mở rộng)** là khả năng của một hệ thống, ứng dụng hoặc dịch vụ để đối phó và hoạt động tốt dưới một khối lượng công việc tăng lên (ví dụ: nhiều người dùng hơn, nhiều giao dịch hơn, nhiều dữ liệu hơn) mà vẫn duy trì hiệu suất. Scalability liên quan đến việc thêm tài nguyên để xử lý tải tăng.

## Các loại Scalability:

### Vertical Scaling (Mở rộng theo chiều dọc - Scale Up):
- **Cách hoạt động**: Tăng cường sức mạnh của một máy chủ duy nhất bằng cách bổ sung thêm tài nguyên như CPU, RAM, hoặc nâng cấp ổ cứng nhanh hơn.
- **Ưu điểm**: Đơn giản để thực hiện, không yêu cầu thay đổi kiến trúc ứng dụng nhiều.
- **Nhược điểm**: Có giới hạn vật lý về khả năng nâng cấp một máy chủ. Chi phí thường tăng theo cấp số nhân khi vượt qua một ngưỡng nhất định. Vẫn là một điểm yếu duy nhất (SPoF).

### Horizontal Scaling (Mở rộng theo chiều ngang - Scale Out):
- **Cách hoạt động**: Thêm nhiều máy chủ vào mạng lưới để phân phối tải công việc giữa chúng, thay vì chỉ nâng cấp một máy chủ.
- **Ưu điểm**: Khả năng mở rộng gần như vô hạn (trong lý thuyết), tăng cường khả năng chịu lỗi (resilience) vì tải được phân tán.
- **Nhược điểm**: Phức tạp hơn trong việc triển khai và quản lý, đòi hỏi thay đổi kiến trúc ứng dụng.

## Các kỹ thuật mở rộng quy mô phổ biến trong PostgreSQL:

### Read Replicas (Bản sao đọc - Horizontal Scaling cho Reads):
- **Cách hoạt động**: Sử dụng Streaming Replication (Hot Standby) để tạo ra các bản sao (standby) của cơ sở dữ liệu chính.
- **Ưu điểm**: Dễ triển khai, hiệu quả cao cho các ứng dụng có nhiều hoạt động đọc.
- **Nhược điểm**: Các thao tác ghi vẫn phải đi qua primary server.

### Connection Pooling (PgBouncer, Pgpool-II):
- **Cách hoạt động**: Trung gian giữa ứng dụng và PostgreSQL, duy trì nhóm kết nối và tái sử dụng.
- **Ưu điểm**: Giảm overhead thiết lập kết nối, tăng throughput và giảm latency.

### Partitioning (Phân vùng dữ liệu):
- **Cách hoạt động**: Chia bảng lớn thành nhiều bảng con nhỏ hơn (partition).
- **Ưu điểm**: Cải thiện hiệu suất truy vấn, dễ quản lý dữ liệu lớn.
- **Nhược điểm**: Cần thiết kế cẩn thận, dễ bị phức tạp nếu không đúng.

### Sharding (Phân mảnh dữ liệu - Horizontal Scaling cho Reads & Writes):
- **Cách hoạt động**: Chia cơ sở dữ liệu thành các shard và phân phối lên nhiều máy chủ PostgreSQL riêng biệt.
- **Ưu điểm**: Mở rộng gần như tuyến tính, khả năng chịu lỗi cao.
- **Nhược điểm**: Rất phức tạp, yêu cầu thay đổi lớn ở ứng dụng, cần công cụ hỗ trợ như CitusData, Greenplum.

### Load Balancing (HAProxy, Pgpool-II):
- **Cách hoạt động**: Phân phối yêu cầu đến các máy chủ khác nhau (read replica, shard).
- **Ưu điểm**: Ngăn quá tải, cải thiện hiệu suất và độ tin cậy.

### Tối ưu hóa Truy vấn và Indexing:
- **Cách hoạt động**: Viết truy vấn hiệu quả, dùng index cho cột tìm kiếm/sắp xếp thường xuyên.
- **Ưu điểm**: Tăng hiệu suất mà không cần thêm phần cứng.

## Mối quan hệ giữa HA và Scalability:
- **HA sử dụng kỹ thuật của Scalability**: Ví dụ, Streaming Replication cung cấp cả HA và Read Scalability.
- **Scalability không tự động mang lại HA**: Một hệ thống sharded không có HA vẫn có thể bị gián đoạn.
- **Thiết kế đồng thời**: Trong hệ thống lớn, mỗi shard có thể là một cluster HA riêng biệt.
