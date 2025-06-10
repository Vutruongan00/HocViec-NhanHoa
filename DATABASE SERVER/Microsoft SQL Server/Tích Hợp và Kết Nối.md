> # Tích Hợp và Kết Nối

# 1. Các giao thức kết nối database
Microsoft SQL Server hỗ trợ một số giao thức mạng khác nhau để cho phép các ứng dụng client kết nối và giao tiếp với database server. Các giao thức này quản lý cách dữ liệu được truyền tải qua mạng giữa client và server.

Các giao thức chính mà SQL Server hỗ trợ là:
- **Shared Memory (Bộ nhớ chia sẻ)**
- **TCP/IP**
- **Named Pipes**

## 1.1 Shared Memory
- **Mô tả**: Giao thức Shared Memory (Bộ nhớ chia sẻ) là một giao thức rất nhanh và hiệu quả, được sử dụng khi ứng dụng client và instance SQL Server đều đang chạy trên cùng một máy tính vật lý. Dữ liệu không đi qua giao thức mạng truyền thống mà được trao đổi trực tiếp thông qua một vùng bộ nhớ chung.
- **Ưu điểm**:
    - Hiệu suất cao nhất: Vì dữ liệu không cần được đóng gói vào các gói mạng và không liên quan đến stack mạng TCP/IP, nó cung실 hiệu suất kết nối nhanh nhất.
    - Độ trễ thấp: Gần như không có độ trễ mạng.
- **Nhược điểm:**
    - Chỉ dùng cục bộ: Không thể sử dụng để kết nối từ một máy tính khác trên mạng.
- Khi sử dụng: Đây là giao thức mặc định và ưu tiên khi client và server cùng trên một máy. SQL Server sẽ tự động thử kết nối bằng Shared Memory trước tiên nếu nó khả dụng.

## 1.2 TCP/IP (Transmission Control Protocol/Internet Protocol)
- **Mô tả**: TCP/IP là giao thức mạng phổ biến và được sử dụng rộng rãi nhất để kết nối đến SQL Server. Nó là giao thức tiêu chuẩn cho các kết nối mạng qua Internet hoặc mạng cục bộ (LAN). Khi một ứng dụng client kết nối với SQL Server bằng TCP/IP, nó sẽ sử dụng địa chỉ IP của máy chủ và số cổng (port number) mà SQL Server đang lắng nghe.
- **Ưu điểm**:
    - Phổ biến và linh hoạt: Cho phép kết nối từ bất kỳ máy tính nào trên mạng, bao gồm cả Internet (nếu được cấu hình và bảo mật đúng cách).
    - Hỗ trợ mã hóa SSL/TLS: Có thể cấu hình để mã hóa dữ liệu truyền tải, bảo mật thông tin nhạy cảm.
    - Hiệu suất tốt: Là giao thức cân bằng giữa khả năng kết nối và hiệu suất.

- **Nhược điểm:**
    - Yêu cầu cấu hình firewall để mở cổng (mặc định 1433 cho instance mặc định).
    - Có thể gặp vấn đề về độ trễ mạng nếu kết nối qua khoảng cách xa hoặc mạng bị tắc nghẽn.

- **Cổng mặc định**:
    - Instance mặc định (Default Instance): SQL Server lắng nghe trên cổng TCP 1433.
    - Named Instances (Instance có tên): Named Instances thường sử dụng các cổng động (dynamic ports) để lắng nghe. Điều này có nghĩa là số cổng có thể thay đổi mỗi khi dịch vụ SQL Server khởi động lại. Để kết nối với Named Instance, client có thể:
        - Sử dụng dịch vụ SQL Server Browser (UDP 1434) để phân giải tên instance thành số cổng động.
        - Cấu hình một cổng tĩnh (static port) cho Named Instance và chỉ định rõ cổng đó trong chuỗi kết nối.

- **Cấu hình:**
    - Sử dụng SQL Server Configuration Manager để bật/tắt TCP/IP, cấu hình cổng tĩnh cho Named Instances, và bật/tắt mã hóa.
    - Cần cấu hình tường lửa Windows (hoặc tường lửa mạng) để cho phép lưu lượng truy cập trên cổng SQL Server.
    
![image](https://github.com/user-attachments/assets/9523f924-dd5a-4e30-b49a-b1fa4292b516)


## 1.3. Named Pipes
- **Mô tả**: Named Pipes là một giao thức mạng được phát triển bởi Microsoft để liên lạc giữa các tiến trình trên cùng một máy tính hoặc giữa các máy tính trên mạng cục bộ (LAN). Nó hoạt động như một kênh giao tiếp có tên, cho phép các ứng dụng trao đổi dữ liệu.

- **Ưu điểm**:
    - Đơn giản hơn so với TCP/IP cho việc kết nối cục bộ (không cần cổng cụ thể).
    - Có thể được sử dụng qua mạng LAN.

- **Nhược điểm:**
    - Hiệu suất thấp hơn TCP/IP đối với kết nối qua mạng (tốn chi phí hơn vì phải đóng gói và giải gói dữ liệu nhiều lần).
    - Ít được sử dụng hơn TCP/IP cho các kết nối mạng hiện đại.
    - Thường không hoạt động tốt qua WAN (Wide Area Network).

- **Khi sử dụng:** Chủ yếu được sử dụng cho các ứng dụng cũ hơn hoặc cho các kết nối cục bộ khi Shared Memory không được sử dụng hoặc không phù hợp.
- **Cấu hình:** Cũng được quản lý thông qua SQL Server Configuration Manager. Thường được bật mặc định.

# 2. ODBC/JDBC và các chuẩn kết nối
**ODBC** (Open Database Connectivity) và **JDBC** (Java Database Connectivity) là hai tiêu chuẩn phổ biến để kết nối với các cơ sở dữ liệu, bao gồm cả SQL Server. 
    - **JDBC** được thiết kế đặc biệt cho môi trường Java, 
    - **ODBC** là một tiêu chuẩn chung cho nhiều ngôn ngữ và hệ thống. 
- JDBC thường được ưa chuộng hơn trong các ứng dụng Java vì nó có hiệu suất tốt hơn và cung cấp nhiều tính năng nâng cao

## 2.1 ODBC (Open Database Connectivity)

### Tổng quan
ODBC là một API chuẩn được phát triển bởi Microsoft vào đầu những năm 1990.  
Nó được thiết kế để cung cấp một cách **độc lập với cơ sở dữ liệu** để truy cập dữ liệu. Điều này có nghĩa là một ứng dụng được viết bằng C, C++, Python, .NET (hoặc các ngôn ngữ khác hỗ trợ ODBC) có thể kết nối với nhiều cơ sở dữ liệu khác nhau (SQL Server, Oracle, MySQL, PostgreSQL, v.v.) chỉ bằng cách sử dụng các hàm ODBC tiêu chuẩn.

ODBC hoạt động thông qua một kiến trúc driver-manager:
- Ứng dụng giao tiếp với **Driver Manager**
- Driver Manager sau đó tải **driver ODBC cụ thể** cho cơ sở dữ liệu mà ứng dụng muốn kết nối

### Thành phần chính
- **Application**: Ứng dụng của bạn (ví dụ: một ứng dụng C++ hoặc một báo cáo Excel).
- **ODBC Driver Manager**: Một thư viện phần mềm (thường là một phần của hệ điều hành Windows) quản lý việc tải và giao tiếp với các driver ODBC cụ thể. Đóng vai trò trung gian giữa ứng dụng và driver.
- **ODBC Driver**: Một thư viện phần mềm cụ thể cho từng DBMS (ví dụ: "SQL Server ODBC Driver"). Driver này chuyển đổi các lệnh ODBC chuẩn thành lệnh SQL cụ thể và ngược lại.
- **Data Source Name (DSN)**: Một cấu hình được lưu trữ trên hệ thống (User DSN, System DSN, File DSN) chứa thông tin kết nối đến cơ sở dữ liệu (tên máy chủ, DB, xác thực). DSN giúp ứng dụng không cần hard-code chuỗi kết nối.

### Ưu điểm
- **Ngôn ngữ bất khả tri (Language-agnostic)**: Dùng được với nhiều ngôn ngữ lập trình.
- **Độc lập với DBMS**: Kết nối được nhiều hệ quản trị khác nhau.
- **Phổ biến trên Windows**: Tích hợp sẵn và phổ biến trong môi trường Windows.

### Nhược điểm
- **Phụ thuộc nền tảng**: Driver ODBC thường là nhị phân riêng cho từng hệ điều hành.
- **Hiệu suất**: Có thể giảm nhẹ do lớp trung gian của Driver Manager và việc chuyển đổi dữ liệu.


## 2.2/ JDBC (Java Database Connectivity)

### Tổng quan
JDBC là một API chuẩn được thiết kế đặc biệt cho các ứng dụng Java để tương tác với cơ sở dữ liệu.  
Nó cung cấp giao diện chuẩn cho các ứng dụng Java thực hiện thao tác SQL, quản lý kết nối và xử lý kết quả.  
Tương tự ODBC, JDBC hoạt động thông qua **driver cụ thể** cho từng DBMS.

### Thành phần chính
- **Java Application**: Ứng dụng Java.
- **JDBC API**: Các lớp và giao diện chuẩn trong Java (ví dụ: `java.sql` package).
- **JDBC Driver Manager**: Một lớp trong JDBC API để tải và quản lý các driver JDBC.
- **JDBC Driver**: Tập hợp các lớp Java chuyển lệnh JDBC thành SQL cụ thể và ngược lại.

### Các loại JDBC Drivers
1. **Type 1 (JDBC-ODBC Bridge)**  
   - Sử dụng driver ODBC gốc  
   - **Không còn được hỗ trợ từ Java 8**

2. **Type 2 (Native-API/Partly Java Driver)**  
   - Kết hợp Java và mã gốc DBMS

3. **Type 3 (Network Protocol/All-Java Driver)**  
   - Sử dụng middleware server để giao tiếp DBMS

4. **Type 4 (Native-Protocol/Pure Java Driver)**  
   - Hoàn toàn viết bằng Java  
   - Giao tiếp trực tiếp với DBMS qua giao thức mạng  
   - **Loại phổ biến và được khuyến nghị nhất**

### Ưu điểm
- **Độc lập nền tảng**: Chạy được trên mọi hệ thống có JVM.
- **Độc lập với DBMS**: Hỗ trợ nhiều loại cơ sở dữ liệu.
- **Hiệu suất tốt**: Đặc biệt với **Type 4** nhờ loại bỏ lớp trung gian.

### Nhược điểm
- **Chỉ dành cho Java**: Không thể dùng cho ngôn ngữ khác.

# 3. API và dịch vụ web cho database

# 4. ETL (Extract, Transform, Load)
