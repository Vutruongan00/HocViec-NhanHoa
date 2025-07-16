># Hướng dẫn cài đặt Check_MK Agent

# 1. Linux

## 1.1 Ubuntu

- Truy cập giao diện Web UI để tải Agent cho client: **Setup - Agent --> Linux** để lấy Agent:

<img width="1135" height="696" alt="image" src="https://github.com/user-attachments/assets/ab11d4cc-caa6-416a-a609-a0b5f8393f25" />

- Chọn loại file:
    - `*.deb` : dành cho host sử dụng **DEBIAN/Ubuntu**
    - `*.rpm` : dành cho host sử dụng **RHEL/CentOS**

<img width="839" height="445" alt="image" src="https://github.com/user-attachments/assets/d9608a27-58b5-448b-b8b6-2b59df53ff52" />

- Copy địa chỉ đường dẫn và để **tải file cài đặt về trên Agent bằng `wget`**:
```bash=
wget http://34.56.236.153/monitor/check_mk/agents/check-mk-agent_2.4.0p7-1_all.deb
```

- Cài đặt **xinetd**:
```bahs=
apt install xinetd -y
```
- Khởi động lại dịch vụ xinetd và cho phép khởi động cùng hệ thống:
```
systemctl start xinetd
systemctl enable xinetd
```

- **Cài đặt Agent:**
```bash=
# dpkg -i <packaged_vừa_tải_về>

dpkg -i check-mk-agent_2.4.0p7-1_all.deb
```

- Sửa cấu hình **xinetd** của checkmk tại **`/etc/xinetd.d/check_mk`**
    - nếu chưa có thì copy file config example từ `/etc/check_mk/xinetd-service-template.cfg`

    ```bash=
    cp /etc/check_mk/xinetd-service-template.cfg /etc/xinetd.d/check_mk
    ```

    - Sau đó sửa: **thêm IP checkMK Server để cho phép Server có thể truy cập Agent**
    ```
    nano /etc/xinetd.d/check_mk
    ```
    
<img width="941" height="556" alt="image" src="https://github.com/user-attachments/assets/5e1a6783-ed8c-4446-8197-8c39876c2dd0" />

- **Khởi động lại xinetd**
```
systemctl restart xinetd
```


## 1.2 CentOS 
(tương tự - cứ thế mà phệt `yum` thôi)

---
