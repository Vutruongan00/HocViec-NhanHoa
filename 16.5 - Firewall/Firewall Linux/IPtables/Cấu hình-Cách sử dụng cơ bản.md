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

- ## 3 Các câu lệnh hay được sử dụng trong iptables
