# I. Database 
- **Database (Cơ sở dữ liệu)** là một tập hợp các dữ liệu có tổ chức, được lưu trữ và quản lý một cách hệ thống để dễ dàng truy cập, truy vấn, cập nhật và quản lý.

    - Ví dụ: Danh sách khách hàng, thông tin sản phẩm, hóa đơn...
    - Dữ liệu có thể ở dạng cấu trúc (structured) hoặc phi cấu trúc (unstructured).

Cơ sở dữ liệu thường được quản lý thông qua một phần mềm gọi là Hệ quản trị cơ sở dữ liệu – DBMS (Database Management System).
- **Database server** là một loại máy chủ server được thiết kế để đáp ứng mục đích lưu trữ, quản lý, truy xuất và phục hồi cơ sở dữ liệu. Một hệ thống database server bao gồm phần cứng vật lý (Ram, CPU…); phần mềm quản trị cơ sở dữ liệu (Database Management System – DBMS); cơ sở dữ liệu (database); giao thức kết nối và cơ chế bảo mật.
# Kiến trúc cơ bản của Hệ thống Database
## 1. Mô hình Client-Server
- **Database server** sử dụng mô hình **Client-server**, trong đó các ứng dụng khách gửi yêu cầu đến máy chủ để xử lý yêu cầu, giúp phân chia công việc và tối ưu hóa hiệu suất.
    - **Client**: Là các ứng dụng (giao diện web, phần mềm desktop, hệ thống ERP...) gửi yêu cầu (query) đến server.

    -**Database Server**: Là máy chủ xử lý yêu cầu, thực hiện truy vấn dữ liệu và trả kết quả về cho client.
- **Database server** và **Client** tiếp với nhau qua giao thức TCP/IP, đảm bảo rằng dữ liệu được truyền tải chính xác và an toàn.

## 2. Các thành phần của Database Server
- **Hệ quản trị cơ sở dữ liệu (Database Management System – DBMS)**: Đây là phần mềm chính quản lý cơ sở dữ liệu, bao gồm việc lưu trữ, truy xuất, cập nhật và quản lý dữ liệu. Các DBMS phổ biến gồm MySQL, PostgreSQL, Microsoft SQL Server và Oracle Database.
Cơ sở dữ liệu (Database): Tập hợp các bảng, chỉ mục, và các đối tượng dữ liệu khác được lưu trữ và quản lý bởi DBMS. Cơ sở dữ liệu chứa thông tin thực tế mà các ứng dụng và người dùng tương tác.
- **Máy chủ (Server Hardware)**: Phần cứng thực tế mà DBMS chạy trên đó, bao gồm CPU, RAM, ổ cứng và các thành phần mạng. Phần cứng này cần đủ mạnh để xử lý các yêu cầu và duy trì hiệu suất cao.

- **Hệ điều hành (Operating System)**: Phần mềm hệ thống quản lý tài nguyên phần cứng và cung cấp các dịch vụ cơ bản cho DBMS và các ứng dụng khác chạy trên máy chủ. Các hệ điều hành phổ biến cho máy chủ cơ sở dữ liệu gồm Windows Server, Linux và UNIX.

- **Công cụ bảo mật (Security Tools)**: Các biện pháp bảo mật như tường lửa, hệ thống phát hiện xâm nhập (IDS), và mã hóa dữ liệu để bảo vệ cơ sở dữ liệu khỏi truy cập trái phép và các mối đe dọa bảo mật

- **Công cụ sao lưu và khôi phục (Backup and Recovery Tools)**: Các công cụ và quy trình để sao lưu dữ liệu định kỳ và khôi phục dữ liệu trong trường hợp mất mát hoặc hỏng hóc.

- **Quy trình (Procedures)**
Quy trình là một tập hợp các hướng dẫn và quy tắc sử dụng các phương pháp đã được tiêu chuẩn hóa để thiết kế và vận hành cơ sở dữ liệu, giúp hướng dẫn người sử dụng và quản lý chúng theo các quy trình chuẩn mực.

## 3. Điểm khác nhau giữa Database server và Database

# So sánh Database Server và Database

| **Tiêu Chí** | **Database Server** | **Database** |
|--------------|---------------------|---------------|
| **Định Nghĩa** | Một hệ thống máy tính hoặc phần mềm cung cấp dịch vụ lưu trữ, quản lý và truy xuất dữ liệu. | Một tập hợp có cấu trúc của dữ liệu được tổ chức và lưu trữ trong một hệ thống quản lý cơ sở dữ liệu. |
| **Chức Năng Chính** | Quản lý, duy trì và cung cấp các dịch vụ liên quan đến cơ sở dữ liệu, bao gồm lưu trữ, bảo mật và sao lưu. | Lưu trữ và tổ chức dữ liệu theo một cấu trúc nhất định để dễ dàng truy cập và quản lý. |
| **Thành Phần Chính** | Bao gồm phần cứng (máy chủ vật lý hoặc máy chủ ảo) và phần mềm quản lý cơ sở dữ liệu (DBMS). | Bao gồm các bảng, chỉ mục, thủ tục lưu trữ, và các dữ liệu được lưu trữ trong hệ quản trị cơ sở dữ liệu (DBMS). |
| **Quản Lý** | Được quản lý bởi các quản trị viên hệ thống (DBA) và yêu cầu các kỹ thuật quản lý máy chủ và bảo mật. | Được quản lý bởi các quản trị viên cơ sở dữ liệu (DBA) và các nhà phát triển, tập trung vào quản lý dữ liệu và cấu trúc dữ liệu. |
| **Bảo Mật** | Cung cấp các cơ chế bảo mật như xác thực, mã hóa và kiểm soát truy cập để bảo vệ dữ liệu. | Áp dụng các chính sách bảo mật và quyền truy cập để bảo vệ dữ liệu bên trong cơ sở dữ liệu. |
| **Hiệu Suất** | Đảm bảo hiệu suất tốt cho việc xử lý nhiều truy vấn và giao dịch đồng thời. | Phụ thuộc vào cách dữ liệu được tổ chức và chỉ mục hóa để tối ưu hóa truy vấn và thao tác dữ liệu. |
| **Khả Năng Mở Rộng** | Có thể mở rộng bằng cách thêm tài nguyên phần cứng hoặc tối ưu hóa cấu hình máy chủ. | Có thể mở rộng bằng cách tối ưu hóa cấu trúc dữ liệu, phân mảnh hoặc phân phối dữ liệu trên nhiều cơ sở dữ liệu. |
| **Ví Dụ** | MySQL Server, Microsoft SQL Server, Oracle Database Server. | Một cơ sở dữ liệu MySQL, một cơ sở dữ liệu SQL Server, một cơ sở dữ liệu Oracle. |
| **Mục Đích Sử Dụng** | Cung cấp một nền tảng để lưu trữ và quản lý một hoặc nhiều cơ sở dữ liệu, phục vụ các ứng dụng khác nhau. | Lưu trữ dữ liệu của một ứng dụng hoặc một phần mềm cụ thể, phục vụ cho các yêu cầu cụ thể của ứng dụng đó. |

## 4. Các loại database server phổ biến (RDBMS, NoSQL, NewSQL)

### a. RDBMS (Relational Database Management Systems - Hệ quản trị cơ sở dữ liệu quan hệ)
- Quản lý dữ liệu theo mô hình quan hệ (bảng).
- Hỗ trợ SQL (Structured Query Language).
- Hỗ trợ ràng buộc (constraint), khóa ngoại, chuẩn hóa dữ liệu.
- Ví dụ: **MySQL, PostgreSQL, Oracle, SQL Server, MariaDB**

### b. NoSQL (Not Only SQL)
- Không sử dụng mô hình bảng – linh hoạt hơn.
- Phù hợp với dữ liệu lớn, phi cấu trúc, yêu cầu mở rộng nhanh.
- Các mô hình: document (MongoDB), key-value (Redis), column (Cassandra), graph (Neo4j).
- Ví dụ: **MongoDB, Cassandra, Redis, CouchDB**

### c. NewSQL
- Kết hợp ưu điểm của RDBMS và khả năng mở rộng của NoSQL.
- Duy trì tính ACID nhưng có khả năng mở rộng ngang tốt.
- Ví dụ: **Google Spanner, CockroachDB, TiDB, VoltDB**

---
# II. Hệ Quản Trị Cơ Sở Dữ Liệu (DBMS)
Như đã giới thiệu ở trên, DBMS (Database management System) là một phần mềm hoặc một tập hợp các chương trình cho phép người dùng và các ứng dụng tương tác với database. Nó cung cấp các công cụ để địnnh nghĩa, tạo, truy vấn, cập nhật và quản lý dữ liệu trong CSDL.
## 1. MySQL/MariaDB
**MariaDB** và **MySQL** là hai trong số các hệ quản trị cơ sở dữ liệu quan hệ (RDBMS) phổ biến nhất hiện nay. Cả hai hệ quản trị này đều mang lại hiệu năng cao và tính linh hoạt trong quản lý dữ liệu.
- **MySQL**: Là một trong những RDBMS mã nguồn mở phổ biến nhất thế giới. Được sở hữu bởi Oracle Corporation.
- **MariaDB**: Là một "fork" (phiên bản tách ra) của MySQL, được phát triển bởi những người tạo ra MySQL ban đầu, nhằm mục đích duy trì tính mã nguồn mở hoàn toàn sau khi MySQL được Oracle mua lại. MariaDB tương thích cao với MySQL.

| Tiêu chí|**MariaDB**|**MySQL**|
| ---- | ---- | ---- |
| **Nguồn gốc** | Được phát triển bởi cộng đồng, do các nhà sáng lập MySQL tạo ra sau khi Oracle mua lại MySQL.| Được phát triển bởi Oracle Corporation sau khi mua lại MySQL.|
| **Giấy phép** | Hoàn toàn mã nguồn mở (GPL v2). | Có phiên bản mã nguồn mở và phiên bản thương mại (Enterprise).
| **Tính năng mới** | Thêm nhiều tính năng mới như engine Aria, ColumnStore, hỗ trợ JSON nâng cao, và nhiều plugin mở rộng. | Phát triển chậm hơn về tính năng mới; một số tính năng chỉ có ở bản Enterprise.                |
| **Hiệu suất**            | Tối ưu hóa tốt hơn cho hiệu suất cao, đặc biệt trong các hệ thống lớn.                                | Hiệu suất ổn định, nhưng có thể thấp hơn MariaDB trong một số trường hợp.                      |
| **Khả năng tương thích** | Tương thích cao với MySQL, có thể thay thế trực tiếp trong nhiều trường hợp.                          | Tương thích với các ứng dụng sử dụng MySQL, nhưng ít linh hoạt hơn MariaDB trong việc mở rộng. |
| **Cộng đồng hỗ trợ**     | Cộng đồng mã nguồn mở tích cực, phát triển nhanh chóng.                                               | Cộng đồng lớn, nhưng chịu sự kiểm soát của Oracle.                                             |

## 2. Microsoft SQL Server (RDBMS)
**Microsoft SQL Server** là một hệ quản trị cơ sở dữ liệu quan hệ (Relational Database Management System – RDBMS) được phát triển bởi Microsoft từ năm 1988. Nó được thiết kế để quản lý và lưu trữ dữ liệu, cho phép người dùng truy vấn, thao tác và quản lý dữ liệu một cách hiệu quả và an toàn. 

### Cấu trúc của SQL Server
---
####  Database Engine:
- Storage Engine: Quản lý việc lưu trữ dữ liệu vật lý trên đĩa.
- Query Processor: Xử lý và tối ưu hóa các truy vấn SQL.
- Relational Engine: Quản lý các đối tượng cơ sở dữ liệu như bảng, chỉ mục và mối quan hệ.
#### SQLOS (SQL Server Operating System)
- Memory Management: Quản lý bộ nhớ cho SQL Server.
- Scheduler: Lập lịch cho các tác vụ và truy vấn.
- I/O Management: Quản lý các hoạt động nhập/xuất dữ liệu
- Synchronization: Đồng bộ hóa các tiến trình và tài nguyên.
#### External Protocol
Quản lý giao tiếp giữa SQL Server và các ứng dụng bên ngoài thông qua các giao thức như TDS (Tabular Data Stream).

### Tính năng của SQL Server
Microsoft cung cấp tính năng quản lý dữ liệu cùng SQL Server với các dịch vụ tích hợp lập trình SQL Server, SQL Server Data Quality và SQL Server Master. Ngoài ra, hai bộ công cụ dành riêng cho quản trị viên cơ sở dữ liệu (DBAs) và lập trình viên: 

- SQL Server Data Tools: Được sử dụng trong việc phát triển cơ sở dữ liệu.
- SQL Server Management Studio được ứng dụng để triển khai, giám sát và quản lý cơ sở dữ liệu.
- SQL Server còn được trang bị tính năng kinh doanh giúp người dùng có thể thực hiện phân tích dữ liệu thông qua:
    - SQL Server Analysis Services (SSAS): sử dụng để phân tích các dữ liệu.
    - SQL Server Reporting Services: để tạo ra báo cáo dễ dàng hơn.
### Ưu điểm của SQL Server
   - Bạn có thể sử dụng nhiều phiên bản MS SQL khác nhau trên cùng 1 máy
   - Bạn có thể phát triển và duy trì riêng biệt các môi trường thử nghiệm khác nhau
   - Xây dựng và duy trì các loại máy chủ dự phòng
   - Hạn chế tối đa các vấn đề rủi ro trên cơ sở dữ liệu
  
### Nhược điểm của SQL Server
- Phải thanh toán chi phí thêm để có thể khởi chạy nhiều Database khác nhau trên Microsoft SQL Server
### Lợi ích của việc sử dụng SQL Server:
- Tính bảo mật: SQL Server cung cấp các tính năng bảo mật mạnh mẽ, bảo vệ dữ liệu khỏi truy cập trái phép. 
- Hiệu suất: SQL Server được tối ưu hóa để xử lý lượng lớn dữ liệu và các truy vấn phức tạp. 
Tính linh hoạt:
- SQL Server có thể được triển khai trên nhiều nền tảng, bao gồm cả Windows và Linux. 
- Tích hợp: SQL Server tích hợp tốt với các sản phẩm khác của Microsoft, như Azure, Power BI, và các ứng dụng .NET. 

## 3. Oracle Database 
**Oracle Database** là một hệ quản trị cơ sở dữ liệu quan hệ (RDBMS) mạnh mẽ, được sử dụng rộng rãi để quản lý, lưu trữ và truy xuất dữ liệu. Nó nổi tiếng với khả năng mở rộng, bảo mật và hiệu suất cao

### Tính năng:
- **Hiệu suất và khả năng mở rộng:**
Oracle Database được tối ưu hóa cho hiệu suất cao và có khả năng mở rộng dễ dàng, đáp ứng nhu cầu của các doanh nghiệp lớn. 
- **Bảo mật**:
Oracle Database cung cấp các tính năng bảo mật mạnh mẽ, đảm bảo dữ liệu được bảo vệ khỏi các mối đe dọa từ bên ngoài. 
- **Tích hợp**:
Oracle Database có thể tích hợp với nhiều nền tảng, ngôn ngữ lập trình và công nghệ khác nhau. 
- **Tự động hóa**:
Các tính năng tự động hóa của Oracle Database giúp tiết kiệm thời gian và công sức trong việc quản lý cơ sở dữ liệu. 
- **Phân tích dữ liệu**:
Oracle Database hỗ trợ các công cụ phân tích dữ liệu tiên tiến, giúp doanh nghiệp đưa ra các quyết định dựa trên dữ liệu. 
- Oracle RAC:
Hỗ trợ công nghệ Real Application Clusters để tăng cường khả năng chịu lỗi và phân tải. 
### Ưu điểm:
- Khả năng tương thích cao: Oracle Database tương thích với nhiều nền tảng và ứng dụng khác nhau. 
- Hiệu suất và khả năng mở rộng: Oracle Database được thiết kế để đáp ứng hiệu suất cao và có khả năng mở rộng. 
- Bảo mật: Oracle Database cung cấp các tính năng bảo mật mạnh mẽ, đảm bảo dữ liệu được bảo vệ. 
- Tích hợp: Oracle Database có thể tích hợp với nhiều nền tảng và công nghệ khác nhau. 
- Hỗ trợ từ nhà phát triển: Oracle cung cấp hỗ trợ kỹ thuật và tài liệu đầy đủ. 
- Cộng đồng lớn: Cộng đồng Oracle rộng lớn và sẵn sàng hỗ trợ lẫn nhau. 
### Nhược điểm 
- **Chi phí cao**: Chi phí bản quyền, bảo trì và hỗ trợ rất đắt đỏ, không phù hợp cho các dự án nhỏ hoặc ngân sách hạn chế.
- **Phức tạp**: Cài đặt, cấu hình, quản lý và tối ưu hóa Oracle đòi hỏi kiến thức chuyên sâu và đội ngũ DBA có kinh nghiệm.
- **Tiêu tốn tài nguyên**: Yêu cầu phần cứng mạnh mẽ và tài nguyên hệ thống lớn để đạt hiệu suất tối ưu.
- **Lock-in**: Dễ bị phụ thuộc vào hệ sinh thái của Oracle.

## 4. PostgreSQL (RDBMS)

### Tính năng nổi bật:

- **Tuân thủ SQL chặt chẽ**: Rất mạnh về tính năng SQL, hỗ trợ các chuẩn ANSI SQL mới nhất.  
- **Tính năng nâng cao**: Hỗ trợ đa dạng kiểu dữ liệu (JSONB, XML, UUID, Array, Enum, v.v.), Indexing tiên tiến (GIN, GiST, BRIN), Window Functions, Common Table Expressions (CTEs).  
- **Mở rộng thông qua Extension**: Có một hệ sinh thái extension phong phú (như PostGIS cho dữ liệu không gian, Citus Data cho phân cụm).  
- **Tính toàn vẹn dữ liệu và độ bền cao**: Tuân thủ ACID, hỗ trợ MVCC (Multi-Version Concurrency Control) hiệu quả, Write-Ahead Logging (WAL) cho phục hồi mạnh mẽ.  
- **Mã nguồn mở và miễn phí**: Hoàn toàn miễn phí sử dụng, không có chi phí bản quyền.  

### Ưu điểm:

- **Độ tin cậy và mạnh mẽ**: Được đánh giá cao về độ bền, tính nhất quán và khả năng xử lý các workload phức tạp, thường được xem là đối thủ "mã nguồn mở" của Oracle.  
- **Linh hoạt và có thể mở rộng**: Dễ dàng thêm các chức năng mới thông qua extension mà không cần sửa đổi mã nguồn chính.  
- **Cộng đồng lớn và năng động**: Hỗ trợ miễn phí từ cộng đồng, nhiều tài liệu và công cụ.  
- **Phù hợp với nhiều loại ứng dụng**: Từ các ứng dụng web nhỏ đến các hệ thống doanh nghiệp lớn, phân tích dữ liệu.  

### Nhược điểm:

- **Hiệu suất cho workload đơn giản**: Trong một số trường hợp rất cụ thể (ví dụ: các ứng dụng web với số lượng đọc/ghi nhỏ, đơn giản), MySQL có thể có hiệu suất cao hơn một chút (tuy nhiên, điều này ngày càng ít rõ rệt và phụ thuộc nhiều vào cấu hình).  
- **Độ phức tạp quản lý**: Có thể phức tạp hơn MySQL đối với người mới bắt đầu hoặc các tác vụ quản trị cơ bản.  
- **Mở rộng chiều ngang**: Mặc dù có các giải pháp như Citus, việc mở rộng chiều ngang vẫn đòi hỏi cấu hình và quản lý phức tạp hơn so với một số database NoSQL được thiết kế riêng cho mục đích này.  

---
## 5. MongoDB (NoSQL - Document Database)

### Tính năng nổi bật

- **Mô hình tài liệu (Document-Oriented)**: Lưu trữ dữ liệu dưới dạng tài liệu JSON (thực tế là BSON - Binary JSON), cho phép các tài liệu lồng ghép (nested documents) và mảng (arrays). Điều này rất linh hoạt, không yêu cầu lược đồ cố định (schemaless).
- **Khả năng mở rộng chiều ngang (Horizontal Scalability)**: Được thiết kế để dễ dàng mở rộng bằng cách sử dụng sharding, phân tán dữ liệu trên nhiều server (cluster) để xử lý lượng dữ liệu và lưu lượng truy cập lớn.
- **Truy vấn phong phú**: Hỗ trợ ngôn ngữ truy vấn dựa trên JSON (MongoDB Query Language - MQL), cung cấp nhiều khả năng truy vấn mạnh mẽ bao gồm truy vấn các tài liệu lồng ghép, biểu thức regular, và các toán tử truy vấn phức tạp.
- **Chỉ mục (Indexing)**: Hỗ trợ nhiều loại chỉ mục (single-field, compound, multi-key, text, geospatial) để tối ưu hóa hiệu suất truy vấn.
- **Replication (Bộ bản)**: Sử dụng các Replica Sets (tập hợp các instance của MongoDB) để cung cấp khả năng sẵn sàng cao (High Availability) và phục hồi sau lỗi (failover) tự động.
- **Aggregation Framework**: Một pipeline xử lý dữ liệu mạnh mẽ cho phép thực hiện các thao tác tổng hợp phức tạp (nhóm, lọc, biến đổi dữ liệu) tương tự như GROUP BY trong SQL.

### Ưu điểm

- **Linh hoạt về lược đồ (Schema Flexibility)**: Rất phù hợp với các ứng dụng có cấu trúc dữ liệu thay đổi thường xuyên, dữ liệu phi cấu trúc hoặc bán cấu trúc. Bạn không cần phải định nghĩa lược đồ trước khi thêm dữ liệu.
- **Dễ dàng mở rộng**: Khả năng sharding tích hợp giúp mở rộng quy mô dữ liệu và tải một cách tương đối dễ dàng, phù hợp cho các ứng dụng Big Data và có lượng người dùng lớn.
- **Tốc độ phát triển nhanh**: Mô hình tài liệu trực quan và linh hoạt giúp các nhà phát triển nhanh chóng đưa các tính năng mới vào sản phẩm.
- **Hiệu suất cao**: Đặc biệt tốt cho việc truy xuất dữ liệu theo tài liệu hoàn chỉnh và các truy vấn đọc/ghi cụ thể.
- **Phù hợp cho dữ liệu phức tạp, lồng ghép**: Dễ dàng biểu diễn các đối tượng phức tạp với các mối quan hệ lồng ghép trong một tài liệu duy nhất.

### Nhược điểm

- **Tính nhất quán cuối cùng (Eventual Consistency)**: Mặc định, MongoDB ưu tiên tính khả dụng và chịu lỗi phân vùng hơn tính nhất quán mạnh mẽ (ACID). Mặc dù có thể cấu hình mức độ nhất quán cao hơn, nhưng điều này có thể ảnh hưởng đến hiệu suất và khả năng mở rộng. Không lý tưởng cho các ứng dụng yêu cầu tính toàn vẹn giao dịch ACID nghiêm ngặt (ví dụ: giao dịch tài chính).
- **Không có JOINs truyền thống**: Mặc dù có các cách để thực hiện liên kết (ví dụ: $lookup trong aggregation pipeline), nhưng chúng không hiệu quả hoặc linh hoạt như JOINs trong RDBMS. Điều này có thể dẫn đến việc trùng lặp dữ liệu hoặc truy vấn phức tạp cho các mối quan hệ nhiều-tới-nhiều.
- **Tiêu tốn tài nguyên (RAM/Disk)**: Có thể tiêu tốn nhiều RAM và dung lượng đĩa hơn RDBMS cho cùng một lượng dữ liệu do việc lưu trữ các tài liệu (đặc biệt là khi có sự trùng lặp dữ liệu do thiếu JOINs).
- **Khó khăn khi thay đổi lược đồ phức tạp**: Mặc dù linh hoạt, nhưng việc thay đổi cấu trúc dữ liệu trên các tài liệu đã tồn tại có thể phức tạp nếu không được quản lý tốt.
- **Hạn chế về truy vấn phức tạp giữa các tài liệu**: Các truy vấn phức tạp cần liên kết nhiều tập dữ liệu có thể khó khăn và kém hiệu quả hơn so với RDBMS.

## 6. Redis (NoSQL - Key-Value Store / Data Structure Server)

### Tính năng nổi bật:

- **In-Memory Data Store**: Dữ liệu được lưu trữ chủ yếu trong RAM, mang lại tốc độ truy xuất cực nhanh.  
- **Data Structure Server**: Hỗ trợ nhiều cấu trúc dữ liệu phong phú ngoài cặp Key-Value đơn giản: Strings, Hashes, Lists, Sets, Sorted Sets, Streams, Bitmaps, Geospatial indexes.  
- **Atomic Operations**: Các thao tác trên cấu trúc dữ liệu là nguyên tử, đảm bảo tính nhất quán ngay cả trong môi trường đồng thời.  
- **Persistence (Độ bền)**: Hỗ trợ lưu trữ dữ liệu lên đĩa (RDB snapshotting và AOF logging) để phục hồi sau khi khởi động lại, dù mục đích chính không phải là database bền vững hoàn toàn.  
- **Pub/Sub (Publish/Subscribe)**: Hỗ trợ mô hình nhắn tin publish/subscribe, thường dùng cho real-time messaging.  
- **Replication và Clustering**: Hỗ trợ sao chép dữ liệu (replication) và phân cụm (clustering) cho khả năng sẵn sàng cao và mở rộng.  

### Ưu điểm:

- **Tốc độ cực nhanh**: Do hoạt động trên bộ nhớ, Redis có độ trễ rất thấp và thông lượng cao, lý tưởng cho các ứng dụng yêu cầu phản hồi tức thì.  
- **Linh hoạt với cấu trúc dữ liệu**: Cho phép lưu trữ và thao tác dữ liệu một cách hiệu quả cho nhiều trường hợp sử dụng khác nhau.  
- **Đơn giản và dễ sử dụng**: API đơn giản, dễ tích hợp vào các ứng dụng.  
- **Đa năng**: Phù hợp cho nhiều vai trò như cache, session store, message broker, leaderboard, real-time analytics.  

### Nhược điểm:

- **Phụ thuộc vào RAM**: Dung lượng dữ liệu được lưu trữ bị giới hạn bởi lượng RAM có sẵn trên server. Việc mở rộng thường đòi hỏi mua thêm RAM hoặc sử dụng clustering.  
- **Không phù hợp làm database chính**: Mặc dù có cơ chế persistence, nhưng nó không được thiết kế để thay thế hoàn toàn RDBMS hoặc các NoSQL database bền vững khác cho việc lưu trữ dữ liệu chính có tính toàn vẹn cao.  
- **Chi phí RAM cao**: Khi dữ liệu lớn, chi phí RAM có thể trở nên đáng kể so với việc lưu trữ trên ổ đĩa.  
- **Không có JOINs hoặc truy vấn phức tạp**: Không có ngôn ngữ truy vấn phức tạp như SQL, các truy vấn thường là đơn giản dựa trên khóa hoặc các thao tác trên cấu trúc dữ liệu.  
 
 
# So sánh ưu nhược điểm từng hệ thống

| Đặc điểm  | MySQL / MariaDB | Microsoft SQL Server  | Oracle Database| PostgreSQL | MongoDB (NoSQL)  | Redis (NoSQL)  |
|------|--------|----------|--------|---------|------|-----|
| **Bản chất**    | RDBMS mã nguồn mở   | RDBMS thương mại của Microsoft  | RDBMS thương mại hàng đầu   | RDBMS mã nguồn mở mạnh mẽ    | NoSQL (Document Database) | NoSQL (Key-Value / In-Memory) |
| **Ưu điểm**      | - Dễ học, dễ sử dụng, cộng đồng lớn.  <br> - Hiệu suất tốt cho các ứng dụng web thông thường. <br> - Đáng tin cậy, đã được chứng minh. <br> - MariaDB duy trì tính mã nguồn mở hoàn toàn. | - Tích hợp chặt chẽ với hệ sinh thái Microsoft. <br> - Công cụ quản lý và phát triển mạnh mẽ (SSMS, Visual Studio). <br> - Hiệu suất cao, đáng tin cậy. <br> - Phiên bản miễn phí (Express) cho các dự án nhỏ. | - Rất ổn định, đáng tin cậy, hiệu suất cao cho dữ liệu lớn. <br> - Hỗ trợ nhiều tính năng cấp doanh nghiệp (phân cụm, bảo mật, phục hồi). <br> - Khả năng mở rộng mạnh mẽ (RAC). | - Tuân thủ SQL chặt chẽ, nhiều tính năng nâng cao (JSONB, GIS, Full-Text Search). <br> - Độ tin cậy và tính toàn vẹn dữ liệu cao. <br> - Cộng đồng mạnh mẽ, mã nguồn mở. <br> - Hỗ trợ mở rộng thông qua các extension. | - Linh hoạt về lược đồ (schemaless). <br> - Dễ dàng mở rộng theo chiều ngang (sharding). <br> - Phù hợp cho dữ liệu phi cấu trúc/bán cấu trúc. <br> - Hiệu suất cao cho các ứng dụng Big Data và real-time. | - Tốc độ cực nhanh (in-memory). <br> - Hỗ trợ nhiều cấu trúc dữ liệu (lists, sets, hashes). <br> - Phù hợp cho caching, session management, message queues, real-time analytics. <br> - Atomic operations. |
| **Nhược điểm**   | - Tính năng nâng cao và tối ưu hóa có thể kém hơn PostgreSQL/Oracle. <br> - Quyền sở hữu của Oracle có thể gây lo ngại về tương lai mã nguồn mở (được giải quyết bởi MariaDB). | - Chi phí bản quyền cao cho các phiên bản Enterprise. <br> - Thường yêu cầu phần cứng mạnh. <br> - Chủ yếu tối ưu cho môi trường Windows, dù có phiên bản Linux. | - Chi phí bản quyền rất cao. <br> - Yêu cầu phần cứng và tài nguyên lớn. <br> - Phức tạp để cài đặt và quản lý. <br> - Không lý tưởng cho các dự án nhỏ. | - Hiệu suất có thể kém hơn MySQL/MariaDB trong một số workload đọc/ghi đơn giản (tùy thuộc phiên bản và cấu hình). <br> - Ít phổ biến hơn MySQL trong hosting giá rẻ. <br> - Cộng đồng nhỏ hơn MySQL nhưng rất năng động. | - Không có JOINs truyền thống. <br> - Tính nhất quán cuối cùng (eventual consistency) có thể không phù hợp cho các ứng dụng yêu cầu ACID cao. <br> - Tài nguyên RAM tiêu thụ cao hơn RDBMS khi tập dữ liệu lớn. | - Dữ liệu chính thường nằm trong RAM, có nguy cơ mất dữ liệu nếu không được cấu hình bền vững (persistence). <br> - Không phù hợp để lưu trữ dữ liệu chính cần dung lượng lớn và bền vững. <br> - Chi phí RAM có thể cao. <br> - Hỗ trợ các kiểu truy vấn phức tạp hạn chế. |
| **Độ phổ biến**  | Rất cao (Web applications)  | Cao (Enterprise Windows environments)   | Cao (Large Enterprises)    | Cao (Enterprise, Data Science, Web)   | Rất cao (Big Data, Web, Mobile)  | Cao (Caching, Real-time)    |


# III. Tiêu chí lựa chọn DBMS phù hợp

Việc lựa chọn DBMS phù hợp là một quyết định quan trọng, ảnh hưởng lớn đến hiệu suất, khả năng mở rộng, bảo mật và chi phí của ứng dụng. Dưới đây là các tiêu chí chính cần xem xét:

## 1. Loại dữ liệu và Mô hình dữ liệu

| Loại dữ liệu / Tình huống sử dụng | Gợi ý DBMS phù hợp                     | Ví dụ ứng dụng                                         |
|-----------------------------------|----------------------------------------|--------------------------------------------------------|
| Dữ liệu có cấu trúc, quan hệ chặt chẽ | RDBMS (MySQL, PostgreSQL, SQL Server, Oracle) | Hệ thống thông tin người dùng, đơn hàng, tài chính     |
| Dữ liệu phi cấu trúc / bán cấu trúc, thay đổi linh hoạt | NoSQL Document DB (MongoDB)            | Log file, dữ liệu IoT, nội dung do người dùng tạo      |
| Dữ liệu dạng Key-Value, cần tốc độ cao | NoSQL Key-Value Store (Redis)          | Session user, cache, leaderboards                     |
| Dữ liệu có mối quan hệ phức tạp     | NoSQL Graph DB (Neo4j, ArangoDB...)     | Mạng xã hội, hệ thống khuyến nghị                      |

## 2. Khả năng mở rộng (Scalability)

| Kiểu mở rộng       | Mô tả                                                                 | Phù hợp với DBMS             |
|--------------------|----------------------------------------------------------------------|------------------------------|
| Mở rộng chiều dọc (Scale-up) | Nâng cấp phần cứng server (RAM, CPU, Disk)                           | RDBMS truyền thống            |
| Mở rộng chiều ngang (Scale-out) | Phân tán dữ liệu và tải trên nhiều server                           | NoSQL, NewSQL                 |
| Câu hỏi gợi ý       | Ứng dụng có phát triển lớn không? Cần xử lý tải cao theo chiều ngang không? |                              |

## 3. Tính nhất quán giao dịch (ACID Compliance)

| Yêu cầu             | Mô tả                                                                 | Phù hợp với DBMS             |
|---------------------|----------------------------------------------------------------------|------------------------------|
| ACID cao             | Giao dịch tài chính, ngân hàng, TMĐT cần đảm bảo toàn vẹn dữ liệu    | RDBMS, NewSQL                 |
| Tính nhất quán cuối cùng | Chấp nhận trễ nhỏ để đạt hiệu suất và khả năng mở rộng tốt hơn       | NoSQL                         |

## 4. Hiệu suất và Tốc độ truy vấn

| Loại truy vấn / yêu cầu | DBMS phù hợp                         | Ghi chú                                           |
|--------------------------|--------------------------------------|--------------------------------------------------|
| Truy vấn phức tạp (JOINs) | RDBMS                               | MySQL, PostgreSQL, SQL Server, Oracle            |
| Đọc/Ghi đơn giản, tốc độ cao | NoSQL Key-Value (Redis)            | Tối ưu cho cache và real-time                    |
| Truy cập theo tài liệu     | NoSQL Document (MongoDB)            | Phù hợp dữ liệu bán cấu trúc, nội dung linh hoạt |

## 5. Cộng đồng và Hỗ trợ

| Loại DBMS           | Ưu điểm                                                                   | Nhược điểm                                               |
|---------------------|--------------------------------------------------------------------------|----------------------------------------------------------|
| Mã nguồn mở         | Cộng đồng lớn, nhiều tài liệu, plugin, diễn đàn                          | Hỗ trợ thương mại cần từ bên thứ ba                      |
| Thương mại          | Hỗ trợ chính thức từ nhà cung cấp, phù hợp môi trường doanh nghiệp       | Chi phí bản quyền cao                                    |

## 6. Chi phí

| Loại                | Ví dụ DBMS                          | Ghi chú                                                  |
|---------------------|-------------------------------------|----------------------------------------------------------|
| Miễn phí (OSS)      | MySQL, MariaDB, PostgreSQL, MongoDB CE, Redis | Tiết kiệm chi phí nhưng cần đội ngũ kỹ thuật nội bộ      |
| Thương mại (Có phí) | SQL Server, Oracle                 | Có chi phí bản quyền, đổi lại có nhiều tính năng và hỗ trợ |

