# 1. Prometheus

## Là gì?

**Prometheus** là một hệ thống giám sát và cảnh báo mã nguồn mở được phát triển bởi SoundCloud. Nó chuyên về thu thập *metrics* (các chỉ số số học theo thời gian - *time-series data*).

## Chức năng chính

- **Thu thập Metrics**:  
  Prometheus sử dụng mô hình "kéo" (*pull model*), nghĩa là nó chủ động kết nối đến các ứng dụng hoặc máy chủ thông qua các **exporter** (đại lý thu thập dữ liệu) để kéo dữ liệu metrics.  
  Ví dụ:
  - `Node Exporter`: cho các chỉ số hệ thống Linux (CPU, RAM, Disk I/O, Network)
  - `cAdvisor`: cho các chỉ số container (Docker, Kubernetes)
  - `JMX Exporter`: cho ứng dụng Java

- **Lưu trữ Time-Series Data**:  
  Prometheus có một cơ sở dữ liệu *time-series* tích hợp, tối ưu cho việc lưu trữ và truy vấn dữ liệu dạng chuỗi thời gian.

- **Ngôn ngữ truy vấn mạnh mẽ (PromQL)**:  
  Cho phép bạn thực hiện các truy vấn phức tạp trên dữ liệu metrics để tính toán, tổng hợp và phân tích hiệu suất.

- **Cảnh báo (Alerting)**:  
  Tích hợp với **Alertmanager** để gửi thông báo cảnh báo qua email, Slack, PagerDuty, v.v., khi các ngưỡng metrics vượt quá giới hạn.

## Ưu điểm

- Mô hình thu thập dữ liệu đơn giản, linh hoạt.
- Ngôn ngữ truy vấn mạnh mẽ cho phép phân tích sâu về hiệu suất.
- Khả năng mở rộng tốt.
- Cộng đồng lớn và nhiều exporter sẵn có.
- Nhẹ và hiệu quả cho việc thu thập metrics.

## Tích hợp

Prometheus thường được sử dụng kết hợp với **Grafana** để trực quan hóa dữ liệu:  
- **Prometheus** là nguồn dữ liệu  
- **Grafana** là giao diện hiển thị

## Khi nào sử dụng?

Prometheus lý tưởng để giám sát hiệu suất hoạt động của các hệ thống, ứng dụng, dịch vụ:
- CPU, RAM
- Network latency
- Request rate
- Error rate
- Số lượng phiên làm việc

Nó là xương sống cho việc giám sát:
- **Black-box**: giám sát từ bên ngoài dịch vụ  
- **White-box**: giám sát từ bên trong ứng dụng



# 2. Grafana

## Là gì?

**Grafana** là một nền tảng phân tích và trực quan hóa dữ liệu mã nguồn mở. Nó **không tự thu thập hay lưu trữ dữ liệu**, mà kết nối với nhiều **nguồn dữ liệu** khác nhau để tạo ra các **bảng điều khiển (dashboards)** đẹp mắt và tương tác.

## Chức năng chính

- **Trực quan hóa dữ liệu**:  
  Biến dữ liệu thô từ nhiều nguồn thành các biểu đồ, đồ thị, bảng biểu dễ hiểu:  
  `line charts`, `bar charts`, `pie charts`, `heatmaps`, `gauge panels`, v.v.

- **Dashboarding**:  
  Tạo các bảng điều khiển tùy chỉnh, cho phép người dùng theo dõi nhiều chỉ số cùng lúc trên một màn hình.

- **Kết nối đa nguồn dữ liệu**:  
  Hỗ trợ rất nhiều **nguồn dữ liệu** như:
  - Prometheus
  - Elasticsearch
  - InfluxDB
  - MySQL
  - PostgreSQL
  - Microsoft SQL Server
  - Graphite
  - Loki
  - Splunk
  - CloudWatch
  - ...

- **Cảnh báo (Alerting)**:  
  Tính năng cảnh báo tích hợp, cho phép bạn định nghĩa các **ngưỡng** và gửi thông báo khi dữ liệu vượt quá giới hạn.

- **Khả năng chia sẻ**:  
  Dễ dàng chia sẻ các dashboard với đồng nghiệp hoặc công khai.

## Ưu điểm

- Giao diện người dùng trực quan, dễ sử dụng (kéo và thả để tạo dashboard).
- Tính linh hoạt cao trong việc hiển thị dữ liệu.
- Hỗ trợ mạnh mẽ cho việc khám phá và phân tích dữ liệu.
- Cộng đồng lớn, có sẵn nhiều template dashboard.

## Tích hợp

Grafana là một công cụ **bổ trợ tuyệt vời** cho:
- **Prometheus**
- **ELK Stack**
- và bất kỳ hệ thống lưu trữ dữ liệu nào khác mà bạn muốn trực quan hóa.

## Khi nào sử dụng?

Khi bạn muốn có một **cái nhìn tổng quan, trực quan** về:
- Hiệu suất
- Trạng thái hệ thống
- Các xu hướng dữ liệu

Từ **nhiều nguồn khác nhau**, giúp các:
- Nhà quản trị hệ thống
- Kỹ sư vận hành
- Quản lý cấp cao  
dễ dàng theo dõi tình hình hoạt động.


# 3. ELK Stack (Elasticsearch, Logstash, Kibana)

## Là gì?

**ELK Stack** là một bộ công cụ mã nguồn mở mạnh mẽ và phổ biến được thiết kế để:
- Thu thập
- Xử lý
- Tìm kiếm
- Phân tích
- Trực quan hóa **dữ liệu log** và các **dữ liệu phi cấu trúc** khác.

**ELK** là viết tắt của ba thành phần chính:
- **Elasticsearch**
- **Logstash**
- **Kibana**

> Đôi khi còn được gọi là **Elastic Stack** khi bổ sung thêm các thành phần như **Filebeat** và các **Beats** khác.

---

## Các thành phần chính

### Elasticsearch (E)
- Là một **công cụ tìm kiếm và phân tích phân tán** dựa trên Apache Lucene.
- Đóng vai trò là **cơ sở dữ liệu chính** của ELK Stack.
- **Lưu trữ và lập chỉ mục (indexed)** tất cả dữ liệu log đã được xử lý để **tìm kiếm và phân tích nhanh chóng**.

### Logstash (L)
- Là một **pipeline xử lý dữ liệu động**, có khả năng:
  - **Input (Thu thập)**: Dữ liệu log từ nhiều nguồn như `file`, `syslog`, `Kafka`, `Redis`, `database`, v.v.
  - **Filter (Xử lý)**: Phân tích cú pháp (parse), chuyển đổi (transform), làm phong phú dữ liệu (enrichment).
  - **Output (Gửi đi)**: Đưa dữ liệu đã xử lý đến đích, phổ biến nhất là **Elasticsearch**.

### Kibana (K)
- Là công cụ **trực quan hóa dữ liệu** và **khám phá dữ liệu** cho Elasticsearch.
- Cung cấp **giao diện web** cho phép:
  - Tạo biểu đồ, đồ thị, dashboards.
  - Tìm kiếm phức tạp.
  - Phân tích xu hướng dữ liệu log.

---

## Ưu điểm

- **Tập trung log**: Gom log từ hàng trăm, hàng ngàn máy chủ về một nơi duy nhất.
- **Tìm kiếm mạnh mẽ**: Full-text search rất nhanh và chính xác.
- **Phân tích dữ liệu phi cấu trúc**: Hiệu quả trong xử lý log không có định dạng cố định.
- **Trực quan hóa linh hoạt**: Kibana hỗ trợ nhiều công cụ tạo dashboard và báo cáo.
- **Khả năng mở rộng**: Có thể mở rộng để xử lý lượng dữ liệu log khổng lồ.

---

## Tích hợp

- **Filebeat**:
  - Agent nhẹ thuộc họ Beats.
  - Cài trên các máy chủ nguồn để thu thập log từ file.
  - Gửi log trực tiếp đến Logstash hoặc Elasticsearch.

- **Grafana**:
  - Có thể tích hợp để trực quan hóa dữ liệu từ Elasticsearch.

---

## Sử dụng khi:

- Khi cần **quản lý và phân tích log tập trung từ nhiều nguồn**.
- **Gỡ lỗi và khắc phục sự cố** nhanh chóng bằng cách tìm kiếm log.
- **Phát hiện sự kiện bảo mật** và xây dựng hệ thống **SIEM** cơ bản.
- **Phân tích xu hướng và hành vi người dùng** từ dữ liệu log.
- Trong môi trường **DevOps/SRE**, để giám sát hoạt động của microservices và hệ thống phân tán.

