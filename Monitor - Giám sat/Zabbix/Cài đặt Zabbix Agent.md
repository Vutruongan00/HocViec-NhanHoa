
# Install Zabbix Agent on Ubuntu 22.04

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

![image](https://hackmd.io/_uploads/ryurbUmLgx.png)

## Bước 3: Thêm Zabbix Client đến Zabbix Server
- **Monitoring --> Hosts --> Create Host**
- Hoặc/ **Data collection --> Hosts -->Create Host**
- Điền những thông tin cần thiết

![image](https://hackmd.io/_uploads/r1HLqLQLex.png)

![image](https://hackmd.io/_uploads/Hy7-fF7Igg.png)

![image](https://hackmd.io/_uploads/HyTfGKQLll.png)

- Kiểm tra trạng thái nếu hiện **Enabled** và **ZBX** màu xanh như ảnh thì khi đó máy chủ Zabbix sẽ bắt đầu nhận dữ liệu từ Zabbix Agent

![image](https://hackmd.io/_uploads/HJD2lcXUge.png)

