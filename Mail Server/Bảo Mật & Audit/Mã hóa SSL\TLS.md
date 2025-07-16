> # Mã hóa (TLS/SSL) cho Zimbra Mail
> - Chuẩn bị SSL Certificate: self-signed,  (cho môi trường test), Let's Encrypt Certificate hoặc CA-signed (cho production).
> - Cài đặt Certificate vào Zimbra.
> - Kiểm tra các dịch vụ sau khi cài đặt.

# A. Tổng quan - TLS/SSL cho SMTP, IMAP, POP
- Zimbra hỗ trợ mã hóa bằng **STARTTLS** và **SSL/TLS** ở cả 3 giao thức:

| Giao thức      | Cổng không mã hóa | STARTTLS (mã hóa nâng cao) | SSL/TLS (mã hóa ngay từ đầu) |
| -------------- | ----------------- | -------------------------- | ---------------------------- |
| SMTP gửi mail  | 25                | 587 (STARTTLS)             | 465 (SMTPS)                  |
| IMAP nhận mail | 143               | 143 (STARTTLS)             | 993 (IMAPS)                  |
| POP3 nhận mail | 110               | 110 (STARTTLS)             | 995 (POP3S)                  |

---
# B. Hướng dẫn cài đặt chứng chỉ SSL/TLS cho Zimbra Mail Server

---
## 1. Cài đặt chứng chỉ SSL/TLS bằng Zimbra Administration Console

- **Bước 1: Truy Cập: Configure --> Certificates --> Install Certificate**

![image](https://github.com/user-attachments/assets/15005666-5691-42fc-9a4b-2d41c773b3d4)

![image](https://github.com/user-attachments/assets/5b738760-6efb-4769-bb09-71341ca6330c)

- Chọn máy chủ mục tiêu:

![image](https://github.com/user-attachments/assets/4d7eac3d-a38c-41d8-81cf-8ad2183f8cdd)

- **BƯỚC 2: Tùy chọn cài đặt**: 
    - Cài đặt **chứng chỉ tự ký** 
    - **Tạo CSR **để mua chứng chỉ thương mại từ CA ✅
        - Common Name:
        - Organization: Tên công ty 
        - City / State / Country
    - Cài đặt chứng chỉ thương mại (nếu đã có)
    ![image](https://github.com/user-attachments/assets/562e4238-8402-4e92-be4f-6eebfbb03d38)

    ![image](https://github.com/user-attachments/assets/f4c06470-b375-4e22-baf9-373964e819df)


- **BƯỚC 3: Dùng file .csr để cấp chứng chỉ**
    - Gửi .csr này lên ZeroSSL, BuyPass, hoặc nhà cung cấp SSL bất kỳ để cấp chứng chỉ
    - Sau khi xác minh domain, bạn sẽ được cung cấp:
        - certificate.crt
        - ca_bundle.crt
        - hoặc fullchain.pem)


- **BƯỚC 4: Tải chứng chỉ đã cấp vào Zimbra**
    - Quay lại **Admin Console** → **Configure** → **Certificates**
    - Chọn server → nhấn **Install Certificate** → **Install the commercial certificate**
    - Tải lên các file:
        - **Commercial Certificate** → `certificate.crt`
        - **Root CA Certificate Chain** → `ca_bundle.crt`
        - ![image](https://github.com/user-attachments/assets/55541721-bba6-4803-a75a-5f7bf8b40c4a)


    - **Install**

- **BƯỚC 5: Restart Zimbra** 
Cài đặt xong, khởi động lại Zimbra để áp dụng cấu hình:
```
zmcontrol restart
```

---



## 2. Cài đặt chứng chỉ SSL/TLS Dòng Lệnh CLI

> - Sử dụng chứng chỉ Let's Encrypt cho các dịch vụ SMTP, IMAPS, POP3S, Webmail (HTTPS),

### 🔧 BƯỚC 1: Cài đặt Certbot
```bash!
sudo apt update
sudo apt install certbot -y
```
- Dừng các dịch vụ của Zimbra để giải phóng port 80, 443
```bash
su - zimbra
zmcontrol stop
```

- Yêu cầu chứng chỉ Let's Encrypt bằng Certbot cho tên miền `mail.antvpro.io.vn`
```bash
sudo certbot certonly --standalone -d mail.antvpro.io.vn
```

- **Xác định vị trí các file chứng chỉ:** Certbot sẽ lưu chứng chỉ tại:
`/etc/letsencrypt/live/mail.antvpro.io.vn/`
    - `cert.pem`  : Chứng chỉ chính
	- `chain.pem ` : Chứng chỉ trung gian
	- `fullchain.pem` : Kết hợp cert.pem + chain.pem
	- `privkey.pem` : Khóa riêng tư
    
- **Khởi động lại** các dịch vụ của của Zimbra:
```bash!
su - zimbra
zmcontrol restart
```

### 🔧 BƯỚC 2: Copy  chứng chỉ và triển khai vào Zimbra
- Copy các file chứng chỉ sang thư mục của Zimbra đồng thời đổi tên file cho phù hợp:
    - `privkey.pem` --> `commercial.key`
    - `cert.pem`    --> `commercial.crt`
    - `chain.pem` --> `commercial_ca.crt`
```bash!
# Copy Private Key
cp /etc/letsencrypt/live/mail.antvpro.io.vn/privkey.pem /opt/zimbra/ssl/zimbra/commercial/commercial.key
# Copy Server Certificate
cp /etc/letsencrypt/live/mail.antvpro.io.vn/cert.pem /opt/zimbra/ssl/zimbra/commercial/commercial.crt
# Copy CA Chain (chuỗi trung gian)
cp /etc/letsencrypt/live/mail.antvpro.io.vn/chain.pem /opt/zimbra/ssl/zimbra/commercial/commercial_ca.crt
```

#### Lưu ý quan trọng:
- Để cấu hình được SSL, Zimbra yêu cầu một chuỗi đầy đủ **bao gồm cả chứng chỉ gốc và chứng chỉ trung gian** (chain.pem)
- Tải chứng chỉ gốc **ISRG Root X1** của Let's Encrypt từ trang web của hãng:
```bash
sudo wget -O /tmp/ISRG_Root_X1.pem https://letsencrypt.org/certs/isrgrootx1.pem
```
- Nối thêm giá trị của file `ISRG_Root_X1.pem` vào file `commercial_ca.crt`
```bash
sudo cat /tmp/ISRG_Root_X1.pem >> /opt/zimbra/ssl/zimbra/commercial/commercial_ca.crt
```

### 🔧 BƯỚC 3: Đặt quyền sở hữu và quyền hạn cho Zimbra (rất quan trọng)
- Khóa riêng tư (commercial.key) phải có quyền chặt chẽ (600 hoặc 640)
```bash!
sudo chmod 600 /opt/zimbra/ssl/zimbra/commercial/commercial.key
sudo chmod 644 /opt/zimbra/ssl/zimbra/commercial/commercial.crt
sudo chmod 644 /opt/zimbra/ssl/zimbra/commercial/commercial_ca.crt
```

### 🔧 BƯỚC 4: Triển khai chứng chỉ cho Zimbra

- Trước hết cần **kiểm tra sự khớp nối giữa các chứng chỉ**:
```bash!
# su - zimbra
# cd /opt/zimbra/ssl/zimbra/commercial

/opt/zimbra/bin/zmcertmgr verifycrt comm commercial.key commercial.crt commercial_ca.crt
```
--> Output trả về như này là OKE
```lua!
zimbra@mail:~/ssl/zimbra/commercial$ cd /opt/zimbra/ssl/zimbra/commercial
zimbra@mail:~/ssl/zimbra/commercial$ /opt/zimbra/bin/zmcertmgr verifycrt comm commercial.key commercial.crt commercial_ca.crt
** Verifying 'commercial.crt' against 'commercial.key'
Certificate 'commercial.crt' and private key 'commercial.key' match.
** Verifying 'commercial.crt' against 'commercial_ca.crt'
Valid certificate chain: commercial.crt: OK
```
![image](https://github.com/user-attachments/assets/072d1354-cd1b-4685-b3b9-042a36ef5bdc)

- **DEPLOY chứng chỉ:** bằng  `zmcertmgr`

    ```bash
    /opt/zimbra/bin/zmcertmgr deploycrt comm commercial.crt commercial_ca.crt
    ```
    - Output:
    ![image](https://github.com/user-attachments/assets/6de1b5fb-3dea-49bf-b5ce-6913cdeebf26)


- Kiểm tra truy cập Webmail Zimbra: https://mai.antvpro.io.vn

### BƯỚC 5 (tuỳ chọn): Tự động gia hạn

---
## 3. Cài đặt SSL SAN  với Let's Encrypt cho Zimbra Multi-Domain
- **Giả định có thêm 2 domain:**  `mail2.antvpro.io.vn` và `mail3.antvpro.io.vn`
- **Tạo domain:**
```
zmprov cd mail2.antvpro.io.vn
zmprov cd mail3.antvpro.io.vn
```
- Kiểm tra:
```
zmprov -l gad
```
<img width="352" height="119" alt="image" src="https://github.com/user-attachments/assets/2de511c7-5ca4-4d01-993a-4cf4b8e11231" />

- **Tạo User trong domain (Phải có ít nhất 1 user)**
```
zmprov ca user1@antvpro.io.vn 123456
zmprov ca user2@mail2.antvpro.io.vn 123456
zmprov ca user3@mail3.antvpro.io.vn 123456
```
- **Gán Virtual Host cho domain:** (Cái này quan trọng, đừng nên bỏ qua)
> Zimbra dùng zimbraVirtualHostName để biết domain nào sẽ có server_name riêng trong file cấu hình NGINX.
```bash
zmprov md mail2.antvpro.io.vn zimbraVirtualHostName mail2.antvpro.io.vn
zmprov md mail3.antvpro.io.vn zimbraVirtualHostName mail3.antvpro.io.vn
```

- **Chạy certbot để cấp SSL SAN Multi-domain:**
```
sudo certbot certonly --standalone \
-d mail.antvpro.io.vn \
-d mail2.antvpro.io.vn \
-d mail3.antvpro.io.vn
```
- **Các bước Cài chứng chỉ vào Zimbra và Deploy chứng chỉ (Tương tự phần 2)** 

- **Restart Zimbra để sinh lại nginx config:**
```
zmcontrol restart
```

- Kiểm tra lại truy cập Webmail
	* [https://mail.antvpro.io.vn](https://mail.antvpro.io.vn)
	* [https://mail2.antvpro.io.vn](https://mail2.antvpro.io.vn)
	* [https://mail3.antvpro.io.vn](https://mail3.antvpro.io.vn)

<img width="1025" height="573" alt="image" src="https://github.com/user-attachments/assets/ce8afc34-8bb4-475d-882d-e08d83d705a4" />


---

# C. Kiểm tra bằng openssl s_client, testssl.sh.

## 1. Kiểm tra bằng `openssl s_client`
- **Kiểm tra SMTPS (Gửi email an toàn - Cổng 465):**
```bash!
openssl s_client -connect mail.antvpro.io.vn:465 -servername mail.antvpro.io.vn
```

- **SMTP (TLS trên port 587 - STARTTLS):**
```bash!
openssl s_client -connect mail.antvpro.io.vn:587 -starttls smtp
```
**--> Kết quả:** thấy `Verify return code: 0 (ok)` --> Chứng chỉ được xác minh thành công và được tin cậy

![image](https://github.com/user-attachments/assets/c7b3284d-f9c7-492e-957c-8839cede4866)

- **Kiểm tra IMAPS (Nhận email an toàn - Cổng 993):**
```bash!
openssl s_client -connect mail.antvpro.io.vn:993 -servername mail.antvpro.io.vn
```
- **Kiểm tra POP3S (Nhận email an toàn - Cổng 995):**
```bash!
openssl s_client -connect mail.antvpro.io.vn:995 -servername mail.antvpro.io.vn
```

## 2. Kiểm tra bằng `testssl.sh`
> **testssl.sh** là một script bash mạnh mẽ và toàn diện để kiểm tra cấu hình TLS/SSL của máy chủ trên bất kỳ cổng nào. Nó sẽ kiểm tra nhiều khía cạnh như chứng chỉ, giao thức được hỗ trợ, bộ mật mã, lỗ hổng (ví dụ: Heartbleed, POODLE, ROBOT), v.v.

- Cài đặt **testssl.sh**
```bash!
git clone https://github.com/drwetter/testssl.sh.git
cd testssl.sh

./testssl.sh mail.antvpro.io.vn:465
./testssl.sh mail.antvpro.io.vn:993
...
```






