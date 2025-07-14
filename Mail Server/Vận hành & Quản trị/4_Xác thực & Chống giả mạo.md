> # Thực hành với ZIMBRA

# 1. Xác thực & chống giả mạo

## **1.1. SPF (Sender Policy Framework)**

### Mục đích:

Giúp **Mail server** nhận biết đâu là **IP được phép gửi** mail cho domain của bạn.

### Cách tạo SPF record:

* Tạo bản ghi **TXT** trong DNS của domain:

```txt!
antvpro.io.vn.  IN  TXT  "v=spf1 ip4:<IP-của-mail-server> -all"
```

<img width="1003" height="644" alt="image" src="https://github.com/user-attachments/assets/bb931791-6952-4ccd-8a15-aabb6f98f9f2" />

> * Ý nghĩa:
> 
>   * `v=spf1`: Phiên bản SPF.
>   * `ip4:...`: IP được phép gửi mail.
>   * `-all`: Từ chối tất cả IP khác.

###  **Cách kiểm tra SPF**:

```bash!
dig TXT antvpro.io.vn +short
```
- output trả về như sau:

<img width="452" height="84" alt="image" src="https://github.com/user-attachments/assets/f3d67e6c-17e3-40be-aa43-08632777d77d" />

- Dùng công cụ online:

  * [https://mxtoolbox.com/spf.aspx](https://mxtoolbox.com/spf.aspx)
  * [https://dmarcian.com/spf-survey/](https://dmarcian.com/spf-survey/)

<img width="1033" height="425" alt="image" src="https://github.com/user-attachments/assets/3d4535ef-e26f-41b3-b84e-3188e41de2b1" />


### Debug
---

## 1.2. DKIM (DomainKeys Identified Mail)

### Mục đích:

Chứng minh email không bị chỉnh sửa và xác thực nguồn gửi bằng chữ ký số.

### Tạo DKIM trong Zimbra:

- Chạy dưới tài khoản `zimbra`:  

    ```bash!
    su - zimbra

    /opt/zimbra/libexec/zmdkimkeyutil -a -d antvpro.io.vn
    ```

    --> Kết quả sẽ ra **public key**, bạn phải thêm vào DNS:
    
    - Public key có dạng như sau:
```bash!    
DKIM Public signature:
F500E190-5C71-11F0-90F0-29241681A8C9._domainkey IN      TXT     ( "v=DKIM1; k=rsa; "
          "p=MIIBIjANBgkqXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX7qifIz8qeis"
          "vB0HNkFXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXAQAB" )  ; ----- DKIM key F500E190-5C71-11F0-90F0-29241681A8C9 for antvpro.io.vn
```
    
<img width="880" height="261" alt="image" src="https://github.com/user-attachments/assets/cc87a6a9-25b1-4b4c-9e52-95e3b4b98ff3" />

- **Tạo bản ghi TXT**:
    - Lấy giá trị bắt đầu từ `( "v=DKIM1; k=rsa; "` và kết thúc ở  `...IDAQAB" )`
    - **Ghép nối và loại bỏ các ký tự không cần thiết** như dấu ngoặc kép (`""`) và các khoảng trắng giữa các chuỗi bị ngắt dòng --> để **tạo thành chuỗi liên tục** duy nhất

```txt!
zm._domainkey.antvpro.io.vn. IN TXT "v=DKIM1; k=rsa; p=MIGfMA0GC...IDAQAB"
```
<img width="756" height="452" alt="image" src="https://github.com/user-attachments/assets/0ef2c605-f86a-49cd-b86e-19e7a35c91ed" />


### Kiểm tra DKIM:
- Kiểm tra nhanh bằng dòng lệnh sau khi thêm **DKIM**

```bash!
dig +short TXT selector1._domainkey.yourdomain.com
```
<img width="859" height="178" alt="image" src="https://github.com/user-attachments/assets/16a2a2fe-d260-4ba5-9e91-f044b8f822a1" />

- Hoặc dùng tools online:
    - https://mxtoolbox.com/SuperTool.aspx?action=dkim
    - https://dkimcore.org/c/keycheck
    - <img width="1624" height="837" alt="image" src="https://github.com/user-attachments/assets/e971c8d6-f0f6-4594-99d6-7162351a9b38" />


### Debug
---

## **1.3. DMARC (Domain-based Message Authentication, Reporting, and Conformance)**

### Mục đích:

- Thiết lập chính sách kiểm tra SPF, DKIM để báo cáo và xử lý email đáng ngờ.
- **DMARC** dựa vào **SPF** & **DKIM** để ra quyết định
- Chấp nhận / 
- Cách ly / 
- Từ chối mail không hợp lệ

### Cách tạo bản ghi DMARC:

- Thêm **TXT record** vào **DNS** _ cú pháp:
```lua!
_dmarc.yourdomain.com IN TXT "v=DMARC1; p=quarantine; rua=mailto:dmarc@yourdomain.com"
```
- Ví dụ:
```lua!
_dmarc.antvpro.io.vn. IN TXT "v=DMARC1; p=quarantine; rua=mailto:admin@antvpro.io.vn; ruf=mailto:admin@antvpro.io.vn"
```
<img width="979" height="542" alt="image" src="https://github.com/user-attachments/assets/ee0cfc27-d49e-45a4-a862-612905a2d851" />


> * `p=quarantine`: Cách xử lý (có thể là `none` || `quarantine`- Cách ly ||hoặc  `reject` - từ chối).
> * `rua`, `ruf`: Địa chỉ nhận báo cáo thống kê / chi tiết.

### Cách kiểm tra DMARC:

* Sử dụng tools online để kiểm tra: 
    * [https://dmarcian.com/dmarc-inspector/](https://dmarcian.com/dmarc-inspector/)
    * Hoặc https://mxtoolbox.com/SuperTool.aspx?

<img width="1447" height="770" alt="image" src="https://github.com/user-attachments/assets/8955a135-08aa-43d7-9374-0527b572cb93" />

---

## **1.4. SMTP AUTH – Xác thực người gửi**
Là cơ chế **xác thực người gửi** qua username + password khi gửi email (đặc biệt qua cổng 587 hoặc 465).

### **Mục đích**:
Yêu cầu người dùng cung cấp username/password để gửi email, tránh bị open relay.

### **Kiểm tra SMTP AUTH có bật không**:

```bash!
su - zimbra
zmprov gs `zmhostname` | grep Auth
```
> #### **Đảm bảo `zimbraMtaAuthEnabled` là `TRUE`**
> #### và  **`zimbraMtaSmtpdSaslAuthEnable`: `yes`**

<img width="679" height="275" alt="image" src="https://github.com/user-attachments/assets/dbcf837a-5bc1-4032-a51d-1161d2d64850" />

- Tại đây ta thấy:
    - ✅ **Các mục đang bật:**
        - `zimbraMtaAuthEnabled`: `TRUE` → Xác thực MTA đang bật.
        - `zimbraMtaSaslAuthEnable`: `yes` → SASL đang được bật cho MTA.
        - `zimbraMtaAuthPort`: `7073` → Port nội bộ Zimbra dùng để xác thực.
        - `zimbraMtaBrokenSaslAuthClients`: `yes` → Cho phép client lỗi chuẩn SASL vẫn xác thực được.
        - `zimbraMtaTlsAuthOnly`: `TRUE` → Chỉ cho phép xác thực khi kết nối TLS.
    - ❌ **Các mục chưa bật:**
        - `zimbraMtaSmtpSaslAuthEnable`: `no` → SMTP AUTH chưa bật cho Postfix (rất quan trọng nếu bạn muốn gửi mail từ client như Outlook, Thunderbird).

- **Bật SMTP AUTH cho Zimbra:**
```bash!
su - zimbra

zmprov ms `zmhostname` zimbraMtaSmtpSaslAuthEnable yes

zmmtactl restart
```

- **Kiểm tra lại SMTP AUTH hoạt động:**

```bash!
openssl s_client -connect mail.antvpro.io.vn:587 -starttls smtp
```
- Sau khi kết nối, gõ: 
```
EHLO test
AUTH LOGIN
```
<img width="864" height="408" alt="image" src="https://github.com/user-attachments/assets/b2dd7446-de8b-4907-a766-9fa668de6acd" />

- Nếu thấy phản hồi như **`334 VXNlcm5hbWU6`**, tức là server đã bật xác thực.


### Debug
---

## **1.5. SASL (Simple Authentication and Security Layer)**

### **Mục đích**:

Là giao thức hỗ trợ SMTP AUTH – quản lý xác thực ở cấp độ thấp hơn.

> Zimbra dùng **Cyrus SASL** để xác thực SMTP qua Postfix.

### **Kiểm tra hoạt động của SASL**:

```bash
ps aux | grep saslauthd
```

<img width="1198" height="168" alt="image" src="https://github.com/user-attachments/assets/0c8ddfee-ce21-4117-8351-7cf0a1e5ce51" />

- Cấu hình nằm ở: 
```bash!
/opt/zimbra/conf/sasl2/smtpd.conf
```

