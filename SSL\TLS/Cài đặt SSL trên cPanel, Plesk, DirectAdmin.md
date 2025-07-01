
## Cài đặt SSL trên CPanel

[Hướng dẫn cài đặt tham khảo tại đây](https://wiki.nhanhoa.com/kb/huong-dan-cai-ssl-tren-hosting-cpanel/)
### Cài đặt CPanel 
```bash!
#cài đặt gói cần thiết
apt -y install perl perl-base
#Chạy script cài đặt
cd /home && curl -o latest -L https://securedownloads.cpanel.net/latest && sh latest
```
- Cài đặt hoàn tất
![image](https://github.com/user-attachments/assets/87fbc26a-36c0-4990-a4b8-2b3513389304)

- Đăng ký gói dùng thử và Truy cập giao diện quản trị admin WHM
![image](https://github.com/user-attachments/assets/5c79bb8e-09a1-4f82-9118-f2143de14ce2)

- Tạo Account cho Client: 
`Account Functions` --> `Create a New Account`
![image](https://github.com/user-attachments/assets/9b1fda0e-9203-43de-9e89-b0b0207178e7)

- Điền thông tin: `pj8gvycyG9YN%@x{` --> Chọn `Create` để tạo tk
![image](https://github.com/user-attachments/assets/5c7a0e2d-db92-40af-adb1-c57b22fe65d6)

![image](https://github.com/user-attachments/assets/a3708439-26b4-42ea-b774-2648df2cb44b)

- Đăng nhập user vừa tạo vào Cpanel để cấu hình SSL  https://35.194.238.59:2083/
- Tạo website:
![image](https://github.com/user-attachments/assets/3af42a10-8210-4ae7-8803-033307de5f56)

- Upload file Source code vào thư mục `Public_html`
![image](https://github.com/user-attachments/assets/f97d4c33-5b76-4b0f-b6f2-62d0ad08bbb2)

### Cài đặt SSL

![image](https://github.com/user-attachments/assets/cd95c1f2-62a5-4697-a491-befb777a0c79)

- Tùy chọn tạo/upload chứng chỉ(tôi chọn upload)
![image](https://github.com/user-attachments/assets/18e66972-4db5-44cd-88cd-b31ab0f67f83)

![image](https://github.com/user-attachments/assets/62dba514-f6cf-4a2c-99a4-070751013365)

- Install:
![image](https://github.com/user-attachments/assets/33638b55-61ac-4f62-b0aa-ebe5bba7fae5)

- Bổ sung Private key và Install Certificate

![image](https://github.com/user-attachments/assets/b622f410-83fa-4c62-9f52-d763a7a7e517)

- Thành công: 

![image](https://github.com/user-attachments/assets/de97ca3e-b1a9-4eb4-a52e-98d897249369)

- Kiểm tra truy cập web: https://antvpro.io.vn
![image](https://github.com/user-attachments/assets/89395494-6407-46ce-85eb-b312cd615420)




---
## Cài đặt SSL trên Plesk
Cài đặt Plesk [Tham khảo tại đây](https://kdata.vn/cam-nang/cai-dat-plesk-tren-ubuntu-2204-install-plesk-on-ubuntu-2204)

- Đăng nhập trang quản trị Plesk bằng tài khoản Admin:

![image](https://github.com/user-attachments/assets/66e9b03e-574d-4a05-a648-10c8ccfe181a)

- Add Customer:
![image](https://github.com/user-attachments/assets/c8a55ec6-c7e4-43d2-8ce2-1b6e03ea0fb3)

- Điền thông tin của khách hàng: (thông tin liên lạc, thông tin đăng nhập, và tên domain của khách,...)
- Chọn Service plan (cấu hình Service plan
tùy thuộc vào nhu cầu của Customer hoặc Reseller)

![image](https://github.com/user-attachments/assets/7b93e48e-ab71-444e-9779-1a365d074216)

![image](https://github.com/user-attachments/assets/75a54844-0e8a-40dd-9237-5bdf10ba4a7d)

- Đăng nhập bằng user1 để quản trị tên miền (Hoặc truy câp thẳng từ admin)
![image](https://github.com/user-attachments/assets/0c0e56f6-2ead-4b4f-9425-d826dcaf38ae)


- Cuộn xuống dưới và chọn 1 trong 3 cách để cài chứng chỉ SSL cho domain
- Giả sử tôi có 3 chứng chỉ:
    - certificate.crt
    - ca_bundle.crt
    - antvpro.key
![image](https://github.com/user-attachments/assets/ab737731-82f1-45c5-b2ab-27190804838b)

- Gộp 3 file thành 1 file `fullchain.pem`
```bash!
#Linux
cat antvpro.key certificate.crt ca_bundle.crt > fullchain.pem

#Windows
#type antvpro.key certificate.crt ca_bundle.crt > fullchain.pem
```
-  Chọn **`Upload .pem file`** 
- Kiểm tra 
![image](https://github.com/user-attachments/assets/503c8823-e611-42fe-a175-e9981507d5d1)





---

## Cài đặt SSL trên DirectAdmin

[Hướng dẫn cài đặt tham khảo tại đây](https://wiki.nhanhoa.com/kb/huong-dan-cai-dat-ssl-free-tren-directadmin/)


--- 

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
   - **Lợi ích:** Tiết kiệm chi phí và đơn giản hóa việc quản lý khi có nhiều tên miền phụ. Chỉ cần một chứng chỉ duy nhất cho tất cả chúng.
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
   - **Lợi ích:** Cực kỳ linh hoạt, lý tưởng cho các công ty có nhiều thương hiệu, nhiều trang web riêng biệt, hoặc các dịch vụ cần bảo mật nhiều tên miền trên cùng một máy chủ (hoặc thậm chí trên các máy chủ khác nhau nếu có thể quản lý việc cài đặt chứng chỉ).


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


