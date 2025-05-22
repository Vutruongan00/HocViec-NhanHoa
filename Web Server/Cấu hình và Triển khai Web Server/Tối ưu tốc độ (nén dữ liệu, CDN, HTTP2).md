# 1. Nén dữ liệu (Data Compression)
- Nén dữ liệu là quá trình giảm kích thước của các tập tin (HTML, CSS, JavaScript, hình ảnh, v.v.) trước khi chúng được gửi từ máy chủ web đến trình duyệt của người dùng. Kích thước tập tin nhỏ hơn có nghĩa là cần ít băng thông hơn để truyền tải và quá trình tải xuống diễn ra nhanh hơn
## a. Phân loại
Có hai loại chính của Data Compression:
- **Lossless Compression (Nén không mất dữ liệu)**: Nén không mất dữ liệu có thể khôi phục lại dữ liệu gốc hoàn toàn sau khi giải nén. Loại nén này thường được sử dụng cho các loại dữ liệu mà độ chính xác là rất quan trọng, chẳng hạn như tài liệu văn bản, mã nguồn và hình ảnh y tế.
- **Lossy Compression (Nén mất dữ liệu)**: Nén mất dữ liệu loại bỏ một số dữ liệu khỏi tệp tin để giảm kích thước tệp hơn nữa. Điều này có thể dẫn đến mất một số chất lượng dữ liệu, nhưng thường không đáng kể đối với mắt người. Loại nén này thường được sử dụng cho các loại dữ liệu mà chất lượng hình ảnh hoặc âm thanh không quan trọng bằng kích thước tệp, chẳng hạn như hình ảnh, video và âm nhạc.

## b. Cách hoạt động:
Các máy chủ web (như Nginx, Apache) thường sử dụng thuật toán nén như Gzip hoặc Brotli. Khi một trình duyệt yêu cầu một tập tin, máy chủ sẽ kiểm tra xem trình duyệt có hỗ trợ nén hay không. Nếu có, máy chủ sẽ nén tập tin trước khi gửi đi. Trình duyệt sau đó sẽ giải nén tập tin và hiển thị nội dung.
## c. Lợi ích
- Tiết kiệm dung lượng lưu trữ: Nén dữ liệu có thể giúp tiết kiệm dung lượng lưu trữ bằng cách giảm kích thước của các tập tin dữ liệu. Điều này có thể hữu ích cho việc lưu trữ dữ liệu trên ổ cứng, ổ đĩa flash và các thiết bị lưu trữ khác.
- Giảm băng thông: Nén dữ liệu có thể giúp giảm băng thông cần thiết để truyền dữ liệu. Điều này có thể hữu ích cho việc truyền dữ liệu qua internet và các mạng khác.
- Tăng tốc độ tải xuống: Nén dữ liệu có thể giúp tăng tốc độ tải xuống dữ liệu từ internet và các nguồn khác.
- Giảm thời gian sao lưu: Nén dữ liệu có thể giúp giảm thời gian cần thiết để sao lưu dữ liệu
## d. Các thuật toán Data Compression phổ biến:
Có nhiều thuật toán Data Compression khác nhau, mỗi thuật toán có ưu và nhược điểm riêng. Một số thuật toán phổ biến bao gồm:
- GZIP: GZIP là một thuật toán nén lossless phổ biến được sử dụng cho các tập tin văn bản và các loại dữ liệu khác.
- ZIP: ZIP là một định dạng tệp nén phổ biến có thể chứa một hoặc nhiều tệp được nén bằng các thuật toán khác nhau, chẳng hạn như GZIP.
- LZW: LZW là một thuật toán nén lossless phổ biến được sử dụng cho các hình ảnh và các loại dữ liệu khác.
- JPEG: JPEG là một thuật toán nén lossy phổ biến được sử dụng cho các hình ảnh.
- MP3: MP3 là một thuật toán nén lossy phổ biến được sử dụng cho âm nhạc.
## Ví dụ:
- Cấu hình nén Gzip trên Nginx, chỉnh sửa file cấu hình enable gzip `/etc/nginx/nginx.conf`
```cat=
http {
    # Bật nén Gzip
    gzip on;
    
    # Nén các phản hồi có kích thước từ 1024 byte (1KB) trở lên.
    gzip_min_length 1024;
    # Chỉ định các loại tệp mà Nginx sẽ nén (thường là tệp văn bản như HTML, CSS, JavaScript). Lưu ý: /text/css có thể là lỗi đánh máy, thường là text/css.
    gzip_types text/plain
               text/css
               text/xml
               text/javascript
               application/javascript
               application/x-javascript
               application/xml
               application/xml+rss
               application/json
               application/vnd.ms-fontobject
               application/x-font-ttf
               font/opentype
               image/svg+xml
               image/x-icon;
    # Mức độ nén
    gzip_comp_level 6;
```

# 2. CDN (Content Delivery Network)
- **CDN** là mạng lưới máy chủ lưu trữ bản sao của các nội dung tĩnh bên trong website và phân phối đến các máy chủ PoP (Points of Presence).
- Thông qua CDN, bản sao nội dung trên server gần nhất sẽ được trả về cho người dùng khi họ truy cập website.
## a. Cách hoạt động:
- Khi một người dùng truy cập trang web của bạn, yêu cầu của họ sẽ được chuyển hướng đến máy chủ CDN gần nhất (gọi là "điểm hiện diện" hay PoP - Point of Presence) thay vì máy chủ gốc của bạn. Nếu nội dung đã được lưu trữ (cached) trên PoP đó, nó sẽ được gửi trực tiếp đến người dùng. Nếu chưa, PoP sẽ lấy nội dung từ máy chủ gốc, lưu trữ lại và sau đó gửi cho người dùng. 

## b. Thành phần
CDN có 3 phần:
![image](https://github.com/user-attachments/assets/1aea8679-f20a-42d4-9baa-adf50f4c75a4)

- **Point of Presence (PoP)**
    - Đây là một điểm địa lý riêng lẻ chứa nhiều caching server, chẳng hạn như Singapore hay HongKong trong ví dụ ở trên.
    - Nhiều PoP này kết hợp lại sẽ tạo thành CDN, nếu đủ lớn thì sẽ trở thành CDN toàn cầu.
- **Caching server:**
    - Có thể hiểu đây là máy chủ nhưng không xử lý logic như origin server, chủ yếu nó cache file tĩnh và trả về.
    - Ví dụ PoP ở Singapore có 1000 caching server nhưng PoP ở Ấn Độ chỉ có 800 caching server, số lượng này phụ thuộc nhà cung cấp.
- SSD/HDD + RAM:
    - Trong mỗi caching server thì dữ liệu được lưu trữ trong ổ cứng và RAM.
## c. Ưu điểm
**- Cải thiện thời gian tải website nhờ:**
    - Giảm khoảng cách địa lý.
    - Giảm dung lượng file bằng cách nén ảnh, nén file, làm gọn file css, js ...

-> Từ đó, nâng cao trải nghiệm người dùng, giảm tỉ lệ thoát trang, kéo người dùng ở lại và gắn bó lâu dài hơn, thuận lợi cho SEO.

**- Giảm tải cho origin server:**
    - Giảm hoặc giải quyết tình trạng nghẽn cố chai giữa client và server.
    - Tăng khả năng xử lý được nhiều requets đồng thời cho origin server.
    - Tăng tính khả dụng của nội dung tĩnh: Khi origin server gặp sự cố thì hệ thống vẫn có thể cung cấp nội dung tĩnh, hơn nữa nhờ bản chất phân tán của CDN nên nó có thể xử lý được nhiều lưu lượng hơn, khả năng chịu lỗi tốt hơn origin server.
    - Tăng luôn cả tính khả dụng của toàn hệ thống nhờ load balancing (cân bằng tải), phân phối hợp lý lưu lượng tới các caching server, an toàn hơn khi hệ thống tăng nhanh, đột biến về lưu lượng. Giả sử có một caching server gặp sự cố thì vẫn còn những đồng đội khác bọc lót hỗ trợ nhau.
    - Giảm thiểu lượng băng thông tiêu thụ, có trường hợp, caching server gánh gần hết băng thông của hệ thống:
       
**- Tăng cường bảo mật**:
    - Lọc request có hành vi khác thường, phát hiện và chặn các hành động như crawler hoặc attack ...
    - Tính năng ẩn IP, giúp che giấu IP của origin server khỏi sự dòm ngó của hacker, hạn chế được tác động khi bị tấn công DDoS.

# 3. HTTP/2
- HTTP/2 là phiên bản lớn thứ hai của Giao thức truyền tải siêu văn bản (Hypertext Transfer Protocol - HTTP), dựa trên SPDY, một giao thức thử nghiệm do Google phát triển. Mục tiêu chính của HTTP/2 là giảm độ trễ, tăng tốc độ tải trang và cải thiện khả năng phản hồi của các ứng dụng web bằng cách giải quyết những hạn chế cố hữu của HTTP/1.1. Mặc dù vẫn giữ nguyên ngữ nghĩa cốt lõi của HTTP (như các phương thức GET, POST, tiêu đề, URI), HTTP/2 thay đổi cách dữ liệu được đóng gói và vận chuyển giữa trình duyệt và máy chủ.
## 3.1. Các Tính Năng Chính Của HTTP/2 và Cơ Chế Tối Ưu Tốc Độ
HTTP/2 mang đến một loạt các tính năng cải tiến nhằm khắc phục những hạn chế trên, đáng chú ý nhất là:
**a. Ghép Kênh (Multiplexing) Đa Luồng Trên Một Kết Nối Đơn**
- Đây là tính năng cốt lõi và mang tính cách mạng nhất của HTTP/2. Thay vì yêu cầu nhiều kết nối cho nhiều tài nguyên, HTTP/2 cho phép:
    - Nhiều yêu cầu và phản hồi được gửi và nhận đồng thời (bất đồng bộ) trên một kết nối TCP duy nhất. Điều này hoàn toàn loại bỏ vấn đề chặn đầu hàng ở cấp ứng dụng của HTTP/1.1.
    - Cách thức hoạt động: Các yêu cầu và phản hồi được chia thành các "frame" nhỏ, sau đó được mã hóa nhị phân và gửi qua một "luồng" (stream) trên cùng một kết nối. Máy chủ và trình duyệt có thể gửi và nhận các frame từ các luồng khác nhau mà không cần chờ luồng trước hoàn thành.
    - Lợi ích tối ưu tốc độ: Giảm đáng kể chi phí thiết lập kết nối TCP/TLS, giảm độ trễ tổng thể, và cải thiện hiệu suất sử dụng băng thông.
    
**b. Ưu Tiên Luồng (Stream Prioritization)**
Với ghép kênh, nhiều luồng có thể được truyền cùng lúc. HTTP/2 cho phép trình duyệt chỉ định mức độ ưu tiên cho từng luồng.
- Cách thức hoạt động: Trình duyệt có thể thông báo cho máy chủ biết tài nguyên nào quan trọng hơn và nên được gửi trước (ví dụ: file CSS quan trọng hơn hình ảnh ẩn).
- Lợi ích tối ưu tốc độ: Đảm bảo các tài nguyên thiết yếu cho việc hiển thị trang (như CSS, JavaScript chính) được tải nhanh hơn, giúp người dùng thấy nội dung trang web nhanh chóng hơn (First Contentful Paint, Largest Contentful Paint).

**c.  Nén Tiêu Đề (Header Compression - HPACK)**
HTTP/1.1 gửi các tiêu đề (headers) dưới dạng văn bản thuần túy và không nén, dẫn đến lặp lại thông tin không cần thiết. HTTP/2 sử dụng thuật toán HPACK để:
- Nén các tiêu đề: Loại bỏ các trường tiêu đề trùng lặp và sử dụng mã hóa Huffman để nén các giá trị còn lại.
- Lưu trữ bảng chỉ mục: Cả client và server duy trì một bảng chỉ mục của các tiêu đề đã được gửi trước đó, cho phép họ chỉ gửi các thay đổi hoặc tham chiếu đến các mục đã có.
- Lợi ích tối ưu tốc độ: Giảm đáng kể kích thước của các yêu cầu và phản hồi, tiết kiệm băng thông, đặc biệt hữu ích cho các ứng dụng có nhiều yêu cầu nhỏ.

**d. Server Push**
**e. Nhị Phân Hóa Giao Thức (Binary Framing Layer)**
