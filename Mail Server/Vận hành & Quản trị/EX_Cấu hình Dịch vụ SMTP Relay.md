### 3.3  Ví Dụ cụ thể về Xử lý eMail bị treo

#### Bước 1: Test gửi nhận email
- Test thử dùng **`user1`** và **`user2`** gửi thư qua lại cho nhau

![image](https://github.com/user-attachments/assets/ad548947-0695-4bec-b9fc-7fd99243849b)

--> không thấy nhảy tin nhắn trong **Inbox**

- **Kiểm tra log queue**: 
```bash
postqueue -p
```
![image](https://github.com/user-attachments/assets/2ab50ae5-ef41-483a-a212-aa98d710f879)
![image](https://github.com/user-attachments/assets/dd576572-86f9-4c96-a570-f7f4cf28e42b)

--> **Hai email này chưa được Zimbra MTA gửi đến LMTP/Mailbox server**, tức là vẫn **chưa vào inbox** do đang nằm trong queue.

#### Bước 2: Đi tìm nguyên nhân

- **Những nguyên nhân dẫn đến email bị kẹt trong hàng đợi queue**
    -  **Lỗi kết nối đến máy chủ đích:**
        -  Máy chủ nhận email không phản hồi(có thể do bảo trì hoặc quá tải...) 
        -  DNS không phân giải được địa chỉ người nhận)
    - **Cấu hình sai hoặc thiếu thông tin**
        - Sai địa chỉ IP, hostname, hoặc cổng gửi mail (SMTP)
        - Thiếu hoặc sai thông tin xác thực (username/password).

    - **Bị chặn bởi hệ thống chống spam**
    - **Hệ thống đích quá tải hoặc tạm thời không nhận thư**
    - **Kích thước thư quá lớn**
    - **Lỗi nội bộ trong Zimbra**:
        - Dịch vụ **postfix**, **mta**, hoặc **amavis** bị treo hoặc chưa khởi động.
        - **Queue** bị đầy hoặc lỗi phân quyền.

-->  **Kiểm tra kết nối từ MTA đến LMTP-Local Mail Transfer Protocol (mailbox)**
```bash!
netstat -tnlp | grep 7025
```
- Kết quả phải là `*:7025` hoặc `127.0.0.1:7025`
- Nếu **LMTP chưa mở cổng 7025** thì Postfix không gửi được mail đến mailbox.

--> **Kiểm tra chi tiết lỗi của mail queue:**

- Dùng lệnh sau để xem log liên quan đến 2 mã queue `659D7BF437` và `EC310BF438`

```bash!
grep 659D7BF437 /var/log/zimbra.log
grep EC310BF438 /var/log/zimbra.log
```

![image](https://github.com/user-attachments/assets/5eaa458b-9441-45ab-b9f6-bab65496a9a6)
> không thấy log delivery đến `user2@antvpro.io.vn`

--> **Ép gửi lại queue và Xem log ngay sau khi chạy**
```bash
# su - zimbra 
/opt/zimbra/common/sbin/postqueue -f

tail -f /var/log/zimbra.log
```
![image](https://github.com/user-attachments/assets/2e4ed779-5909-43d9-aac0-ea4c136c42bb)

> Postfix (thành phần MTA) đang cố gắng gửi mail ra ngoài thông qua SMTP relay --> do đang bị **Google Cloud chặn port 25** nhưng chưa cấu hình thông tin xác thực (username/password) để gửi qua **`relay`** ⇒ lỗi `smtp_sasl_password_maps`.



#### Bước 3: Xử lý - Cấu hình Zimbra sử dụng SMTP Relay qua SendGrid

1. **Cấu hình máy chủ relay**
zmprov ms mail.antvpro.io.vn zimbraMtaRelayHost smtp.sendgrid.net
2. **Tạo file thông tin xác thực:**
```bash!
echo "smtp.sendgrid.net apikey:SG.xxxxxxxxxxxxxxxxxxxx" > /opt/zimbra/conf/relay_password
```
> - Tạo tài khoản **Sendgrid**: https://www.twilio.com/en-us
> - Add tên miền và cấu hình DNS record để xác minh: **Settings -> Sender Authentication -> click vào nút Authenticate Your Domain**
> - **Lấy API Key:** "xxxxxxxxxxxxxxxxxxxx"
> ![image](https://github.com/user-attachments/assets/644d7f87-b21a-4fae-a7b4-950406bc6cd2)
> - **Thiết lập SMTP Relay theo IP**: Chọn **IP Access Management -> Add IP Address:** nhập **IP server Zimbra**
> ![image](https://github.com/user-attachments/assets/55d1805e-4344-4fc1-84ec-3f8bdeef0b90)

3. **Tạo bản đồ LMDB cho Postfix**
```bash!
postmap /opt/zimbra/conf/relay_password
postmap -q smtp.sendgrid.net /opt/zimbra/conf/relay_password
```
4. **Gán quyền file an toàn cho Zimbra:**
```bash!
chmod 600 /opt/zimbra/conf/relay_password
chown zimbra:zimbra /opt/zimbra/conf/relay_password
```
5. **Cấu hình relay SMTP trong Zimbra:**
```bash
zmprov ms mail.antvpro.io.vn zimbraMtaSmtpSaslPasswordMaps lmdb:/opt/zimbra/conf/relay_password
zmprov ms mail.antvpro.io.vn zimbraMtaSmtpSaslAuthEnable yes
zmprov ms mail.antvpro.io.vn zimbraMtaSmtpCnameOverridesServername no
zmprov ms mail.antvpro.io.vn zimbraMtaSmtpTlsSecurityLevel may
zmprov ms mail.antvpro.io.vn zimbraMtaSmtpSaslSecurityOptions noanonymous
```
6. **Reload Postfix:**
```bash!
postfix reload
```
#### Bước 4: Test Gửi/Nhận email
