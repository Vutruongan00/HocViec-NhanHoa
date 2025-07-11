
># Hướng Dẫn Sử Dụng

# 1. Tạo chữ ký (Signature)
[Tham khảo tại đây](https://github.com/Vutruongan00/HocViec-NhanHoa/blob/main/Mail%20Server/Zimbra/HDSD%20c%C6%A1%20b%E1%BA%A3n%20ZIMBRA.md#3-thi%E1%BA%BFt-l%E1%BA%ADp-ch%E1%BB%AF-k%C3%BD-email-zimbra)

# 2. Thiết lập forward email Zimbra
> Thiết lập **forward email trong Zimbra** giúp người dùng tự động chuyển tiếp thư đến một địa chỉ email khác. Đây là một tính năng cực kỳ hữu ích trong nhiều tình huống:
> 
> - **Tiết kiệm thời gian**: người dùng không cần đăng nhập vào nhiều tài khoản để kiểm tra thư, chỉ cần một hộp thư trung tâm là đủ.
> - **Chia sẻ thông tin nhanh chóng**: Email có thể được chuyển tiếp đến đồng nghiệp hoặc nhóm làm việc mà không cần soạn lại nội dung.
> - **Giữ liên lạc khi thay đổi địa chỉ email**: Nếu bạn đổi tên miền hoặc nhân viên nghỉ việc, forward email giúp đảm bảo không bỏ lỡ thư quan trọng.
> - **Lưu trữ và theo dõi**: Bạn có thể forward email đến một địa chỉ khác để lưu trữ hoặc theo dõi sau này.

..

```
zmprov ma tenuser@yourdomain.com zimbraMailForwardingAddress emailkhac@example.com
```
- #Hoặc:
![image](https://github.com/user-attachments/assets/124d481e-19e6-4188-840a-4f4f04856f03)

# 3. Lọc thư (Filters)
1. **Đăng nhập** vào Zimbra Webmail.
2. Nhấn vào tab **Preferences (Tùy chọn)**.
3. Chọn mục **Filters** (Bộ lọc) trong menu bên trái.
4. Nhấn vào nút **Create Filter** (Bộ lọc mới).

<img width="958" height="728" alt="image" src="https://github.com/user-attachments/assets/d890eed8-498f-4104-8fce-bd45d987a009" />

5. **Đặt tên** cho bộ lọc
6. Trong phần "**of the following conditions are met**" (Với thư đến đáp ứng):
    - Chọn `Any` (bất kỳ điều kiện nào) hoặc 
    - `All` (tất cả các điều kiện).
    
<img width="590" height="429" alt="image" src="https://github.com/user-attachments/assets/7636cb78-1361-4686-b9fc-91fa9825e2db" />

- Thêm các điều kiện lọc: Nhấn + để thêm dòng điều kiện.


7. Trong phần "**Perform the following actions**" (Thực hiện các hành động sau):

- Nhấn + để thêm hành động. Ví dụ:

    <img width="672" height="441" alt="image" src="https://github.com/user-attachments/assets/eada2d20-b7ad-46e7-8541-6e762f081c38" />

    - `File into` (Di chuyển vào): Chọn một thư mục (Folder) có sẵn hoặc tạo thư mục mới.
    - `Discard` (Bỏ qua): Xóa thư.
    - `Mark as Read` (Đánh dấu đã đọc): Tự động đánh dấu thư là đã đọc.
    - `Tag with` (Gắn thẻ với): Gắn một thẻ (Tag) cụ thể.

8. Nhấn **OK** để lưu bộ lọc. 


# 4. Thiết lập trả lời tự động (Out-of-office reply)

1. **Đăng nhập** vào Zimbra Webmail.
2. Nhấn vào tab **Preferences (Tùy chọn)**.
3. Chọn mục **Out of Office (Vắng mặt)** trong menu bên trái.
4. **Đánh dấu chọn "Send auto-reply messages" (Gửi tin nhắn trả lời tự động).**
5. **Nhập nội dung tin nhắn** trả lời tự động vào ô soạn thảo.
6. **Cấu hình thời gian (tùy chọn):**

   * **"Start Date" và "End Date":** Đặt ngày bắt đầu và ngày kết thúc cho chế độ trả lời tự động. Sau ngày kết thúc, tính năng sẽ tự động tắt.
7. **Cấu hình tần suất (tùy chọn):**

   * **"Send a reply message every":** Đặt số ngày để gửi lại tin nhắn cho cùng một người gửi (ví dụ: mỗi 7 ngày). Điều này giúp tránh gửi quá nhiều tin nhắn tự động đến cùng một người nếu họ gửi nhiều email cho bạn.
8. Nhấn **Save (Lưu)**.

<img width="922" height="632" alt="image" src="https://github.com/user-attachments/assets/8a42c513-212d-480e-9382-be69ef6cf184" />


# 5. Quota Mailbox (Hạn mức hộp thư)

**Quota Mailbox** là giới hạn dung lượng lưu trữ mà người dùng được cấp cho hộp thư email của mình.

* **Dung lượng:** Mỗi tài khoản email có một dung lượng lưu trữ nhất định (ví dụ: 5GB, 10GB). Dung lượng này bao gồm tất cả email, file đính kèm, danh bạ, và lịch lưu trữ trong hộp thư của người dùng
* **Mục đích:** Hạn mức này được đặt ra để:

  * Đảm bảo hiệu suất hoạt động của hệ thống email.
  * Ngăn chặn một người dùng chiếm quá nhiều dung lượng máy chủ.
  * Khuyến khích người dùng quản lý hộp thư của mình hiệu quả (xóa thư không cần thiết, lưu trữ file lớn ở nơi khác).
  
* **Khi gần đạt hoặc vượt Quota:**

  * Người dùng sẽ nhận được thông báo từ hệ thống khi hộp thư của họ gần đạt đến giới hạn (ví dụ: 80% hoặc 90% dung lượng đã sử dụng).
  * Nếu hộp thư **vượt quá hạn mức**, Người dùng có thể **không gửi hoặc nhận được email mới nữa**. Email gửi đến có thể bị trả lại cho người gửi với thông báo lỗi "mailbox full" (hộp thư đầy).
  
* **Cách quản lý Quota:**

  * **Xóa email cũ hoặc không cần thiết:** Đặc biệt là những email có file đính kèm lớn. Nhớ xóa cả trong thư mục "Trash" (Thùng rác) để giải phóng dung lượng thực sự.
  * **Lưu trữ file đính kèm lớn:** Tải các file đính kèm quan trọng về máy tính hoặc lưu trữ trên các dịch vụ đám mây khác (Google Drive, OneDrive, Dropbox) thay vì để trong hộp thư.
  * **Liên hệ quản trị viên:** Nếu bạn thường xuyên hết dung lượng và cần nhiều hơn, hãy liên hệ với quản trị viên hệ thống để yêu cầu tăng hạn mức (nếu chính sách của tổ chức cho phép).

## Cách chỉnh quota account email trong Zimbra

1. **Trang chủ -->> Manage Account (Quản lý tài khoản)**

<img width="1844" height="840" alt="image" src="https://github.com/user-attachments/assets/028478df-2261-406b-96fb-e30651fdef8d" />

2. **Lựa chọn account muốn thực hiện chỉnh sửa quota 
--> Nhấp chuột phải --> Edit (sửa)

<img width="1023" height="498" alt="image" src="https://github.com/user-attachments/assets/cc701e66-854f-4ab1-b388-391703b05e24" />

3. Click **Advanced --> Qouta**

<img width="1214" height="865" alt="image" src="https://github.com/user-attachments/assets/57898962-5365-4815-a509-c6d076fd7aa2" />

- Tùy chỉnh các than số:
    - Giới hạn địa chỉ chuyển tiếp theo người dùng trong phạm vi (ký tự)
    - Số lượng tối đa các địa chỉ chuyển tiếp theo người dùng
    - Hạn mức tài khoản (MB) (0 là không có giới hạn)
    - Số lượng liên hệ tối đa cho phép trong thư mục
    - Ngưỡng phần trăm cho các thư cảnh báo hạn mức (%)
    - Khoảng thời gian tối thiểu giữa các cảnh báo hạn mức
    - Mẫu thư cảnh báo hạn mức

4. **SAVE để lưu các chỉnh sửa**
