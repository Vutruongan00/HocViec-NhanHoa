> # Bảo Mật MongoDB 

# 1. Mã hóa dữ liệu (Encryption)
> MongoDB cung cấp các giải pháp mã hóa ở nhiều cấp độ:
> 
> 1. **Mã hóa khi truyền tải (Encryption in Transit)**: Sử dụng TLS/SSL.
> 2. **Mã hóa khi lưu trữ (Encryption at Rest)**: Mã hóa dữ liệu trên đĩa. -- **Chỉ hỗ trợ trong MongoDB Enterprise**
> 3. **Mã hóa cấp trường (Client-Side Field Level Encryption - CSFLE)**: Mã hóa các trường dữ liệu cụ thể ở phía client. -- **Chỉ hỗ trợ trong MongoDB Enterprise**

### Mã hóa khi truyền tải (Encryption in Transit) - SSL/TLS
- Mã hóa SSL/TLS giữa client ↔ server.

- Bảo vệ khỏi sniffing (gián điệp mạng).

### Cách triển khai:
- Tạo thư mục lưu chứng chỉ:
```bash!
mkdir -p /etc/mongod/ssl
```
- Cài(chuẩn bị) chứng chỉ SSL/TLS - Hoặc tự sinh:
```bash=
cd /etc/mongod/ssl

openssl genrsa -out ca.key 4096
openssl req -new -x509 -days 365 -key ca.key -out ca.crt 
openssl genrsa -out server.key 4096
openssl req -new -key server.key -out server.csr 
openssl x509 -req -days 365 -in server.csr CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt
cat server.key server.crt > server.pem
```
- Chỉnh sửa file cấu hình: ` nano /etc/mongod.conf`
- Thêm/sửa các thông số sau để bật TLS:
```yaml
net:
  tls:
 	mode: requireTLS
 	certificateKeyFile: /etc/mongod/ssl/server.pem
 	CAFile: /etc/mongod/ssl/ca.crt
 	allowConnectionsWithoutCertificates: true
```
- Restart để apply: ` systemctl restart mongod `

- Lưu file `ca.crt` từ máy chủ để truy cập được từ Client


---
# 2. Audit và theo dõi truy cập (MongoDB Enterprise)
# 

