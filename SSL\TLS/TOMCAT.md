
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


## Cấu Hình SSL trên TOMCAT

