> # Hướng dẫn cài đặt Zabbix 7.2 trên Ubuntu 22.04

## Bước 1: Cập nhật hệ thống
```bash!
sudo apt update -y
sudo apt upgrade -y
```

## Bước 2: Cài đặt Webserver
```bash!
sudo apt install apache2 -y
sudo systemctl enable apache2
sudo systemctl start apache2
sudo systemctl status apache2
```

## Bước 3: Cài đặt và cấu hình MariaDB
- **Cài thêm kho lưu trữ MariaDB:**
```bash
sudo curl -LsS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup | sudo bash -s -- --mariadb-server-version=10.11
```

- **Cập nhật hệ thống và cài đặt MariaDB:**
```bash!
sudo apt update
sudo apt install mariadb-server mariadb-client -y
```
- **Kiểm tra version MariaDB: **
```
mariadb --version
```
<img width="884" height="97" alt="image" src="https://github.com/user-attachments/assets/750ca5fa-f983-4ac5-bedc-312faa7d9ae0" />

- **Cấu hình MariaDB:**
```
mysql_secure_installation
```
<img width="927" height="652" alt="image" src="https://github.com/user-attachments/assets/fafa1aba-86a6-4125-88c2-4b425fbb1f45" />

<img width="855" height="645" alt="image" src="https://github.com/user-attachments/assets/c7d5b9c5-03c9-47dc-8ffa-12e26f805ea0" />

- **Khởi động lại MariaDB**
```shel
systemctl enable mariadb
systemctl restart mariadb
systemctl status mariadb
```

- **Tạo Database cho Zabbix:**
```sql!
-- mysql -uroot -p

CREATE DATABASE zabbix_antv character set utf8mb4 collate utf8mb4_bin;
CREATE USER zabbix_antv@localhost IDENTIFIED by 'Qwerty123@xxx';
GRANT ALL PRIVILEGES ON zabbix_antv.* TO zabbix_antv@localhost;
FLUSH PRIVILEGES; 
QUIT
```
<img width="808" height="262" alt="image" src="https://github.com/user-attachments/assets/aa71a552-7307-4cd2-85c8-9b3e9f94be7b" />


## Bước 4: Cài đặt PHP và các module cần thiết
```
sudo apt install php php-mysql php-ldap php-bcmath php-gd php-xml libapache2-mod-php -y
```

## Bước 5: Cài đặt Zabbix repository, Zabbix server, frontend, agent.
- Tải gói cài đặt & tiến hành cài đặt:
```bash=
wget https://repo.zabbix.com/zabbix/7.2/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.2+ubuntu22.04_all.deb

dpkg -i zabbix-release_latest_7.2+ubuntu22.04_all.deb

apt update

apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent -y
```

- Xác minh lại phiên bản cài đặt:
```
apt-cache policy zabbix-server-mysql
```
<img width="568" height="178" alt="image" src="https://github.com/user-attachments/assets/0ec07298-0ff1-4a5e-96c4-94ec5fcaec36" />



## Bước 6: Cấu hình Database với  Zabbix 7.2

- Import cơ sở dữ liệu vào Database vừa tạo:
```
zcat /usr/share/zabbix/sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -u zabbix_antv -p zabbix_antv
```

- Mở file cấu hình và chỉnh  sửa các thông tin: `nano/etc/zabbix/zabbix_server.conf`

    - **DBName**=`zabbix_antv`  - - dòng 100
    - **DBUser**=`zabbix_antv`  - - dòng 116
    - **DBPassword**=`xxxxxxxxx` - -  dòng 124

<img width="768" height="580" alt="image" src="https://github.com/user-attachments/assets/593fde7b-9061-4b96-bf88-774364f5783e" />

- **Lưu (Ctrl+S) và khởi động lại ZABBIX và các dịch vụ để áp dụng cấu hình:**

```
sudo systemctl restart zabbix-server zabbix-agent apache2
sudo systemctl enable zabbix-server zabbix-agent apache2

sudo systemctl status zabbix-server
sudo systemctl status apache2
```

## Bước 7: Cấu hình Firewall - Mở port (Nếu cần)

## Bước 8: Truy cập ZABBIX 7.2 

- Truy cập qua giao diện Web Zabbix:
`http://<địa chỉ IP hoặc hostname>/zabbix`

<img width="955" height="686" alt="image" src="https://github.com/user-attachments/assets/14568d20-09d8-4e1c-bb6b-cca231c603a8" />

<img width="807" height="496" alt="image" src="https://github.com/user-attachments/assets/ddaee54d-dd7f-4c46-9b5b-ce155406fd6f" />

- Điền thông tin **DB name, User, Password:**

<img width="858" height="547" alt="image" src="https://github.com/user-attachments/assets/776f9718-27eb-4991-9696-faba4c1680ea" />

- Điền tên Server Zabbix (tùy ý)

<img width="889" height="530" alt="image" src="https://github.com/user-attachments/assets/92428903-4f13-4724-95a8-8c0ea5de34d0" />

- Kiểm tra lại thông tin: --> **Next**

<img width="858" height="508" alt="image" src="https://github.com/user-attachments/assets/97d4d11d-8ce2-42fe-a4da-2378123576c2" />

- Complete! --> **Finish**

<img width="855" height="505" alt="image" src="https://github.com/user-attachments/assets/5a08a378-66b6-4412-ad59-f3bcb6f9f32c" />

- Đăng nhập với tài khoản mặc định
    - Username: **Admin**
    - Password: **zabbix**

<img width="410" height="398" alt="image" src="https://github.com/user-attachments/assets/79a0f191-162e-47e9-bfc1-2925dc1298f2" />

- Giao diện tổng quan Zabbix:

<img width="1909" height="995" alt="image" src="https://github.com/user-attachments/assets/aef1e887-bbb8-4c5f-8c96-ab42848108a4" />



