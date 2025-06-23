# SỬ DỤNG

## 1. Khởi tạo User
```bash!
su - zimbra
zmprov ca tenuser@yourdomain.com matkhau123 displayName "Tên Người Dùng"
```
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
- Hoặc thiết lập trên giao diện Web Mail
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

![image](https://github.com/user-attachments/assets/124d481e-19e6-4188-840a-4f4f04856f03)

