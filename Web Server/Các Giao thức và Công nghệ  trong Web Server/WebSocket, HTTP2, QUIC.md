# WebSocket
## 1. Khái niệm
WebSocket là một giao thức mạng cung cấp kết nối hai chiều (full-duplex) giữa client và server trên một kết nối TCP duy nhất.
Nó được thiết kế để hỗ trợ các ứng dụng web cần cập nhật theo thời gian thực như:

- Chat

- Game online

- Cập nhật chứng khoán

- Ứng dụng theo dõi vị trí/GPS

- Cảnh báo thời gian thực (real-time alert system)

## 2. Kiến trúc WebSocket
Một khi kết nối WebSocket được thiết lập, client và server có thể gửi dữ liệu bất kỳ lúc nào, không cần phải chờ yêu cầu từ phía client (như HTTP).

![image](https://github.com/user-attachments/assets/b11b2aab-55ce-41f9-a85e-9a8e23b0a74e)

 ## 3. Cách hoạt động
- **Giai đoạn 1**: Máy khách khởi tạo kết nối HTTP.

- **Giai đoạn 2**: WebSocket thiết lập quá trình handshake giữa máy khách và server để giúp WebSocket tương thích với các cổng HTTP (80 và 443) và proxy.

- **Giai đoạn 3**: Cả máy khách và server đều trao đổi tin nhắn một cách tự do thông qua kết nối mở, liên tục, không yêu cầu polling hoặc yêu cầu HTTP mới.

- **Giai đoạn 4**: Kết nối mở cho đến khi một bên đóng channel, lúc đó kết nối sẽ đóng và sự tương tác giữa máy khách và server kết thúc.

## 4.  Ưu điểm và nhược điểm
**Ưu điểm:**
- Thời gian thực: Phản hồi tức thì, không cần polling.

- Hiệu suất cao: Không cần HTTP header lặp lại → tiết kiệm băng thông.

- Tương tác liên tục: Không cần thiết lập lại kết nối nhiều lần.

**Nhược điểm:**
- Không phù hợp cho nội dung tĩnh: như hình ảnh, HTML, CSS...

- Phức tạp trong bảo mật và xử lý lỗi: phải xử lý session, reconnect thủ công.

- Tường lửa/Proxy: Một số mạng hoặc proxy có thể chặn WebSocket nếu không cấu hình đúng.

# HTTP2

**HTTP/2** là phiên bản thứ hai chính thức của giao thức HTTP, được IETF chuẩn hóa vào năm 2015, thay thế cho HTTP/1.1 
HTTP/2 giữ lại mô hình request/response quen thuộc nhưng cải thiện hiệu năng truyền tải bằng:

- **Multiplexing:** Trong HTTP/1, cụ thể là Pipelining, khi client muốn tối ưu connection bằng cách thực hiện nhiều request một lượt rồi đợi response trả về. Điểm hạn chế chúng ta đã biết response phải đúng thứ tự so với request. Việc này gây hiện tượng HOL.

    - Để khắc phục việc này, HTTP/2 sử dụng Multiplexing cho cả Request và Response. Có thể tạm hiểu rằng chúng được định danh để biết được response nào của request nào. Từ đó chúng có thể hoạt động độc lập mà không cần tuân thủ thứ tự như trước.

- **Header Compression** 
    - Trong lúc client thực hiện các request đến server sẽ có vô số các header bị dư thừa và lặp đi lặp lại. HTTP/2 sử dụng HPACK như một cách tiếp cập đơn giản và an toàn để compress (nén) các header này.

    - Ở cả client và server đều phải lưu trữ các head đã được sử dụng trước đó, từ head đã được nén, chúng sẽ tra cứu thông tin và tiến hành khôi phục lại header đầy đủ. Việc này mang lại hiệu năng cao và tối ưu băng thông đáng kể.

- **Binary Protocol**: (giao thức nhị phân) sẽ hiệu quả hơn để phân tích (parse), gọn nhẹ hơn để giao tiếp và quan trọng nhất là chúng sẽ ít bị lỗi hơn so với dạng text (văn bản). Đơn giản là bởi vì dữ liệu nhị phân không cần xử lý những trường hợp như khoảng trắng, in hoa, xuống dòng, dòng trống, emoji,…

- **Server Push**
HTTP/2 thường chạy qua HTTPS (TLS), nhưng cũng có thể dùng qua TCP thuần (không khuyến khích).

## Ưu điểm và nhược điểm của HTTP/2
- **Ưu điểm**:
    - Nhanh hơn nhiều so với HTTP/1.1.

    - Giảm số lượng kết nối TCP → nhẹ cho server.

    - Giảm băng thông nhờ nén headers.

    - Hỗ trợ server chủ động gửi tài nguyên.

- Nhược điểm:
    - Vẫn chạy trên TCP → còn Head-of-Line Blocking ở tầng TCP.

    - Yêu cầu mã hóa TLS để tương thích với trình duyệt.

    - Server và client phải hỗ trợ giao thức.

# QUIC
- **QUIC (Quick UDP Internet Connections)** là một giao thức truyền tải mới do Google phát triển, được thiết kế để thay thế TCP + TLS + HTTP/2 trong việc truyền dữ liệu web nhanh hơn và an toàn hơn.

    + QUIC hoạt động trên nền UDP, không phải TCP.

    + QUIC tích hợp cả mã hóa TLS 1.3 ngay từ đầu.

    + Được sử dụng bởi HTTP/3 – giao thức HTTP thế hệ mới.

**QUIC là xương sống của HTTP/3.**

