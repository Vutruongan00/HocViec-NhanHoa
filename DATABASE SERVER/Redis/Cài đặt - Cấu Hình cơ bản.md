
># REDIT
> **Yêu cầu phần cứng tối thiểu:**
>   - OS: Linux (Ubuntu, CentOS), Windows (chỉ dùng bản Redis port hoặc WSL)
> 	- CPU: Tối thiểu 1 core, khuyến nghị ≥ 2 cores
> 	- RAM: Tối thiểu 512MB, khuyến nghị ≥ 1GB
> 	- Ổ đĩa: Tối thiểu 50MB trống, tùy vào cấu hình persistence
> 	- network: Cổng mặc định Redis: 6379

# Cài Đặt 
## Cài đặt trên Linux (Ubuntu)
```
sudo apt update
sudo apt install redis-server -y
```
- Kiểm tra 
```
systemctl status redis-server
redis-server -v
```
![image](https://github.com/user-attachments/assets/5ffbc8a3-d962-425e-8bf2-42c0970e3b40)

- **File cấu hình chính:** `/etc/redis/redis.conf`

```bash!
nano /etc/redis/redis.conf
```




# Cấu hình tối ưu hiệu năng
- Các thông số cần cấu hình điều chỉnh để tối ưu hiệu năng:

| Thông số                 | Mô tả                                                            |
| ------------------------ | ---------------------------------------------------------------- |
| `maxmemory`              | Giới hạn bộ nhớ tối đa Redis sử dụng                             |
| `maxmemory-policy`       | Chính sách khi đầy bộ nhớ (`noeviction`, `allkeys-lru`, ...)     |
| `appendonly`             | Bật AOF để backup an toàn nhưng làm Redis chậm đi                |
| `save`                   | Thiết lập RDB snapshot để lưu định kỳ (có thể tắt nếu không cần) |
| `tcp-keepalive`          | Giảm tình trạng connection treo lâu                              |
| `timeout`                | Đóng kết nối client không hoạt động sau N giây                   |
| `lazyfree-lazy-eviction` | Cho phép xóa dữ liệu không đồng bộ giúp giảm độ trễ              |

- Ví dụ cấu hình sau: `nano /etc/redis/redis.conf`

```ini!
# Giới hạn RAM Redis dùng tối đa 512MB
maxmemory 512mb
maxmemory-policy allkeys-lru

# Tắt RDB (nếu không cần backup định kỳ)
save ""

# Bật AOF (nhưng cần dùng thêm cấu hình optimize)
appendonly yes
appendfsync everysec

# Kết nối
tcp-keepalive 300
timeout 60
```

# Cấu hình bảo mật cơ bản

> - Bảo vệ Redis khỏi truy cập trái phép (Redis mặc định không có auth!)
> - Hạn chế IP truy cập
> - Mã hóa kết nối (nếu cần)

- **Đặt mật khẩu cho Redis:** `nano /etc/redis/redis.conf`

```ini!
requirepass Qwerty123@
```
![image](https://github.com/user-attachments/assets/f4b1d38c-5167-4eaa-98aa-3c6cd24c8758)

- **Chỉ cho phép truy cập từ localhost (mặc định)**
```ini!
bind 127.0.0.1
# Nếu cần truy cập từ xa: dùng [bind 0.0.0.0] nhưng phải kết hợp firewall/IP whitelist.
```
- **Sử dụng tường lửa/IPTables/UFW để giới hạn IP truy cập**
```bash!
sudo ufw allow from 192.168.1.100 to any port 6379
sudo ufw deny 6379
```
- **Ẩn các lệnh nguy hiểm**
```ini!
rename-command FLUSHALL ""
rename-command FLUSHDB ""
rename-command CONFIG ""
rename-command DEBUG ""
```
