> Trước khi tìm hiểu về Các phương pháp xử lý tải cao trên Server(Server Overload) cần hiểu rõ **Quá tải Server là gì?**

## Quá tải Server là gì?
**Quá tải Server (Server Overload)** là **sự cố do Server bị hết tài nguyên, nên Server không thể xử lý các yêu cầu từ máy khách hoặc người dùng**. Các tác nhân như tấn công độc hại từ hacker hoặc lưu lượng truy cập tăng đột biến bất thường có thể gây ra tình trạng Server quá tải. 

## Dấu hiệu của server bị quá tải
- Website chậm bất thường, phản hồi HTTP 5xx (502, 503, 504)

- Dịch vụ như MySQL, Apache, Nginx bị “đơ”
- CPU luôn 100%
- RAM cạn kiệt, hệ thống dùng swap nhiều
- Load average cao bất thường (uptime, top)
- Connection timeout khi truy cập website hoặc SSH

## Nguyên nhân quá tải Server

| Nguyên nhân                 | Mô tả                                                                   |
| -------------------------------- | -------------------------------------------------------------------------- |
| **Lượng truy cập lớn**           | Spike traffic, bùng nổ người dùng (ví dụ: flash sale, livestream, DDoS...) |
| **Cấu hình không tối ưu**        | Web server (Nginx, Apache), database chưa được tinh chỉnh phù hợp          |
| **Mã nguồn web kém**             | Truy vấn DB chậm, vòng lặp vô tận, memory leak, không có cache             |
| **Không dùng caching**           | Không dùng cache HTML, static files, query DB quá thường xuyên             |
| **Tấn công DDoS**                | Hacker gửi hàng ngàn request giả → khiến server nghẽn                      |
| **Tài nguyên yếu**               | Server chỉ có 1 vCPU, RAM 1GB chạy nhiều dịch vụ nặng                      |
| **Chạy nhiều ứng dụng cùng lúc** | Apache + MySQL + Cron + Backup cùng lúc gây nghẽn                          |
| **Lỗi cấu hình firewall/nginx**  | Reverse proxy sai, loop request, port mở lung tung                         |
| **Thiếu giám sát / tự bảo vệ**   | Không giới hạn kết nối, không dùng WAF hay rate limit                      |

# Cách khắc phục khi quá tải Server
| Biện pháp                       | Mô tả                                                    |
| ------------------------------- | -------------------------------------------------------- |
| **Caching (proxy, static, DB)** | Dùng Nginx proxy cache, Memcached, Redis, FastCGI cache… |
| **Giới hạn kết nối**            | `limit_conn`, `rate_limit` trong Nginx hoặc Firewall     |
| **Reverse Proxy**               | Tách request web khỏi backend xử lý                      |
| **Load Balancer**               | Chia tải sang nhiều server (HAProxy, Nginx, LVS...)      |
| **Tối ưu mã nguồn và DB**       | Index, cache query, giảm vòng lặp nặng                   |
| **Giám sát tài nguyên**         | Dùng `htop`, `netdata`, `grafana`, `zabbix`, `monit`...  |
| **Tường lửa/WAF**               | Dùng UFW, CSF, iptables, ModSecurity...                  |
| **Tăng tài nguyên (scalability)**             | Nâng cấp RAM, CPU, SSD hoặc dùng **Cloud Auto Scaling**      |

## Scaling theo Chiều Dọc (Vertical Scaling - Scale Up)
Scaling theo chiều dọc, hay còn gọi là "**Scale Up**", là chiến lược tăng cường tài nguyên của một máy chủ hoặc một thành phần hệ thống hiện có. Điều này bao gồm việc nâng cấp phần cứng như tăng số lượng CPU, dung lượng RAM, tốc độ ổ đĩa (ví dụ: chuyển từ HDD sang SSD/NVMe) hoặc cải thiện băng thông mạng của máy chủ.

**Ưu điểm**
- Đơn giản trong triển khai: Thường dễ dàng thực hiện hơn so với scaling ngang, chỉ cần nâng cấp phần cứng trên một máy chủ duy nhất.
- Quản lý dễ dàng: Chỉ cần quản lý một máy chủ hoặc một phiên bản duy nhất, giảm độ phức tạp trong quản lý hệ thống và đồng bộ dữ liệu.
- Phù hợp cho các ứng dụng không dễ phân tán: Một số ứng dụng có kiến trúc monolith hoặc yêu cầu trạng thái (stateful) khó để phân tán trên nhiều máy chủ sẽ hưởng lợi từ việc scale up.
**Nhược điểm** 
- Giới hạn vật lý: Khả năng scale up bị giới hạn bởi phần cứng sẵn có. Không thể tăng CPU hoặc RAM vô hạn trên một máy chủ vật lý.
- Điểm lỗi duy nhất (Single Point of Failure - SPOF): Nếu máy chủ duy nhất này gặp sự cố, toàn bộ dịch vụ sẽ ngừng hoạt động, ảnh hưởng nghiêm trọng đến khả năng sẵn sàng.
- **downtime khi nâng cấp:** Việc nâng cấp phần cứng thường đòi hỏi downtime cho máy chủ, gây gián đoạn dịch vụ.
- Chi phí tăng theo cấp số nhân: Giá thành của việc nâng cấp phần cứng thường tăng không tuyến tính, nghĩa là để tăng hiệu suất gấp đôi có thể tốn chi phí nhiều hơn gấp đôi.
- Không giải quyết được vấn đề cơ bản: Scale up không thay đổi kiến trúc của ứng dụng để xử lý tải cao một cách thực sự phân tán, mà chỉ hoãn lại vấn đề.

## Scaling theo Chiều Ngang (Horizontal Scaling - Scale Out)
Scaling theo chiều ngang, hay còn gọi là "Scale Out", là chiến lược thêm nhiều máy chủ hoặc các phiên bản thành phần hệ thống giống hệt nhau vào một nhóm, phân phối tải trọng giữa chúng. Đối với Web Server, điều này có nghĩa là chạy nhiều instance của Web Server trên các máy chủ khác nhau và sử dụng một bộ cân bằng tải (Load Balancer) để phân phối các yêu cầu đến chúng.
**Ưu điểm**
- Khả năng mở rộng gần như không giới hạn: Có thể thêm bao nhiêu máy chủ tùy ý để đáp ứng nhu cầu, chỉ bị giới hạn bởi ngân sách và khả năng quản lý.
- Khả năng chịu lỗi cao (High Availability/Fault Tolerance): Nếu một máy chủ bị lỗi, bộ cân bằng tải sẽ tự động chuyển hướng lưu lượng đến các máy chủ còn lại, đảm bảo dịch vụ vẫn hoạt động.
- Không downtime khi mở rộng: Có thể thêm hoặc bớt máy chủ mà không làm gián đoạn dịch vụ hiện có.
- Hiệu quả chi phí: Thường rẻ hơn khi mở rộng ở quy mô lớn so với việc mua một máy chủ cực kỳ mạnh.
- Phù hợp với kiến trúc phân tán: Thúc đẩy việc thiết kế các ứng dụng không trạng thái (stateless) và kiến trúc microservices.

**Nhược điểm**
