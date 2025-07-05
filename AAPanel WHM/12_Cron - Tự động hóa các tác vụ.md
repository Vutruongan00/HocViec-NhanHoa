# Cron - Tự động hóa các tác vụ

- **Cron Jobs** trong aaPanel có chức năng chính như:
    - Sao lưu website, cơ sở dữ liệu, thư mục
    - Thực thi lệnh hoặc script định kỳ
    - Gửi yêu cầu đến URL API
    - Giải phóng bộ nhớ
    - Dọn dẹp hệ thống

- Bảng mô tả các chức năng của Task Scheduler (Cron Jobs) trong aaPanel:

| **Chức năng**    | **Mô tả**    |
| --------------- | ------------ |
| **Add Task**          | Tạo mới tác vụ định kỳ.                                                            |
| **All categories**    | Lọc theo loại tác vụ (VD: backup DB, script...).                                   |
| **Name**              | Tên của tác vụ định kỳ.                                                            |
| **Status**            | Trạng thái: Đang chạy (Running) hoặc Đã dừng (Stopped).                            |
| **Execute cycle**     | Chu kỳ chạy: Theo ngày, giờ, tuần, tháng hoặc mỗi vài giây.                        |
| **Number of Save**    | Số bản backup được giữ lại, quá số lượng sẽ bị xóa.                                |
| **Backup to**         | Vị trí lưu: ổ đĩa local, FTP, Google Drive, S3,... (cần cấu hình trong App Store). |
| **Last execute time** | Thời gian chạy gần nhất.                                                           |
| **Execute**           | Chạy tác vụ ngay lập tức.                                                          |
| **Edit**              | Chỉnh sửa tác vụ.                                                                  |
| **Log**               | Xem log thực thi tác vụ.                                                           |
| **Delete**            | Xóa tác vụ đã lên lịch.                                                            |

## 1. Add Task or Edit -- Cron Job

![image](https://github.com/user-attachments/assets/7782aed3-a353-48a5-bfce-e7927c24266e)
![image](https://github.com/user-attachments/assets/36eec75b-1927-4295-86bc-9543d71bfe89)

### Shell Script
- Thực thi lệnh hoặc script Linux định kỳ.
- Ví dụ: chạy script backup, xóa logs, cập nhật hệ thống.

### Backup Site
- Tự động sao lưu website.
- Chọn website cụ thể, thiết lập chu kỳ, nơi lưu trữ (Local, FTP, Google Drive, AWS S3…).

![image](https://github.com/user-attachments/assets/a6562605-892b-45fa-803d-8575c1a56e60)

### Backup Database
- Sao lưu cơ sở dữ liệu MySQL định kỳ.
- Chọn database cụ thể, thiết lập chu kỳ, nơi lưu trữ.

![image](https://github.com/user-attachments/assets/311023c0-0b59-42b5-a62f-9322d09df22a)

### Access URL
- Gửi yêu cầu HTTP đến một URL định kỳ.
- Dùng để gọi API hoặc kiểm tra trạng thái website.

![image](https://github.com/user-attachments/assets/620bfebd-c038-4030-a2c7-463d320b9d08)

### Backup Directory / Cut Log / Sync Time / Free RAM ...

### Execute cycle 
- Thiết lập chu kỳ thực thi: Hàng ngày/Mỗi N ngày; Hàng giờ/Mỗi N giờ; ...

![image](https://github.com/user-attachments/assets/2421d5d1-939b-410f-b40a-611044cbffec)

### Log -- Cron Job
- Kiểm tra nhật ký thực hiện tác vụ theo lịch trình và xem `execution results` dựa trên nhật ký.

![image](https://github.com/user-attachments/assets/f46c4fe0-d001-488b-a190-02ef957650c6)


## Task Scheduling - Lên lịch tác vụ
- Tự động thực hiện tác vụ tiếp theo dựa trên kết quả trả về của tác vụ

![image](https://github.com/user-attachments/assets/f747a1ee-e83d-42d5-a1e3-dff542a8d996)


## Script library
- Xem thư viện các tập lệnh đã tích hợp

![image](https://github.com/user-attachments/assets/581dc185-1f64-44a0-9e08-d3816745eff2)

- Tạo Script mới:

![image](https://github.com/user-attachments/assets/06304f1e-8fc8-43ea-825e-5a84fe29371d)
