# Monitor - Giám sát hệ thống

![image](https://github.com/user-attachments/assets/37130dd3-9672-42c1-82f7-5ae818c437cc)

- Chức năng **Monitor** trong aaPanel dùng để giám sát hệ thống và gửi cảnh báo khi có sự cố. 
- Theo dõi **Load Average**, **CPU**, **RAM**, **Disk** , **Network**...

## Các biển đồ monitor 
- Với mỗi biểu đồ aaPanel cung cấp tính năng có thể view trạng thái theo ngày: hôm nay, hôm qua, 7 ngày gần nhất hoặc theo thời gian cụ thể 

![image](https://github.com/user-attachments/assets/940609a9-a280-40c8-877b-af72a01c6e6a)

- Có thể làm mới dữ liệu thủ công bằng icon refresh 

![image](https://github.com/user-attachments/assets/94a2caa5-0e4e-4e26-acd2-c77cee48bea6)

- Di chuột vào biểu đồ sẽ hiện pop-up thông tin chi tiết về giá trị tại từng thời điểm sẽ hiện lên. 

![image](https://github.com/user-attachments/assets/e78bf8c9-d90a-4d7c-84a3-110ae410d3ef)

- Với mỗi biểu đồ aaPanel cung cấp 1 thanh time-line giúp bạn tuỳ biến khoảng thời gian cần giám sát 

![image](https://github.com/user-attachments/assets/a1e720b9-a394-49d9-b63a-4576fc6bdb2d)


### Load Average
- Đây là biểu đồ thể hiện trạng thái tải của server 

![image](https://github.com/user-attachments/assets/9aa24fcf-4bef-4734-92a3-a172c3e46a48)

- Gồm 2 biểu đồ nhỏ 
	- System resource usage: thể hiện xu hướng sử dụng tài nguyên hệ thống 
![image](https://github.com/user-attachments/assets/4fc30e79-44fb-4946-8828-8019a40d79bc)

  - Khi rê chuột vào thời điểm cụ thể, hệ thống hiển thị danh sách các tiến trình đang chạy lúc đó, bao gồm:
			- PID: Mã định danh tiến trình.
			- Process name: Tên tiến trình.
			- CPU usage: Mức sử dụng CPU của tiến trình.
			- Startup user: Người dùng khởi chạy tiến trình.
		- Giao diện này giúp bạn:
			- Theo dõi mức tải hệ thống theo thời gian thực.
			- Xác định thời điểm hệ thống bị quá tải.
			- Kiểm tra tiến trình nào đang tiêu tốn tài nguyên tại thời điểm cụ thể.
	- Load Details: thể hiển mức tải trung bình của hệ thống trong 1 phút, 5 phút và 15 phút.
		- ![image](https://github.com/user-attachments/assets/2b22b9ff-6730-46ef-afc4-5785237723e2)

		- Mỗi đường màu đại diện cho một khoảng thời gian:
			- Màu xanh dương: 1 phút
			- Màu xanh lá: 5 phút
			- Màu tím: 15 phút
		- Khi rê chuột vào thời điểm cụ thể, hệ thống hiển thị bảng các tiến trình đang hoạt động tại thời điểm đó, gồm:
			- PID: Mã định danh tiến trình
			- Process name: Tên tiến trình
			- CPU usage: Mức sử dụng CPU
			- Startup user: Người dùng khởi chạy tiến trình
		- Biểu đồ giúp bạn theo dõi xu hướng tải hệ thống theo thời gian và phát hiện các thời điểm bất thường.

### CPU 
- Biểu đồ giám sát mức sử dụng CPU theo thời gian 

![image](https://github.com/user-attachments/assets/16df6e64-72b9-41ad-828a-03aa2e6c6357)

- Hiển thị mức sử dụng CPU theo từng thời điểm, giúp bạn quan sát xu hướng tăng giảm.
- Khi rê chuột vào thời điểm cụ thể, hệ thống hiển thị danh sách các tiến trình đang chạy và mức sử dụng CPU của từng tiến trình:
	- PID: Mã định danh tiến trình
	- Process name: Tên tiến trình
	- CPU usage: Mức sử dụng CPU
	- Startup user: Người dùng khởi chạy tiến trình
- Giao diện này giúp bạn:
	- Theo dõi mức sử dụng CPU theo thời gian thực.
	- Phát hiện các thời điểm hệ thống bị quá tải.
	- Xác định tiến trình nào đang tiêu tốn tài nguyên tại thời điểm cụ thể.

### RAM 
- Biểu đồ giám sát mức sử dụng RAM theo thời gian 

![image](https://github.com/user-attachments/assets/b75e38d1-c786-4ea2-a51c-e7f007e3ab6f)

- Hiển thị mức sử dụng RAM theo từng thời điểm, giúp bạn quan sát xu hướng tăng giảm.
- Khi rê chuột vào thời điểm cụ thể, hệ thống hiển thị danh sách các tiến trình đang chạy và mức sử dụng RAM của từng tiến trình:
	- PID: Mã định danh tiến trình
	- Process name: Tên tiến trình
	- Memory Usage: Mức sử dụng CPU
	- Startup user: Người dùng khởi chạy tiến trình
- Giao diện này giúp bạn:
	- Theo dõi mức sử dụng RAM  theo thời gian thực.
	- Giúp phát hiện tiến trình ngốn bộ nhớ bất thường.
	- Hỗ trợ chẩn đoán nguyên nhân gây chậm hệ thống hoặc tràn RAM.
	- Cung cấp dữ liệu để tối ưu hóa cấu hình dịch vụ hoặc nâng cấp tài nguyên.

### Disk I/O 
![image](https://github.com/user-attachments/assets/e786bc49-547d-4373-aeee-96fa524e10fb)

- Biểu đồ giám sát hoạt động đọc/ghi ổ đĩa (Disk I/O) trong aaPanel.
- Nó cho phép bạn theo dõi mức độ sử dụng ổ cứng theo thời gian và xác định các tiến trình đang tác động đến đĩa tại từng thời điểm cụ thể.
- Biểu đồ Disk I/O: Hiển thị tốc độ đọc (read) và ghi (write) dữ liệu trên ổ đĩa theo thời gian. Mỗi loại hoạt động được biểu diễn bằng màu riêng biệt (ví dụ: đỏ cho đọc, xanh cho ghi).
- Khi rê chuột vào thời điểm cụ thể, hệ thống hiển thị:
	- Tốc độ đọc và ghi tại thời điểm đó.
	- Số lần đọc/ghi mỗi giây và thời gian phản hồi trung bình.
	- Danh sách các tiến trình đang sử dụng ổ đĩa, kèm theo lượng dữ liệu đọc/ghi và người dùng khởi chạy.
- Giao diện này: 
	- Giúp phát hiện tiến trình gây tải đĩa cao.
	- Hỗ trợ chẩn đoán nguyên nhân hệ thống phản hồi chậm do I/O.
	- Cung cấp dữ liệu để tối ưu hóa dịch vụ hoặc phân bổ lại tài nguyên lưu trữ.

### Network I/O 
![image](https://github.com/user-attachments/assets/7675fee3-673c-42b7-8768-cd0cd4b075d7)
- Biểu đồ giám sát hoạt động mạng (Network I/O) trong aaPanel. Nó giúp bạn theo dõi lưu lượng dữ liệu vào và ra của máy chủ theo thời gian. 
- Giao diện này dùng để kiểm soát tình trạng sử dụng mạng, phát hiện các thời điểm có lưu lượng tăng đột biến hoặc bất thường, từ đó hỗ trợ chẩn đoán các vấn đề liên quan đến kết nối hoặc bảo mật.
- Biểu đồ lưu lượng mạng: Hiển thị tốc độ truyền dữ liệu:
	- Upstream (gửi đi): thường là dữ liệu máy chủ gửi ra ngoài.
	- Downstream (nhận vào): dữ liệu máy chủ nhận từ bên ngoài.
- Chọn giao diện mạng: Bạn có thể lọc theo từng card mạng như lo (nội bộ), ens4 (mạng chính), hoặc xem tất cả.
- Chọn khoảng thời gian: Xem dữ liệu theo ngày hôm nay, hôm qua, 7 ngày gần nhất hoặc tùy chỉnh.
- Chi tiết tại thời điểm cụ thể: Khi rê chuột vào biểu đồ, bạn sẽ thấy tốc độ gửi/nhận tại thời điểm đó.
- Giao diện này: 
	- Giúp phát hiện các tiến trình hoặc dịch vụ đang sử dụng băng thông lớn.
	- Hỗ trợ chẩn đoán sự cố mạng hoặc tấn công bất thường (như DDoS).
	- Cung cấp dữ liệu để tối ưu hóa cấu hình mạng hoặc phân tích hiệu suất hệ thống.
