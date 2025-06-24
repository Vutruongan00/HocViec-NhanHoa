# SỬ DỤNG

## 1. Khởi tạo User
```bash!
su - zimbra
zmprov ca tenuser@yourdomain.com matkhau123 displayName "Tên Người Dùng"
```
- #Hoặc
![image](https://github.com/user-attachments/assets/8399b737-4aa3-4f23-ad47-d11b31730148)

![image](https://github.com/user-attachments/assets/d8b8ef9d-110f-4595-86c3-20d2acfad111)
- Cấu hình tên, username và mật khẩu

![image](https://github.com/user-attachments/assets/59c46993-8c34-4c49-8e34-ebfad9b57621)

![image](https://github.com/user-attachments/assets/9a2d3b08-b6b4-4475-8712-a25b374bfb3e)

- Kiểm tra bằng cách đăng nhập user/pass trên zimbra webmail http://192.168.137.140

![image](https://github.com/user-attachments/assets/c53a9440-856d-4dcc-b97c-3e54cb4a8728)

## 2. Thiết lập chính sách về mật khẩu account email
- Cấu hình chính sách chung cho tất cả tài khoản:
```bash!
su - zimbra

zmprov mcf zimbraPasswordMinLength 8
zmprov mcf zimbraPasswordMinUpperCaseChars 1
zmprov mcf zimbraPasswordMinLowerCaseChars 1
zmprov mcf zimbraPasswordMinNumericChars 1
zmprov mcf zimbraPasswordMinPunctuationChars 1
zmprov mcf zimbraPasswordLockoutEnabled TRUE
```
- Kiểm tra lại:
```bash!
zmprov gacf | grep zimbraPassword
```
- #Hoặc
![image](https://github.com/user-attachments/assets/1ab1aa3d-abed-453a-9dff-9f035ce69973)

![image](https://github.com/user-attachments/assets/14b6c687-6203-4c0f-acbf-cb8eef9d12ea)

## 3. Thiết lập chữ ký email Zimbra
```bash!
zmprov ma user1@antv.com zimbraPrefMailSignatureEnabled TRUE
zmprov ma user1@antv.com zimbraPrefMailSignature "Thankiuu, \nVu Truong An"
```
- Kiểm tra chữ ký:
```bash!
zmprov ga user1@antv.com zimbraPrefMailSignature
```
- #Hoặc
![image](https://github.com/user-attachments/assets/fe00d806-6397-49ad-abf9-9c9be076df83)

## 4. Thiết lập forward email Zimbra
> Thiết lập **forward email trong Zimbra** giúp bạn tự động chuyển tiếp thư đến một địa chỉ email khác. Đây là một tính năng cực kỳ hữu ích trong nhiều tình huống:
> 
> - **Tiết kiệm thời gian**: Bạn không cần đăng nhập vào nhiều tài khoản để kiểm tra thư, chỉ cần một hộp thư trung tâm là đủ.
> - **Chia sẻ thông tin nhanh chóng**: Email có thể được chuyển tiếp đến đồng nghiệp hoặc nhóm làm việc mà không cần soạn lại nội dung.
> - **Giữ liên lạc khi thay đổi địa chỉ email**: Nếu bạn đổi tên miền hoặc nhân viên nghỉ việc, forward email giúp đảm bảo không bỏ lỡ thư quan trọng.
> - **Lưu trữ và theo dõi**: Bạn có thể forward email đến một địa chỉ khác để lưu trữ hoặc theo dõi sau này.

..

```
zmprov ma tenuser@yourdomain.com zimbraMailForwardingAddress emailkhac@example.com
```
- #Hoặc:
![image](https://github.com/user-attachments/assets/124d481e-19e6-4188-840a-4f4f04856f03)


## 5. Tìm ID mailbox account trong email Zimbra
```
zmprov gmi user1@antv.com
```
![image](https://github.com/user-attachments/assets/bddd7902-e76b-4492-90b5-279224c5a342)

## 6. Đổi mật khẩu account admin Zimbra
- Kiểm tra user đang có quyền admin:
```z
zmprov gaaa
```
- Đổi mật khẩu admin:
```z!
zmprov sp admin@yourdomain.com matkhaumoi123@
```

## 7. Kiểm tra log gửi/nhận email Zimbra (đặc biệt chú ý)

- **Test gửi/nhận email Zimbra:**
- 
![image](https://github.com/user-attachments/assets/ccb14be7-a9e7-4bf9-900a-a1240589e640)

- **Kiểm tra log** gửi/nhận tại máy chủ:
```
sudo tail -f /var/log/zimbra.log
```
![image](https://github.com/user-attachments/assets/979fcb28-22f0-492f-8b63-9f15e308a100)

> - Khi Hệ thống `Postfix` sẵn sàng & Có kết nối mới, `postfix/postscreen` sẽ có nhiệm vụ sàng lọc các kết nối rác và độc hại trước khi chúng đến được quy trình xử lý mail chính.
> - Server xử lý bước đầu xong sẽ gửi mai và chuyển tiếp qua bộ lọc `smtp-amavis` đang chạy trên `127.0.0.1` (localhost) tại cổng `10026` để xử lý thêm
> - `Amavis` tiếp nhận và bắt đầu kiểm tra ( *Checking* ), `Amavis` sẽ gọi `ClamAV` để quét virus
> - `ClamAV` kiểm tra Virus --> OK --> trả lại mail cho `Amavis`
> - Amavis xác nhận email an toàn `*Passed CLEAN*` --> trả lại cho `Postfix` 
> - `Postfix` thêm email vào hàng đợi và gửi mail vào hòm thư - Giao hàng thành công cho `user2` (`status=sent (250 2.0.0 OK ...)`)


- Hoặc lọc log theo từ khóa:
```bash!
grep 'status=sent' /var/log/zimbra.log          # Email gửi thành công
grep 'status=bounced' /var/log/zimbra.log       # Email bị lỗi
```

## Thay đổi logo trong Zimbra

> - Logo trước khi đăng nhập (440×60 px)
![image](https://github.com/user-attachments/assets/99815055-b8de-47e9-823a-de7f761ff417)
> - Logo sau khi đăng nhập:
![image](https://github.com/user-attachments/assets/96ef91ac-a71b-4568-ae02-a668fcc51e7a)




- Thay logo trong thư mục này: 
```
/opt/zimbra/jetty/webapps/zimbra/skins/_base/logos
```
![image](https://github.com/user-attachments/assets/9cdf01eb-cb9f-46d0-ae9c-1b04419678a4)

-->
![image](https://github.com/user-attachments/assets/31cde9b6-6990-4feb-bfec-d6f6c3bc10d8)



