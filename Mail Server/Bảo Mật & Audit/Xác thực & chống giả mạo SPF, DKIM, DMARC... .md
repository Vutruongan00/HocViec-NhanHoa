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

![image](https://github.com/user-attachments/assets/7d6a6161-af93-476a-86c2-33fc6edd48b1)

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

![image](https://github.com/user-attachments/assets/dbde19fb-834a-4b21-a119-bc06c29677da)


- Dùng công cụ online:

  * [https://mxtoolbox.com/spf.aspx](https://mxtoolbox.com/spf.aspx)
  * [https://dmarcian.com/spf-survey/](https://dmarcian.com/spf-survey/)

![image](https://github.com/user-attachments/assets/5981fd58-0020-45d3-bb77-643ef056a2d3)


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
          "p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA2PGB5PDtjGtiNkNbZWZd6nFFoo/GPGcnM1/Ipx2WmjpvLOhko8uMQLIBMCKGAkyDO0vhn2UGkv8pvUF3TdNsnxPVGUGQv2BOlSr2T3zoBcxNAtbP+jOAjiJ+XRT9ZZQ9L4eeZ61wEQ6/M8Ztr5lcrJlBWRiofcNOwejkfB8jQNyyJzbiiksat/VO66WpALmUiJq7qifIz8qeis"
          "vB0HNkFBIeNtHF0eHrYR/yZwNHH9gleUNhB9AEEPSgoT3sxaELq+qOD4nsQ/xXp1dVXvHeeJY/KjbeUflOkN0iCM/3f7GoMKCymzapp0+qI9cjBXvGCqJKXcMrMwqjTvm9DXh5swIDAQAB" )  ; ----- DKIM key F500E190-5C71-11F0-90F0-29241681A8C9 for antvpro.io.vn
```
    
![image](https://github.com/user-attachments/assets/3535b028-e51c-4dfd-875c-645fb51a5daf)

- **Tạo bản ghi TXT**:
    - Lấy giá trị bắt đầu từ `( "v=DKIM1; k=rsa; "` và kết thúc ở  `...IDAQAB" )`
    - **Ghép nối và loại bỏ các ký tự không cần thiết** như dấu ngoặc kép (`""`) và các khoảng trắng giữa các chuỗi bị ngắt dòng --> để **tạo thành chuỗi liên tục** duy nhất

```txt!
zm._domainkey.antvpro.io.vn. IN TXT "v=DKIM1; k=rsa; p=MIGfMA0GC...IDAQAB"
```
![image](https://github.com/user-attachments/assets/2febf986-c1bf-4c36-a439-369fb10bb035)


### Kiểm tra DKIM:
- Kiểm tra nhanh bằng dòng lệnh sau khi thêm **DKIM**

```bash!
dig +short TXT selector1._domainkey.yourdomain.com
```
![image](https://github.com/user-attachments/assets/d75afe63-cea8-47f2-ae6a-c5f54c51c6f2)

- Hoặc dùng tools online:
    - https://mxtoolbox.com/SuperTool.aspx?action=dkim
    - https://dkimcore.org/c/keycheck
    - ![image](https://github.com/user-attachments/assets/32fe0462-7663-4ef8-a762-1f9e9dd27db2)



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
![image](https://github.com/user-attachments/assets/3cfd781b-a98b-4bbb-8f12-d42a487779fb)


> * `p=quarantine`: Cách xử lý (có thể là `none` || `quarantine`- Cách ly ||hoặc  `reject` - từ chối).
> * `rua`, `ruf`: Địa chỉ nhận báo cáo thống kê / chi tiết.

### Cách kiểm tra DMARC:

* Sử dụng tools online để kiểm tra: 
    * [https://dmarcian.com/dmarc-inspector/](https://dmarcian.com/dmarc-inspector/)
    * Hoặc https://mxtoolbox.com/SuperTool.aspx?

![image](https://github.com/user-attachments/assets/8963ed23-f084-4901-b41a-d36d9a8c929a)

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

![image](https://github.com/user-attachments/assets/739100b2-ee27-4ab4-9cfb-0c7b7e500c78)

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
![image](https://github.com/user-attachments/assets/f164e0b1-979c-4340-92d3-c467e53a6335)

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

![image](https://github.com/user-attachments/assets/370fa572-d02f-42c3-b604-b779eed5c1a6)

- Cấu hình nằm ở: 
```bash!
/opt/zimbra/conf/sasl2/smtpd.conf
```

