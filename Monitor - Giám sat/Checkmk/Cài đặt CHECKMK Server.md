
># Hướng dẫn cài đặt Check_MK trên Ubuntu

# 1. Tổng quan về Check_MK

**Checkmk** là một giải pháp giám sát (monitoring) toàn diện, mã nguồn mở, dùng để theo dõi tình trạng của:
- Máy chủ (Linux, Windows)
- Thiết bị mạng (Router, Switch)
- Ứng dụng (Web, Database, Mail server,…)
- Dịch vụ cloud (AWS, Azure,…)

**Checkmk cung cấp:**
- Giao diện Web hiện đại
- Tự động phát hiện dịch vụ cần giám sát
- Hệ thống cảnh báo (email, SMS,…)
- Hỗ trợ plugin mở rộng (gần 2000 loại service/plugin)

# 2. Cài đặt Check_MK trên Ubuntu 22.04

- Cập nhật trước khi cài đặt:
```bash=
sudo apt update -y
sudo apt upgrade -y
```
## Bước 1: Tải và cài đặt Check_MK
-  Truy cập https://checkmk.com/download/archive để lấy các link tải các phiên bản Check_MK tại kho lưu trữ.

<img width="1840" height="895" alt="image" src="https://github.com/user-attachments/assets/bd85d8a3-f7ba-4a12-b1e7-f5ca68d9d570" />

> **Cứ hàng mới mà phệt thôi, hỏng thì gỡ đi cài lại...**

```
wget https://download.checkmk.com/checkmk/2.4.0p7/check-mk-raw-2.4.0p7_0.jammy_amd64.deb
```
- Sau đó tiến hành cài đặt Check_MK từ package vừa tải về:
```
sudo apt -y install ./checkmk/2.4.0p7/check-mk-raw-2.4.0p7_0.jammy_amd64.deb
```
- Kiểm tra lại version:
```
omd version
```

## Bước 2: Tạo một file giám sát 
- Tạo file: 
```bash
omd create monitor  # đặt tên tùy ý
```
<img width="533" height="172" alt="image" src="https://github.com/user-attachments/assets/025a3c83-cc27-4dda-99f0-380fcddc96ba" />

- **Start site để có thể sử dụng:**
```bash=
omd start monitor
```
<img width="391" height="152" alt="image" src="https://github.com/user-attachments/assets/66199e19-ff1b-47b9-b73d-87cd13489657" />

## Bước 3: Truy cập trang web Check_MK của site đã tạo
```
http://<your-IP>/<name-site>
```
- Thông tin đăng nhập:
    - `Username`: **cmkadmin** (mặc định)
    - `Password`: đặt mật khẩu bằng lệnh sau:
    ```bash
    # omd su monitor   -- chuyển sang user monitor
    
    cmk-passwd cmkadmin
    ```
    <img width="445" height="87" alt="image" src="https://github.com/user-attachments/assets/14f258c2-5cdb-4f49-bb1a-848026df43ab" />

<img width="1345" height="682" alt="image" src="https://github.com/user-attachments/assets/4c25bffb-aff1-48c4-b8f2-93491ceda964" />


- **Giao diện Check_MK:**

<img width="1906" height="964" alt="image" src="https://github.com/user-attachments/assets/7e0590ba-e383-490b-be49-f80f10b6d190" />
