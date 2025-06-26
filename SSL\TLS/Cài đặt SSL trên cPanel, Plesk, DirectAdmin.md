
## Cài đặt SSL trên CPanel

[Hướng dẫn cài đặt tham khảo tại đây](https://wiki.nhanhoa.com/kb/huong-dan-cai-ssl-tren-hosting-cpanel/)

## Cài đặt SSL trên Plesk

[Hướng dẫn cài đặt tham khảo tại đây](https://wiki.nhanhoa.com/kb/huong-dan-cai-dat-ssl-tren-plesk-de-dang/)

## Cài đặt SSL trên DirectAdmin

[Hướng dẫn cài đặt tham khảo tại đây](https://wiki.nhanhoa.com/kb/huong-dan-cai-dat-ssl-free-tren-directadmin/)

## SSL cho tên miền chính và subdomain
Có hai phương án phổ biến để cấp chứng chỉ SSL cho cả tên miền chính và các subdomain
### 1. Chứng chỉ Wildcard SSL:
   - **Mục đích:** Bảo vệ một tên miền chính và **tất cả các tên miền phụ ở một cấp độ** của tên miền đó.
   - **Cấu trúc:** Được cấp cho `*.yourdomain.com`. Dấu hoa thị (`*`) đại diện cho bất kỳ tên miền phụ nào ở cấp độ đó.
   - **Ví dụ:** Một chứng chỉ Wildcard cho `*.example.com` sẽ bảo mật:
     - `blog.example.com`
     - `shop.example.com`
     - `mail.example.com`
   - **Không bảo mật:**
     - `example.com` (tên miền gốc - thường cần được thêm riêng vào SAN của chứng chỉ Wildcard khi yêu cầu).
     - `sub.blog.example.com` (tên miền phụ ở cấp độ sâu hơn).
   - **Lợi ích:** Tiết kiệm chi phí và đơn giản hóa việc quản lý khi bạn có nhiều tên miền phụ. Bạn chỉ cần một chứng chỉ duy nhất cho tất cả chúng.
### 2. Chứng chỉ Multi-Domain SSL (SAN Certificate)
   - **Mục đích:** Bảo vệ nhiều tên miền khác nhau hoặc nhiều tên miền phụ ở các cấp độ khác nhau, bao gồm cả tên miền chính, trong một chứng chỉ duy nhất.
   - **Cấu trúc:** Sử dụng trường **Subject Alternative Name (SAN)** để liệt kê tất cả các tên miền mà chứng chỉ sẽ bảo vệ.
   - **Ví dụ:** Một chứng chỉ SAN có thể bảo mật:
     - `yourdomain.com`
     - `www.yourdomain.com`
     - `blog.yourdomain.com`
     - `anothersite.net`
     - `dev.anothersite.net`
     - `mail.yetanothersite.org`
   - **Lợi ích:** Cực kỳ linh hoạt, lý tưởng cho các công ty có nhiều thương hiệu, nhiều trang web riêng biệt, hoặc các dịch vụ cần bảo mật nhiều tên miền trên cùng một máy chủ (hoặc thậm chí trên các máy chủ khác nhau nếu bạn có thể quản lý việc cài đặt chứng chỉ).


## Tự động gia hạn (Auto-renewal) với Let's Encrypt

- **Lệnh gia hạn thủ công:**
```
sudo certbot renew
```
> Lệnh này sẽ kiểm tra tất cả chứng chỉ đã cài và gia hạn nếu còn dưới 30 ngày.

- Thiết lập tự động bằng `cron` hoặc `systemd`
- **Với cron (Linux)**:
```
0 3 * * * /usr/bin/certbot renew --quiet
```
>Chạy mỗi ngày lúc 3h sáng, không in ra log nếu không có lỗi.


- **Với systemd (Ubuntu 20.04+): Certbot cài sẵn timer:**
```
sudo systemctl list-timers | grep certbot
```

- **Kiểm tra thử gia hạn**
```
sudo certbot renew --dry-run
```

