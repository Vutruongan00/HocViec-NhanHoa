
# Khái niệm HTTP là gì?
- **HTTP** - **HyperText Transfer Protocol**, một giao thức lớp ứng dụng cho các hệ thống thông tin siêu phương tiện phân tán, cộng tác. HTTP là nền tảng của truyền thông dữ liệu cho World Wide Web, nơi các tài liệu, siêu văn bản, bao gồm cả các siêu liên kết truyền đến các tài nguyên khác để người dùng có thể truy cập dễ dàng.
- Giao tiếp giữa client và web server được thực hiện bằng cách gửi HTTP request và nhận HTTP response. Client thường là các trình duyệt (Chrome, Edge, Safari), nhưng chúng có thể là bất kỳ loại chương trình hoặc thiết bị nào.

# Khái niệm về HTTPS
- **HTTPS - Hypertext Transfer Protocol Secure** – Giao thức truyền tải siêu văn bản bảo mật, là một phần mở rộng của HTTP. Giao thức HTTPS được sử dụng để giao tiếp an toàn qua mạng máy tính và được sử dụng rộng rãi trên Internet. Trong HTTPS, giao thức truyền thông được mã hóa bằng TLS (Transport Layer Security) hay trước đây là SSL (Secure Sockets Layer). Do đó, giao thức này còn được gọi là HTTP qua TLS, hoặc HTTP qua SSL.

- Điều này đặc biệt quan trọng khi người dùng truyền dữ liệu nhạy cảm. Chẳng hạn như bằng cách đăng nhập vào tài khoản ngân hàng,dịch vụ email hoặc nhà cung cấp bảo hiểm y tế.

# Sự khác nhau giữa HTTP và HTTPS
-  Về mặt kỹ thuật, HTTPS không phải là một giao thức tách biệt với HTTP. Nó chỉ đơn giản là sử dụng mã hóa TLS/SSL qua giao thức HTTP. HTTPS xảy ra dựa trên việc truyền các chứng chỉ (certificate) TLS/SSL. Các chứng chỉ này xác minh rằng một nhà cung cấp có đúng chính xác là đối tượng bạn muốn truy cập hay không.

- Khi người dùng kết nối với một trang web. Trang web sẽ gửi SSL Certificate của nó chứa Public Key cần thiết để bắt đầu secure session. Hai máy tính, client và server, sau đó trải qua một quá trình được gọi là bắt tay (handshake) SSL/TLS, là một loạt các thỏa thuận (negotiation) qua lại được sử dụng để thiết lập kết nối an toàn.

| Tiêu chí                   | HTTP (HyperText Transfer Protocol)                                            | HTTPS (HTTP Secure / HTTP over SSL/TLS)                                                     |
| -------------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| **Bảo mật**                | Không mã hóa dữ liệu, dễ bị tấn công nghe lén (sniffing) hoặc giả mạo (MITM). | Mã hóa dữ liệu bằng SSL/TLS, đảm bảo tính **bảo mật**, **toàn vẹn**, **xác thực**.          |
| **Cổng sử dụng (Port)**    | Mặc định là cổng **80**                                                       | Mặc định là cổng **443**                                                                    |
| **Chứng chỉ số**           | Không cần                                                                     | Bắt buộc phải có **SSL/TLS Certificate** (chứng chỉ số)                                     |
| **Hiệu suất**              | Nhanh hơn một chút do không mã hóa dữ liệu                                    | Chậm hơn do quá trình mã hóa và giải mã (hiện nay rất ít chênh lệch với phần cứng hiện đại) |
| **Mức độ tin cậy**         | Thấp, không đảm bảo dữ liệu không bị thay đổi hoặc rò rỉ                      | Cao, được các trình duyệt và người dùng tin tưởng                                           |
| **Sử dụng trong thực tế**  | Trang web không quan trọng (thử nghiệm, demo nội bộ)                          | Trang web xử lý thông tin nhạy cảm như thanh toán, đăng nhập, giao dịch                     |
| **Biểu tượng trình duyệt** | Không có biểu tượng ổ khóa                                                    | Có biểu tượng ổ khóa 🔒 (đôi khi kèm "https\://" trên thanh địa chỉ)                        |
| **SEO (Google)**           | Không được ưu tiên                                                            | Google ưu tiên HTTPS, có thể ảnh hưởng đến xếp hạng SEO                                     |
| **Chi phí triển khai**     | Miễn phí                                                                      | Có thể tốn phí (chứng chỉ thương mại), nhưng có các lựa chọn miễn phí như Let's Encrypt     |

# HTTP Request
- **HTTP request** là thông tin được gửi từ client lên server, để yêu cầu server tìm hoặc xử lý một số thông tin, dữ liệu mà client muốn. HTTP request có thể là một file text dưới dạng XML hoặc Json mà cả hai đều có thể hiểu được. Dưới đây là một HTTP request sử dụng phương thức GET
## Các phương thức của HTTP Request
### Phương thức GET
- Get là phương thức được Client gửi dữ liệu lên Server thông qua đường dẫn URL nằm trên thanh địa chỉ của Browser. Server sẽ nhận đường dẫn đó và phân tích trả về kết quả cho bạn. Hơn nữa, nó là một phương thức được sử dụng phổ biến mà không cần có request body.

- Ví dụ điển hình là khi bạn mở một trang web, Client sẽ gửi một phương thức Get lên server để truy xuất nội dung hiển thị của trang web. Nó tương đương với thao tác đọc.

- Một số đặc điểm chính của phương thức Get là:

    -    Giới hạn độ dài của các giá trị là 255 kí tự.
    -    Chỉ hỗ trợ các dữ liệu kiểu String.
    -    Có thể lưu vào bộ nhớ cache.
    -    Các tham số truyền vào được lưu trữ trong lịch sử trình duyệt.
    -    Có thể được bookmark (đánh dấu rồi xem lại sau) do được lưu trong lịch sử trình duyệt.

### Phương thức POST
- Phương thức Post là phương thức gửi dữ liệu đến server giúp bạn có thể thêm mới dữ liệu hoặc cập nhật dữ liệu đã có vào database.

- Chúng ta sẽ gửi thông tin cần thêm hoặc cập nhật trong phần body request.

- Một số đặc điểm chính của Post là:

    - Dữ liệu cần thêm hoặc cập nhật không được hiển thị trong URL của trình duyệt.
    - Dữ liệu không được lưu trong lịch sử trình duyệt.
    - Không có hạn chế về độ dài của dữ liệu.
    - Hỗ trợ nhiều kiểu dữ liệu như: String, binary, integers,..
### Phương thức PUT
Cách hoạt động tương tự như Post nhưng nó chỉ được sử dụng để cập nhật dữ liệu đã có trong database. Khi sử dụng nó, bạn phải sửa toàn bộ dữ liệu của một đối tượng.
### Phương thức PATCH
Tượng tự như Post và Put, nhưng Patch được sử dụng khi phải cập nhật một phần dữ liệu của đối 
### Phương thức DELETE
Giống như tên gọi, khi sử dụng phương thức Delete sẽ xoá các dữ liệu của server về tài nguyên thông qua URI. Cũng giống như GET, phương thức này không có body request.
### Phương thức HEAD
HEAD gần giống giống với lại GET, tuy nhiên nó không có response body.

# HTTP Response
- Cấu trúc HTTP response gần giống với HTTP request, chỉ khác nhau là thay vì Request-Line, thì HTTP có response có Status-Line. Và giống như Request-Line, Status-Line cũng có ba phần như sau:
    - HTTP-version: phiên bản HTTP cao nhất mà server hỗ trợ.
    - Status-Code: mã kết quả trả về.
    - Reason-Phrase: mô tả về Status-Code.

## HTTP Status Codes

### Information responses(100–199)
- **1xx**: Thông tin. Mã này nghĩa là yêu cầu đã được nhận và tiến trình đang tiếp tục, các status code này chỉ có tính chất tạm thời, client có thể không quan tâm.
### Successful responses (200–299)
**2xx: Successful**. khi đã xử lý thành công request của client, server trả về status dạng này:
- **200 OK**: request thành công.
- **202 Accepted**: request đã được nhận, nhưng không có kết quả nào trả về, thông báo cho client tiếp tục chờ đợi.
- **203 Non-Authoritative Information** Phản hồi này có nghĩa là thông tin meta được trả về không hoàn toàn giống với thông tin có sẵn từ máy chủ .
- **204 No Content**: request đã được xử lý nhưng không có thành phần nào được trả về.
- **205 Reset**: giống như 204 nhưng mã này còn yêu câu client reset lại document view.
- **206 Partial Content**: server chỉ gửi về một phần dữ liệu, phụ thuộc vào giá trị range header của client đã gửi.
- **207 Multi-Status( WebDAV )** Truyền tải thông tin về nhiều tài nguyên, trong các tình huống có thể có nhiều trạng thái phù hợp
- **208 Already Reported( WebDAV )** Được sử dụng bên trong một dav:propstat Phần tử phản hồi để tránh liên tục liệt kê nhiều ràng buộc .
- **226 IM Used( Mã hóa HTTP Delta )** Máy chủ đã thực hiện một GET yêu cầu và phản hồi là một đại diện cho kết quả của một hoặc nhiều thao tác thể hiện được áp dụng cho thể hiện hiện tại.
### Redirects (300–399)
**3xx Redirection**: server thông báo cho client phải thực hiện thêm thao tác để hoàn tất request:
- 301 Moved Permanently: tài nguyên đã đưuọc chuyển hoàn toàn tới địa chỉ Location trong HTTP response
- 303 See other: tài nguyên đã được chuyển tạm thời tới địa chỉ Location trong HTTP response.
- 304 Not Modified: tài nguyên không thay đổi từ lần cuối client request, nên client có thể sử dụng đã lưu trong cache.
### Client errors (400–499)
**4xx: Lỗi Client**. Mã này nghĩa là yêu cầu chứa cú pháp không chính xác hoặc không được thực hiện.
- **400 Bad Request**: request không đúng dạng, cú pháp
- **401 Unauthorized**: client chưa xác thực.
- **403 Forbidden**: client không có quyền truy cập.
- **404 Not Found**: không tìm thấy tài nguyên.
- **405 Method Not Allowed**: phương thức không được server hỗ trợ.
### Server errors (500–599)
**5xx Server Error**: lỗi của server
- **500 Internal Server Error**: có lỗi trong quá trình xử lý của server.
- **501 Not Implemented**: server không hỗ trợ chức năng client yêu cầu.
- **503: Service Unavailable**: Server bị quá tải, hoặc bị lỗi xử lý.
