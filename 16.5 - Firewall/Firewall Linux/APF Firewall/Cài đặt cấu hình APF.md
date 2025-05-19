**Để VPS của bạn hoạt động một cách hiệu quả và bảo mật cao, bạn cần phải thường xuyên kiểm tra các dấu vết truy cập trái phép. Theo đó, một tường lửa là giải pháp đầu tiên mà bạn nên nghĩ đến. 

APF Firewall là một tường lửa có khả năng chặn khá tốt (ở mức cơ bản) những kết nối không mong muốn.**

## Cài đặt APF Firewall cho CenOS 7

Bước 1 : Truy cập vào thư mục gốc -> Tiến hành dowload và giải nén file APF

Dùng lệnh :

```
cd /usr/local/src
```

Để truy cập vào thư mục muốn tải 

Tiếp theo sử dụng lệnh 

```
#wget http://rfxnetworks.com/downloads/apf-current.tar.gz

tar -zxf apf-current.tar.gz
```
Để tải là giải nén file apf

![image]()

Bước 2: Tiến hành cài đặt APF Firewall

`cd apf-1.7.6` | di chuyển đến file cài đặt

`./install.sh` | tiến hành cài đặt

`apf -s`       | khởi động apf

Nếu như kết quả trả về:

```
Installing APF 1.7.6: Completed.

Installation Details: 
Install path: /etc/apf/ 
Config path: /etc/apf/conf.apf 
Executable path: /usr/local/sbin/apf 
AntiDos install path: /etc/apf/ad/
AntiDos config path: /etc/apf/ad/conf.antidos
DShield Client Parser: /etc/apf/extras/dshield/

Other Details: Listening TCP ports: 1,21,22,25,53,80,110,111,143,443,465,993,995,2082, 2083,2086,2087,2095,2096,3306 Listening UDP ports: 53,55880 Note: These ports are not auto-configured; they are simply presented for information purposes. You must manually configure all port options.
```

Có nghĩa là đã cài đặt thành công. Ngược lại, kết quả trả về như hình dưới thì chúng ta bắt đầu sang bước thứ 3.

![image]

Bước 3 : Tiến hành Cấu hình APF Firewall

Truy cập vào file apf/conf.apf bằng lệnh

```
nano /etc/apf/conf.apf
```

chỉnh sửa các thông số sau 

```
DEVEL_MODE=”1″ -> DEVEL_MODE=”0″
```
```
IFACE_UNTRUSTED="enth0" -> IFACE_UNTRUSTED="ens33"
```
 Lưu file và reload lại APF bằng lệnh `apf -r`
 
 **Một số lỗi thường gặp**
 
*Nếu lỗi xuất phát từ thông báo này: “eth0: error fetching interface information: Device not found“, bạn hãy chỉnh sửa thông số sau:
```
IFACE_IN=”eth0″
IFACE_OUT=”eth0″
```
thành
```
IFACE_IN=”venet0″
IFACE_OUT=”venet0″
```
*Nếu lỗi xuất phát từ thông báo này: “unable to load iptables module {ip_tables}“, bạn chỉnh sửa thông số sau:
```
SET_MONOKERN=”0″
```
thành
```
SET_MONOKERN=”1″
```
Cuối cùng, hãy lưu file vào. Bây giờ, bạn phải khởi động APF Firewall nhé.

## Một số câu lệnh command APF Firewall

|Câu lệnh|Tác dụng|
|-----------|-----------|
|apf -s | Khởi động APF Firewall|
|apf -r | Update và khởi động lại APF Firewall|
|apf -f | Dọn sạch tường lửa|
|apf -st | Trạng thái hiện tại của APF Firewall|
|apf -a IP {Lý do mở} | Cho phép một IP nào đó vượt qua tường lửa và điền vào nhóm “Cho phép truy cập” (allow_hosts.rules)|
|apf -d IP {Lý do khóa} | Khóa một IP nào đó và xếp vào nhóm “Chặn truy cập” (deny_hosts.rules)|
|chkconfig –level 2345 apf on | Thêm APF Firewall vào danh sách tự động chạy khi khởi động lại VPS|
|chkconfig –del apf | Gỡ bỏ APF Firewall khỏi list chạy tự động sau khi khởi động lại VPS.|
 
