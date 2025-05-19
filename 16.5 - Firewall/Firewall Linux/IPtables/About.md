# IPtables
## 1 Giới thiệu về iptables
### 1.1 Vai trò, chức năng

- `iptables` là một ứng dụng dùng để quản lý filtering gói tin và NAT rules hoạt động trên console của linux rất nhỏ và tiện dụng. Được cung cấp miễn phí nhằm nâng cao tính bảo mật trên hệ thống Linux.

- `iptables` bao gồm 2 phần là `netfilter` nằm bên trong nhân Linux và `iptables` nằm ở vùng ngoài nhân. `iptables` chịu trách nhiệm giao tiếp với người dùng và sau đó đẩy rules của người dùng vào cho `netfilter` xử lý. `netfilter` thực hiện công việc lọc các gói tin ở mức IP. `netfilter` làm việc trực tiếp ở trong nhân của Linux nhanh và không làm giảm tốc độ của hệ thống.

- `iptables` cung cấp các tính năng sau:

    - Có khả năng phân tích gói tin hiệu quả.
    - Filtering gói tin dựa vào MAC và một số cờ hiệu (flags) trong TCP Header.
    - Cung cấp kỹ thuật NAT, chi tiết cho các tùy chọn để ghi nhận sự kiện hệ thống.
    - Có khả năng ngăn chặn một số cơ chế tấn công theo kiểu DoS.
    - Xây dựng một hệ thống tường lửa (firewall).
    - Cung cấp, xây dựng và quản lý các rule để xử lý các gói tin.

        
### 1.2 Cách cài đặt iptales 

- `iptables` được cài đặt mặc định trong các hệ điều hành Linux. Riêng trên hệ điều hành (CentOS) có cung cấp `firewalld` được sử dụng mặc định thay cho `iptables` và trên (Ubuntu) có cung cấp `ufw` dành riêng cho chức năng `netfilter firewall`. Về bản chất thì các phần mềm hoạt động với chức năng là như nhau nên trong khuôn khổ nội dung của bài viết này, ta chỉ sử dụng và đề cập đến `iptables` nhiều hơn tất cả.

- Đối với Ubuntu, để sử dụng `iptables`, ta cần khởi động nó với 2 câu lệnh sau:

                service ufw start
                service ufw re-start

- Đối với CentOS, để sử dụng `iptables`, ta cần phải cài đặt nó và dừng hoạt động `firewalld` với các câu lệnh sau:

                yum install -y iptables iptables-services

                systemctl stop firewalld
                systemctl disable firewalld

                systemctl start iptables
                systemctl enable iptables

- Tiếp theo ta cần cấu hình cho `iptables` để có thể lưu lại các rule đã tạo ra khi ta restart lại iptables bằng cách thực hiện như sau:

                vi /etc/sysconfig/iptables-config

    tìm đến 2 dòng có nội dung:

                IPTABLES_SAVE_ON_STOP="no"
                IPTABLES_SAVE_ON_RESTART="no"

            ta đổi các giá trị `no` thành `yes` sau đó lưu lại để hoàn tất.


## 2. Các khái niệm cần biết trước khi tìm hiểu về iptables</a>
### 2.1 Khái niệm về NAT

- Mỗi một kết nối trước khi được xử lý đều có địa chỉ nguồn (source ip address) và địa chỉ đích (destination ip address) được chứa trong thông tin của các gói tin. NAT trong `netfilter` đơn giản là việc thực hiện thay đổi địa chỉ đích và port theo một cách mong muốn.

- Khi các gói tin được nhận, các kết nối cũng sẽ được thực so sánh lại một lần nữa với địa chỉ đích (bao gồm cả port). Việc không phân mảnh cũng là một yêu cầu quan trọng dành cho NAT. (Nếu cần thiết, các gói tin IPv4 cũng sẽ được sắp xếp lại theo ngăn xếp giống như bình thường.)


### 2.2 Khái niệm về filtering

- Là quá trình chặn bắt gói tin theo một số tiêu chí mà ta đề ra.

- Giả sử: Khi một gói tin đi đến sẽ chứa các giá trị về địa chỉ IP nguồn (source address), địa chỉ IP đích (destination address) và các port tương ứng (port nguồn và port đích). Khi ta thực hiện `filtering` gói tin theo tiêu chí địa chỉ IP nguồn. Thì các gói tin có địa chỉ IP nguồn khớp với địa chỉ IP mà ta đề ra sẽ được giữ lại để chờ xử lý.


### 2.3 Khái niệm về mangle

- Là quá trình bóc tách gói tin và chịu trách nhiệm thay đổi bits của QoS (Quality of Services) trong IP Header bởi vì `mangle` làm việc với các gói tin IP.

### 2.4 Khái niệm về chain

- `chain` là một quy tắc xử lý các gói tin bao gồm nhiều rules có liên quan tới nhau.

- Mỗi `table` sẽ được tạo với một hoặc nhiều `chain`. `chain` cho phép lọc gói tin tại các điểm khác nhau. `iptable` có thể được thiết lập đối với các loại `chain` như sau:

    + chain `PREROUTING`: Các rule thuộc `chain` này sẽ được áp dụng ngay sau khi gói tin vừa đi vào đến dải mạng (Network Interface). `chain` này chỉ có thể có ở table NAT, RAW và MANGLE.

    + chain `INPUT`: Các rule thuộc `chain` này sẽ áp dụng cho các gói tin ngay trước khi gói tin đi vào hệ thống. `chain` này có trong table MANGLE và FILTER.

    + chain `OUTPUT`: Các rule thuộc `chain` này áp dụng ngay cho các gói tin đi ra từ hệ thống. `chain` có trong table MANGLE, RAW và FILTER.

    + chain `FORWARD`: Các rule thuộc `chain` này áp dụng các gói tin được chuyển tiếp qua hệ thống. `chain` có trong table MANGLE.

    + chain `POSTROUTING`: Các rule thuộc `chain` này áp dụng cho các gói tin tới dải mạng (Network Interface). `chain` này có trong table MANGLE và NAT.
            
    + Hãy quan sát hình ảnh dưới đây để hiểu về thứ tự xử lý các table và chain trong khi xử lý gói tin:

                > ![table-flow](../images/table-flow.png)


### 2.5 Khái niệm về rule
 `Rule` là là một luật, hành động cụ thể xử lý gói tin ứng với mỗi trường hợp, tiêu chí mà ta đề ra.

### 2.6 Khái niệm về port

- `port` là một vị trí nào đó mà gói tin TCP/UDP vào và ra trong thiết bị. Một địa chỉ IP có rất nhiều `port`.
- Tất cả các `port` đều được định danh bởi các con số - `port number`
- Một vài dịch vụ và port tương ứng:

    | Services | Port number |
    | ---| ---|
    | DNS      |      53     |
    | HTTP     |      80     |
    | FTP      |     20/21   |
    | SSH      |      22     |
    | Telnet   |      23     |
    | ICMP     |      5813   |
    | POP3     |      110    |
    | SMTP     |      25     |

### 2.7 Khái niệm về target

- Mỗi một `chain` là một danh sách các luật có thể được thiết lập cho các gói tin. Mỗi một luật sẽ cần phải khai báo những gì cần phải làm với gói tin được gọi là `target`.

- Nói một cách đơn giản thì các hành động áp dụng cho các gói tin được gọi là `target` . Đối với những gói tin đúng theo rule mà chúng ta đặt ra thì các hành động (`target`) có thể thực hiện được đó là:

- `ACCEPT`: chấp nhận gói tin, cho phép gói tin đi qua hay đi vào hệ thống.

- `DROP`: loại bỏ gói tin, không phản hồi lại gói tin giống như việc gói tin đó được gửi đến một hệ thống không tồn tại.

- `RETURN`: Dừng thực thi xử áp dụng `rules` tiếp theo trong `chain` hiện tại đối với gói tin. Việc kiểm soát sẽ được trả về đối với `chain` đang gọi.

- `REJECT`: Thực hiện loại bỏ gói tin và gửi lại gói tin phản hồi thông báo lỗi. Ví dụ: 1 bản tin “connection reset” đối với gói TCP hoặc bản tin “destination host unreachable” đối với gói UDP và ICMP.

- `LOG`: Chấp nhận gói tin và có ghi lại log.


## 3 Các bảng và chức năng sử dụng trong iptables
- Trong kiến trúc của iptable có sử dụng các bảng để quy định các `chain` cùng thực hiện các chức năng trong một không gian nhất định với công việc nhất định. Hiện tại, `iptable` cung cấp cho chúng ta 5 loại table (để có thể sử dụng được các table, trong kernel ta cần phải có module của nó). Một số module:

    + `iptables_nat module` cần cho một số loại NAT.

    + `ip_conntrack_ftp module` cần cho việc thêm vào giao thức FTP.

    + `ip_conntrack module` giữ trạng thái liên kết với giao thức TCP.

    + `ip_nat_ftp module` cần được tải cho những máy chủ FTP sau một firewall NAT

        > CHÚ Ý: file /etc/sysconfig/iptables không cập nhật những môdun tải về, vì vậy chúng ta phải thêm những trạng thái đó vào file /etc/rc.local và chạy nó tại cuối mỗi lần boot lại.

- Những mẫu trong phần này bao gồm những trạng thái được lưu trong file `/etc/rc.local`:

        ```sh
        #File:/etc/rc.local

        #Module to track the state of connections modprobe ip_conntrack

        #Load the iptables active FTP module, requires ip_conntrack modprobe

        #ip_conntrack_ftp

        #Load iptables NAT module when required modprobe iptable_nat

        #Module required for active an FTP server using NAT modprobe ip_nat_ftp
        ```

### 3.1 NAT Table

- Cho phép route các gói tin đến các host khác nhau trong mạng bằng cách thay đổi IP nguồn và IP đích của gói tin. Table này quy định và cho phép các kết nối có thể truy cập tới các dịch vụ không được truy cập trực tiếp. Bao gồm 3 thành phần:

    + `PREROUTING chain` – Thay đổi gói tin trước khi định tuyến, điều này có nghĩa là việc dịch gói tin sẽ xảy ra ngay lập tức sau khi gói tin đến hệ thống. Điều này thực hiện thay đổi địa chỉ IP đích thành một địa chỉ nào đó sao cho phù hợp với việc định tuyến trên máy chủ cục bộ - DNAT.

    + `POSTROUTING chain` – Thay đổi gói tin sau khi định tuyến, điều này có nghĩa là dịch gói tin khi gói tin ra khỏi hệ thống. Điều này thực hiện thay đổi địa chỉ IP nguồn của gói tin thành một địa chỉ nào đó phù hợp với việc định tuyến trên máy chủ đích - SNAT.

    + `OUTPUT chain` – thực hiện NAT cho các gói tin được thực hiện cục bộ trên firewall.

### 3.2 FILTER Table</a>

- Đây là table được sử dụng mặc định bởi `iptables` khi bạn tạo các `chain mà không khai báo cho `chain` đó thuộc vào table nào. Table hoạt động với việc quy định việc quyết định có cho phép gói tin được chuyển đến địa chỉ đích hay không. Bao gồm 3 thành phần:

    + `INPUT chain`: Các gói tin đến firewall. Áp dụng đối với các gói tin đến máy chủ cục bộ.

    + `OUTPUT chain`: Các gói tin đi ra khỏi firewall. Áp dụng với các gói tin được tạo ra cục bộ và đi ra khỏi máy chủ.

    + `FORWARD chain`: Áp dụng đối với các gói tin được định tuyến đi qua máy chủ.

### m3.3 MANGLE Table

- Table này liên quan đến việc sửa header của gói tin, ví dụ chỉnh sửa giá trị các trường TTL, MTU, Type of Service.

- Bao gồm các thành phần sau:


    + PREROUTING chain

    + OUTPUT chain

    + FORWARD chain

    + INPUT chain

    + POSTROUTING chain


### 3.4 RAW Table

- Bảng này được sử dụng chủ yếu dành cho việc cấu hình sử dụng `chain` có sẵn. Bao gồm:

    + PREROUTING chain

    + OUTPUT chain


### 3.5 Security Table

- Đây là bảng được sử dụng cho `Mandatory Access Control (MAC)` - kiểm soát truy cập bắt buộc đối với các rule về network.

- MAC được triển khai bởi `Linux Security Modules` được biết đến như là SELinux. Gói tin được chuyển đến table này sau khi đi qua `FILTER table` và cho phép một vài `Discretionary Access Control (DAC)` - kiểm soát truy cập tùy ý trong `FILTER table` gây ảnh hưởng trước các MAC rule. Table này cung cấp các chain có sẵn là:

    + INPUT chain.

    + OUTPUT chain.

    + FORWARD chain.

## 4. Cách thức hoạt động cơ bản của IPtables

Toàn bộ dữ liệu trong gói tin gửi đi được internet định dạng. Tiếp đến, Linux kernel thực hiện lọc chúng thông qua giao diện bảng các bộ lọc. Và IPtables chính là ứng dụng dòng lệnh, đồng thời cũng là tường lửa của Linux. Nó giúp người dùng cấu hình, duy trì và quản lý các bảng này.

Người dùng có thể thiết lập nhiều bảng khác nhau. Trong đó, mỗi bảng chứa nhiều chuỗi và mỗi chuỗi ứng với một quy tắc. Các quy tắc này sẽ định nghĩa hành động được thực thi khi gói tin phù hợp với nó.

Một mục tiêu (target) chỉ đưa ra khi đã xác định được một gói tin. Mục tiêu có thể là chuỗi khớp với một trong những giá trị như: ACCEPT (gói tin được đi qua), DROP (gói tin bị từ chối, không được đi qua), RETURN (chuỗi hiện tại bị bỏ qua, quy lại quy tắc kế tiếp của chuỗi mà gói tin được gọi).

## 5. Các quy tắc trong IPtables

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
