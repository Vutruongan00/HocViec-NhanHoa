# CGI là gì
**CGI - Common Gateway Interface** là một giao diện chuẩn cho phép web server (như Apache, Nginx) chạy các chương trình ứng dụng bên ngoài (như script PHP, Python, Perl...) và gửi kết quả trả lại trình duyệt.
- Mỗi khi có request động, web server sẽ chạy một chương trình/shell script (gọi là CGI program), và truyền input qua biến môi trường hoặc stdin, kết quả sẽ gửi lại client.
## Các tính năng của CGI

| Tính năng                   | Mô tả                                                                                 |
| --------------------------- | ------------------------------------------------------------------------------------- |
| **Sinh nội dung động**      | CGI cho phép tạo các trang HTML động tùy theo dữ liệu người dùng.                     |
| **Tương thích đa ngôn ngữ** | Có thể viết CGI script bằng bất kỳ ngôn ngữ nào có thể đọc từ stdin và ghi ra stdout. |
| **Đơn giản, phổ biến**      | Hầu hết các web server lớn (Apache, Nginx) đều hỗ trợ CGI.                            |
| **Tách biệt rõ ràng**       | Web server và ứng dụng chạy riêng biệt, tăng tính modular.                            |

## Cách hoạt động của CGI

Client (trình duyệt) → Web server (Apache/Nginx) → CGI script → Output HTML → Client

![image](https://github.com/user-attachments/assets/b05d709a-1842-4c9e-9440-93dd8c977487)

Trong thực tế CGI sẽ hoạt động như sau:

- Máy trạm gửi yêu cầu đến máy chủ web, máy chủ web nhận yêu cầu và chuyển tiếp cho ứng dụng cổng. CGI sẽ thực thi một câu lệnh tương ứng phù hợp với ứng dụng đó.
- Các thông tin chi tiết về yêu cầu được ứng dụng truyền qua bằng các biến môi trường, trong khi đó dữ liệu bằng các phương pháp POST hoặc PUT sẽ được truyền qua các cổng nhập tiêu chuẩn. Tức là CGI xử lý dữ liệu của nó song song với dữ liệu chính.
- Ứng dụng sẽ viết nội dung cần trả lời để máy chủ trả thông tin về cho người yêu cầu.
  
## Ưu nhược điểm của CGI
- **Ưu điểm**:
    - Code sẵn có để áp dụng vào phần mềm của bạn mà không phải code lại từ đầu.

    - CGI mạnh mẽ tương thích hầu hết với các ngôn ngữ và bất cứ nền tảng nào, miễn chúng có những đặc điểm kỹ thuật phù hợp.

    - Các tác vụ nâng cao vốn khó trong Java giờ có thể thực hiện dễ dàng hơn với CGI.

- **Nhược điểm**:
    - CGI rất tốn thời gian để xử lý.

    - Không lưu lại cache sau mỗi lần tải lại trang.

    - Mỗi lần tải lại sẽ tốn thêm thời gian do phải tải lại các chương trình vào bộ nhớ.
 ## Ưu nhược điểm của CGI
- **Ưu điểm**:
    - Code sẵn có để áp dụng vào phần mềm của bạn mà không phải code lại từ đầu.

    - CGI mạnh mẽ tương thích hầu hết với các ngôn ngữ và bất cứ nền tảng nào, miễn chúng có những đặc điểm kỹ thuật phù hợp.

    - Các tác vụ nâng cao vốn khó trong Java giờ có thể thực hiện dễ dàng hơn với CGI.

- **Nhược điểm**:
    - CGI rất tốn thời gian để xử lý.

    - Không lưu lại cache sau mỗi lần tải lại trang.

    - Mỗi lần tải lại sẽ tốn thêm thời gian do phải tải lại các chương trình vào bộ nhớ.

# FastCGI
**FastCGI** là một **giao thức nâng cao của CGI** (Common Gateway Interface), được thiết kế để giải quyết các vấn đề hiệu suất của CGI truyền thống.

🔁 Trong khi CGI khởi tạo một tiến trình mới cho mỗi request, FastCGI giữ các tiến trình chạy nền sẵn (persistent), giúp giảm tải cho server và tăng tốc độ xử lý.

## Cách hoạt động của FastCGI

**Luồng xử lý cơ bản:**

Client → Web Server → FastCGI Process → Ứng dụng (PHP, Python, v.v.) → Response → Web Server → Client

**Mô tả chi tiết:**
- Web server (Apache/Nginx) nhận HTTP request.

- Thay vì tạo một tiến trình mới (như CGI), server gửi request tới một tiến trình FastCGI đã tồn tại (qua socket hoặc TCP).

 - Tiến trình FastCGI xử lý request, chạy ứng dụng (ví dụ PHP script).

- Kết quả được gửi lại cho Web server → trình duyệt người dùng

##  Ưu điểm - nhược diểm của FastCGI

| Ưu điểm                        | Mô tả                                                     |
| ------------------------------ | --------------------------------------------------------- |
|  **Hiệu suất cao hơn CGI**    | Giảm overhead do **không tạo tiến trình mới mỗi request** |
**Tái sử dụng tiến trình**   | Tiến trình được giữ sống lâu dài để phục vụ nhiều request |
|  **Tách biệt logic – server** | Ứng dụng backend có thể chạy riêng khỏi web server        |
|  **Triển khai linh hoạt**     | Có thể chạy ứng dụng ở server khác (qua TCP)              |
|  **Quản lý tài nguyên dễ**    | Có thể giới hạn số tiến trình, RAM, timeout...            |

| Nhược điểm                                 | Mô tả                                               |
| ------------------------------------------ | --------------------------------------------------- |
| ❌ **Cấu hình phức tạp hơn CGI**            | Cần quản lý socket, tiến trình, file cấu hình riêng |
| ❌ **Debug khó hơn**                        | Phải giám sát tiến trình FastCGI riêng biệt         |
| ❌ **Chưa hỗ trợ tốt cho các app realtime** | FastCGI chủ yếu phù hợp với request-response        |

# WSGI
- WSGI (Web Server Gateway Interface) là một chuẩn giao tiếp giữa server và ứng dụng Python, giúp web framework (Django, Flask, v.v.) hoạt động độc lập với web server (Apache, Nginx...).

- WSGI KHÔNG phải là một phần mềm — nó là một GIAO THỨC (interface).
Nó định nghĩa cách web server gọi ứng dụng Python và nhận lại response.

## Hoạt động của **WSGI**

WSGI chia hệ thống thành hai phần:

Web server (phía “WSGI server”): xử lý HTTP request, gửi thông tin request cho app.

Ứng dụng (application callable): xử lý logic, trả về response.

🔁**Luồng hoạt động:**

Client → Web Server (Nginx/Apache) → WSGI server (Gunicorn/uWSGI) → Ứng dụng Python → Response → Client

## Ưu - Nhược điểm của 

| Ưu điểm                       | Mô tả                                                              |
| ----------------------------- | ------------------------------------------------------------------ |
| ✅ **Chuẩn hóa**               | Mọi framework Python hiện đại đều tuân thủ WSGI                    |
| ✅ **Tách biệt server và app** | Bạn có thể đổi WSGI server (Gunicorn ↔ uWSGI) mà không cần sửa app |
| ✅ **Hiệu suất cao**           | WSGI server như uWSGI/Gunicorn hỗ trợ multi-thread, pre-fork       |
| ✅ **Triển khai linh hoạt**    | Dùng chung được với Nginx, Apache, Docker...                       |

| Nhược điểm                           | Mô tả                                                                  |
| ------------------------------------ | ---------------------------------------------------------------------- |
| **Không hỗ trợ async gốc**         | WSGI không hỗ trợ tốt async/await → cần ASGI cho FastAPI, WebSocket... |
|  **Phức tạp khi cấu hình thủ công** | Nếu không dùng framework hoặc tool, cần cấu hình rõ từng bước          |
