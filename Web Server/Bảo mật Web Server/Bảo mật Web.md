# 1. Phòng Chống DDoS và Brute Force

## 1.1. Phòng Chống Tấn Công Từ Chối Dịch Vụ Phân Tán (DDoS)

Tấn công DDoS nhằm mục đích làm quá tải máy chủ bằng cách gửi một lượng lớn lưu lượng truy cập hoặc yêu cầu độc hại, khiến dịch vụ không thể phản hồi các yêu cầu hợp lệ.

**Nguyên tắc chung:**
* **Hạn chế tốc độ (Rate Limiting):** Giới hạn số lượng yêu cầu mà một IP cụ thể có thể thực hiện trong một khoảng thời gian nhất định.
* **Giới hạn kết nối:** Giới hạn số lượng kết nối đồng thời từ một IP.
* **Chặn IP xấu:** Tự động chặn các IP có hành vi đáng ngờ.
* **Sử dụng CDN/DDoS Protection Services:** Các dịch vụ chuyên dụng này có thể hấp thụ và lọc lưu lượng DDoS ở biên mạng.

**Ví dụ cấu hình:**

**a) Nginx (Rate Limiting):**
Nginx có module `ngx_http_limit_req_module` và `ngx_http_limit_conn_module` để giới hạn yêu cầu và kết nối.

```nginx
# Bước 1: Định nghĩa vùng giới hạn yêu cầu (request limit zone)
# Trong khối http {}
http {
    limit_req_zone $binary_remote_addr zone=mylimit:10m rate=10r/s;
    # mylimit: tên vùng nhớ
    # 10m: 10 megabyte, đủ lớn để lưu trữ trạng thái cho khoảng 160.000 IP
    # 10r/s: Tối đa 10 yêu cầu mỗi giây từ một IP
    # (Nếu muốn giới hạn trung bình 10 yêu cầu/s, nhưng cho phép "burst" cao hơn một chút ban đầu)
    # limit_req_zone $binary_remote_addr zone=mylimit:10m rate=10r/s burst=20 nodelay;
    # burst=20: Cho phép 20 yêu cầu tăng vọt ban đầu
    # nodelay: Không trì hoãn xử lý các yêu cầu trong burst

# Bước 2: Định nghĩa vùng giới hạn kết nối (connection limit zone)
    limit_conn_zone $binary_remote_addr zone=perip:10m;
    # perip: tên vùng nhớ
    # 10m: 10 megabyte
    # $binary_remote_addr: Dùng địa chỉ IP nhị phân để tiết kiệm bộ nhớ

    server {
        listen 80;
        server_name your_domain.com;

        # Áp dụng giới hạn yêu cầu cho toàn bộ server hoặc các location cụ thể
        location / {
            limit_req zone=mylimit burst=5 nodelay; # Giới hạn 5 yêu cầu/s, cho phép burst 5, không delay
            limit_conn perip 10; # Giới hạn 10 kết nối đồng thời từ một IP
            # ... các cấu hình khác ...
        }

        # Áp dụng cho các yêu cầu đăng nhập nhạy cảm hơn (ví dụ: chỉ 1 yêu cầu/s)
        location /login {
            limit_req zone=mylogin:1m rate=1r/s burst=2 nodelay;
            # mylogin: cần định nghĩa một zone khác cho login
            # limit_req_zone $binary_remote_addr zone=mylogin:1m rate=1r/s;
            # ...
        }
    }
}
```

**b) Apache (mod_evasive, mod_qos):**
Apache không có chức năng rate limiting tích hợp mạnh mẽ như Nginx. Thường cần dùng các module như `mod_evasive` hoặc `mod_qos`.
- `mod_evasive`: Phát hiện và chặn các cuộc tấn công DDoS/Brute Force đơn giản bằng cách theo dõi số lượng yêu cầu và các sự kiện xảy ra trên server.
```apache=
# Cài đặt mod_evasive (ví dụ trên CentOS/RHEL):
# sudo yum install mod_evasive

# Cấu hình trong httpd.conf hoặc file cấu hình mod_evasive.conf
<IfModule mod_evasive20.c>
    DOSHashTableSize    3097
    DOSPageCount        2   # Số lượng yêu cầu tối đa cho một URL trong 1 giây
    DOSSiteCount        50  # Số lượng yêu cầu tối đa cho toàn bộ site trong 1 giây
    DOSPageInterval     1   # Khoảng thời gian (s) để đếm DOSPageCount
    DOSSiteInterval     1   # Khoảng thời gian (s) để đếm DOSSiteCount
    DOSBlockingPeriod   10  # Thời gian (s) để chặn IP khi bị phát hiện tấn công
    # DOSLogDir         "/var/log/mod_evasive" # Thư mục lưu log (phải được ghi bởi Apache)
    # DOSWhitelist      127.0.0.1  # IP được phép (ví dụ: máy chủ nội bộ)
</IfModule>
```
Khi bị chặn, người dùng sẽ nhận được lỗi 403 Forbidden.
- `mod_qos`: Module mạnh mẽ hơn, cung cấp nhiều tính năng kiểm soát chất lượng dịch vụ (QoS) và bảo vệ DDoS.
```apache=
# Cấu hình ví dụ trong httpd.conf
<IfModule mod_qos.c>
    # Giới hạn tổng số kết nối đến server
    QS_MaxClients 200

    # Giới hạn số lượng yêu cầu/giây từ một IP
    QS_SrvMaxConnPerIP 5
    QS_SrvMaxReqPerIP 10

    # Phát hiện và chặn các cuộc tấn công flooding
    QS_EventPerSec 100
    QS_EventLimit 200
    QS_EventBlocktime 300

    # Các cài đặt khác...
</IfModule>
```

**c) Sử dụng CDN và Dịch vụ bảo vệ DDoS:**
Các dịch vụ như Cloudflare, Akamai, Sucuri cung cấp lớp bảo vệ đầu tiên bằng cách:

- Proxy lưu lượng: Lưu lượng truy cập đi qua mạng lưới của họ trước khi đến server gốc của bạn.
- Lọc bot và lưu lượng độc hại: Sử dụng các thuật toán và quy tắc để xác định và chặn lưu lượng DDoS.
- Hấp thụ tấn công: Phân tán lưu lượng tấn công trên mạng lưới lớn của họ, làm giảm đáng kể áp lực lên server của bạn.

## 1.2. Phòng Chống Tấn Công Brute Force
Tấn công Brute Force nhằm mục đích đoán mật khẩu, tên người dùng hoặc các thông tin đăng nhập khác bằng cách thử vô số sự kết hợp.

Biện pháp phòng chống:

- **Giới hạn số lần thử đăng nhập thất bại**: Chặn IP sau một số lần thử sai nhất định.
- **Captchas**: Yêu cầu xác minh con người sau một số lần thử sai.
- **Xác thực hai yếu tố (2FA):** Tăng cường lớp bảo mật bằng mã xác thực bổ sung.
- Sử dụng mật khẩu mạnh và không dễ đoán.

Ví dụ cấu hình (chủ yếu ở cấp độ ứng dụng hoặc Web Server proxy):

**a) Nginx (Rate Limiting cho trang đăng nhập):**

Đây là cách hiệu quả nhất để ngăn Brute Force ở cấp độ Web Server.
```nginx=
http {
    # ... (các cấu hình limit_req_zone khác) ...
    limit_req_zone $binary_remote_addr zone=login_limit:10m rate=1r/s;
    # login_limit: 1 yêu cầu/giây từ một IP cho trang đăng nhập

    server {
        # ...

        location /login {
            limit_req zone=login_limit burst=3 nodelay; # Cho phép 3 lần thử sai, sau đó 1 lần/giây
            # ... proxy_pass đến ứng dụng backend của bạn ...
        }
    }
}
```

**b) Apache (mod_evasive hoặc fail2ban):**
- `mod_evasive`: Có thể cấu hình để chặn IP nếu có quá nhiều yêu cầu đến trang đăng nhập trong một khoảng thời gian ngắn.
- `Fail2ban`: Một công cụ rất mạnh mẽ ở cấp độ hệ thống. Nó phân tích log của các dịch vụ (SSH, Apache, Nginx, FTP, v.v.) và tự động thêm các IP có hành vi đáng ngờ vào tường lửa (iptables).
```
# Ví dụ cấu hình trong /etc/fail2ban/jail.local
[apache-auth]
enabled = true
port    = http,https
filter  = apache-auth
logpath = /var/log/httpd/error_log # hoặc /var/log/apache2/error.log
maxretry = 3                      # Chặn sau 3 lần thử sai
bantime  = 3600                   # Chặn trong 1 giờ (3600 giây)
```
# 2. Cấu hình Firewall (iptables, Cloudflare)
Firewall là tuyến phòng thủ đầu tiên, kiểm soát lưu lượng truy cập vào và ra khỏi server.
## 2.1. Iptables (Linux Firewall)
Iptables là tường lửa mặc định và mạnh mẽ trên Linux. Nó hoạt động ở cấp độ hạt nhân.

**Nguyên tắc chung:**

- Default Deny: Mặc định từ chối tất cả các kết nối đến và chỉ cho phép các kết nối cần thiết.
- Chỉ cho phép cổng cần thiết: Mở các cổng mà dịch vụ của bạn sử dụng (HTTP: 80, HTTPS: 443, SSH: 22 hoặc cổng tùy chỉnh, DNS: 53, v.v.).
- Giới hạn số lượng kết nối/kết nối mới: Chống lại các cuộc tấn công SYN Flood.

**Ví dụ cấu hình Iptables (CentOS/RHEL, Ubuntu dùng UFW dễ hơn):**
```
# Xóa tất cả các quy tắc hiện có
sudo iptables -F
sudo iptables -X
sudo iptables -Z

# Thiết lập chính sách mặc định (Default Deny)
sudo iptables -P INPUT DROP    # Mặc định từ chối tất cả traffic đến
sudo iptables -P FORWARD DROP  # Mặc định từ chối forwarding
sudo iptables -P OUTPUT ACCEPT # Mặc định cho phép tất cả traffic đi ra

# Cho phép traffic trên giao diện loopback (127.0.0.1)
sudo iptables -A INPUT -i lo -j ACCEPT

# Cho phép các kết nối đã thiết lập hoặc liên quan
sudo iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT

# Cho phép kết nối SSH (ví dụ: cổng 22)
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Cho phép kết nối HTTP (cổng 80)
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT

# Cho phép kết nối HTTPS (cổng 443)
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# (Tùy chọn) Chống SYN-Flood
sudo iptables -A INPUT -p tcp --syn -m limit --limit 1/s --limit-burst 3 -j ACCEPT

# Lưu các quy tắc (quan trọng để chúng không bị mất khi khởi động lại)
# Trên CentOS/RHEL 7/8:
# sudo yum install iptables-services -y
# sudo systemctl enable iptables
# sudo /usr/libexec/iptables/iptables.init save
# Hoặc sudo service iptables save

# Trên Ubuntu/Debian (dùng UFW - đơn giản hơn):
# sudo ufw enable
# sudo ufw allow 22/tcp
# sudo ufw allow 80/tcp
# sudo ufw allow 443/tcp
# sudo ufw default deny incoming
# sudo ufw default allow outgoing
```

## 2.2. Cloudflare (Web Application Firewall - WAF)

Cloudflare không chỉ là một CDN mà còn là một dịch vụ bảo mật mạnh mẽ, cung cấp WAF và bảo vệ DDoS.

**Nguyên tắc:**
- WAF (Web Application Firewall): Lọc và chặn các cuộc tấn công ở tầng ứng dụng (SQL injection, XSS, v.v.) trước khi chúng đến server của bạn.
- DDoS Protection: Hấp thụ và phân tán các cuộc tấn công DDoS ở quy mô lớn.
- Bot Management: Quản lý và chặn các bot độc hại.
- SSL/TLS: Cung cấp HTTPS miễn phí và hiệu quả.

**Cách hoạt động**: Bạn thay đổi DNS nameservers của tên miền để trỏ về Cloudflare. Mọi lưu lượng truy cập sẽ đi qua mạng lưới của Cloudflare trước tiên.

**Ưu điểm:**

- Bảo mật toàn diện: Kết hợp nhiều lớp bảo vệ.
- Dễ sử dụng: Cấu hình chủ yếu qua giao diện web.
- Khả năng mở rộng: Bảo vệ hiệu quả cho cả những cuộc tấn công lớn
- Cải thiện hiệu suất: CDN và tối ưu hóa khác giúp tăng tốc độ.

# 3. Hardening Server (Tăng cường bảo mật máy chủ) 

Hardening server là quá trình giảm thiểu bề mặt tấn công của máy chủ bằng cách loại bỏ các dịch vụ không cần thiết và cập nhật hệ thống thường xuyên.

**a) Tắt dịch vụ không cần thiết:**
Mỗi dịch vụ chạy trên server là một cổng tiềm năng cho kẻ tấn công.

- Liệt kê dịch vụ đang chạy:
```bash=
sudo systemctl list-units --type=service --state=running # Systemd
sudo service --status-all # SysVinit
```
- **Tắt và vô hiệu hóa dịch vụ không dùng:**
    - Ví dụ: Nếu bạn không cần SSH, FTP (hoặc dùng SFTP/SCP), mail server (Postfix/Sendmail), v.v
    ```
    sudo systemctl stop <tên_dịch_vụ>
  sudo systemctl disable <tên_dịch_vụ>
  ```

**b) Cập nhật bảo mật thường xuyên:**
Các bản vá bảo mật vá lỗi và lỗ hổng được phát hiện.

- Luôn cập nhật hệ điều hành và các phần mềm:
    - CentOS/RHEL: `sudo yum update`
    - Ubuntu/Debian: `sudo apt update && sudo apt upgrade -y`

- Cập nhật Web Server (Nginx, Apache): Đảm bảo bạn đang chạy phiên bản mới nhất, hoặc phiên bản có các bản vá bảo mật quan trọng.
- Cập nhật ngôn ngữ lập trình và framework: PHP, Python, Node.js, Ruby, Laravel, Django, v.v., đều cần được cập nhật.

**c) Cấu hình SSH an toàn:**

- Không sử dụng user `root` để đăng nhập trực tiếp: Tạo user riêng và dùng `sudo`.
- Sử dụng xác thực khóa SSH (SSH Keys) thay vì mật khẩu: An toàn hơn rất nhiều.
- Thay đổi cổng SSH mặc định (từ 22 sang một cổng khác): Giảm bớt các cuộc tấn công Brute Force tự động.
- Tắt xác thực mật khẩu (Password Authentication): Bắt buộc dùng khóa SSH.
- Giới hạn đăng nhập từ IP cụ thể: Nếu có thể.
```
# Trong file /etc/ssh/sshd_config
PermitRootLogin no        # Không cho phép root đăng nhập trực tiếp
PasswordAuthentication no # Tắt xác thực bằng mật khẩu
Port 2222                 # Đổi cổng SSH thành 2222 (ví dụ)
AllowUsers your_username  # Chỉ cho phép user "your_username" đăng nhập
# Sau khi thay đổi, khởi động lại SSH service
# sudo systemctl restart sshd
```
**d) Giới hạn quyền truy cập file/thư mục:**

- Web server (người dùng `apache` hoặc `nginx`) chỉ nên có quyền đọc đối với các file web và quyền ghi đối với các thư mục cần thiết (ví dụ: thư mục upload).

- Quyền truy cập thư mục: `chmod 755 /var/www/html`

- Quyền truy cập file: `chmod 644 /var/www/html/index.html`

- Tuyệt đối không cấp quyền 777.


**e) Loại bỏ thông tin nhạy cảm:**

- Tắt hiển thị phiên bản server: Ngăn kẻ tấn công biết được phiên bản Nginx/Apache của bạn (giúp họ tìm lỗ hổng).
    - Nginx: Trong `http {}` hoặc `server {}`, thêm `server_tokens off`;
    - Apache: Trong `httpd.conf`, đặt `ServerTokens Prod` và `ServerSignature Off`.

- Không hiển thị danh sách thư mục (Directory Listing): Tắt `autoindex` trong Nginx hoặc `Options Indexes` trong Apache.


# 4. Xử lý các Lỗ hổng Ứng dụng Web

Các lỗ hổng ở tầng ứng dụng là nguyên nhân chính của nhiều cuộc tấn công. Web server có thể đóng vai trò là tuyến phòng thủ đầu tiên, nhưng việc xử lý tận gốc phải ở cấp độ mã nguồn ứng dụng.

## **a) SQL Injection**:
Kẻ tấn công chèn mã SQL độc hại vào các trường nhập liệu để thao túng cơ sở dữ liệu.

**Phòng chống:**
- Sử dụng Prepared Statements (hoặc Parametrized Queries): Đây là biện pháp hiệu quả nhất. Tách biệt mã SQL và dữ liệu đầu vào.
- Validating Input: Kiểm tra và làm sạch tất cả dữ liệu đầu vào từ người dùng.
- Principle of Least Privilege: Giới hạn quyền của tài khoản cơ sở dữ liệu.

- **WAF (Web Application Firewall)**: Có thể phát hiện và chặn các mẫu tấn công SQL Injection thông thường. (Ví dụ: ModSecurity cho Nginx/Apache, hoặc WAF của Cloudflare).


## b) Cross-Site Scripting (XSS):
Kẻ tấn công chèn mã script độc hại (JavaScript) vào trang web hợp pháp, được thực thi trên trình duyệt của người dùng khác.

**Phòng chống:**
- **Output Encoding/Escaping:** Mã hóa tất cả dữ liệu do người dùng tạo trước khi hiển thị trên trang web.
- **Content Security Policy (CSP**): Một header HTTP cho phép bạn chỉ định các nguồn tài nguyên đáng tin cậy cho trình duyệt.

## c) Cross-Site Request Forgery (CSRF):
Kẻ tấn công lừa người dùng đã đăng nhập thực hiện các hành động không mong muốn trên một trang web mà họ đã được xác thực.

- Phòng chống (chủ yếu ở cấp độ ứng dụng):
    - **CSRF Tokens**: Sử dụng các token ngẫu nhiên, duy nhất cho mỗi phiên hoặc mỗi yêu cầu.
    - **Same-Site Cookies**: Ngăn chặn trình duyệt gửi cookie xác thực cùng với yêu cầu từ các trang web bên ngoài.
