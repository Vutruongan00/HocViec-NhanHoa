# Cách sử dụng cơ bản về iptables

## 1. Ý nghĩa một vài tham số, tùy chọn trong câu lệnh iptables</a>

### 1.1. Các tùy chọn chính (COMMAND OPTIONS)

| **Tùy chọn đầy đủ**  | **Viết tắt** | **Giá trị theo sau**                 | **Ý nghĩa** |
|----------------------|--------------|--------------------------------------|-------------|
| `--append`           | `-A`         | `chain_name rule_spec`              | Thêm rule vào cuối chuỗi (chain) đã chỉ định |
| `--check`            | `-C`         | `chain_name rule_spec`              | Kiểm tra xem rule đã tồn tại trong chain chưa |
| `--delete`           | `-D`         | `chain_name rule_spec` hoặc `rulenum` | Xóa rule khỏi chain |
| `--insert`           | `-I`         | `chain_name [rulenum] rule_spec`    | Thêm rule vào đầu hoặc vị trí cụ thể trong chain |
| `--replace`          | `-R`         | `chain_name rulenum rule_spec`      | Thay thế rule tại vị trí rulenum |
| `--list`             | `-L`         | `[chain_name]`                       | Liệt kê rule trong chain hoặc toàn bộ table (mặc định: `filter`) |
| `--list-rules`       | `-S`         | `[chain_name]`                       | In ra các rule dưới dạng rule_spec |
| `--flush`            | `-F`         | `[chain_name]`                       | Xóa tất cả rule trong chain hoặc tất cả chain |
| `--zero`             | `-Z`         | `[chain_name] [rulenum]`            | Đặt lại bộ đếm packet/byte |
| `--new-chain`        | `-N`         | `chain_name`                         | Tạo một chain mới |
| `--delete-chain`     | `-X`         | `[chain_name]`                       | Xóa chain rỗng (không có rule) |
| `--policy`           | `-P`         | `chain_name target`                 | Đặt chính sách mặc định cho chain |
| `--rename-chain`     | `-E`         | `old_chain new_chain`               | Đổi tên chain do người dùng tạo |

### 1.2. Các tham số cho rule (RULE MATCH PARAMETERS)

| **Tham số đầy đủ**   | **Viết tắt** | **Giá trị theo sau**                 | **Ý nghĩa** |
|----------------------|--------------|--------------------------------------|-------------|
| `[!] --protocol`     | `[!] -p`     | `protocol_name`                      | Giao thức (tcp, udp, icmp, all, ...) |
| `[!] --source`       | `[!] -s`     | `address[/mask][,...]`               | Địa chỉ IP nguồn |
| `[!] --destination`  | `[!] -d`     | `address[/mask][,...]`               | Địa chỉ IP đích |
| `--jump`             | `-j`         | `target`                             | Hành động khi packet khớp rule (ACCEPT, DROP, REJECT, ...) |
| `--goto`             | `-g`         | `chain`                              | Chuyển xử lý sang chain khác (nhưng không quay lại chain cũ) |
| `[!] --in-interface` | `[!] -i`     | `name[+]`                            | Tên interface nhận packet (INPUT, FORWARD, PREROUTING) |
| `[!] --out-interface`| `[!] -o`     | `name[+]`                            | Tên interface gửi packet (OUTPUT, FORWARD, POSTROUTING) |
| `[!] --fragment`     | `[!] -f`     | *(không cần giá trị)*               | Nhắm vào các gói tin phân mảnh |
| `--set-counters`     | `-c`         | `packets bytes`                      | Khởi tạo bộ đếm cho rule |

## 2. Xem rules và ý nghĩa các cột trong iptables</a>
- Để xem các rule hiện có trong `iptables` ta sử dụng câu lệnh sau:

            iptables --list hoặc iptables -L

kết quả sẽ hiển thị tương tự như sau:

![image](https://github.com/user-attachments/assets/7241228c-90cd-4d98-b984-ebd399a990fe)


nhìn vào kết quả trên, ta có thể thấy được nội dung với các cột như sau:

- `target`: Thể hiện giá trị của `target` bao gồm các giá trị: ACCEPT, DROP, REJECT, RETURN, LOG ...
- `prot`: Quy định protocol của rule được match với rule. chúng bao gồm các protocol có trong `/etc/protocols`.
- `opt`: Ít khi được sử dụng, nó mô tả các tùy chọn có liên quan đến IP
- `source`: Chỉ ra một địa chỉ IP, subnet là nơi xuất phát của traffic hoặc có thể bất cứ đâu (anywhere).
- `destination`: Chỉ ra một địa chỉ IP, subnet là đích đến của traffic hoặc có thể bất cứ đâu (anywhere).

    mỗi một dòng sau `target prot opt source destination` được xem là một rule trong 1 `chain`, và `Chain INPUT (policy ACCEPT)` biểu thị `chain` có tên là `INPUT` và `policy` của `chain` là `ACCEPT` và `1 references` biểu thị số lượng `chain` có liên quan đến `chain` này. Điều này đúng cho thông tin của các chain.

- Mặc định khi sử dụng câu lệnh trên, ta chỉ có thể xem được các rule bên trong table `filter`. Để có thể xem được các rule trong các tables khác, ta cần sử dụng tham số `-t` hoặc `--table` để khai báo tên tables ta muốn xem rules có bên trong. Cụ thể của câu lệnh được thực hiện như sau:

            iptables -t tables_name -L

    hoặc

            iptables --table tables_name --list

     trong đó giá trị của `tables_name` có thể là tên của 1 trong 5 bảng của iptables.

## 3. Cách thức hoạt động cơ bản của IPtables

Toàn bộ dữ liệu trong gói tin gửi đi được internet định dạng. Tiếp đến, Linux kernel thực hiện lọc chúng thông qua giao diện bảng các bộ lọc. Và IPtables chính là ứng dụng dòng lệnh, đồng thời cũng là tường lửa của Linux. Nó giúp người dùng cấu hình, duy trì và quản lý các bảng này.

Người dùng có thể thiết lập nhiều bảng khác nhau. Trong đó, mỗi bảng chứa nhiều chuỗi và mỗi chuỗi ứng với một quy tắc. Các quy tắc này sẽ định nghĩa hành động được thực thi khi gói tin phù hợp với nó.

Một mục tiêu (target) chỉ đưa ra khi đã xác định được một gói tin. Mục tiêu có thể là chuỗi khớp với một trong những giá trị như: ACCEPT (gói tin được đi qua), DROP (gói tin bị từ chối, không được đi qua), RETURN (chuỗi hiện tại bị bỏ qua, quy lại quy tắc kế tiếp của chuỗi mà gói tin được gọi).

## 4. Các quy tắc trong IPtables

Để xem các quy tắc đang có trong IPtables Linux, bạn sử dụng câu lệnh sau: 

IPtables -L –v

TARGET    PROT OPT  IN OUT SOURCE DESTINATION

ACCEPT    all --   lo any anywhere   anywhere

ACCEPT    all --   any any anywhere   anywhere ctstate  RELATED,ESTABLISHED

ACCEPT    tcp --   any any anywhere   anywhere tcp dpt:ssh

ACCEPT    tcp --   any any anywhere   anywhere tcp dpt:http

ACCEPT    tcp --   any any anywhere   anywhere tcp dpt:https

DROP      all --   any any anywhere   anywhere

Trong đó:

- TARGET: Hành động được thực thi.
- PROT: Chính là Protocol. Nó là giao thức được sử dụng để thực thi quy tắc với 3 chọn lựa all, TCP hay UDP. Hầu hết những ứng dụng phổ biến như FTP, sFTP, SSH,… đều dùng TCP.
- IN: Quy tắc được áp dụng đối với những gói tin đi vào từ một interface được xác định hay cho tất cả các interface.
- OUT: Quy tắc được áp dụng đối với những gói tin đi ra từ một interface được xác định hay cho tất cả các interface.
- DESTINATION: Là địa chỉ của lượt truy cập áp dụng quy tắc. 
- ACCEPT    all - - lo any anywhere   anywhere: Là chấp nhận tất cả các gói tin từ interface lo (đây là một interface ảo của nội bộ).
- ACCEPT    all - - any any anywhere   anywhere ctstate  RELATED,ESTABLISHED: Là chấp nhận tất cả các gói tin của kết nối. 
- ACCEPT    tcp - - any any anywhere   anywhere tcp dpt:ssh: Là chấp nhận tất cả các gói tin của giao thức SSH đến từ bất kỳ interface, không loại trừ bất cứ IP nguồn hay đích. Mặc định, hệ thống hiển thị dpt:ssh, tức là cổng 22 của SSH. Trong trường hợp, bạn muốn đổi thành cổng khác thì điền giá trị cổng vào. 
- ACCEPT    tcp - - any any anywhere   anywhere tcp dpt:http: Mặc định hiển thị là http, tức cho phép kết nối đến cổng 80.
- ACCEPT    tcp - - any any anywhere   anywhere tcp dpt:https: Mặc định hiển thị là https, tức cho phép kết nối đến cổng 443.
- DROP      all - -   any any anywhere   anywhere: Toàn bộ những gói tin không khớp với các quy tắc trên sẽ bị loại bỏ.

Các tùy chọn của IPtables Centos 7

Một số tùy chọn được dùng để chỉ định thông số của IPtables là:
```
-t: Tên table
-p: Loại giao 
-i: Card mạng vào
-o: Card mạng ra
-s <địa_chỉ_ip_nguồn>: Địa chỉ IP nguồn
-d <địa_chỉ_ip_đích>: Địa chỉ IP đích
–sport: Cổng nguồn
–dport: Cổng đích
```

Các tùy chọn được dùng thao tác với chain là: 
```
IPtables-N: Tạo chain mới
IPtables –X: Xóa các quy tắc đã tạo trong chain 
IPtables –P: Thiết lập chính sách cho các chain
IPtables –L: Liệt kê các quy tắc trong chain
IPtables –F: Xóa các quy tắc trong chain 
IPtables –Z: Chuyển bộ đếm packet về giá trị 0
```

Các tùy chọn được dùng thao tác với quy tắc là:
```
-A: Thêm quy tắc
-D: Xóa quy tắc
-R: Thay thế quy tắc
-I: Chèn thêm quy tắc
```

Các lệnh cơ bản của IPtables 

Tạo một quy tắc mới, bạn sử dụng câu lệnh sau
```
IPtables -A INPUT -i lo -j ACCEPT
```

Trong đó:

- `A INPUT`: Là kiểu kết nối được áp dụng.

- `i lo`: Là thông tin thiết bị mạng được áp dụng.

- `j ACCEPT`: Là hành động được áp dụng đối với quy tắc.

Bạn thực hiện nhập lại lệnh `IPtables -L –v` thì một quy tắc mới sẽ xuất hiện.

`after-created-IPtables-rule`

Sau khi tiến hành thêm mới hoặc điều chỉnh quy tắc, bạn phải gõ lệnh lưu rồi tái khởi động IPtables để các thay đổi được cập nhật và có thể áp dụng. 

Câu lệnh:
```
service IPtables save 

service IPtables restart
```
Sau đó, bạn tiếp tục thêm một quy tắc mới để lưu lại các kết nối đang có, đồng thời, tránh tình trạng tự block khỏi máy chủ. Cú pháp câu lệnh:

```
IPtables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
```

Kế đến, bạn nhập lệnh để cho phép các port (SSH có port là 22, HTTP có port là 80, HTTPs có port là 443) có thể được truy cập từ ngoài vào bằng giao thức TCP. 

Câu lệnh như sau:
```
IPtables -A INPUT -p tcp --dport 22 -j ACCEPT
```

Trong đó:
```
-p tcp: Giao thức áp dụng (ví dụ như tcp, udp, all)

- - dport 22: Cổng áp dụng và 22 là giá trị của SSH
```

Sau cùng, bạn tiến hành chặn tất cả các kết nối từ bên ngoài không đáp ứng điều kiện của các quy tắc trên. Câu lệnh có dạng:

```
IPtables -A INPUT -j DROP
```

Bổ sung 1 quy tắc mới

Nếu muốn chèn 1 quy tắc mới vào bất kỳ vị trí nào, bạn chỉ việc thay tham số -A table bằng –I (INSERT).

```
IPtables -I INPUT 2 -p tcp --dport 8080 -j ACCEPT
```

Xóa 1 quy tắc

Để xóa 1 quy tắc ở vị trí bất kỳ, bạn dùng tham số -D.

Ví dụ, xóa 1 rule ở hàng 4 thì câu lệnh có dạng:
```
IPtables -D INPUT 4
```
Nếu muốn xóa tất cả các quy tắc chứa hành động DROP thì câu lệnh là: 
```
IPtables -D INPUT -j DROP
```
Cài đặt IPtables trên Centos 7 để mở port VPS

Để mở cổng trong IPtables, bạn chèn chuỗi ACCEPT PORT vào cấu trúc câu lệnh mở port xxx:
```
# IPtables -A INPUT -p tcp -m tcp --dport xxx -j ACCEPT
```
Với A (viết tắt của Append) là chèn vào chuỗi INPUT, tức chèn ở vị trí cuối.
```
# IPtables -I INPUT -p tcp -m tcp --dport xxx -j ACCEPT
```
Với I (viết tắt của Insert) là chèn vào chuỗi INPUT, tức chèn ở dòng được chỉ định rulenum.

Đồng thời, bạn chèn quy tắc vào đầu để tránh xung đột với quy tắc gốc.

Mở port SSH

Khi thực hiện mở port SSH để truy cập VPS sẽ cho phép kết nối SSH ở mọi thiết bị tại bất kỳ vị trí nào và không phân biệt người dùng.

Cú pháp câu lệnh:
```
# IPtables -I INPUT -p tcp -m tcp --dport 22 -j ACCEPT

ACCEPT

tcp  -- anywhere
anywhere

cp dpt:ssh 
```
Nếu bạn đổi cổng khác thì IPtables hiển thị giá trị cổng tương ứng. 

Trong trường hợp, bạn chỉ cho kết nối VPS thông qua SSH từ một IP xác định thì câu lệnh lúc này là: 
```
# IPtables -I INPUT -p tcp -s xxx.xxx.xxx.xxx -m tcp --dport 22 -j ACCEPT
```
Với xxx.xxx.xxx.xxx là địa chỉ của IP cho phép kết nối.

Như vậy, IPtables sẽ thêm quy tắc:
```
ACCEPT  tcp  -- xxx.xxx.xxx.xxx    anywhere         tcp dpt:ssh
```
Mở port Web Server
Khi bạn muốn cho các truy cập vào máy chủ web thông qua cổng mặc định là 80 và 443 thì câu lệnh có dạng:
```
# IPtables -I INPUT -p tcp -m tcp --dport 80 -j ACCEPT

# IPtables -I INPUT -p tcp -m tcp --dport 443 -j ACCEPT
```
Lúc đó, IPtables sẽ hiển thị theo mặc định tương ứng với cổng là HTTP và HTTPS:
```
ACCEPT tcp -- anywhere       anywhere    tcp dpt:http

ACCEPT tcp -- anywhere      anywhere     tcp dpt:https
```

Mở port Mail
- Khi bạn muốn cho phép user sử dụng máy chủ SMTP thông qua cổng mặc định 25 và 465 thì câu lệnh sẽ là:
```
# IPtables -I INPUT -p tcp -m tcp --dport 25 -j ACCEPT

# IPtables -I INPUT -p tcp -m tcp --dport 465 -j ACCEPT
```
Lúc này IPtables cũng hiển thị tương ứng SMTP và URD mặc định là
```
ACCEPT   tcp  -- anywhere      anywhere           tcp dpt:smtp

ACCEPT   tcp  -- anywhere      anywhere           tcp dpt:urd
```
- Tương tự như trên, nếu muốn mở cổng POP3 có giá trị mặc định là 110 và 995 để cho phép user có thể đọc mail trên máy chủ thì câu lệnh như sau:
```
# IPtables -A INPUT -p tcp -m tcp --dport 110 -j ACCEPT

# IPtables -A INPUT -p tcp -m tcp --dport 995 -j ACCEPT
```
Và IPtables hiển thị POP3 và POP3S tương ứng là:
```
ACCEPT tcp  --  anywhere         anywhere        tcp dpt:pop3

ACCEPT tcp  --  anywhere         anywhere        tcp dpt:pop3s
```
- Đối với giao thức IMAP mail protocol có cổng là 143 và 993 thì câu lệnh như sau:
```
# IPtables -A INPUT -p tcp -m tcp --dport 143 -j ACCEPT

# IPtables -A INPUT -p tcp -m tcp --dport 993 -j ACCEPT
```
Lúc này, IPtables hiển thị IMAP và IMAPS mặc định sẽ là:
```
ACCEPT tcp  --  anywhere         anywhere        tcp dpt:imap

ACCEPT tcp  --  anywhere         anywhere        tcp dpt:imaps
```

Chặn truy cập của một địa chỉ IP 
Câu lệnh có dạng:
```
# IPtables -A INPUT -s IP_ADDRESS -j DROP
```
Nếu muốn chặn 1 địa chỉ IP cụ thể truy cập vào 1 port xác định thì câu lệnh sẽ là: 
```
#IPtables -A INPUT -p tcp -s IP_ADDRESS –dport PORT -j DROP
```
Nếu muốn kiểm tra lại tất cả các rule sau khi thiết lập, bạn sử dụng một trong 2 câu lệnh sau:
```
# service IPtables status
```
Hay:
```
# IPtables -L –n
```
Trong đó:

Tham số -n: là địa chỉ IP. Ví dụ, khi muốn chặn kết nối từ 1 IP xác định, lúc này IPtables hiển thị là xxx.xxx.xxx.xxx với tham số -n

Sau đó, bạn lưu lại tất cả các thiết lập. Câu lệnh là: 
```
# IPtables-save | sudo tee /etc/sysconfig/IPtables
```
Hay có thể sử dụng lệnh sau:
```
# service IPtables save
```
IPtables: Saving firewall rules to /etc/sysconfig/IPtables:[ OK ]
