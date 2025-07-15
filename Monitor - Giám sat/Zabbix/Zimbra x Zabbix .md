
# Giám sát Zimbra qua Zabbix
> Giám sát trạng thái hoạt động của các dịch vụ Zimbra (zmcontrol status) bằng Zabbix Server thông qua Zabbix Agent.

### 1. Trên máy Zimbra (client)

- Cài đặt Zabbix Agent ( )
- Cấu hình Zabbix Agent ( )

- **Tạo file `UserParameter` để định nghĩa các khóa theo dõi**

```bash=
# sudo nano /etc/zabbix/zabbix_agentd.d/zimbra_custom.conf

UserParameter=zimbra.amavis.status,awk '/amavis/{print $2}' /tmp/zmcontrol_status
UserParameter=zimbra.antispam.status,awk '/antispam/{print $2}' /tmp/zmcontrol_status
UserParameter=zimbra.antivirus.status,awk '/antivirus/{print $2}' /tmp/zmcontrol_status
UserParameter=zimbra.mailbox.status,awk '/mailbox/{print $2}' /tmp/zmcontrol_status
UserParameter=zimbra.mta.status,awk '/mta/{print $2}' /tmp/zmcontrol_status
UserParameter=zimbra.proxy.status,awk '/proxy/{print $2}' /tmp/zmcontrol_status
UserParameter=zimbra.webmail.status,awk '/service webapp/{print $3}' /tmp/zmcontrol_status
UserParameter=zimbra.webadm.status,awk '/zimbraAdmin/{print $3}' /tmp/zmcontrol_status
UserParameter=zimbra.snmp.status,awk '/snmp/{print $2}' /tmp/zmcontrol_status
UserParameter=zimbra.opendkim.status,awk '/opendkim/{print $2}' /tmp/zmcontrol_status
UserParameter=zimbra.apache.status,awk '/apache/{print $2}' /tmp/zmcontrol_status
UserParameter=zimbra.zmconfigd.status,awk '/zmconfigd/{print $2}' /tmp/zmcontrol_status
```

- **Thêm cron để tự động ghi zmcontrol status vào file**
```bash=
# sudo crontab -e

*/3 * * * * sudo -u zimbra /opt/zimbra/bin/zmcontrol status > /tmp/zmcontrol_status
```
> Cứ 3 phút, Zimbra sẽ ghi trạng thái các dịch vụ vào file /tmp/zmcontrol_status

- **Cấp quyền `sudo` không cần mật khẩu cho user `zabbix`**
```bash=
# sudo visudo

# Thêm dòng sau vào cuối file:
zabbix ALL=(zimbra) NOPASSWD: /opt/zimbra/bin/zmcontrol
```
> Cho phép Zabbix Agent gọi zmcontrol status mà không bị hỏi password

- **Khởi động lại Zabbix Agent:**
```bash=
sudo systemctl restart zabbix-agent
```

### 2. Trên Zabbix Server

- **Import template** - Chọn theo version của bạn 
[Download Template Zimbra](https://github.com/zabbix/community-templates/tree/main/Applications/Mail_servers/template_zimbra_zmcontrol_status) - Chọn theo **Zabbix version** của bạn

<img width="1912" height="773" alt="image" src="https://github.com/user-attachments/assets/4c22299a-fc95-4fd8-8c9d-7d4ce586640f" />
<img width="1902" height="850" alt="image" src="https://github.com/user-attachments/assets/a318411a-8d3e-4acb-9cbf-28361928a9d2" />

- **Tạo Host Zimbra trong Zabbix** (...)
    - **Monitoring --> Hosts --> Create Host**
    - Hoặc/ **Data collection --> Hosts -->Create Host**
    - Điền những thông tin cần thiết

    <img width="1913" height="899" alt="image" src="https://github.com/user-attachments/assets/e1986289-0afb-43b6-9ca8-134966d8ec12" />


- Kiểm tra trạng thái nếu hiện **Enabled** và **ZBX** màu xanh như ảnh thì khi đó máy chủ Zabbix sẽ bắt đầu nhận dữ liệu từ Zabbix Agent

<img width="1916" height="738" alt="image" src="https://github.com/user-attachments/assets/9d2b9384-947a-40cd-90b0-695cb56befa5" />

- **Kiểm tra kết nối Server - Agent**
Trên Zabbix Server:
```bash
zabbix_get -s 35.185.137.50 -k zimbra.mailbox.status
```
<img width="632" height="88" alt="image" src="https://github.com/user-attachments/assets/bebbf74c-01a2-4e80-b5d0-cc082c10ea79" />

---
***Giải thích các thành phần chính**

| Thành phần              | Giải thích                                                                                 |
| ----------------------- | ------------------------------------------------------------------------------------------ |
| `UserParameter=...`     | Định nghĩa custom item để agent có thể trả về giá trị riêng, ví dụ trạng thái từng dịch vụ |
| `zmcontrol status`      | Lệnh kiểm tra tình trạng các dịch vụ Zimbra                                                |
| `/tmp/zmcontrol_status` | File trung gian để agent đọc nhanh mà không phải chạy lệnh nặng                            |
| `crontab */3`           | Cập nhật file mỗi 3 phút, tránh timeout                                                    |
| `zabbix_get`            | Dùng trên Zabbix Server để test agent trả về gì                                            |
| Template                | Giao diện tự động tạo các item và trigger trên Zabbix UI                                   |

