# SECURITY on aaPanel

Chức năng **Security** giúp bảo vệ máy chủ khỏi các mối đe dọa bằng cách quản lý:

- **Firewalls** – Quản lý các quy tắc tường lửa để kiểm soát lưu lượng mạng vào/ra máy chủ.
- **SSH** – Cấu hình và bảo vệ dịch vụ SSH, bao gồm thay đổi cổng và giới hạn truy cập.
- **Brute Force Protection** – Phát hiện và ngăn chặn các cuộc tấn công dò mật khẩu đăng nhập.
- **Compiler Access** – Giới hạn quyền sử dụng trình biên dịch để ngăn mã độc tự biên dịch trên máy chủ.
- **Anti Intrusion** – Giám sát và chặn các hành vi truy cập bất thường hoặc đáng ngờ. (Pro)
- **System Hardening** – Áp dụng các thiết lập bảo mật nâng cao để giảm thiểu lỗ hổng hệ thống. (Pro)


## 1. Firewall 
![image](https://github.com/user-attachments/assets/d217153b-87bf-4970-83b3-e1222e1868f9)

**Các nút chức năng:**
- **Bật/Tắt Firewall**
- **Block ICMP**: Vô hiệu hóa ping từ thiết bị khác.
- **Quản lý Port**:
    - Thêm/Sửa/Xóa quy tắc cổng.
    - Chọn giao thức (TCP/UDP), cổng (1–65535), IP nguồn, chiến lược (Allow/Deny), hướng (Inbound/Outbound).
- **Quản lý IP:**
    - Thêm/Sửa/Xóa quy tắc IP.
    - Chọn IP nguồn, chiến lược (Block/Release), hướng truy cập.
- **Port Forwarding**: Chuyển tiếp lưu lượng từ một cổng đến cổng khác hoặc IP khác (NAT).
- **Area Rules**: Chặn hoặc cho phép truy cập từ khu vực địa lý cụ thể.

### Port rule - Quản lý quy tắc cổng
> Từ chối hoặc cho phép truy cập IP vào cổng

- Các chức năng chính: 
    - **Thêm**/**Sửa**/**Xóa** quy tắc cổng
    - **Chọn giao thức** (TCP/UDP), cổng (1–65535), IP nguồn, chiến lược (Allow/Deny), hướng (Inbound/Outbound)

| **Chức năng**           | **Mô tả**                                    |
| ----------------------- | -------------------------------------------- |
| **Add Port Rule**       | Thêm quy tắc cổng mới (cho phép hoặc chặn).  |
| **Import/Export Rules** | Nhập hoặc xuất danh sách quy tắc từ/tới tệp. |
| **All directions**      | Xem tất cả quy tắc (cả inbound & outbound).  |
| **Inbound / Outbound**  | Xem quy tắc vào hoặc ra riêng biệt.          |
| **Protocol**            | Giao thức sử dụng: TCP, UDP hoặc cả hai.     |
| **Port**                | Cổng hoặc dải cổng áp dụng quy tắc.          |
| **Status**              | Trạng thái cổng: đang lắng nghe hoặc không.  |
| **Strategy**            | Hành động: Cho phép hoặc Từ chối.            |
| **Direction**           | Hướng của kết nối: vào hoặc ra.              |
| **Source IP**           | IP nguồn được áp dụng: toàn bộ hoặc cụ thể.  |
| **Remarks**             | Ghi chú cho quy tắc.                         |
| **Add Time**            | Thời điểm tạo quy tắc.                       |
| **Edit / Delete**       | Sửa hoặc xóa quy tắc.                        |

#### Add Port Rule or Edit
![image](https://github.com/user-attachments/assets/d9cd7c02-462e-4402-acb0-872468aca82b)

- Nhập các trường:
    - **Protocol**:  Chọn loại giao thức tùy chọn `TCP`, `TCP/UDP`,`UDP`
    - **Port**: Cổng đầu vào, phạm vi cổng là `1-65535`
    - **Source IP**: Nguồn IP có thể lựa chọn `All` hoặc `Specify IP`
    - **Strategy**: Chính sách cổng có thể lựa chọn `Allow` hoặc `Deny`
    - **Direction**: hướng của lưu lượng mạng tùy chọn vào `Inbound` hoặc ra `Outbound`
    - **Remark**: 

### IP rules - Quản lý quy tắc IP
- Chức năng chính: Chặn / Cho phép truy cập IP cụ thể
- Các chức năng khác tương tự như **Port rule**

- **Add ip Rule or Edit**
![image](https://github.com/user-attachments/assets/49b44a44-7428-48d2-b336-4cfc2a51552c)

    - Các thông tin cần điền:
        - **Source IP** : nhập IP nguồn
        - **Strategy**  
        - **Direction**  
        - **Remarks**  

### Port forward - Chuyển tiếp cổng

- Chức năng này cho phép cấu hình chuyển tiếp lưu lượng truy cập từ cổng này sang cổng khác trên máy cục bộ hoặc cổng của máy chủ đích. 
- **Add port forwardhoặcEdit:**
- ![image](https://github.com/user-attachments/assets/6651ac98-75bb-4d31-8ab4-80dd46491156)
    - Nhập các thông tin như: **Giao thức**, **port** (nguồn/đích), **IP** đích (hoặc localhost **127.0.0.1**)...

### Area rules
> Giới hạn quyền truy cập theo khu vực địa lý (dựa trên IP)

- **Add area rule** hoặc **Edit**
- ![image](https://github.com/user-attachments/assets/d92bb2ed-e9ed-491c-94f7-3ab19cbdc5f3)

---

## 2. SSH

![image](https://github.com/user-attachments/assets/f47ee667-8368-407b-b8d1-88c2f85b8efb)

- **Basic setup (SSH**):
    - Bật/tắt SSH 
    - Thay đổi port 
    - Cấu hình đăng nhập bằng **Password** hoặc **SSH key**.
    - Cảnh báo khi root đăng nhập

- **SSH login logs** (chức năng này yêu cầu nâng cấp gói **pro**)

--- 

## 3. Brute force protection 

![image](https://github.com/user-attachments/assets/5af3a000-3d19-4456-8e1a-fd3312ff0072)

### Configuration

- **Username-based**: Khóa tài khoản aaPanel sau nhiều lần đăng nhập sai 

![image](https://github.com/user-attachments/assets/c5ca44b0-b874-4b4a-a3fd-99b8da2a31ed)

- **IP-based**: Khóa IP truy cập SSH sau nhiều lần sai.
![image](https://github.com/user-attachments/assets/b230d6b9-e975-4ed1-b013-e45257208766)

### WhileList
Thêm nhưng IP không muốn bị ảnh hưởng bởi cấu hình

![image](https://github.com/user-attachments/assets/4ebdb3ec-28a8-4751-9bfb-1a0182b9fe38)

### BackList
Những IP thêm vào danh sách này sẽ bị cấm truy cập vào tất cả các cổng của máy chủ

### History Reports
Xem lịch sử đăng nhập SSH
- Đăng nhập không thành công
- Địa chỉ IP bị chặn

---

## 4. Anti-Intrusion (Chống xâm nhập) 
> Chức năng này cần nâng cấp gói **Pro**

## 5. System Hardening (Pro)

