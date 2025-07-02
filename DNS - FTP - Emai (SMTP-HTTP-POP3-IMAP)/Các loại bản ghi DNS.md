# **Các loại bản ghi DNS**
- Bản ghi tên miền (DNS records) - là các thông tin cấu hình được lưu trữ trong hệ thống DNS (Domain Name System), dùng để chỉ định cách tên miền hoạt động, như: Trỏ tên miền về địa chỉ IP nào; xác định dịch vụ email; xác thực tên miền hoặc các chức năng khác
- Các bản ghi này bao gồm các tệp văn bản được viết bằng cú pháp DNS
- Mỗi bản ghi DNS chứa 4 trường chính: ***Name, Type, Data và TTL.***
  - ***Name***: Trường này cho phép thêm tiền tố (hay chính xác hơn là hậu tố, vì tên miền về mặt kỹ thuật được đọc từ phải sang trái) vào tên miền chính. (Nếu thêm bản ghi cho một domain phụ, chẳng hạn như shop.example.com, ta sẽ nhập “shop” vào trường này)
  - ***Type***: Loại bản ghi DNS xác định phần domain mà mỗi bản ghi sẽ thay đổi. Các loại bản ghi quan trọng nhất để giúp bạn thiết lập và vận hành được đề cập bên dưới.
  - ***Data*** : Trường dữ liệu chứa thông tin khác nhau tùy thuộc vào loại bản ghi đang tạo.
  - ***TTL*** (Time to Live): là thời gian tính bằng giây để mọi thay đổi đối với bản ghi DNS có hiệu lực. Với TTL là 3600, tất cả các thay đổi đối với bản ghi ví dụ này sẽ được làm mới sau mỗi 3600 giây (1 giờ).

## 1. **Record SOA**
### a. **Khái niệm: SOA (Start of Authority)**
- Là bản ghi DNS xác định "vùng quyền quản lý" của tên miền và chứa thông tin quản trị của zone đó.
- Hiểu đơn giản r.SOA là nơi bắt đầu quản lý DNS cho tên miền này, và tại đây là thông tin cấu hình chính.”

### b. **Vai trò - ứng dụng của Record SOA**
- Xác định **DNS server chính** cho một tên miền
- Quản lý **đồng bộ dữ liệu DNS giữa các máy chủ chính - phụ**
- Cung cấp **cấu hình cache & cập nhật** cho hệ thống DNS
- Luôn **là bản ghi đầu tiên** trong file zone của tên miền

### c. **Cấu trúc thành phần của bản ghi SOA**
- Một bản ghi SOA có dạng như sau:

[tên miền] IN SOA [tên-server-dns] [địa-chỉ-email] (serial number; refresh number; retry number; expire number; time-to-live number)

- Ví dụ cú pháp:
```
vutruongan.com. IN  SOA  ns1.zonedns.com.  dnsadmin.vutruongan.com. (
       	  2025042601 ; Serial
          3600       ; Refresh
          1800       ; Retry
          1209600    ; Expire
          86400 )    ; Minimum TTL
```
Trong đó: 

- `vutruongan.com.`  -  Là tên miền của tôi cần khai báo SOA
- `ns1.zonedns.com.` -  **Primary name server -NS:** giá trị DNS chính của tên miền hoặc máy chủ
- `dnsadmin.vutruongan.com.`  - Là email của quản trị DNS của tên miền  (viết dạng DNS, tức là  <dnsadmin@vutruongan.com>)
- **Serial**: [2025042601] -  Mã phiên bản cấu hình DNS (dạng: YYYYMMDDNN), giúp các DNS phụ biết khi nào cần cập nhật
- **Refresh**: [3600] - Sau 3600 giây (1 tiếng), các DNS phụ sẽ hỏi máy chính xem bản ghi có thay đổi không
- **Retry**: [1800] -  Nếu hỏi thất bại, sẽ thử lại sau 1800 giây (30 phút)
- **Expire**: [1209600] - Nếu sau 14 ngày mà không cập nhật được, DNS phụ sẽ ngừng phục vụ
- **Minimum TTL**: [86400] - Giá trị TTL mặc định cho các bản ghi DNS nếu không chỉ định rõ (1 ngày)

### d. **Lưu ý khi sử dụng bản ghi SOA**
- Mỗi zone DNS chỉ có đúng 1 bản ghi SOA.
- Nếu cấu hình sai thông số SOA, DNS phụ có thể không đồng bộ → gây lỗi truy cập.

## 2. **Record A**
### a. **Khái niệm**: 
- Bản ghi A (Address) là loại bản ghi DNS dùng để liên kết một tên miền với một địa chỉ IP dạng IPv4.
### b. **Vai trò và ứng dụng của bản ghi A**
- Truy cập website: Trỏ tên miền chính hoặc phụ đến máy chủ web (Apache, Nginx, v.v.).
- Trỏ subdomain (tên miền phụ), VD: mail.nhanhoa.com, api.nhanhoa.com, 
- Cấu hình với các hệ thống CDN, proxy, API, app mobile cũng yêu cầu bản ghi A để xác định máy chủ đích.
- Hạ tầng ảo hóa hoặc DNS nội bộ cũng sử dụng bản ghi A để định tuyến nội bộ.
### c. **Cấu trúc thành phần trong bản ghi A**
- Cú pháp khai báo record A: **[domain\_name]  IN  A  [địa-chỉ-Ipv4].**

  Ví dụ: vutruongan.com. IN A 14.248.82.194 , Trong đó:

  1. vutruongan.com. - là tên miền đầy đủ muốn tạo, Dấu chấm ở cuối là cú pháp chuẩn DNS để chỉ rõ đây là tên miền gốc (root).
  1. IN - là viết tắt của **Internet**, chỉ loại hệ thống sử dụng (chuẩn phổ biến nhất trong DNS).
  1. A - Loại bản ghi
  1. 14.248.82.194 - Địa chỉ **IPv4** mà tên miền trỏ tới – chính là địa chỉ IP của máy chủ (server).
### d. **Lưu ý khi sử dụng bản ghi A**
- **Không trùng với bản ghi CNAME** cho cùng tên miền.
- Không nên dùng bản ghi A cho nhiều subdomain nếu bạn định chuyển IP thường xuyên ~> nên dùng CNAME
- Có thể tạo **nhiều bản ghi A cho cùng tên miền** để load balancing.

## 3. **Record AAAA**

Tương tự như bản ghi A, nhưng thay vì sử dụng IPv4 sẽ là địa chỉ IPv6.

## 4. **Record CNAME** 
### a. **Khái niệm**: 
- Bản ghi **CNAME (Canonical name Record)** hay còn gọi là Bản ghi bí danh. Bản ghi CNAME cho phép một server có thể có nhiều tên. Nói cách khác bản ghi CNAME cho phép nhiều tên miền cùng trỏ đến một địa chỉ IP cho trước.
- Để có thể khai báo bản ghi CNAME, bắt buộc phải có bản ghi kiểu A để khai báo tên của máy. Tên miền được khai báo trong bản ghi kiểu A trỏ đến địa chỉ IP của máy được gọi là tên miền chính (canonical domain). Các tên miền khác muốn trỏ đến máy tính này phải được khai báo là bí danh của tên máy (alias domain).
### b. **Vai trò**
- Tạo bí danh (alias) cho một tên miền khác.
- Giúp nhiều subdomain trỏ về cùng một tên miền chính, mà không cần trỏ trực tiếp đến địa chỉ IP.
- Đơn giản hóa việc quản lý DNS: Khi tên miền chính thay đổi địa chỉ IP, chỉ cần cập nhật một nơi.
- Hỗ trợ trỏ domain phụ về dịch vụ bên ngoài như GitHub Pages, Blogger, dịch vụ email, CDN...
### c. **Cấu trúc thành phần trong bản ghi**
- Cấu trúc bản ghi CNAME:  **<subdomain>  IN  CNAME  <tên-miền-đích>.** 

  Ví dụ: shop.vutruongan.com. IN CNAME vutruongan.com.

Trong đó:

- shop.vutruongan.com. -  là tên miền phụ cần trỏ
- IN CNAME - tham số bản ghi
- vutruongan.com. – Tên miền chính mà subdomain sẽ trỏ đến

### d. **Lưu ý khi sử dụng**
- **Không được dùng CNAME tại tên miền gốc**: Lý do domain gốc thường phải chứa các bản ghi khác như A, MX, TXT → CNAME gây xung đột (không thể tồn tại cùng các bản ghi khác).

- chỉ dùng CNAME cho subdomain như www, blog, shop,…v.v.

- **Không dùng IP làm gì trị cho CNAME**: lý do CNAME chỉ trỏ tới tên miền, không trỏ tới IP
- **Nên kết thúc tên miền đích bằng dấu chấm (.)** trong cấu hình chuẩn để chỉ rõ là FQDN (tên miền đầy đủ).

## 5. **Record MX**
### a. **Khái niệm**: Bản ghi MX có tác dụng xác định, chuyển thư đến domain hoặc subdomain đích
### b. **Vai trò của MX**
- Xác định máy chủ email (mail server) sẽ xử lý email gửi đến tên miền.
- Cho phép tên miền nhận email qua các hệ thống email chuyên dụng như Google Workspace, Microsoft 365, Zoho Mail…
- Hỗ trợ nhiều máy chủ dự phòng, đảm bảo ổn định và độ tin cậy của dịch vụ email.
### c. **Cấu trúc, thành phần trong bản ghi MX**
- Cấu trúc: **<domain>  IN  MX  <priority>  <mail-server>**
- Ví dụ: vutruongan.com. IN MX 10 mail.vutruongan.com.  
  - vutruongan.com. - Là tên miền chính
  - IN  MX - Tham số bản ghi
  - 10 - Mức độ ưu tiên <**priority>**  (số càng nhỏ, ưu tiên càng cao)
  - mail.vutruongan.com.  - Là tên miền máy chủ xử lý email
### d. **Lưu ý khi sử dụng bản ghi MX**
- **Mail server phải là một FQDN (tên miền đầy đủ)** – không được dùng địa chỉ IP.
- **Không sử dụng CNAME cho mail server** – tên trong bản ghi MX phải trỏ bằng bản ghi A hoặc AAAA. 
- Phải đảm bảo **mail server trỏ về đúng địa chỉ IP** (có bản ghi A tương ứng).
  
## 6. **Record NS**
### a. **Khái niệm** 
- **NS (Name Server) record** là một loại bản ghi DNS dùng để **xác định máy chủ tên miền (DNS server)** nào chịu trách nhiệm quản lý **các bản ghi DNS** của một tên miền **(NS sẽ cho biết “tên miền này đang được quản lý DNS ở đâu”)**
### b. **Vai trò và ứng dụng của bản ghi NS**
- Chỉ định máy chủ DNS nào sẽ chịu trách nhiệm quản lý bản ghi DNS cho một tên miền.
- Nói cách khác: bản ghi NS giúp phân quyền điều khiển DNS cho tên miền của bạn.
- Cho phép tên miền hoạt động đúng trên Internet bằng cách trỏ đến DNS server lưu trữ các bản ghi A, MX, CNAME, TXT,...
### c. **Cấu trúc, thành phần của bản ghi**
- Cấu trúc 1 bản ghi NS: [domain\_name] IN NS [DNS-Server\_name]
- Ví dụ: Khi tôi mua tên miền vutruongan.com dùng dịch vụ DNS của Nhân Hòa để quản lý các bản ghi, tôi cần trỏ tên miền về NS của họ như: ns1.zonedns.vn , ns2.zonedns.vn , ns3.zonedns.vn và (hoặc) ns4.zonedns.vn
- Cú pháp: vutruongan.com. IN NS ns1.zonedns.vn.

`          `vutruongan.com. IN NS ns2.zonedns.vn.

### d. **Lưu ý khi sử dụng bản ghi**
- Một tên miền thường nên có ít nhất 2 bản ghi **NS** để đảm bảo hoạt động liên tục (dự phòng)
- Tên máy chủ NS **không được là địa chỉ IP**, mà phải là **tên miền đầy đủ**
- Khi thay đổi bản ghi NS, có thể mất thời gian để **DNS toàn cầu cập nhật (propagation)** – thường từ vài phút đến 24-48 giờ.
- Khi đã trỏ về NS khác, toàn bộ bản ghi DNS cần được **quản lý bên trong hệ thống đó**, không còn tác dụng nếu chỉnh ở nơi cũ.
- Bản ghi NS cấp cao (được quản lý ở nơi đăng ký tên miền) quyết định nơi quản lý toàn bộ DNS của tên miền.

## 7. **Record PTR  (pointer)**
### a. **Khái niệm:** PTR (Pointer) record **là bản ghi DNS dùng để** tra cứu ngược (Reverse DNS lookup) **–** tức là **từ địa chỉ IP suy ra tên miền.**
### b. **Vai trò của PTR**
- **Xác thực IP trong hệ thống email**: Rất quan trọng để chống spam, nhiều máy chủ email yêu cầu IP gửi mail phải có PTR record hợp lệ.
- Giúp các dịch vụ **ghi lại log rõ ràng hơn** (ghi tên miền thay vì IP).
- **Tăng độ tin cậy** của hệ thống mạng, nhất là trong môi trường doanh nghiệp hoặc dịch vụ hosting.
### c. **Cấu trúc, thành phần của bản ghi PTR**

<địa chỉ IP ngược dạng in-addr.arpa.> IN PTR <tên-miền>.

- Ví dụ: 194.82.248.14.in-addr.arpa. IN PTR vutruongan.com.

Trong đó: 194.82.248.14.in-addr.arpa.  - Tên miền tra cứu ngược tương ứng với địa chỉ IP 14.248.82.194

### d. **Lưu ý khi sử dụng PTR**
- Để cấu hình bản ghi, cần liên hệ với đơn vị cung cấp IP cho bạn như các nhà cung cấp ISP , chứ không phải nhà cung cấp tên miền

  Ví dụ bạn cần cấu hình PTR của IP 14.248.82.194 về vutruongan.com

  Vì IP 14.248.82.194  do Viettel quản lý thì bạn cần liên hệ với ISP Viettel cung cấp IP 14.248.82.194 thay vì nhà cung cấp nhanhoa.com

- Nên có **bản ghi A tương ứng** cho tên miền dùng trong PTR.

## 8. **Record SRV**
### a. **Khái niệm**
- Bản ghi **SRV (Service Record)** là một loại bản ghi trong DNS dùng để chỉ định máy chủ cung cấp dịch vụ cụ thể cho một tên miền. Không giống như các bản ghi DNS khác (như A, CNAME hay MX), bản ghi SRV được sử dụng để xác định máy chủ lưu trữ dịch vụ dựa trên **tên dịch vụ, giao thức, độ ưu tiên,** và **cổng** mà dịch vụ đó đang sử dụng.
### b. **Vai trò - ứng dụng của Record SRV**
- Bản ghi SRV rất quan trọng đối với các ứng dụng mạng, đặc biệt là các ứng dụng dựa trên IP như VoIP (thoại qua IP), XMPP (giao thức nhắn tin mở rộng), LDAP (dịch vụ thư mục), và nhiều dịch vụ khác.
- Bản ghi SRV giúp DNS không chỉ cung cấp địa chỉ IP mà còn thông tin về:
  - Tên dịch vụ (service)
  - Giao thức (TCP/UDP)
  - Tên miền cung cấp dịch vụ
  - Cổng mà dịch vụ chạy
  - Độ ưu tiên và trọng số để quyết định máy chủ nào sẽ được chọn nếu có nhiều máy chủ cung cấp dịch vụ.

### c. **Cấu trúc thành phần bản ghi SRV**
- Bản ghi SRV có cấu trúc gồm các thành phần chính sau:
  - **Service**: Tên dịch vụ (ví dụ: \_sip cho dịch vụ VoIP, \_xmpp cho dịch vụ nhắn tin).
  - **Protocol**: Giao thức mà dịch vụ sử dụng, có thể là \_tcp hoặc \_udp.
  - **Priority**: Độ ưu tiên của máy chủ. Máy chủ có độ ưu tiên thấp hơn sẽ được chọn trước.
  - **Weight:** Trọng số để phân phối tải giữa các máy chủ có cùng độ ưu tiên.
  - **Port**: Cổng mà dịch vụ hoạt động (ví dụ: cổng 5060 cho SIP).
  - **Target:** Tên miền của máy chủ cung cấp dịch vụ.
- Cú pháp bản ghi SRV:

\_service.\_protocol.name. TTL IN SRV priority weight port target 

Trong đó:

|Thành phần|Mô tả|
| :-: | :-: |
|\_service|Dịch vụ đang sử dụng (ví dụ: \_sip, \_ldap)|
|\_protocol|Giao thức mạng (TCP hoặc UDP)|
|name|Tên miền chính (vd: vutruongan.com.)|
|TTL|Thời gian sống trong cache DNS|
|priority|Mức ưu tiên – số nhỏ hơn = ưu tiên cao hơn|
|weight|Trọng số – chia tải nếu có nhiều server cùng mức ưu tiên|
|port|Số cổng dịch vụ (vd: 5060 cho SIP, 389 cho LDAP)|
|target|Tên máy chủ (hostname) xử lý dịch vụ|

- Ví dụ: \_sip.\_tcp.vutruongan.com. 3600 IN SRV 10 20 5060 sipserver.vutruongan.com.
### d. **Lưu ý khi sử dụng**
- Trỏ đến hostname: target trong bản ghi SRV phải là hostname, không phải địa chỉ IP. Hostname đó cần có bản ghi A/AAAA.
- Kiểm tra cổng: Phải đảm bảo **port** được mở và dịch vụ đang chạy tại server đích
- Không dùng chung với **CNAME:** target không được là bản ghi CNAME (phải là A/AAAA)

## 9. **Record TXT**
### a. **Khái niệm**
- Bản ghi TXT là bản ghi dùng để **lưu trữ dữ liệu văn bản (text)**, Cung cấp thông tin cho các máy chủ, dịch vụ (không phải con người đọc); Xác minh tên miền; Thiết lập bảo mật email (SPF, DKIM, DMARC).
### b. **Vai trò ứng dụng của bản ghi TXT**
- **Vai trò chính của bản ghi TXT:**
  - Chứa các thông tin dạng text được dịch vụ bên ngoài (Google, Microsoft, mail server...) kiểm tra.
  - Đảm bảo an toàn khi gửi email, chống giả mạo, ngăn chặn thư rác
  - Giúp xác minh quyền sở hữu tên miền.
- **Ứng dụng:**

  |**Ứng dụng**|**Nội dung TXT**|
  | :-: | :-: |
  |**SPF** – chống giả mạo email|v=spf1 include:\_spf.google.com ~all|
  |**DKIM** – chữ ký số cho email|v=DKIM1; k=rsa; p=MIGfMA...|
  |**DMARC** – chính sách xác thực email|v=DMARC1; p=none; rua=mailto:admin@domain.com|
  |**Google Site Verification**|google-site-verification=abc123XYZ...|
  |**Microsoft 365 / AWS**|TXT đặc biệt để xác minh sở hữu tên miền|


### c. **Cấu trúc thành phần bản ghi TXT**
- Cấu trúc một bản ghi TXT gồm:

  Tên\_miền. TTL IN  TXT "Nội\_dung\_text"

- Ví dụ 1: cấu hình SPF chống giả mạo email

vutruongan.com. 3600 IN TXT "v=spf1 include:\_spf.nhanhoa.com ~all"

trong đó:  "v=spf1 include:\_spf.nhanhoa.com ~all" → Nội dung bản ghi (chính là quy tắc SPF)

- Ví dụ 2 - xác minh Google Site:

vutruongan.com. 3600 IN TXT "google-site-verification=abc123456xyz"

- **Một số cấu trúc điển hình:**

|**Loại**|**Cấu trúc nội dung ví dụ**|**Giải thích**|
| :-: | :-: | :-: |
|**SPF**|v=spf1 include:\_spf.google.com ~all|Cho phép server Google gửi email cho domain|
|**DKIM**|v=DKIM1; k=rsa; p=MIGf...|Cung cấp public key để xác thực chữ ký email|
|**DMARC**|v=DMARC1; p=none; rua=mailto:dmarc@domain.com|Chính sách kiểm tra SPF, DKIM|
|**Xác minh dịch vụ**|google-site-verification=abc123...|Google kiểm tra quyền sở hữu domain|

### d. **Lưu ý khi sử dụng bản ghi TXT**
- Phải dùng dấu ngoặc kép "..." quanh nội dung text.
- Không tự ý xuống dòng nếu chưa biết cách nối chuỗi (vì bản ghi DNS có giới hạn ký tự, thường là 255 ký tự mỗi chuỗi).
- Nội dung sai format có thể gây lỗi xác thực (ví dụ: mail bị vào spam).
- Một domain có thể có nhiều bản ghi TXT nhưng nên quản lý rõ ràng từng mục đích.
- SPF chỉ nên có một bản ghi duy nhất, nếu cần thêm nhiều IP, thì gom chung trong 1 bản ghi.

||
| :- |
|**Loại bản ghi**|**Chức năng**|**Giá trị bản ghi**|
| :- | :- | :- |
|**A**|Trỏ tên miền đến IPv4|example.com → 192.0.2.1|
|**AAAA**|Trỏ tên miền đến IPv6 (nếu có)|example.com → 2001:db8::1|
|**CNAME**|Alias - trỏ tên miền này đến tên miền khác|www.example.com → example.com|
|**MX**|Chỉ định máy chủ mail|mail.example.com |
|**NS**|Xác định máy chủ DNS ủy quyền|ns1.example.com|
|**PTR**|Phân giải ngược IP → tên miền|192\.0.2.1 → example.com|
|**TXT**|Lưu văn bản tùy ý, dùng trong SPF/DKIM, xác thực tên miền|v=spf1 include:\_spf.google.com ~all|
|**SRV**|Dùng cho các dịch vụ như SIP, XMPP|\_sip.\_tcp.example.com → 10 60 5060 sipserver.example.com|
|**SOA**|Thông tin gốc của miền (Start of Authority)|Chứa email admin, serial, refresh time...|
#
