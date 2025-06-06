# Tích Hợp và Kết Nối (MySQL / MariaDB)

## 1. Các giao thức kết nối Database

### a. TCP/IP (Transmission Control Protocol/Internet Protocol)

-  Đây là giao thức mạng phổ biến nhất được sử dụng để kết nối với các database server qua mạng, bao gồm cả Internet và mạng cục bộ. MySQL/MariaDB mặc định sử dụng TCP/IP qua cổng 3306.
-  Cách hoạt động:
    - Client gửi yêu cầu thiết lập kết nối TCP/IP đến địa chỉ IP và cổng của database server (ví dụ: 192.168.1.100:3306).
    - Sau khi kết nối được thiết lập, mọi thông tin (câu lệnh SQL, kết quả truy vấn, thông báo lỗi) được trao đổi qua kết nối TCP đó.
    - Bảo mật: Để ngăn chặn việc nghe lén (eavesdropping) hoặc giả mạo (tampering) dữ liệu khi truyền qua mạng, kết nối TCP/IP thường được bảo vệ bằng cách sử dụng SSL/TLS (Secure Sockets Layer/Transport Layer Security) để mã hóa (như đã trình bày chi tiết ở phần "Mã hóa dữ liệu khi truyền tải (Encryption In Transit)" trong mục V. Bảo Mật Database Server).
- **Cấu hình cho MySQL/MariaDB (`/etc/mysql/mysql.conf.d/mysqld.cnf` trên Linux;  `my.ini` trên Windows)**
 Các tham số này nằm trong phần `[mysqld]`.
    - `port` : Xác định số cổng TCP mà MySQL/MariaDB sẽ lắng nghe các kết nối.
    - `bind-address`: Xác định địa chỉ IP mà MySQL/MariaDB sẽ lắng nghe
        - `127.0.0.1` hoặc `localhost`: Server chỉ chấp nhận kết nối từ chính máy chủ database. Đây là cài đặt an toàn nhất nếu database và ứng dụng đều nằm trên cùng một máy chủ
        `- 0.0.0.0`: Server lắng nghe trên tất cả các địa chỉ IP có sẵn trên máy chủ. Cần rất cẩn trọng khi sử dụng trong môi trường sản xuất mà không có Firewall được cấu hình chặt chẽ, vì nó sẽ mở cổng ra tất cả các interface mạng.
        - Một địa chỉ IP cụ thể (ví dụ: 192.168.1.100): Server chỉ lắng nghe trên địa chỉ IP đó. Hữu ích nếu server có nhiều IP và bạn muốn giới hạn.
        
        
### b. Sockets / Pipes (Kết nối cục bộ)
- Là giao thức được sử dụng để kết nối với database server khi client (ứng dụng) chạy trên cùng một máy chủ với database server. Nó nhanh hơn và bỏ qua lớp mạng TCP/IP cho các kết nối cục bộ.

- **Cách hoạt động:**
    - **Unix Domain Sockets** (Linux/Unix): Giao tiếp xảy ra thông qua một file đặc biệt trên hệ thống file (socket file), thường là /var/run/mysqld/mysqld.sock hoặc /tmp/mysql.sock. Các tiến trình trên cùng máy chủ có thể đọc/ghi vào file này để giao tiếp.
    - **Named Pipes** (Windows): Là một cơ chế giao tiếp liên tiến trình (IPC - Inter-Process Communication) của Windows, tạo ra một "đường ống" ảo để các ứng dụng trên cùng máy tính trao đổi dữ liệu.

-  **Cấu hình MySQL/MariaDB (`/etc/mysql/mysql.conf.d/mysqld.cnf` trên Linux, my.ini trên Windows)**
    - `socket` (Linux/Unix): Chỉ định đường dẫn đến file socket.

    ```ini!
     [mysqld]
    socket = /var/run/mysqld/mysqld.sock
    ```
    - Client cũng cần biết đường dẫn này để kết nối cục bộ:
    ```bash!
    mysql -u root -p --socket=/var/run/mysqld/mysqld.sock
    ```
    
## 2. ODBC/JDBC và các chuẩn kết nối

Các chuẩn kết nối này cung cấp một API (Application Programming Interface) thống nhất, cho phép các ứng dụng viết bằng các ngôn ngữ lập trình khác nhau kết nối và tương tác với nhiều loại database khác nhau mà không cần phải viết lại code kết nối riêng cho từng DBMS.

### a. ODBC (Open Database Connectivity)
- **Mô tả**: Là một chuẩn API của Microsoft, cho phép các ứng dụng viết bằng C/C++ (hoặc các ngôn ngữ hỗ trợ gọi API native) kết nối với nhiều hệ quản trị cơ sở dữ liệu khác nhau. ODBC hoạt động như một lớp trừu tượng giữa ứng dụng và database.

- **Cấu trúc**:

    - Application: Ứng dụng (ví dụ: một phần mềm kế toán, ứng dụng C++, công cụ báo cáo).
    - ODBC Driver Manager: Một thư viện hệ thống (do hệ điều hành cung cấp) quản lý việc tải và chuyển tiếp các lệnh ODBC đến đúng driver.
    - Database-Specific ODBC Driver: Là một thư viện (DLL trên Windows, .so trên Linux) do nhà cung cấp database (hoặc bên thứ ba) cung cấp. Driver này dịch các lệnh ODBC chung thành các lệnh riêng của database đó.
    - Data Source Name (DSN): Một cấu hình lưu trữ thông tin kết nối (server, database, user, password, driver) để ứng dụng có thể kết nối mà không cần biết chi tiết.
    - Database: Hệ quản trị cơ sở dữ liệu (MySQL, SQL Server, Oracle...).
    
- **Cài đặt và Cấu hình ODBC Driver cho MySQL/MariaDB:**


    #### Windows
    - Tải xuống ODBC Driver:
    - **MySQL**: Truy cập dev.mysql.com/downloads/connector/odbc/. Tải xuống phiên bản MySQL Connector/ODBC phù hợp với kiến trúc hệ thống (32-bit hoặc 64-bit).
    - **MariaDB**: Truy cập mariadb.com/downloads/connectors/connector-odbc/. Tải xuống MariaDB Connector/ODBC.
    - **Cài đặt Driver: Chạy file cài đặt (.msi) đã tải xuống.**
    - Cấu hình Data Source Name (DSN):

    #### Linux
    
    - Cài đặt unixODBC và driver MySQL/MariaDB
    ```bash
    #ubuntu
    sudo apt update
    sudo apt install -y unixodbc # Cài đặt ODBC Driver Manager
    sudo apt install -y libmyodbc # Hoặc mariadb-connector-odbc
    
    #CentOS
    sudo yum install -y unixODBC # Cài đặt ODBC Driver Manager
    sudo yum install -y mysql-connector-odbc # Hoặc mariadb-connector-odbc
    ```
    - Cấu hình: Chỉnh sửa file `etc/odbcinst.ini`
        - Thêm/sửa mục drive cho MySQL/MariaDB:
    ```ini!
    [MySQL ODBC 8.0 Unicode Driver] # Tên driver
    Description = MySQL ODBC 8.0 Unicode Driver
    Driver = /usr/lib/x86_64-linux-gnu/odbc/libmyodbc8w.so # Đường dẫn chính xác đến file driver (tìm kiếm trên hệ thống)
    Setup = /usr/lib/x86_64-linux-gnu/odbc/libmyodbc8w.so
    FileUsage = 1
    ```
    
    - Chỉnh sửa file `etc/odbc.ini` (định nghĩa DSN):
    ```ini!
    [MyAppDataDSN]
    Description = MySQL Database for MyApp
    Driver = MySQL ODBC 8.0 Unicode Driver # Phải khớp với tên driver trong odbcinst.ini
    SERVER = 192.168.1.100 # Địa chỉ IP hoặc hostname của MySQL/MariaDB server
    PORT = 3306
    DATABASE = your_database_name
    UID = your_db_user
    PWD = your_db_password
    ```
    
    - Kiểm tra DSN:
    ```bash
    isql MyAppDataDSN [your_db_user] [your_db_password]
    ```
    
### b. JDBC (Java Database Connectivity)

- **Mô tả**: Là một chuẩn API của Java, cho phép các ứng dụng Java kết nối và tương tác với database. JDBC được thiết kế theo kiến trúc tương tự ODBC nhưng dành riêng cho Java.

- **Cấu trúc:**

    - **Java Application**: Ứng dụng Java .
    - **JDBC API**: Các lớp và giao diện chuẩn của Java (trong package java.sql).
    - **JDBC Driver Manager**: Quản lý việc tải và sử dụng các driver JDBC.
    - **Database-Specific JDBC Driver**: Là một file .jar (Java Archive) do nhà cung cấp database (hoặc bên thứ ba) cung cấp. Driver này dịch các lệnh JDBC chung thành các lệnh riêng của database đó.
    - **Database**: Hệ quản trị cơ sở dữ liệu.

## 3. API và Dịch vụ Web cho Database

Trong kiến trúc ứng dụng hiện đại, thay vì cho phép các ứng dụng client (đặc biệt là ứng dụng di động, web frontend chạy trong trình duyệt) kết nối trực tiếp đến database, người ta thường sử dụng một lớp trung gian là **API (Application Programming Interface)** hoặc **dịch vụ web**. Lớp này đóng vai trò như một "cổng" an toàn và được kiểm soát chặt chẽ để truy cập dữ liệu.

---

### a. RESTful API (Representational State Transfer)

**Mô tả:**  
Đây là một kiến trúc phổ biến để xây dựng các dịch vụ web. Một RESTful API cung cấp một tập hợp các *endpoint* (địa chỉ URL) mà qua đó các ứng dụng client có thể gửi các yêu cầu HTTP (`GET`, `POST`, `PUT`, `DELETE`) để thực hiện các thao tác CRUD (Create, Read, Update, Delete) trên các tài nguyên (*resource*).

**Cách hoạt động:**

- **Client (Web/Mobile App):** Gửi một yêu cầu HTTP (ví dụ: `GET /api/users`) đến một endpoint của API server.
- **API Server (Backend):** Ứng dụng backend (được phát triển bằng các framework như Node.js Express, Python Flask/Django, Java Spring Boot, PHP Laravel/Symfony, Go Fiber/Gin, v.v.) nhận yêu cầu.
- **Xử lý và Kết nối Database:** Backend thực hiện các logic nghiệp vụ cần thiết, sau đó sử dụng các Connector/Driver (JDBC, ODBC, v.v.) để kết nối và thực thi các câu lệnh SQL tương ứng trên MySQL/MariaDB.
- **Phản hồi:** Backend xử lý dữ liệu trả về từ database, định dạng nó thành JSON (phổ biến nhất) hoặc XML, và gửi lại cho client dưới dạng phản hồi HTTP.

**Ưu điểm:**

- **Tách biệt (Decoupling):** Frontend (client) và backend (API server và database) hoàn toàn độc lập, có thể được phát triển và triển khai riêng biệt.
- **Bảo mật:** Database không bao giờ trực tiếp lộ ra Internet. Backend có thể thực hiện xác thực người dùng, ủy quyền, kiểm tra dữ liệu đầu vào và các lớp bảo mật khác trước khi dữ liệu đến được database.
- **Đa nền tảng:** Bất kỳ client nào có thể gửi yêu cầu HTTP đều có thể tương tác với API (ví dụ: trình duyệt web, ứng dụng iOS/Android, phần mềm desktop).
- **Khả năng mở rộng:** API server có thể được mở rộng bằng cách chạy nhiều instance và sử dụng Load Balancer, độc lập với khả năng mở rộng của database.

**Ví dụ các endpoint RESTful:**

- `GET https://your-api.com/api/users` (Lấy danh sách người dùng)
- `POST https://your-api.com/api/users` (Tạo người dùng mới với dữ liệu trong body của request)
- `GET https://your-api.com/api/users/123` (Lấy thông tin người dùng có ID là 123)
- `PUT https://your-api.com/api/users/123` (Cập nhật thông tin người dùng 123)
- `DELETE https://your-api.com/api/users/123` (Xóa người dùng 123)

---

### b. GraphQL

**Mô tả:**  
Là một ngôn ngữ truy vấn cho API và một runtime để thực hiện các truy vấn đó với dữ liệu. Không giống RESTful API với nhiều endpoint cố định, **GraphQL** cung cấp một endpoint duy nhất và cho phép client mô tả chính xác cấu trúc dữ liệu mà họ cần trong một yêu cầu.

**Ưu điểm:**

- **Hiệu quả:** Client tránh được việc "over-fetching" (lấy quá nhiều dữ liệu không cần thiết) hoặc "under-fetching" (phải gửi nhiều yêu cầu để lấy đủ dữ liệu).
- **Mạnh mẽ:** Client có thể định nghĩa các truy vấn phức tạp để lấy dữ liệu từ nhiều tài nguyên liên quan trong một lần gọi API duy nhất.
- **Phát triển nhanh chóng:** Giúp giảm thiểu việc client và server phải thay đổi code khi cấu trúc dữ liệu thay đổi.

**Cách hoạt động:**  
Client gửi một truy vấn GraphQL đến server. Server nhận truy vấn, phân tích cú pháp, sau đó thực hiện các *resolver* để lấy dữ liệu từ database (hoặc các dịch vụ khác) và định dạng kết quả theo đúng cấu trúc mà client đã yêu cầu.

---

### c. RPC (Remote Procedure Call)

**Mô tả:**  
RPC là một giao thức cho phép một chương trình máy tính gọi một thủ tục (hàm hoặc phương thức) trên một máy tính từ xa như thể nó đang gọi một thủ tục cục bộ. Các framework hiện đại như **gRPC (Google Remote Procedure Call)** sử dụng HTTP/2 để vận chuyển và **Protobuf (Protocol Buffers)** để tuần tự hóa dữ liệu, giúp giao tiếp rất hiệu quả và nhanh chóng.

**Ưu điểm:**

- **Hiệu suất cao:** Do sử dụng các giao thức và định dạng dữ liệu hiệu quả, RPC thường nhanh hơn RESTful API cho các giao tiếp giữa các dịch vụ.
- **Kiểu dữ liệu chặt chẽ:** Protobuf định nghĩa rõ ràng cấu trúc của dữ liệu.
- **Hỗ trợ đa ngôn ngữ:** Dễ dàng tạo client và server bằng nhiều ngôn ngữ lập trình khác nhau.

**Khi sử dụng:**  
Thường được sử dụng trong các hệ thống **microservices nội bộ**, nơi các dịch vụ cần giao tiếp với nhau một cách hiệu quả và đáng tin cậy.

## 4. ETL (Extract, Transform, Load)

**ETL** là một quy trình quan trọng trong lĩnh vực **kho dữ liệu (data warehousing)**, **tích hợp dữ liệu**, và **phân tích dữ liệu**. Nó bao gồm ba bước chính:

### Extract (Trích xuất)

- **Mô tả**: Bước đầu tiên là đọc (trích xuất) dữ liệu từ một hoặc nhiều hệ thống nguồn.
- **Nguồn dữ liệu**:
  - Database quan hệ: MySQL, MariaDB, Oracle, SQL Server
  - Database NoSQL: MongoDB, Redis
  - File phẳng: CSV, XML, JSON, log files
  - Hệ thống ERP (Enterprise Resource Planning)
  - Hệ thống CRM (Customer Relationship Management)
  - Các API của bên thứ ba, v.v.
- **Cách trích xuất**: Sử dụng các câu lệnh SQL (ví dụ: `SELECT`), API, hoặc các connector/driver của công cụ ETL.

### Transform (Chuyển đổi)

- **Mô tả**: Dữ liệu được trích xuất thường không ở định dạng phù hợp với hệ thống đích. Bước này liên quan đến việc **làm sạch**, **chuẩn hóa**, và **chuyển đổi dữ liệu**.
- **Các thao tác phổ biến**:
  - **Làm sạch dữ liệu**: Xóa dữ liệu trùng lặp, xử lý giá trị thiếu (null), sửa lỗi chính tả, chuẩn hóa chuỗi.
  - **Chuyển đổi định dạng**: Thay đổi kiểu dữ liệu (ví dụ: chuỗi thành số), định dạng ngày tháng.
  - **Tổng hợp (Aggregation)**: Tính toán tổng, trung bình, đếm, nhóm dữ liệu.
  - **Kết hợp (Joining/Merging)**: Ghép dữ liệu từ nhiều nguồn hoặc bảng khác nhau.
  - **Áp dụng logic nghiệp vụ**: Tính toán các giá trị mới dựa trên quy tắc kinh doanh (ví dụ: tính doanh thu, lợi nhuận).
  - **Kiểm tra tính hợp lệ (Validation)**: Đảm bảo dữ liệu tuân thủ các quy tắc nghiệp vụ.

### Load (Tải)

- **Mô tả**: Sau khi dữ liệu đã được chuyển đổi và sẵn sàng, nó được ghi (tải) vào hệ thống đích.
- **Hệ thống đích**:
  - Data Warehouse (phân tích dữ liệu lịch sử)
  - Data Lake
  - Database khác phục vụ báo cáo
  - Hệ thống ứng dụng khác
- **Các loại tải**:
  - **Full Load**: Tải toàn bộ dataset mỗi lần.
  - **Incremental Load**: Chỉ tải các thay đổi hoặc dữ liệu mới kể từ lần tải cuối cùng.

---

## Các công cụ ETL phổ biến

### Công cụ ETL chuyên dụng (Commercial & Open Source)

- **Talend Open Studio**: Công cụ mã nguồn mở mạnh mẽ với giao diện kéo thả trực quan.
- **Pentaho Data Integration (Kettle)**: Công cụ ETL mã nguồn mở tương tự Talend.
- **Apache Nifi**: Hệ thống mạnh mẽ và dễ sử dụng để xử lý và phân phối dữ liệu.
- **Các dịch vụ ETL đám mây**:
  - AWS Glue
  - Azure Data Factory
  - Google Cloud Dataflow

### Tích hợp vào ngôn ngữ lập trình

- **Python**:
  - `pandas`: xử lý và biến đổi dữ liệu
  - `SQLAlchemy`: kết nối database
  - `PyMySQL`, `psycopg2`: thư viện kết nối database

### Database-specific tools

- `mysqldump` / `mysqlpump`: dùng để **trích xuất** dữ liệu (Extract)
- `LOAD DATA INFILE`: dùng để **tải** dữ liệu (Load)

#### Cú pháp SQL:

```sql
-- Ví dụ: Tải dữ liệu từ file CSV vào bảng 'your_table_name'
LOAD DATA INFILE 'path/to/your/data.csv' -- Đường dẫn đến file CSV trên server
-- Hoặc LOCAL INFILE nếu file nằm trên máy client và bạn đã bật local_infile
-- LOAD DATA LOCAL INFILE 'path/to/your/data.csv'
INTO TABLE your_table_name
FIELDS TERMINATED BY ','     -- Dữ liệu phân tách bằng dấu phẩy
ENCLOSED BY '"'              -- Các trường bao quanh bởi dấu ngoặc kép
LINES TERMINATED BY '\n'     -- Mỗi dòng kết thúc bằng xuống dòng
IGNORE 1 ROWS;               -- Bỏ qua dòng tiêu đề
```


> **Yêu cầu:**
> - File phải nằm trên server database (nếu không dùng LOCAL).
> - Nếu dùng LOAD DATA LOCAL INFILE, file cần nằm trên máy client.
> - Cần bật tùy chọn local_infile = ON trong file cấu hình my.cnf (mục [mysqld]) và trên client.
> - Người dùng MySQL/MariaDB cần có quyền FILE để thực hiện lệnh này.
