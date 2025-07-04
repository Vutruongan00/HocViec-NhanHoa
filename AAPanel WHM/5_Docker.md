
# Quản lý Docker trong aaPanel

**Docker menu:**
- One-Click Install	
- Overview	
- Container	
- Cloud image	
- Local image
- Compose	
- Network	
- Volume	
- Repository	
- Settings

![image](https://github.com/user-attachments/assets/198f440e-b025-46d5-8f60-e29a270f28a8)

## 1. One Click - Cài đặt nhanh
> **aaPanel** cung cấp chức năng triển khai APP nhanh chóng chỉ bằng một cú nhấp chuột dựa trên container docker

### Ínstalled
- Ví dụ cài đặt WordPress:
    - Điền thông tin và nhập tên miền vào trường `Domain` để ánh xạ (proxy) đến container WordPress chạy bên trong Docker

![image](https://github.com/user-attachments/assets/fbabbcd3-61d3-4bae-b8ca-4065a8ca8003)

- Cài đặt hoàn tất, để ý port `21080` để cấu hình proxy

![image](https://github.com/user-attachments/assets/cb8e725c-0d52-42aa-8bf8-35f9fe272bc5)

- Cấu hình proxy với domain `proxytest.antvpro.io.vn` và chuyển hướng toàn bộ trang web tới cổng `21080` mà container WP đang chạy 
![image](https://github.com/user-attachments/assets/06edb8b3-700b-441f-912b-f47122752ca3)

- truy cập --> http://proxytest.antvpro.io.vn

![image](https://github.com/user-attachments/assets/360007ea-ec3d-4eb1-a0ae-9a1e8b8bc4c9)


#### Backup

![image](https://github.com/user-attachments/assets/1f6e6898-f2f4-4730-b1ee-ba48a56c018c)

- Chức năng này hỗ trợ:
    - Sao lưu
    - Tải lên tệp sao lưu
    - Restore: khôi phục bản sao lưu
    - Download / Delete: tải hoặc xóa bản sao lưu

> - Các chức năng khác như:
>     - Stop / Restart
>     - Details 
>     - Rebuild 
>     - Uninstall 
>     - Open log\

---

## 2. Overview
- Kiểm tra mức sử dụng tài nguyên của máy chủ và mức sử dụng tài nguyên của container.

![image](https://github.com/user-attachments/assets/05b12ffb-bf3e-413f-a973-8c4567194937)

- Click Khi click vào từng container → mở Container Manage, gồm các tab chức năng:

![image](https://github.com/user-attachments/assets/8ac943d9-ff6e-4c57-9346-a6b00b9cf654)


| Tab   | Chức năng    |
| ---------------------- | ----------------------------------------------------------------------------- |
| **Container status**   | Kiểm tra trạng thái container (start/stop/restart).                           |
| **Container terminal** | Mở terminal trực tiếp trong container để thao tác dòng lệnh.                  |
| **Container details**  | Xem thông tin cấu hình chi tiết: cổng, môi trường, volume...                  |
| **Storage volumes**    | Quản lý volume (gắn kết dữ liệu host ↔ container).                            |
| **Container network**  | Cấu hình và xem thông tin mạng container.                                     |
| **Reboot strategy**    | Cài đặt chính sách khởi động lại (restart always / on failure...).            |
| **Create image**       | Tạo image mới từ container hiện tại.                                          |
| **Rename**             | Đổi tên container.                                                            |
| **Real-time logs**     | Xem log thời gian thực của container.                                         |
| **Proxy**              | Tạo reverse proxy domain đến container (ngang với tạo trong Website → Proxy). |

---

## 3. Container
- Xem trạng thái của container: `start`, `stop`, `restart`, `delete`,...

![image](https://github.com/user-attachments/assets/98b55c8f-743f-4953-8681-47151cbb86be)

- Các chức năng khác: 
- 
| **Chức năng** | **Mô tả (Describe)** |
| ------------- | -------------------- |
| **Create Container**        | Tạo container mới từ image hoặc qua Docker Compose.                                         |
| **Log Manage**              | Xem và quản lý log của container.                                                           |
| **Clear Container**         | Xóa dữ liệu container mà không xóa container. **Lưu ý sao lưu trước để tránh mất dữ liệu.** |
| **Container name / Manage** | Tên container – click để quản lý container (trạng thái, log, thiết lập...).                 |
| **Container ID**            | Mã định danh duy nhất của container.                                                        |
| **Status**                  | Trạng thái container – có thể **Stop / Start / Restart / Kill / Pause**.                    |
| **Image**                   | Image và phiên bản container đang sử dụng.                                                  |
| **IP**                      | Địa chỉ IPv4 nội bộ của container.                                                          |
| **Port (Host→Container)**   | Cổng ánh xạ từ máy chủ → container. Không hiển thị nếu dùng **host network mode**.          |
| **Create time**             | Thời gian container được tạo.                                                               |
| **Note**                    | Ghi chú cho container (nếu có).                                                             |
| **Terminal**                | Mở shell/terminal bên trong container để chạy lệnh.                                         |
| **Delete**                  | Xóa container (toàn bộ). **Sao lưu trước để tránh mất dữ liệu.**                            |
| **More**                    | Hiển thị thêm các tính năng khác.                                                           |
| **Rename**                  | Đổi tên container.                                                                          |
| **Path**                    | Mở thư mục gắn kết (volume) container bằng giao diện **Files**.                             |


### 3.1 Create Container

#### Tạo thủ công bằng giao diện và tùy chỉnh các thông số:

![image](https://github.com/user-attachments/assets/2f7f988f-74e5-471d-9597-23c68b592a5f)
![image](https://github.com/user-attachments/assets/7e62fdae-e988-4944-a3d9-8ba093876938)
 

- **Cấu hình chính:**

| **Trường**         | **Giải thích ngắn gọn**                                         |
| ------------------ | --------------------------------------------------------------- |
| **Container name** | Tên container – bạn tự đặt.                                     |
| **Image**          | Chọn image để tạo container (ví dụ: `nginx:latest`, `mysql:8`). |
| **Port**           | Thiết lập ánh xạ cổng (host\:container).                        |
    
- Cấu hình tùy chọn thêm:
    -   **Reboot Rule** (Chế độ khởi động lại container)
    -   **Network & Mount** (Cấu hình chế độ mạng)
    -   Và Các tùy chọn nâng cao
        -   Auto delete	Xoá container sau khi dừng. (Thường KHÔNG nên bật)
        -   Pseudo-TTY (-t)	Gán terminal ảo cho container
        -   Standard input (-i)	Giữ kết nối STDIN mở, dùng khi chạy shell.
        -   Privilege mode	Chạy container với quyền cao nhất (như root trên host).
    
    - Giới hạn tài nguyên:
        - **Min memory**:	Bộ nhớ tối thiểu cấp cho container. (không vượt RAM vật lý)
        - **CPU limit**:	Số core CPU tối đa container được dùng.
        - **Memory limit**:	Bộ nhớ tối đa container được phép dùng.

    - Các thông tin bổ sung: gắn thẻ để phân loại container(`tag`) ; Biến môi trường (`Env variable`) ; hoặc ghi chú riêng cho container (`Remark`).

#### Tạo Container bằng lệnh Docker

- Ví dụ:
```bash
docker run -d --name mysql_test -p 3361:3306 -v /docker/mysql_data/:/var/lib/mysql/ -e MYSQL_ROOT_PASSWORD=my-passwd mysql:5.7
```

---

## 4. Cloud image

### Create Container
> Thao tác tương tự như trên

### Pull
> Chức năng này sử dụng để tải một image từ Docker Registry (thường là Docker Hub)

![image](https://github.com/user-attachments/assets/35cf18f7-b8a8-4c2a-9cd4-c5453ed5bcc0)

---

## 5. Local image 
- Quản lý các image trên local gồm những chức năng chính:

![image](https://github.com/user-attachments/assets/aeedc06f-a54b-43f0-a40c-bc3f9bc4b163)


| **Chức năng**  | **Mô tả** |
| ------------- | --------- |
| **Pull from repository** | Tải image từ Docker Hub về máy.                       |
| **Import image**         | Nhập image từ file `.tar` trên máy chủ.               |
| **Build image**          | Tự tạo image từ Dockerfile hoặc nội dung chỉ định.    |
| **Cloud image**          | Tải image từ Docker Hub (giống pull, giao diện khác). |
| **Clear image**          | Xoá các image không được container nào sử dụng.       |

---

## 6. Compose

**Docker Compose** là công cụ dùng để định nghĩa và chạy tập hợp các container như một dự án. Bạn viết một file `docker-compose.yml` để khai báo các services, volumes, network, và một lệnh sẽ giúp bạn khởi động hoặc dừng toàn bộ nhóm như một thể thống nhất

**Compose trong aaPanel hỗ trợ:**
- **Tạo Docker Compose project**
    - Tải template Compose có sẵn:
    
    ![image](https://github.com/user-attachments/assets/e1a58cbd-1c79-4e13-90cc-6c6cff973f69)

    - Hoặc tự tạo mới bằng cách dán nội dung `docker-compose.yml` vào.
    - ![image](https://github.com/user-attachments/assets/b64a43cb-51ac-4b35-9b9f-97c64022f4db)


- **Quản lý project**:
    - Khởi động/dừng/restart toàn bộ containers trong project. 
    - Xem log từng container.
    - Vào terminal của từng container.
    - Xem và truy cập thư mục (Path).
    - Xoá toàn bộ project (bao gồm các container liên quan).

    ![image](https://github.com/user-attachments/assets/bc563f5c-fdf5-4acc-beb4-f0318ae89f95)

- **Template List**
    - Xem các Compose templates đã lưu và thực hiện các thao tác như chỉnh sửa và xóa....

![image](https://github.com/user-attachments/assets/ca54c851-9946-432b-b769-77746f5089eb)


---

## 7. Network 
Cho phép bạn quản lý các mạng Docker:
- Tạo network (bridge, host, overlay, etc.)

![image](https://github.com/user-attachments/assets/53e8f593-baf4-4a98-8841-2ebd7c840662)

- Xem chi tiết network: các container kết nối, subnet, IP range...

![image](https://github.com/user-attachments/assets/81472dd7-d64d-43ee-8e3c-351eba091ef6)


- Xóa network 

---

## 8. Volume
**Quản lý volume – vùng lưu trữ dữ liệu:**

![image](https://github.com/user-attachments/assets/74011c47-c59c-47a5-b8ab-c555bacfb7dc)

- **Tạo volume**: định nghĩa các vùng dữ liệu container có thể mount
- **Xem chi tiết**: dung lượng sử dụng, đường dẫn
- **Xóa volume** (chỉ khi không còn container nào dùng)

---

## 9. Repository
**Quản lý các repository imange:**

![image](https://github.com/user-attachments/assets/f78791da-3b0d-4e92-a1dd-807dc5bd9aac)

- Đăng nhập vào registry riêng (ngoài Docker Hub)
- Thêm/sửa repo để push/pull image

---

## 10. Settings
