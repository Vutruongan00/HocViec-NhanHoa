# Quản lý FTP trên aaPanel

- Các chức năng chính trong FTP_aaPanel

| **Chức năng**    | **Mô tả**   |
| --------------- | ----------------- |
| **Add FTP**          | Tạo một tài khoản FTP mới. Bạn cần nhập tên người dùng, mật khẩu, thư mục gán và các tùy chọn liên quan.                        |
| **Change FTP Port**  | Thay đổi **cổng kết nối** của dịch vụ FTP (mặc định là `21`). Hữu ích nếu bạn muốn tăng bảo mật hoặc tránh xung đột cổng.       |
| **FTP log analysis** | Phân tích nhật ký hoạt động của dịch vụ FTP: đăng nhập, tải lên, tải xuống, lỗi...                                              |
| **Username**         | Tên người dùng của tài khoản FTP. Dùng để đăng nhập từ FileZilla, WinSCP...                                                     |
| **Password**         | Mật khẩu đăng nhập của tài khoản FTP tương ứng.                                                                                 |
| **Status**           | Trạng thái của tài khoản FTP: **Bật (Enable)** hoặc **Tắt (Disable)**. Có thể dùng để khóa tạm thời mà không cần xóa tài khoản. |
| **Document Root**    | Đường dẫn thư mục gốc được gán cho tài khoản FTP.           |
| **Quota**            | Dung lượng tối đa mà tài khoản FTP được phép sử dụng (chức năng này **chỉ hoạt động khi hệ thống dùng định dạng ổ đĩa XFS**).   |
| **Note**             | Ghi chú về tài khoản FTP, ví dụ: dùng để làm gì, cho ai, v.v.                                                                   |
| **Set Path**         | Đổi thư mục gốc của tài khoản FTP. Có thể dùng để thay đổi thư mục truy cập mà không cần xóa tài khoản.                         |
| **CHG PW**           | Thay đổi mật khẩu cho tài khoản FTP hiện tại.                                                                                   |
| **Log**    | Xem nhật ký hoạt động (log) của tài khoản FTP đó, bao gồm các hành vi truy cập, tải lên/tải xuống.                              |
| **Delete**           | Xoá hoàn toàn tài khoản FTP khỏi hệ thống. Không thể khôi phục sau khi xóa.  |

## Add FTP

- Nhập `Username`,  `Password`, Document Root

![image](https://github.com/user-attachments/assets/b928c2e3-2a85-454c-87f1-ccbf76fe3771)

## Change FTP Port
- Cổng dịch vụ FTP mặc định là 21, nếu đổi cổng chú ý mở tường lửa
  
![image](https://github.com/user-attachments/assets/100bfadc-b40e-46b0-ab3d-49cdc6c691df)

## FTP log analysis (pro)

## FTP service management
- Quản lý dịch vụ FTP (stop,start,reload),
- Cấu hình, sửa đổi
- Quản lý log

![image](https://github.com/user-attachments/assets/5d8c8169-e9e1-4bf5-90b5-04875da698df)

## Quota

![image](https://github.com/user-attachments/assets/1080f084-509d-4efc-8dbc-73b77e8762e6)
- Chức năng này cho phép bạn thiết lập giới hạn dung lượng lưu trữ (quota) cho một người dùng FTP cụ thể
    - `Current used capacity`: Dung lượng đã sử dụng hiện tại của người dùng FTP
    - `Current capacity quota`: Hạn mức dung lượng hiện tại đã được đặt cho người dùng FTP


## Set Path - Cấu hình đường dẫn

![image](https://github.com/user-attachments/assets/9a843df4-7742-4574-b958-6775f123bfd5)

- `Username ` : Tên người dùng của thư mục cần sửa đổi
- `Home`  : Enter hoặc select một thư mục mới
- `Migrate`  : Sao chép dữ liệu vào thư mục mới, dữ liệu thư mục cũ sẽ không bị xóa

![image](https://github.com/user-attachments/assets/a8720850-b75e-4c67-9f41-254c4f87fb99)

## Password - đổi mật khẩu

![image](https://github.com/user-attachments/assets/211f5473-a450-4b0d-ade3-4c6c1f60c642)
