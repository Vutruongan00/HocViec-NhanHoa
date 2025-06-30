
# SSL trên TOMCAT

## Cài đặt TOMCAT
- Tạo user 
```
sudo useradd -m -d /opt/tomcat -U -s /bin/false tomcat
```
- Cài IDK 
```
sudo apt update
sudo apt install default-jdk
```
- Kiểm tra 
```
java -version 
```
- Tải file cài 
```bash!
cd /tmp

wget https://adp-mirror-vm1-he-fi.apache.org/tomcat/tomcat-10/v10.1.4/bin/apache-tomcat-10.1.4.tar.gz
```
- Giải nén 
```
sudo tar xzvf apache-tomcat-10.1.4.tar.gz -C /opt/tomcat --strip-components=1
```
- Cấp quyền 
```
sudo chown -R tomcat:tomcat /opt/tomcat/
sudo chmod -R u+x /opt/tomcat/bin
```
- Tạo service 
```
sudo nano /etc/systemd/system/tomcat.service
```
```
[Unit]
Description=Tomcat
After=network.target

[Service]
Type=forking

User=tomcat
Group=tomcat

Environment="JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64"
Environment="JAVA_OPTS=-Djava.security.egd=file:///dev/urandom"
Environment="CATALINA_BASE=/opt/tomcat"
Environment="CATALINA_HOME=/opt/tomcat"
Environment="CATALINA_PID=/opt/tomcat/temp/tomcat.pid"
Environment="CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC"

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh

RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target
```

- Khởi động 
```
sudo systemctl daemon-reload
sudo systemctl start tomcat
sudo systemctl status tomcat
```
- Mở port 
```
sudo ufw allow 8080
```
![image](https://github.com/user-attachments/assets/ce05bea9-0d95-4774-993f-c205b11d26b2)


### Tạo Virtual Host trong Tomcat
- Mở tệp `server.xml`
```
 nano /opt/tomcat/conf/server.xml
```

- Trong thẻ Engine, Thêm một phần tử `<Host>` mới
```xml!

      <Host name="antvpro.io.vn"  appBase="webapps"
              unpackWARs="true" autoDeploy="true">
            <Alias>www.antvpro.io.vn</Alias>

            <Context path="" docBase="/var/www/antvpro.io.vn/public_html" reloadable="true" />

            <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
                   prefix="antvpro_access_log." suffix=".txt"
                   pattern="%h %l %u %t &quot;%r&quot; %s %b" />
      </Host>

```
![image](https://github.com/user-attachments/assets/cbd1cd94-9b9e-4cde-8740-a19d5ad6e974)

- Khởi động lại Tomcat: 

![image](https://github.com/user-attachments/assets/aa51e712-f0e4-40e8-a938-1b735e07d7da)

---

## Cấu Hình SSL trên TOMCAT

### **Bước 1: Chuẩn bị chứng chỉ SSL**

- Có 3 file sau từ nhà cung cấp chứng chỉ (CA):
    - `certificate.crt`: chứng chỉ chính
    - `private.key`: khóa riêng
    - `ca_bundle.crt`: chuỗi chứng chỉ trung gian (intermediate CA)

### Bước 2: Gộp các file chứng chỉ `.crt` và file private `.key` thành `.pfx` (PKCS#12)

```bash!
openssl pkcs12 -export -out certificate.pfx -inkey antvpro.key -in certificate.crt -certfile ca_bundle.crt
```
### Bước 3: Cấu hình SSL trong `server.xml`
    - Mở file `conf/server.xml` tìm đến đoạn `<Connector>` có `port="8443"` hoặc thêm mới:
```xml!
<Connector port="8443" protocol="org.apache.coyote.http11.Http11NioProtocol"
               SSLEnabled="true"
               maxThreads="150"
               scheme="https" secure="true"
               sslProtocol="TLSv1.2+TLSv1.3">
        <SSLHostConfig hostName="_default_">
            <Certificate certificateKeystoreFile="/etc/ssl/certificate.pfx"
                         certificateKeystorePassword="123456"
                         type="RSA" />
        </SSLHostConfig>
    </Connector>
```

> Nếu chưa dùng cổng 443, có thể để port="8443" và truy cập bằng https://yourdomain.com:8443.

![image](https://github.com/user-attachments/assets/9931f06c-42bd-4893-9282-26c6c5265038)


### Bước 4: Khởi động lại Tomcat
sudo systemctl restart tomcat

![image](https://github.com/user-attachments/assets/09014887-0a59-4dc2-87b4-454d9282ce0e)



