# APACHE
**Định nghĩa về MPM Prefork và Worker trong Apache**
- Khi nói đến việc cấu hình Apache trên máy chủ Linux, có hai **Multi-Processing Module (MPM**) chính có thể được sử dụng: **Prefork** và **Worker**. Cả hai MPM đều xác định cách Apache xử lý các yêu cầu đến và cách sử dụng tài nguyên hệ thống.

- **Prefork MPM**: Mỗi tiến trình con xử lý một kết nối tại một thời điểm.

- **Worker MPM**: Mỗi tiến trình con tạo ra nhiều luồng, mỗi luồng xử lý một kết nối

## 1. Prefork

**Prefork MPM** là một Module hoạt động bằng cách tạo ra một số tiến trình con, mỗi tiến trình có thể xử lý một yêu cầu duy nhất tại một thời điểm. Điều này cho phép Apache xử lý nhiều yêu cầu cùng lúc mà không cần phải tạo một tiến trình mới cho mỗi yêu cầu

- **Mô hình hoat động**:

    Client → Apache → Tạo tiến trình con → Xử lý → Trả     kết quả → Kết thúc hoặc chờ request khác


- **Ưu điểm**: 
    - Các process được xử lý hoàn toàn một cách độc lập không liên quan gì đến nhau, cho nên nếu một process chế thì các process còn lại vẫn sống và vẫn hoàn thành công việc của nó. --> ổn định
    - Vì không sử dụng chung vùng nhớ cho nên các process không quậy phá với nhau được.
- **Nhược điểm**:
    - Tạo ra quá nhiều các process sẽ chiếm dụng lượng RAM lớn --> tốn tài nguyên

**=> Phù hợp với: Apache + mod_php, hệ thống cũ dùng CGI.**

## 2. Worker
**Worker MPM** sử dụng phương pháp đa luồng (threaded), trong đó mỗi tiến trình con có thể xử lý nhiều yêu cầu cùng lúc bằng cách sử dụng nhiều luồng. Cách này giúp giảm lượng bộ nhớ Apache sử dụng, vì mỗi tiến trình con có thể xử lý nhiều yêu cầu với chỉ một tập hợp tài nguyên bộ nhớ duy nhất. Tuy nhiên, điều này cũng đòi hỏi Apache phải được cấu hình để sử dụng các module an toàn với đa luồng (thread-safe), và mỗi yêu cầu được xử lý trong một luồng riêng biệt – điều này có thể dẫn đến một chút chi phí hiệu năng bổ sung.

- **Ưu điểm:**
    - Sử dụng tài nguyên tốt hơn Prefork.
    - Xử lý được nhiều kết nối đồng thời.
-  **Nhược điểm**:
    -  Nếu một thread có vấn đề hoặc là bị crash thì các thread khác trong process cũng có thể bị crash và process sẽ bị crash theo.
    -  Các thread có thể sử dụng chung vùng nhớ cho nên có thể gây ảnh hưởng lẫn nhau

**-> Phù hợp với: Khi chạy backend không phải là PHP (VD: Python WSGI), hoặc hệ thống hiện đại hơn.**

### **So sánh chi tiết giữa Prefork và Worker MPM**

| Tiêu chí  | **Prefork MPM** | **Worker MPM**  |
| -------- | ------- | ------ |
| **Mô hình xử lý**      | Mỗi kết nối sử dụng một tiến trình riêng biệt                     | Mỗi tiến trình sử dụng nhiều luồng để xử lý nhiều kết nối       |
| **Sử dụng tài nguyên** | Tiêu tốn nhiều bộ nhớ do nhiều tiến trình                         | Hiệu quả hơn nhờ chia sẻ bộ nhớ giữa các luồng                  |
| **Độ ổn định**         | Cao, lỗi trong một tiến trình không ảnh hưởng đến tiến trình khác | Trung bình, lỗi trong một luồng có thể ảnh hưởng đến tiến trình |
| **Bảo mật**            | Cao, do tiến trình cách ly                                        | Trung bình, do các luồng chia sẻ không gian địa chỉ             |
| **Hiệu suất**          | Thấp hơn trong môi trường tải cao                                 | Cao hơn, phù hợp với môi trường nhiều kết nối đồng thời         |
| **Ứng dụng phù hợp**   | Ứng dụng không hỗ trợ đa luồng, yêu cầu bảo mật cao               | Ứng dụng hiện đại, cần hiệu suất và khả năng mở rộng            |

# NGINX
- NGINX là một web server được thiết kế theo mô hình Event-driven, nghĩa là nó xử lý các yêu cầu đến dựa trên sự kiện, chứ không phải tạo ra một tiến trình hoặc luồng mới cho mỗi yêu cầu như cách truyền thống (ví dụ như Apache).
- Trong Nginx, **event-driven** là một kiến trúc xử lý hoạt động dựa trên sự kiện, đóng vai trò cốt lõi giúp Nginx trở nên nhẹ, nhanh và xử lý đồng thời (concurrent) hiệu quả, đặc biệt trong môi trường có lượng lớn kết nối đồng thời (như web server hay reverse proxy).
## Event-driven
### 1. Khái niệm
- Event-driven (hướng sự kiện) là một mô hình xử lý trong đó:
    - **Hệ thống không tạo ra một luồng (thread) hoặc tiến trình (process) mới cho mỗi kết nối**.
    - Thay vào đó, nó sử dụng một vòng lặp sự kiện (event loop) để theo dõi các sự kiện I/O (như kết nối đến, nhận dữ liệu, ghi dữ liệu).
    - Khi một sự kiện xảy ra, hàm callback tương ứng sẽ được gọi để xử lý.
### 2. Cơ chế event trong NGINX:
- NGINX sử dụng **vòng lặp sự kiện (event loop)** để:

    - Lắng nghe khi có yêu cầu HTTP đến.

    - Xử lý yêu cầu nếu có dữ liệu.

    - Chuyển sang xử lý tiếp nếu có phản hồi.

- Nó tận dụng cơ chế non-blocking I/O, cụ thể là:

    - epoll (trên Linux),

    - kqueue (trên FreeBSD),

    - select/poll (trên hệ điều hành khác).
