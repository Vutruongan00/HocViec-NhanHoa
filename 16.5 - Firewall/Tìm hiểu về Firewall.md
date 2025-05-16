# TỔNG QUAN VỀ FIREWALL
**Firewall (tường lửa)** là một hệ thống bảo mật giúp kiểm soát lưu lượng mạng đi vào và đi ra khỏi hệ thống. Trên Linux, firewall thường dùng để:
- Chặn hoặc cho phép kết nối dựa trên IP, port, protocol, interface, v.v.
- Bảo vệ server khỏi các cuộc tấn công như port scanning, brute-force, DDoS.
- Quản lý truy cập mạng nội bộ hoặc Internet.
## 
## 1. Một số loại firewall phổ biến trên Linux
| Tên Firewall  |                                              Mô tả                                               |
|:-------------:|:------------------------------------------------------------------------------------------------:|
| **iptables**  | Công cụ cổ điển, mạnh mẽ, cấu hình bằng dòng lệnh, quản lý các rule trên Netfilter (nhân Linux). |
| **nftables**  |            Thay thế iptables, được phát triển bởi Netfilter Project, dễ quản lý hơn.             |
| **firewalld** |       Giao diện quản lý firewall động, dùng cho hệ thống dùng systemd (như CentOS, RHEL).        |
|    **ufw**    |       "Uncomplicated Firewall" - đơn giản hóa việc sử dụng iptables, phổ biến trên Ubuntu.       |
|    **CSF**    |         "ConfigServer Security & Firewall", thường dùng trong hosting, có giao diện web.         |
|    **APF**    |             "Advanced Policy Firewall", tương tự CSF, dùng cho hệ thống web server.              |
## 2. Các giao thức cơ bản trong firewall
Trong tường lửa (firewall) của Linux, các giao thức cơ bản thường được sử dụng để xác định loại lưu lượng mạng cần được kiểm soát: TCP, UDP, ICMP, ...
### 2.1 TCP (Transmission Control Protocol)

- Là giao thức hướng kết nối 
- Dùng cho các dịch vụ yêu cầu độ tin cậy như:
  - **HTTP/HTTPS** (port`80`/`443`)
  - **SSH** (port `22`)
  - **SMTP** (port `25`/`587`)
  - **POP3**, **IMAP, FTP**, v.v.
  
- **Ví dụ**
    - **IPtables**: cho phép mở port 22 (SSH)

            sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
    - **PowerShell**: Thêm rule cho TCP port (VD: mở RDP – TCP 3389)
```
New-NetFirewallRule -DisplayName "Allow RDP TCP 3389" -Direction Inbound -Protocol TCP -LocalPort 3389 -Action Allow
```
👉 Giải thích:
```
-Direction Inbound: Áp dụng cho lưu lượng vào.

-Protocol TCP: Giao thức TCP.

-LocalPort 3389: Cổng RDP.

-Action Allow: Cho phép
```

###  2.2.    UDP (User Datagram Protocol)

- Giao thức không hướng kết nối (_connectionless_)
- Dùng cho các ứng dụng yêu cầu tốc độ nhanh, không quan trọng mất gói:
  - **DNS** (port `53`)
  - **DHCP** (port `67`, `68`)
  - **NTP** (port `123`)

**Ví dụ:**
- **IPtables**: mở port DNS – UDP 53
    
    iptables -A INPUT -p udp --dport 53 -j ACCEPT
- **PowerShell**: 
```
New-NetFirewallRule -DisplayName "Allow DNS UDP 53" -Direction Inbound -Protocol UDP -LocalPort 53 -Action Allow
```

    
###  2.3. ICMP (Internet Control Message Protocol)
- Dùng để gửi thông báo lỗi, kiểm tra kết nối mạng (như ping).
- Không có "port", chỉ có "type" (ví dụ: echo-request, echo-reply)

**Ví dụ:** Cho phép ICMP Echo Request (ping vào máy)

- **IPtables**:

``` iptables -A INPUT -p icmp --icmp-type echo-request -j DROP ```
- **PowerShell**:
```
New-NetFirewallRule -DisplayName "Allow ICMPv4-In" -Protocol ICMPv4 -IcmpType 8 -Direction Inbound -Action Allow
```

### 2.4. ALL (Tất cả các giao thức)
- Dùng khi muốn áp dụng luật cho tất cả các loại giao thức, VD:

        iptables -A INPUT -p all -j DROP
## 3. Quyền quản trị Firewall
-    Quyền quản trị Firewall là quyền được phép cấu hình, điều khiển và thay đổi các thiết lập tường lửa của hệ thống.
-    Người có quyền này thường là quản trị viên hệ thống (**sudo hoặc root**) và có thể thực hiện các tác vụ như:
        -    Thêm/sửa/xóa rule.
        -    Mở/đóng port.
        -    Bật/tắt tường lửa.
        -    Lưu cấu hình tường lửa sau khi chỉnh sửa.
## 4. Những lợi ích của việc sử dụng FIREWALL
#### 4.1. Bảo vệ khỏi truy cập trái phép
- Firewall ngăn chặn các kết nối không hợp lệ từ bên ngoài vào hệ thống hoặc mạng nội bộ. Chỉ những lưu lượng được cho phép mới có thể đi qua firewall.

- Ví dụ: Chặn các máy lạ cố gắng truy cập SSH hoặc RDP vào server.
#### 4.2. Ngăn chặn phần mềm độc hại (malware) và virus
Firewall có thể phát hiện và ngăn các phần mềm độc hại cố gắng kết nối ra ngoài hoặc nhận lệnh từ hacker (Command & Control).
#### 4.3. Kiểm soát lưu lượng mạng
Firewall giúp quản trị viên kiểm soát loại lưu lượng nào được phép đi vào hoặc ra khỏi hệ thống, theo port, IP, giao thức, ứng dụng...

- Ví dụ: Cho phép HTTP/HTTPS, chặn FTP, chỉ cho một số IP nội bộ truy cập database.
#### 4.4. Bảo vệ người dùng và tài nguyên nội bộ
Giúp cô lập người dùng hoặc dịch vụ bị nhiễm, tránh lây lan trong mạng nội bộ.
#### 4.5. Ghi log và giám sát hoạt động mạng
Firewall ghi lại nhật ký (logs) về các kết nối thành công/thất bại, giúp điều tra sự cố bảo mật và theo dõi hành vi bất thường.
#### 4.6. Tùy biến theo nhu cầu bảo mật
Firewall cho phép thiết lập rule linh hoạt: theo thời gian, vị trí địa lý, ứng dụng, vai trò người dùng...
