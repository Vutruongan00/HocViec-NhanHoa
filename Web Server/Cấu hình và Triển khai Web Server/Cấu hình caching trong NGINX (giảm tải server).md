# 1. Tổng quan về caching
**Caching** là quá trình lưu trữ tạm thời các bản sao dữ liệu nhằm giảm thiểu truy xuất lặp lại đến tài nguyên gốc. Trong môi trường server, caching giúp:

- Tăng tốc độ phản hồi cho người dùng

- Giảm tải cho máy chủ ứng dụng, máy chủ cơ sở dữ liệu.

- Tiết kiệm băng thông mạng.
# 2. Các loại caching phổ biến
| Loại caching                  | Mô tả                                                                                                                 | Ví dụ                               |
| ----------------------------- | --------------------------------------------------------------------------------------------------------------------- | ----------------------------------- |
| **Client-side caching**       | Cache được lưu trữ tại trình duyệt người dùng, điều khiển qua các HTTP Header như `Cache-Control`, `Expires`, `ETag`. | Cache ảnh, CSS, JS trên Chrome      |
| **Application-level caching** | Cache tại tầng ứng dụng (PHP, Python, Node.js...), sử dụng bộ nhớ đệm như Redis, Memcached.                           | Cache truy vấn DB hoặc API response |
| **Web server caching**        | Web server (Apache, Nginx) lưu trữ file tĩnh hoặc nội dung động để phục vụ lại khi có yêu cầu giống nhau.             | Nginx FastCGI cache                 |
| **Reverse Proxy caching**     | Reverse Proxy (Nginx, Varnish) đứng trước server backend, cache nội dung để giảm yêu cầu tới server.                  | Varnish cache cho ứng dụng PHP      |
| **Database caching**          | Tầng ứng dụng hoặc middleware lưu trữ kết quả truy vấn DB để không cần truy vấn lại quá thường xuyên.                 | Redis cache cho kết quả query       |

# 3. Mục tiêu của caching
- Giảm tải server backend: Bằng cách giảm số lượng request cần xử lý.

- Cải thiện hiệu suất và thời gian phản hồi.

- Tiết kiệm tài nguyên: RAM, CPU, băng thông

# 4. Các phương pháp caching phổ biến
**a. Static file caching (file tĩnh: HTML, CSS, JS, hình ảnh)**
- Thường do trình duyệt hoặc web server cache.

- Thiết lập thông qua header:

`Cache-Control: public, max-age=31536000`

**b. Page caching / Full page caching**
- Cache toàn bộ trang HTML đầu ra của ứng dụng.

- Phù hợp với trang ít thay đổi (trang blog, tin tức, sản phẩm).

**c. Fragment caching**
- Cache một phần nội dung (ví dụ: sidebar, danh sách sản phẩm mới).

- Ứng dụng trong các CMS hoặc framework (Laravel, Django, etc).

**d. Opcode caching**
- Dành cho PHP: opcode caching giúp lưu trữ bytecode đã biên dịch, tránh dịch lại mỗi lần request.

- Công cụ: OPcache.

**e. Object caching**
- Lưu trữ các đối tượng dữ liệu (dạng key-value) trong RAM.

- Phổ biến: Redis, Memcached.

# 5.Cấu hình
> **Mục đích:**
> - Dùng Nginx caching để lưu tạm kết quả từ Apache.
> - Nếu nội dung không đổi, Nginx sẽ trả về từ cache mà không cần gọi Apache lại.
  
- Tạo thư mục lưu cache:(nếu chưa có)
```
sudo mkdir -p /var/cache/nginx
sudo chown -R nginx:nginx /var/cache/nginx
```
- Thêm cấu hình `proxy_cache` vào file nginx:
`sudo nano /etc/nginx/conf.d/truongan12345.com.conf
`
- Thêm vào phía trên cùng file (trước `server` block):
```
proxy_cache_path /var/cache/nginx levels=1:2 keys_zone=my_cache:10m max_size=100m inactive=60m use_temp_path=off;
```
- Trong khối `location /`, thêm nội dung như sau
```
    proxy_cache my_cache;
    proxy_cache_valid 200 302 10m;
    proxy_cache_valid 404 1m;
    proxy_cache_bypass $http_cache_control;
    add_header X-Cache-Status  $upstream_cache_status;
```

Cụ thể như sau:
![image](https://github.com/user-attachments/assets/d0fc9357-7b8a-4228-9dbc-6c5344cc94b2)

> trong đó:
    - `proxy_cache my_cache`: sử dụng vùng cache đã khai báo
    - `proxy_cache_valid`: thời gian cache theo mã HTTP
    - `proxy_cache_use_stale`: dùng bản cũ nếu backend lỗi
    - `add_header X-Cache-Status`: giúp ta có thể kiểm tra xem request có được lấy từ cache không (`MISS`, `HIT`,)

- Kiểm tra và reload lại Nginx:
```
sudo nginx -t && sudo systemctl reload nginx
```

- **Kiểm tra hoạt động của cache**:

Dùng trình duyệt hoặc curl từ local:
```
curl -I http://truongan12345.com/proxy.php
```
>chú ý: nếu dùng `curl` từ local thì phải sửa file `/etc/hosts` để gán tạm domain vào file `/etc/hosts` do ta chỉ đang test local và chưa đăng ký tên miền thực sự:
>`sudo nano /etc/hosts`
>`127.0.0.1    truongan12345.com www.truongan12345.com`

- Nếu kết quả trả về: `X-Cache-Status: HIT`
  
> MISS: yêu cầu được gửi tới Apache và kết quả được cache lại
>
> HIT: yêu cầu được phục vụ từ cache, không gọi lại Apache → giảm tải thành công


![image](https://github.com/user-attachments/assets/075c786b-dd51-4209-bf6a-55d63e21e3d9)



-->caching thành công

---
**Trong thực tế**, có thể kiểm tra đã caching thành công được hay chưa bằng cách:
-  Kiểm tra log của Apache:
```
sudo tail -f /var/log/httpd/access_log
```
>Trước khi cache: Mỗi lần F5 trang là 1 dòng mới.
>Sau khi cache: F5 nhiều lần → không thấy dòng mới → Nginx đã trả từ cache, không gửi request đến Apache.

- Dùng công cụ monitoring / benchmark:
    - Apache Bench (ab):
    `ab -n 1000 -c 10 http://truongan12345.com/proxy.php
` (tạo 100 kết nối đồng thời và gửi 1000 request đến url http://truongan12345.com/proxy.php)
    - htop / top để xem CPU & RAM load.
    - Grafana + Prometheus để vẽ biểu đồ (nếu triển khai thực tế lâu dài).
