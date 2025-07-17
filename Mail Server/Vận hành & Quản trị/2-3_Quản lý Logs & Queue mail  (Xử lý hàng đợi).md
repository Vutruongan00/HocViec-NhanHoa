> # Quản lý Logs và Xử lý hàng đợi queue  email 

# 2. Quản lý Logs & Giám sát hệ thống

## 2.1. Logs gửi/nhận mail
- **Vị trí lưu**
    - **CentOS/RHEL**: `/var/log/maillog`
    - **Debian/Ubuntu**: `/var/log/mail.log`

- **Log riêng của MTA:**
    - **Postfix**: Thường ghi vào `maillog` hoặc `mail.log`. Tuy nhiên, cấu hình có thể chuyển hướng chúng đến một file riêng (ví dụ: `/var/log/postfix.log`).
    - **Exim**: Thường có các log riêng như `/var/log/exim4/mainlog`, `/var/log/exim4/rejectlog`, `/var/log/exim4/paniclog`.
    - **qmail**: Logs của **qmail** thường nằm trong các thư mục `/var/log/qmail/current` và được quản lý bởi multilog.

## 2.2 Logs authentication (Xác thực)

Các log này ghi lại mọi nỗ lực đăng nhập vào hệ thống hoặc các dịch vụ sử dụng xác thực.

* **Vị trí phổ biến:**

  * `/var/log/secure` (trên CentOS/RHEL)
  * `/var/log/auth.log` (trên Debian/Ubuntu)

## 2.3 Logs truy cập IMAP/POP

Các log này ghi lại hoạt động của người dùng khi họ truy cập hộp thư của mình thông qua các giao thức IMAP hoặc POP3.

* **Vị trí phổ biến:**

  * **Dovecot log:** Thường được tích hợp vào `/var/log/maillog` hoặc `/var/log/mail.log`. Đôi khi, Dovecot có thể được cấu hình để ghi log riêng, chẳng hạn trong thư mục `/var/log/dovecot/`.
  * **Cyrus log:** Cyrus cũng có các file log riêng của nó, thường nằm trong thư mục cài đặt của Cyrus.

## 2.4 Logs chống spam/virus

Đây là các log từ các công cụ Anti-Spam và Anti-Virus, cung cấp thông tin về các mối đe dọa được phát hiện và cách chúng được xử lý.

* **Vị trí phổ biến:**

  * `/var/log/clamav/clamav.log` (hoặc tương tự): Log của ClamAV, ghi lại kết quả quét virus, cập nhật cơ sở dữ liệu virus.
  * `/var/log/spamassassin/spamassassin.log` (hoặc tương tự): Log của SpamAssassin, ghi lại chi tiết về việc chấm điểm spam cho các email, các luật được kích hoạt.
  * **Amavis:** Logs của Amavis thường được ghi vào `maillog` hoặc `mail.log` cùng với các thông tin của MTA, nhưng cũng có thể được cấu hình để ghi riêng.

## 2.5. Log của Zimbra

* **Thư mục log chính:** Hầu hết các log của Zimbra đều nằm trong thư mục `/opt/zimbra/log/`.
* **Logs gửi/nhận mail:**

  * `/var/log/zimbra.log`: Đây là file log tổng hợp chính của Zimbra, ghi lại các sự kiện từ Postfix (MTA), Amavisd, các hoạt động chống spam/virus, và các thành phần khác. Bạn sẽ thấy các dòng log liên quan đến việc gửi và nhận email tại đây.
  * `/opt/zimbra/log/mailbox.log`: Ghi lại tất cả các hoạt động liên quan đến mailbox (hộp thư), bao gồm việc chuyển thư, xóa thư, và các hoạt động của người dùng bên trong mailbox.

* **Logs authentication (Xác thực):**

  * `/opt/zimbra/log/audit.log`: File này chuyên ghi lại các hoạt động xác thực, bao gồm các lần đăng nhập thành công và không thành công. Đây là nơi lý tưởng để kiểm tra các cố gắng brute-force hoặc truy cập trái phép.
  * Các sự kiện xác thực từ Postfix (SMTP AUTH), Dovecot (IMAP/POP3) cũng có thể được ghi vào `/var/log/zimbra.log`.

* **Logs truy cập IMAP/POP:**

  * Các hoạt động của IMAP/POP3 thường được ghi vào `và cũng có thể xuất hiện trong` với các từ khóa như `ImapServer` hoặc `Pop3Server`.
* **Logs chống spam/virus:**

  * `/opt/zimbra/log/clamd.log`: Ghi lại tất cả các hoạt động quét virus của ClamAV.
  * `/opt/zimbra/log/freshclam.log`: Ghi lại quá trình cập nhật cơ sở dữ liệu virus của ClamAV.
  * `/opt/zimbra/log/spamtrain.log`: Log liên quan đến quá trình "huấn luyện" spam (mark as spam/ham).
  * Kết quả của SpamAssassin và Amavis thường được ghi vào \`\`, bạn có thể tìm kiếm các từ khóa như `amavis`, `spam` để xem chi tiết.

## 2.6 Kerio Connect

* **Thư mục log chính:**

  * **Windows:** `C:\Program Files\Kerio\MailServer\store\logs`
  * **macOS:** `/usr/local/kerio/mailserver/store/logs`
  * **Linux:** `/opt/kerio/mailserver/store/logs`

* **Logs gửi/nhận mail:**

  * `mail.log`: File log chính chứa thông tin về tất cả các tin nhắn email được Kerio Connect xử lý, bao gồm quá trình gửi đi, nhận về, và các trạng thái liên quan.

* **Logs authentication (Xác thực):**

  * `security.log`: Ghi lại các sự kiện liên quan đến bảo mật, bao gồm các lần đăng nhập (thành công/thất bại) vào Kerio Connect (web admin, webmail, IMAP/POP3, ActiveSync), các cố gắng xác thực không hợp lệ.
* **Logs truy cập IMAP/POP:**

  * Các hoạt động truy cập qua IMAP/POP3 cũng thường được ghi chi tiết trong `mail.log` và `security.log`.

* **Logs chống spam/virus:**

  * `anti-spam.log`: Ghi lại các quyết định của bộ lọc chống spam (ví dụ: SpamAssassin tích hợp hoặc Bitdefender).
  * `antivirus.log`: Ghi lại kết quả quét virus bởi bộ lọc chống virus tích hợp (ví dụ: Sophos hoặc Bitdefender).
  * Các sự kiện về từ chối hoặc cách ly email do spam/virus cũng có thể xuất hiện trong `mail.log`.
* **Các log hữu ích khác trên Kerio Connect:**

  * `error.log`: Ghi lại các thông báo lỗi nghiêm trọng.
  * `warning.log`: Ghi lại các cảnh báo.
  * `debug.log`: Log gỡ lỗi, thường chỉ bật khi cần thiết do có thể rất chi tiết và tốn dung lượng.
  * `config.log`: Ghi lại các thay đổi trong cấu hình của Kerio Connect.




## 2.7 Tra cứu và Phân tích Logs

### a. Công cụ dòng lệnh cơ bản

* `grep`: **Công cụ mạnh mẽ nhất để tìm kiếm các mẫu văn bản trong file.**

  * **Ví dụ:**

    * Tìm tất cả các email được gửi bởi `user@example.com`: `grep "from=<user@example.com>" /var/log/mail.log`
    * Tìm tất cả các email gửi đến `recipient@yourdomain.com`: `grep "to=<recipient@yourdomain.com>" /var/log/mail.log`
    * Tìm lỗi SMTP: `grep "status=bounced" /var/log/mail.log` `grep "status=deferred" /var/log/mail.log`
    * Tìm các lần đăng nhập SSH thất bại: `grep "Failed password" /var/log/auth.log`
* `awk`: **Công cụ xử lý văn bản mạnh mẽ, cho phép bạn trích xuất và thao tác với các trường dữ liệu trong log.** Thường được sử dụng kết hợp với `grep` hoặc `cat`.

  * **Ví dụ:** Trích xuất địa chỉ email từ log: `grep "status=sent" /var/log/mail.log | awk '{print $7}'` (Giả sử địa chỉ email nằm ở trường thứ 7)
* `zgrep`: Tương tự như `grep` nhưng được sử dụng để **tìm kiếm trong các file nén** (thường là file log cũ đã được rotate và nén với `.gz`).

  * **Ví dụ:** `zgrep "status=bounced" /var/log/mail.log.1.gz`
* `tail -f`: Xem log **thời gian thực**. Rất hữu ích khi bạn đang gỡ lỗi hoặc theo dõi một vấn đề đang xảy ra.

  * **Ví dụ:** `tail -f /var/log/mail.log`

### b. Hệ thống Syslog Server (Quản lý log tập trung)

Đối với các hệ thống lớn hơn hoặc khi bạn quản lý nhiều máy chủ, việc thu thập và phân tích log thủ công trở nên không hiệu quả. Đây là lúc các **syslog server** phát huy tác dụng:

* **Cách thức hoạt động:** Các máy chủ mail sẽ được cấu hình để gửi tất cả log của chúng đến một máy chủ tập trung. Trên máy chủ tập trung này, các công cụ chuyên dụng sẽ tiếp nhận, phân tích, lưu trữ và hiển thị log một cách trực quan.
* **Lợi ích:**

  * **Tập trung hóa:** Tất cả log từ nhiều nguồn được gom về một nơi duy nhất.
  * **Phân tích mạnh mẽ:** Các công cụ này có khả năng tìm kiếm, lọc, tổng hợp, tạo biểu đồ và báo cáo từ dữ liệu log khổng lồ.
  * **Giám sát thời gian thực & Cảnh báo:** Có thể thiết lập cảnh báo tự động khi phát hiện các mẫu log bất thường (ví dụ: quá nhiều lỗi gửi thư, nhiều lần đăng nhập thất bại).
  * **Lưu trữ dài hạn:** Quản lý việc lưu trữ log hiệu quả hơn.
* **Các công cụ phổ biến:**

  * **Graylog:** Một nền tảng quản lý log nguồn mở mạnh mẽ, cung cấp giao diện web trực quan để tìm kiếm, phân tích và dashboard hóa dữ liệu log.
  * **ELK Stack (Elasticsearch, Logstash, Kibana):** Một bộ công cụ rất phổ biến và mạnh mẽ:

    * **Elasticsearch:** Cơ sở dữ liệu tìm kiếm phân tán, lưu trữ và lập chỉ mục dữ liệu log.
    * **Logstash:** Thu thập, xử lý và gửi log từ nhiều nguồn khác nhau đến Elasticsearch.
    * **Kibana:** Giao diện người dùng đồ họa để khám phá, trực quan hóa và dashboard hóa dữ liệu trong Elasticsearch.
  * **Splunk:** Một nền tảng mạnh mẽ và toàn diện (thương mại) để thu thập, lập chỉ mục và phân tích dữ liệu máy móc, bao gồm log.

## Tầm quan trọng của Logs và Giám sát

* **Phát hiện sự cố:** Nhanh chóng nhận diện các vấn đề như queue mail bị kẹt, lỗi gửi/nhận, dịch vụ bị sập.
* **Bảo mật:** Phát hiện các hoạt động đáng ngờ như cố gắng xâm nhập, tấn công brute-force, hoặc hành vi phát tán spam từ server của bạn.
* **Gỡ lỗi:** Cung cấp thông tin chi tiết để chẩn đoán nguyên nhân gốc rễ của các lỗi mail.
* **Tối ưu hiệu suất:** Theo dõi lưu lượng mail và hiệu suất của server để đưa ra các quyết định tối ưu hóa.
* **Kiểm toán (Audit):** Cung cấp bằng chứng về hoạt động của hệ thống, hữu ích cho các yêu cầu tuân thủ.

---

# 3. Quản lý Queue Mail 

- **Queue mail (hàng đợi thư)** là một khu vực lưu trữ tạm thời trên mail server, nơi các email được giữ lại trước khi chúng được xử lý và gửi đi (hoặc chuyển giao cho MDA).
- Khi một email không thể được gửi ngay lập tức (**ví dụ**: máy chủ nhận không khả dụng, lỗi mạng tạm thời, hoặc quá tải), MTA sẽ đưa nó vào hàng đợi và thử gửi lại sau.

## 3.1 Kiểm tra Queue
- `postqueue -p` (**Postfix**): hiển thị nội dung của hàng đợi Postfix.
- `exim -bp` (**Exim**): hiển thị tóm tắt hoặc chi tiết các thư trong hàng đợi của Exim
- `qmail-qstat` (cho **qmail**):

## 3.2. Xử lý Mail "Stuck" (Thư bị kẹt)

Đôi khi, các email có thể bị kẹt trong hàng đợi vĩnh viễn (do lỗi cấu hình, địa chỉ đích không tồn tại, hoặc server đích không bao giờ phản hồi) 
--> **cần xóa chúng**:

- `postsuper -d <Queue_ID>` (cho **Postfix**):
    -  Ví dụ; để xóa thư có **ID** `A1B2C3D4E5` 
    ```
    postsuper -d A1B2C3D4E5
    ```
    
- `exim -Mrm <Message_ID>` (**Cho Exim**):
    - Ví dụ: xóa thư có **ID** `QWERTY12345` 
    ```
    exim -Mrm QWERTY12345
    ```
    
    
- **Zimbra**: Sử dụng postsuper -d như Postfix.

---
## 3.3  Ví Dụ cụ thể về Xử lý eMail bị treo

### Bước 1: Test gửi nhận email
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

### Bước 2: Đi tìm nguyên nhân

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



### Bước 3: Xử lý - Cấu hình Zimbra sử dụng SMTP Relay qua SendGrid

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
### Bước 4: Test Gửi/Nhận email ra ngoài
