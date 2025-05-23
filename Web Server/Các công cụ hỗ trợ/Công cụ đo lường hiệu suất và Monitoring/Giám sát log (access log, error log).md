

# 1. Access Log (Nhật ký truy cập)

Access log ghi lại mọi yêu cầu (request) mà máy chủ web nhận được và xử lý. Nó là nguồn dữ liệu quý giá để phân tích lưu lượng truy cập và phát hiện các hành vi bất thường.

## Nội dung thường thấy trong Access Log

Mỗi dòng trong access log thường đại diện cho một yêu cầu và chứa các thông tin như:

- **Địa chỉ IP nguồn**: Địa chỉ IP của client gửi yêu cầu.
- **Thời gian**: Dấu thời gian của yêu cầu.
- **Phương thức HTTP**: GET, POST, PUT, DELETE, v.v.
- **URL yêu cầu**: Đường dẫn tài nguyên được yêu cầu.
- **Giao thức HTTP**: Ví dụ: HTTP/1.1.
- **Mã trạng thái HTTP**: Ví dụ: 200 (OK), 404 (Not Found), 500 (Internal Server Error).
- **Kích thước phản hồi**: Số byte dữ liệu được gửi về client.
- **User-Agent**: Thông tin về trình duyệt hoặc ứng dụng client gửi yêu cầu.
- **Referer**: URL mà client đến từ (nếu có).

## Vị trí Access Log phổ biến

### Apache HTTP Server
- Trên Debian/Ubuntu: `/var/log/apache2/access.log`
- Trên CentOS/RHEL/Fedora: `/var/log/httpd/access_log`

### Nginx
- Trên Debian/Ubuntu/CentOS/RHEL/Fedora: `/var/log/nginx/access.log` (thường nằm ở đây, có thể khác nếu được cấu hình lại)

## Cách giám sát Access Log cơ bản và những gì cần tìm

### Xem lưu lượng truy cập theo thời gian thực

```bash
sudo tail -f /var/log/apache2/access.log
# hoặc
sudo tail -f /var/log/nginx/access.log
```

### Tìm kiếm các yêu cầu cụ thể (ví dụ: quét lỗ hổng, truy cập tài nguyên nhạy cảm)

```bash=
sudo grep "POST /admin" /var/log/apache2/access.log
# Tìm kiếm các yêu cầu POST đến /admin (có thể là thử đăng nhập trái phép)

sudo grep "wp-login.php" /var/log/nginx/access.log
# Tìm kiếm các truy cập vào trang đăng nhập WordPress (thường bị bot tấn công)

sudo grep ".env" /var/log/apache2/access.log
# Tìm kiếm các yêu cầu truy cập file .env (chứa thông tin cấu hình nhạy cảm)
```

### Xác định các địa chỉ IP truy cập nhiều nhất (có thể là bot, DDoS):

```bash=
sudo awk '{print $1}' /var/log/apache2/access.log | sort | uniq -c | sort -nr | head -10
# Hoặc với Nginx:
sudo awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -nr | head -10
```
> Lệnh này lọc ra cột IP nguồn ($1), sắp xếp, đếm số lần xuất hiện duy nhất, sắp xếp lại theo số lượng giảm dần, và hiển thị 10 IP hàng đầu


### Kiểm tra các mã trạng thái HTTP phổ biến (đặc biệt là lỗi):
```bash=
sudo awk '{print $9}' /var/log/apache2/access.log | sort | uniq -c | sort -nr | head -5
# Hoặc với Nginx:
sudo awk '{print $9}' /var/log/nginx/access.log | sort | uniq -c | sort -nr | head -5
```
> Lọc ra cột mã trạng thái HTTP ($9), sau đó đếm và hiển thị các mã phổ biến nhất.

---

# 2. Error Log (Nhật ký lỗi)

Error log ghi lại tất cả các lỗi, cảnh báo (*warnings*), thông báo (*notices*) và các sự kiện nghiêm trọng khác mà máy chủ web gặp phải trong quá trình hoạt động.

## Nội dung thường thấy trong Error Log

- **Thời gian**: Dấu thời gian của lỗi.
- **Mức độ lỗi**: `error`, `warning`, `notice`, `crit`, `alert`, `emerg`, v.v.
- **Mô tả lỗi**: Chi tiết về vấn đề xảy ra.
- **Nguồn lỗi**: Thường là file, dòng code, hoặc module gây ra lỗi.

## Vị trí Error Log phổ biến

### Apache HTTP Server
- Trên Debian/Ubuntu: `/var/log/apache2/error.log`
- Trên CentOS/RHEL/Fedora: `/var/log/httpd/error_log`

### Nginx
- Trên Debian/Ubuntu/CentOS/RHEL/Fedora: `/var/log/nginx/error.log` (tùy thuộc vào cấu hình, có thể nằm cùng thư mục với access log)

## Cách giám sát Error Log cơ bản và những gì cần tìm

### Xem lỗi theo thời gian thực

```bash
sudo tail -f /var/log/apache2/error.log
# hoặc
sudo tail -f /var/log/nginx/error.log
```
### Tìm các loại lỗi cụ thể:
```bash!
sudo grep -i "error" /var/log/apache2/error.log
# Tìm tất cả các dòng chứa từ "error" (không phân biệt chữ hoa/thường)

sudo grep -i "permission denied" /var/log/nginx/error.log
# Tìm các lỗi liên quan đến quyền truy cập file/thư mục

sudo grep -i "failed to connect" /var/log/apache2/error.log
# Tìm lỗi kết nối đến database hoặc dịch vụ backend khác
```
