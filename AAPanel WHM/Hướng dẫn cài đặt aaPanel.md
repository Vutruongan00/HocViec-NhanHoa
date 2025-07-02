# aaPanel
 
## Yêu cầu hệ thống

- **Hệ điều hành**:
  * CentOS 7/8 (khuyến nghị 64-bit)
  * Ubuntu 18.04/20.04/22.04 (Recoment)
  * Debian 10/11/12
  * CloudLinux 7.x, 8.x
  * AlmaLinux 8.x, 9.x
  * Rocky Linux 8.x
    
-  Phần cứng tối thiểu:
    - **CPU**: 1 Core
    - **RAM**: 512MB
    - **Disk**: 1GB
  
## Hướng dẫn Cài đặt (Ubuntu 22.04) 
> Cập nhật hệ thống trước khi cài đặt
> ```
> sudo apt-get update && apt-get upgrade -y
> ```

**Bước 1: Chạy scrip cài đặt Panel**
```bash!
URL="https://www.aapanel.com/script/install_7.0_en.sh" && \
if [ -f /usr/bin/curl ]; then
    curl -ksSO "$URL"
else
    wget --no-check-certificate -O install_7.0_en.sh "$URL"
fi && \
bash install_7.0_en.sh aapanel
```
- Xác nhận `y` để cài đặt

![image](https://github.com/user-attachments/assets/20996040-60d6-4657-8ec2-0cb43fbcfffb)

- **Cài đặt hoàn tất** --> truy cập theo đường dẫn và để đăng nhập bằng `username` `password` được chỉ định: 

![image](https://github.com/user-attachments/assets/993f8e3e-c0b0-4dec-83c8-68a417fde907)

- Đăng nhập vào aaPanel và tiến hành cài đặt theo như mong muốn
![image](https://github.com/user-attachments/assets/f54e9a64-2eba-41a5-ad51-ef52e339cf7d)

- Hệ thống đề xuất tạo tài khoản để sử dụng các tính năng MailServer Dashboard Management, WP-toolkit, Domain/SSL Management Center,...

![image](https://github.com/user-attachments/assets/af98506d-eac5-493f-a738-43a81b496f31)

- Xác nhận mail gửi về và click  **I haev verified**

![image](https://github.com/user-attachments/assets/21cdaf45-2539-42f0-a58c-2c4bcf6549bc)

- Giao diện aaPanel:
![image](https://github.com/user-attachments/assets/96d048c0-bafa-4a3e-b042-e3c0edd4e2c2)

