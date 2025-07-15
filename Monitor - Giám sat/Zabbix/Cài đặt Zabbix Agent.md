# 1. Cài đặt Zabbix Agent trên Ubuntu 22.04

## Bước 1: **Cài đặt Zabbix Agent:**
```bash!
wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-4+ubuntu22.04_all.deb  

sudo dpkg -i zabbix-release_6.0-4+ubuntu22.04_all.deb 

sudo apt update 

sudo apt install zabbix-agent 
```

## Bước 2: Cấu hình Zabbix Agent
- Cấu hình Zabbix Agent để liên lạc với Zabbix Server:
```bash=
# nano /etc/zabbix/zabbix_agentd.conf 

Server=<Zabbix_Server_IP>
ServerActive=<Zabbix_Server_IP>
Hostname=<Hostname_Of_Ubuntu_Client>
```

- Khởi động và kích hoạt dịch vụ Zabbix Agent:
```bash=
sudo systemctl start zabbix-agent 
sudo systemctl enable zabbix-agent 
sudo systemctl status zabbix-agent
```

<img width="879" height="355" alt="image" src="https://github.com/user-attachments/assets/55bb5812-8ff7-4b29-91cd-c23560b76401" />

## Bước 3: Thêm Zabbix Client đến Zabbix Server
- **Monitoring --> Hosts --> Create Host**
- Hoặc/ **Data collection --> Hosts -->Create Host**
- Điền những thông tin cần thiết

<img width="1913" height="899" alt="image" src="https://github.com/user-attachments/assets/19708b26-74cf-461c-a380-88a6c2ece6ce" />

<img width="1912" height="773" alt="image" src="https://github.com/user-attachments/assets/148db5ca-5113-4068-a074-6bddbb3f2265" />

<img width="1902" height="850" alt="image" src="https://github.com/user-attachments/assets/64fa768d-0db3-4fce-8e11-5edf6d2db86e" />

- Kiểm tra trạng thái nếu hiện **Enabled** và **ZBX** màu xanh như ảnh thì khi đó máy chủ Zabbix sẽ bắt đầu nhận dữ liệu từ Zabbix Agent

<img width="1916" height="738" alt="image" src="https://github.com/user-attachments/assets/29384801-f534-4c02-ba31-12b870a731e5" />

---
[Link tham khảo Zabbix <3 Zimbra Template](https://mpolinowski.github.io/docs/DevOps/Zabbix/2022-07-15-zabbix-for-zimbra/2022-07-15/#zabbix-agent-2)
---







