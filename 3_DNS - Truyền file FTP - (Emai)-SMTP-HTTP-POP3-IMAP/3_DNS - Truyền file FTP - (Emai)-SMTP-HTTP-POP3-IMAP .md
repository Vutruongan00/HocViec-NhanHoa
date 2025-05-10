`		`**BÁO CÁO KẾT QUẢ CÔNG VIỆC NGÀY 23/4**

1. **NHỮNG VIỆC ĐƯỢC TRIỂN KHAI**
1. **Giao thức truyền file – FTP**
1. Khái niệm
1. Hoạt động của FTP
1. Các câu lệnh FTP cơ bản
1. **Thư tín điện tử (E-mail) trên Internet**
1. Giao thức SMTP
1. So sánh SMTP và HTTP
1. Giao thức truy nhập mail: Giao thức POP3 và giao thức IMAP
1. **Dịch vụ tên miền DNS**
1. Các dịch vụ của DNS
1. Cơ chế hoạt động của DNS
1. Các loại bản ghi, tham số và giá trị tương ứng
1. **ĐÃ HOÀN THÀNH**
1. **Giao thức truyền file FTP**
1. **Khái niệm**
- Giao thức FTP (File Transfer Protocol) là một giao thức trao đổi file khá phổ biến hiện nay, nó được sử dụng nhiều nhất vào mục đích truyền tải dữ liệu.
- Mục đích sử dụng: 
- Được sử dụng để trao đổi tập tin qua mạng lưới truyền thông sử dụng TCP/IP (internet, mạng nội bộ, ...)
- Sử dụng để tải lên (upload) tệp từ máy tính lên server hoặc tải xuống (download) tệp từ server về máy tính
- Quản lý tệp trên server từ xa (đổi tên, xóa, tạo thư mục…).
1. **Hoạt động của FTP**
   1. **Mô hình hoạt động của FTP**
- Giống như hầu hết các giao thức TCP/IP, nó dựa trên mô hình **Client – Server:**
  - Client là bên gửi yêu cầu (ví dụ: FileZilla, WinSCP, trình duyệt web, lệnh FTP trong terminal).
  - Server là nơi lưu trữ file, xử lý yêu cầu truyền file.
- Tuy nhiên, khác với các ứng dụng khác chạy trên nền TCP/IP,  

  FTP cần tới **2 kết nối TCP :**

  - **Control connection (Kênh điều khiển)** - Sử dụng cổng 21: Đây là kết nối TCP logic chính được tạo ra khi phiên làm việc được thiết lập. Nó được duy trì trong suốt phiên làm việc và chỉ cho các thông tin điều khiển đi qua ví dụ như lệnh và trả lời. Nó không được sử dụng để gửi dữ liệu.
  - **Data connection (Kênh dữ liệu)** - Sử dụng cổng 20: Mỗi khi dữ liệu được gửi từ sever tới client hoặc ngược lại, một kết nối dữ liệu TCP riêng biệt được thiết lập giữa chúng. Dữ liệu được truyền qua kết nối này. Khi hoàn tất việc truyền dữ liệu, kết nối được hủy bỏ.

![https://images.viblo.asia/full/cda61ae3-5cb4-4256-828c-0785e385defe.png](Aspose.Words.48bf41a9-4faf-41d5-9617-1aefb39404be.001.png)

*FPT gồm 2 đường: Kiểm soát và dữ liệu*

- Sự phân chia thành 2 kênh riêng biệt này tạo ra sự linh động trong cách hoạt động của FTP nhưng nó cũng tạo ra sự phức tạp cho FTP.
  1. **Cấu trúc thành phần trong FTP**
- Do chức năng điều khiển và dữ liệu được truyền tải bằng cách sử dụng các kênh riêng biệt nên mô hình FTP chia phần mềm trên mỗi thiết bị thành 2 thành phần giao thức logic chịu trách nhiệm cho mỗi kênh:
  - **Thành phần Protocol Interpreter (PI):** Là thành phần quản lý kênh điều khiển, phát và nhận lệnh và trả lời.
  - **Thành phần Data Transfer Process (DTP):** chịu trách nhiệm gửi và nhận dữ liệu giữa client và server.

![](Aspose.Words.48bf41a9-4faf-41d5-9617-1aefb39404be.002.png)

1. **Tiến trình hoạt động của từng phần trong mô hình FTP**
- **Tiến trình bên phía server:**
  - **Server Protocol Interpreter (Server-PI):** chịu trách nhiệm quản lý điều khiển kết nối trên server. Nó lắng nghe yêu cầu kết nối hướng từ users trên cổng dành riêng. Khi kết nối đã được thiết lập,nó nhận lện từ User-PI, gửi trả lời lại và quản lí tiến trình truyền dữ liệu trên server.
  - **Server Data Transfer Process (Server-DTP):** Làm nhiệm vụ gửi hoặc nhận file từ bộ phận User-DTP. Server DTP vừa làm nhiệm vụ thiết lập kết nối kênh dữ liệu và lắng nghe một kết nối kênh dữ liệu từ user. Nó tương tác với server file trên hệ thống cục bộ để đọc và chép file.
- **Các tiến trình bên phía Client:**
  - **User Interface:** Đây là chương trình được chạy trên máy tính,nó cung cấp giao diện xử lí cho người dùng. Nó cho phép sử dụng các lệnh đơn giản hướng tới người dùng, và cho phép người điều khiển phiên FTP theo dõi được các thông tin và kết quả xảy ra trong tiến trình.
  - **User Protocol Interpreter (User-PI):** Chịu trách nhiệm quản lý kênh điều khiển phía Client. Nó khởi tạo phiên kết nối FTP bằng việc phát hiện ra yêu cầu tới phía server-PI. Khi kết nối đã được thiết lập, nó xử lí các lệnh nhận được trên giao diện người dùng, gửi chúng tới Server-PI, và nhận phản hồi trở lại. Nó cũng quản lý tiến trình User-DTP.
  - **User Data Transfer Process (User-DTP):** là bộ phận DTP nằm ở phía người dùng, làm nhiệm vụ gửi hoặc nhận dữ liệu từ Server-DTP. User-DTP có thể thiết lập hoặc lắng nghe yêu cầu kết nối kênh dữ liệu trên server. Nó tương tác với thiết bị lưu trữ file phía client.

  1. **Các chế độ truyền dữ liệu**
- **Active Mode**: Server chủ động kết nối đến client để truyền dữ liệu.
  - Phía Server-DTP tạo kênh dữ liệu bằng cách mở một cổng kết nối tới User-DTP. Server sử dụng cổng đặc biệt được dành riêng cho kết nối dữ liệu là cổng số 20. Trên máy Client, cổng mặc định được sử dụng chính là cổng được sử dụng để kết nối điều khiển, nhưng Server sẽ thường chọn mỗi cổng khác nhau cho mỗi chuyển giao.
- **Passive Mode**: Client chủ động kết nối đến server để nhận dữ liệu.​
  - Server sẽ chấp nhận 1 yêu cầu kết nối dữ liệu được khởi tạo từ Client.
  - Server sẽ trả lời lại phía Client với địa chỉ IP cũng như địa chỉ cổng mà Server sẽ sử dụng.
  - Sau đó phía Server-DTP lắng nghe trên cổng này một kết nối TCP đến từ User-DTP.
  - Theo mặc định, phía Client sẽ sử dụng cùng cổng mà nó sử dụng cho Control Connection như trong trường hợp chủ động. Tuy nhiên, trong phương pháp này, Client cũng có thể chọn sử dụng một cổng khác cho Data Connection nếu cần thiết.

  1. **Các phương thức truyền dữ liệu**

Khi Client-DTP và Server-DTP thiết lập xong kênh dữ liệu, dữ liệu sẽ được truyền trực tiếp từ phía Client tới phía Server, hoặc ngược lại, tùy theo các lệnh được sử dụng. Do thông tin điều khiển được gửi đi trên kênh điều khiển, nên toàn bộ kênh dữ liệu có thể được sử dụng để truyền dữ liệu. FTP có ba phương thức truyền dữ liệu, đó là: stream mode, block mode, và compressed mode.

- **Stream mode:**
  - Dữ liệu truyền đi liên tiếp dưới dạng các byte không cấu trúc.
  - Thiết bị gửi chỉ đơn thuần đẩy luồng dữ liệu qua kết nối TCP tới phía nhận.
  - Không có trường tiêu đề nhất định
  - Không có cấu trúc dạng Header, nên việc báo hiệu kết thúc file sẽ đơn giản được thực hiện khi thiết bị gửi ngắt kênh kết nối dữ liệu khi đã truyền dữ liệu xong.
  - Được sử dụng nhiều nhất trong 3 phương thức trong triển khai FTP thực tế. Do:
    - Là phương thức mặc định và đơn giản nhất.
    - Là phương thức phổ biến nhất, vì nó xử lí các file chỉ đơn thuần là xử lí dòng byte, mà không cần để ý tới nội dung.
    - Không tốn 1 lượng byte “overload” nào để thông báo Header.
- **Block Mode:**
  - Phương thức truyền dữ liệu mang tính quy chuẩn hơn.
  - Dữ liệu được chia thành nhiều khối nhỏ và đóng gói thành các FTP block.
  - Mỗi block có 1 trường Header 3 byte: báo hiệu độ dài, và chứa thông tin về các khối dữ liệu đang được gửi.
  - Một thuật toán đặc biệt được sử dụng để kiểm tra các dữ liệu đã truyền đi. Và để phát hiện, khởi tạo lại đối với 1 phiên truyền dữ liệu đã bị ngắt kết nối.
- **Compressed mode (Chế độ nén)**
  - Phương thức truyền dữ liệu sử dụng 1 kỹ thuật nén đơn giản, là “run-lenght encoding (mã hóa chiều dài)” – có tác dụng phát hiện và xử lí các đoạn lặp trong dữ liệu được truyền đi để giảm chiều dài của toàn bộ thông điệp.
  - Thông tin sau khi được nén, sẽ được xử lí như Block mode, với trường Header.
  - Trong thực tế, việc nén dữ liệu thường được thực hiện ở chỗ khác, làm cho phương thức Compressed mode trở nên không cần thiết.
  1. Các kiểu dữ liệu hỗ trợ
- Các tập tin được coi như một tập hợp các byte. FTP không quan tâm nội dung tập tin, sẽ chỉ đơn giản là di chuyển các tệp tin, các byte cùng 1 thời điểm, từ nơi này sang nơi khác.
- Có bốn kiểu dữ liệu khác nhau được quy định trong chuẩn của FTP:
  - ASCII: Định nghĩa một file văn bản ASCII, với dòng được đánh dấu bởi một số loại dòng đánh dấu kết thúc như mô tả ở trên.
  - EBCDIC: Khái niệm tương tự như loại ASCII, nhưng đối với tập tin bằng cách sử dụng kí tự EBCDIC của IBM đặt.
  - Image: Các tập tin đã không có cấu trúc nội bộ chính thức và được gửi một byte tại một thời điểm mà không có bất kỳ xử lý; đây là chế độ "hộp đen" đã đã đề cập ở trên.
  - Local: Kiểu dữ liệu này được sử dụng để xử lý các tập tin có thể lưu trữ dữ liệu trong byte logic có chứa một số bit khác 8. Cách xác định loại này cùng với cách dữ liệu có cấu trúc cho phép dữ liệu được lưu trữ trên hệ thống đích một cách phù hợp với đại diện địa phương của nó.
  1. Tính bảo mật của FTP
- FTP truyền thông tin đăng nhập (username và password) dưới dạng văn bản thuần túy, dễ bị đánh cắp. Để tăng cường bảo mật, có thể sử dụng các biến thể như FTPS (FTP Secure) hoặc SFTP (SSH File Transfer Protocol).

  è**TÓM GỌN LẠI:** 

1) **FTP (File Transfer Protocol)** là một giao thức mạng chuẩn được sử dụng để truyền tệp tin giữa máy tính cá nhân (client) và máy chủ (server) thông qua mạng TCP/IP (thường là Internet). Có chức năng:
   - Tải lên (upload) tệp từ máy tính lên server.
   - Tải xuống (download) tệp từ server về máy tính.
   - Quản lý tệp trên server từ xa (đổi tên, xóa, tạo thư mục…).
1) **Cách hoạt động của FTP:**
- **Mô hình Client – Server:**
  - Client là bên gửi yêu cầu (ví dụ: FileZilla, WinSCP, trình duyệt web, lệnh FTP trong terminal).
  - Server là nơi lưu trữ file, xử lý yêu cầu truyền file.
- **Quá trình hoạt động cơ bản:**
  - **Kết nối tới máy chủ FTP**: Client nhập địa chỉ IP/host, username, password.
  - **Xác thực**: Nếu đúng thông tin đăng nhập, server cho phép truy cập.
  - **Giao tiếp qua 2 kết nối TCP**:
    - **Cổng 21**: dùng để điều khiển (control connection).
    - **Cổng 20**: dùng để truyền dữ liệu (data connection).
- **Hai chế độ truyền:**
  - **Active Mode:** Client mở một cổng ngẫu nhiên để chờ kết nối từ Server**.**
  - **Passive Mode:** Server mở một cổng ngẫu nhiên để chờ Client kết nối đến (giúp vượt qua firewall dễ hơn).
- **Các phương thức truyền dữ liệu**
  - **Stream Mode:** Dữ liệu được truyền dưới dạng dòng byte liên tục.
  - **Block Mode:** Dữ liệu được chia thành các khối, mỗi khối có tiêu đề riêng.
  - **Compressed Mode:** Dữ liệu được nén trước khi truyền để tiết kiệm băng thông.
2. **Các câu lệnh FTP cơ bản**

|**Lệnh**|**Mô tả**|
| :- | :- |
|open <host>|Kết nối tới server FTP|
|user <username> <password>|Đăng nhập vào server|
|ls hoặc dir|Liệt kê file/thư mục trên server|
|cd <folder>|Chuyển thư mục trên server|
|lcd <folder>|Chuyển thư mục cục bộ (local)|
|get <filename>|Tải file từ server về máy|
|put <filename>|Tải file từ máy lên server|
|mget <file1> <file2>|Tải nhiều file về|
|mput <file1> <file2>|Tải nhiều file lên|
|delete <filename>|Xóa file trên server|
|mkdir <folder>|Tạo thư mục trên server|
|rmdir <folder>|Xóa thư mục trên server|
|bye hoặc quit|Thoát khỏi phiên làm việc FTP|

1. **Thư tín điện tử (E-mail) trên Internet**

E-mail là phương tiện trao đổi thông điệp điện tử qua mạng Internet. Quá trình gửi và nhận email bao gồm hai giai đoạn chính:

- Gửi thư: dùng giao thức SMTP.
- Nhận thư: dùng giao thức POP3 hoặc IMAP.
1. **Giao thức SMTP**
1. **Khái niệm**
- **SMTP (Simple Mail Transfer Protocol)** là giao thức chuẩn TCP/IP được dùng để **gửi thư đi** từ một máy khách (email client) đến máy chủ thư (mail server) hoặc từ một máy chủ này đến một máy chủ khác
1. **Chức năng của SMTP**
- Cho phép gửi mail với số lượng lớn,nhanh mà không bị giới hạn như các hòm mail miễn phí
- SMPT Server giúp bạn thao tác gửi thư qua giao thức TCP hoặc IP
1. **Đặc điểm của SMTP**
- Giao thức hoạt động ở tầng ứng dụng trong mô hình OSI.
- Sử dụng port 25 (hoặc 587 cho kết nối có bảo mật TLS).
- Gửi mail theo mô hình **push** (đẩy mail từ người gửi đến server nhận).
- Không hỗ trợ tải thư về máy người dùng – chỉ dùng cho việc gửi.
1. **Phương thức hoạt động của SMTP**

![SMTP hoạt động như thế nào](Aspose.Words.48bf41a9-4faf-41d5-9617-1aefb39404be.003.png)

- Người dùng nhập nội dung Email trên máy tính cá nhân - Client (Gmai, Outlook,…
- Ứng dụng email trên máy người gửi sử dụng SMTP để gửi thông điệp đến Mail Server của người nhận
- Mail Server lưu trữ tạm thời email
- Ứng dụng Email của người nhận sẽ truy xuất thư bằng IMAP hoặc POP3 và kéo nó về hộp thư để người nhận truy cập hoặc tải về
1. **So sánh SMTP và HTTP**
1. **Giống nhau:**
- **Lớp giao thức** : Cả SMTP và HTTP đều hoạt động ở lớp ứng dụng của bộ giao thức Internet, cho phép giao tiếp qua Internet.
- **Mô hình Client-Server** : Cả hai giao thức đều tuân theo kiến trúc Client-Server trong đó máy Client  khởi tạo yêu cầu và phía Server phản hồi các yêu cầu đó. 
  - Ví dụ, máy khách email gửi email bằng SMTP, trong khi trình duyệt web yêu cầu các trang web bằng HTTP.
1. **Khác nhau:**
- **Mục đích:**
  - SMTP được thiết kế riêng để gửi và chuyển tiếp tin nhắn email giữa các máy chủ. Nó chủ yếu được sử dụng để truyền email.
  - HTTP được sử dụng để truyền các tài liệu siêu văn bản, chẳng hạn như các trang web và là nền tảng của truyền thông dữ liệu trên World Wide Web (www).
- **Định dạng dữ liệu:**
  - SMTP truyền dữ liệu ở định dạng văn bản thuần túy, được cấu trúc theo một cách cụ thể (có tiêu đề và nội dung) để tạo điều kiện thuận lợi cho việc liên lạc qua email.
  - HTTP có thể truyền nhiều loại dữ liệu khác nhau (văn bản, hình ảnh, video, v.v.) và sử dụng các loại MIME trong tiêu đề để chỉ định loại nội dung được gửi.


|**Tiêu chí**|**SMTP**|**HTTP**|
| :- | :- | :- |
|Mục đích|Gửi email|Truy cập trang web|
|Kiểu giao tiếp|Giao tiếp **hai chiều**, có lưu trạng thái giữa các phần liên quan (headers, nội dung, đính kèm...)|Giao tiếp **một chiều**, không lưu trạng thái|
|Giao thức push/pull|Push|Pull|
|Port mặc định|25 (587 nếu bảo mật)|80 (443 nếu HTTPS)|
|Dữ liệu truyền|Thường là văn bản, đính kèm file|HTML, hình ảnh, video, JS, CSS,...|

1. **Giao thức truy nhập mail: POP3 và IMAP**
   1. **Giao thức POP3 (Post Office Protocol version 3)**
1. **Khái niệm:** 

**POP3** (**Post Office Protocol version 3)** là một giao thức dùng để **tải email từ máy chủ thư (mail server) về máy tính cá nhân (client)**. POP3 hoạt động theo cơ chế **pull**, nghĩa là client sẽ chủ động kết nối đến mail server để lấy thư.

1. **Cách thức hoạt động của POP3**

Khi người dùng muốn đọc email, chương trình  Email của họ sẽ sử dụng giao thức **POP3** để kết nối đến máy chủ email và tải xuống email về máy tính cá nhân. Thông thường, email sẽ bị xóa khỏi máy chủ sau khi đã được tải xuống. POP3 không hỗ trợ đồng bộ hóa email trên nhiều thiết bị vì nó chỉ tải email về một thiết bị và xóa chúng từ máy chủ.

1. **Ưu điểm của POP3**
- Tiết kiệm dung lượng, tài nguyên trên máy chủ-> tăng hiệu xuất hệ thống
- Truy cập ngoại tuyến dễ dàng: khi tải về có thể đọc mà không cần mạng
- Sử dụng đơn giản, dễ triển khai
- Đảm báo tính riêng tư
1. **Nhược điểm của POP3**
- Không có tính đồng bộ: không thể xem tin nhắn trên các thiết bị khác
- Nguy cơ mất dữ liệu nếu emai bị xóa khỏi máy chủ và thiết bị cá nhân bị hỏng sẽ không thể phục hồi
- Không hỗ trợ đầy đủ chức năng quản lý Email

![Theo dõi một số đặc điểm cơ bản ](Aspose.Words.48bf41a9-4faf-41d5-9617-1aefb39404be.004.jpeg)

1. **IMAP (Internet Message Access Protocol)**
1. **Khái niệm**
- **IMAP** (Internet Message Access Protocol) là một giao thức mạng được sử dụng trong hệ thống email **để truy cập** và **quản lý hộp thư từ xa** trên máy chủ thư. 
- Khác với POP3 thì IMAP cho phép người dùng duy trì trạng thái tin nhắn trên máy chủ giúp đồng bộ và quản lý được hộp thư hiệu quả trên nhiều thiết bị.
1. **Cách thức hoạt động của IMAP**
- **IMAP** hoạt động dựa trên mô hình Client-Server,  trong đó người dùng (client) truy cập và quản lý hộp thư trên máy chủ thư (server). Dưới đây là cách thức hoạt động cơ bản của IMAP:
  - **Thiết lập kết nối**: Người dùng sử dụng một ứng dụng email hoặc chương trình khách IMAP để thiết lập kết nối với máy chủ thư sử dụng giao thức IMAP. Cổng TCP/IP 143 được sử dụng cho kết nối không được mã hóa và cổng 993 được sử dụng cho kết nối được mã hóa bằng SSL/TLS.
  - **Xác thực**: người dùng xác thực bằng **username/password**.
  - **Đồng bộ hóa hộp thư**: IMAP không tải toàn bộ nội dung email ngay lập tức, mà chỉ tải tiêu đề (subject, sender, thời gian…) và cấu trúc thư mục (Inbox, Sent, Trash...).
  - **Truy cập và quản lý tin nhắn**: Người dùng có thể xem danh sách, đọc, gửi, trả lời, chuyển tiếp và xóa tin nhắn, đánh dấu đã/chưa đọc. Các thao tác này sẽ được thực hiện trực tiếp trên máy chủ thư.
  - **Đồng bộ hóa trạng thái**: Mọi thao tác truy cập và quản lý tin nhắn  đều sẽ được cập nhật trên máy chủ. Điều này cho phép người dùng duy trì trạng thái tin nhắn giữa các thiết bị khác nhau
  - **Quản lý thư mục**: tạo xóa, đổi tên, di chuyển tin nhắn vào thư mục trong hộp thư àdễ dàng quản lý tin nhắn theo nhu cầu người dùng
1. **Ưu điểm/ Nhược điểm của IMAP**

|**Ưu điểm**|**Nhược điểm**|
| :- | :- |
|Đồng bộ giữa nhiều thiết bị|Cần phải có kết nối Internet|
|Truy cập thư từ mọi nơi|Tốn dung lượng lưu trữ trên máy chủ nếu không quản lý tốt|
|Dễ quản lý|Cấu hình phức tạp hơn POP3|
|Không lo mất email khi hỏng/mất thiết bị|Tải chậm nếu hộp thư quá lớn|
|Tương thích với các ứng dụng email hiện đại|Có nguy cơ bị lộ lọt thông tin nếu máy chủ không được bảo mật tốt hoặc bị tấn công|

1. **Sự khác biệt giữa POP3 và IMAP**

|**Tiêu chí**|**POP3**|**IMAP**|
| :- | :- | :- |
|**Tên đầy đủ**|Post Office Protocol version 3|Internet Message Access Protocol|
|**Mục đích**|Tải thư từ server về máy, lưu trữ cục bộ|Quản lý thư trực tiếp trên server|
|**Phương thức hoạt động**|Tải toàn bộ thư về máy, thường xóa khỏi server|Chỉ tải nội dung cần thiết, giữ thư trên server|
|**Đồng bộ hóa**|Không đồng bộ giữa các thiết bị|Đồng bộ hóa hoàn toàn giữa các thiết bị|
|**Làm việc offline**|Có thể đọc thư offline sau khi tải|Hạn chế nếu chưa tải thư|
|**Quản lý thư mục**|Không hỗ trợ|Hỗ trợ tạo thư mục, phân loại, gắn nhãn|
|**Tìm kiếm trên server**|Không hỗ trợ|Hỗ trợ tìm kiếm từ xa trên server|
|<p>**Độ an toàn** </p><p>**dữ liệu**</p>|Dễ mất thư nếu thiết bị lỗi hoặc mất|An toàn hơn vì thư nằm trên server|
|**Port mặc định**|110 (POP3), 995 (POP3S – bảo mật SSL/TLS)|143 (IMAP), 993 (IMAPS – bảo mật SSL/TLS)|
|**Phù hợp với ai?**|Người dùng một thiết bị, cần lưu trữ cục bộ|Người dùng nhiều thiết bị, cần đồng bộ và linh hoạt|
|**Yêu cầu kết nối mạng**|Không cần thường xuyên|Cần kết nối mạng để hoạt động hiệu quả|
|**Tài nguyên máy chủ**|Tiết kiệm tài nguyên server|Tốn tài nguyên lưu trữ trên server|

1. **Dịch vụ tên miền DNS**	
1. **Khái niệm**
- **DNS (Domain Name System)** là hệ thống phân giải tên miền. DNS là hệ thống cho phép thiết lập tương ứng giữa địa chỉ IP và tên miền trên Internet.
1. **Chức năng của DNS**
- **Phân giải tên miền**: DNS chuyển đổi các tên miền dễ nhớ thành địa chỉ IP mà máy tính sử dụng để giao tiếp với nhau. Điều này giúp người dùng truy cập trang web mà không cần nhớ các địa chỉ IP phức tạp.
- **Quản lý tên miền**: DNS cho phép tổ chức và quản lý các tên miền, bao gồm việc đăng ký, cập nhật, và hủy bỏ tên miền. Nó giúp xác định quyền sở hữu và thông tin liên quan đến từng tên miền.
- **Cung cấp thông tin bổ sung**: Ngoài địa chỉ IP, DNS còn có thể cung cấp thông tin bổ sung như:
  - **Thông tin máy chủ thư (MX records):** Xác định máy chủ gửi và nhận email cho một tên miền.
  - **Thông tin về dịch vụ (SRV records)**: Cung cấp thông tin về các dịch vụ mạng khác nhau mà tên miền hỗ trợ.
  - **Thông tin về bảo mật (TXT records)**: Sử dụng để xác minh danh tính và cung cấp các thông tin bảo mật cho tên miền.
- **Tăng tốc độ truy cập**: DNS lưu trữ các bản ghi tên miền trong bộ nhớ cache, giúp tăng tốc độ truy cập cho các tên miền đã được tìm kiếm trước đó. Khi người dùng truy cập lại, máy chủ có thể trả lại địa chỉ IP mà không cần thực hiện yêu cầu đến các máy chủ DNS khác.
- **Đảm bảo tính khả dụng**: DNS có thể phân phối tải giữa nhiều máy chủ thông qua các bản ghi DNS, giúp cải thiện tính khả dụng và hiệu suất của các dịch vụ trực tuyến. Nếu một máy chủ không khả dụng, DNS có thể chuyển hướng yêu cầu đến máy chủ khác.
- **Cung cấp bảo mật**: DNS có thể tích hợp các tính năng bảo mật như DNSSEC (DNS Security Extensions) để bảo vệ chống lại các cuộc tấn công như giả mạo DNS (DNS spoofing) và đảm bảo rằng thông tin trả về từ máy chủ DNS là chính xác.
1. **Cách thức hoạt động của DNS**
- **Yêu cầu DNS**: Khi bạn nhập một tên miền vào trình duyệt, máy tính của bạn sẽ gửi yêu cầu đến máy chủ DNS để tìm địa chỉ IP tương ứng với tên miền đó.
- **Máy chủ DNS cục bộ**: Nếu máy tính của bạn đã lưu trữ địa chỉ IP của tên miền trong bộ nhớ cache, nó sẽ sử dụng địa chỉ này ngay lập tức. Nếu không, yêu cầu sẽ được gửi đến máy chủ DNS cục bộ (thường là máy chủ của nhà cung cấp dịch vụ Internet).
- **Máy chủ DNS phân cấp**: Nếu máy chủ DNS cục bộ không có thông tin, nó sẽ gửi yêu cầu đến các máy chủ DNS phân cấp. Hệ thống DNS được tổ chức theo cấu trúc phân cấp như sau
  - **Máy chủ DNS gốc**: Xác định máy chủ DNS cho tên miền cấp cao hơn (như .com, .org, .net).
  - **Máy chủ DNS tên miền cấp hai**: Xác định máy chủ DNS cho miền cụ thể (như example.com).
- **Giải quyết địa chỉ IP**: Khi máy chủ DNS gốc hoặc cấp hai nhận được yêu cầu, nó sẽ tìm kiếm thông tin về tên miền và trả về địa chỉ IP tương ứng. Quá trình này có thể bao gồm nhiều bước, với nhiều máy chủ DNS khác nhau tham gia.
- **Trả kết quả**: Sau khi tìm thấy địa chỉ IP, máy chủ DNS sẽ gửi kết quả trở lại máy chủ DNS cục bộ, sau đó máy chủ này gửi kết quả về máy tính của bạn.
- **Kết nối đến máy chủ**: Giờ đây, máy tính của bạn có địa chỉ IP của trang web và có thể gửi yêu cầu đến máy chủ web để tải trang.
- **Lưu trữ trong bộ nhớ cache**: Thông tin về địa chỉ IP có thể được lưu trữ trong bộ nhớ cache của máy tính và máy chủ DNS trong một khoảng thời gian nhất định, giúp tăng tốc độ truy cập trong các lần tiếp theo.
1. **Phân loại DNS Server và vai trò của nó**

   Các DNS Server bao gồm:

   1. **Root Name Server**: Là một tập hợp các máy chủ DNS trên toàn cầu,chịu trách nhiệm cho việc phân giải tên miền trên internet. Hiện có khoảng 13 DNS root Server trên toàn thế giới
   - DNS root Server quản lý tất cả các tên miền Top-level-domain (cấp cao nhất bao gồm .com, .org, .net, .gov, .edu, .vn, .us, .uk, .fr,…). Khi có yêu cầu phân giải một Domain Name thành một địa chỉ IP, client sẽ gửi yêu cầu đến DNS gần nhất (DNS ISP). DNS ISP sẽ kết nối tới DNS root Server để hỏi địa chỉ của Domain Name.
   - DNS root Server sẽ căn cứ và dựa vào các Top Level của tên miền và từ đó có những tài liệu hướng dẫn phù hợp để chuyển hướng cho khách hàng đến đúng địa chỉ cần truy vấn.** 
   1. **DNS Recursor (DNS Resolver)**: Là máy chủ trung gian nhận yêu cầu từ người dùng và thực hiện truy vấn trên các máy chủ khác để lấy thông tin. Nó sẽ truy vấn từ Root Name Server đến các TLD nameservers và cuối cùng là Authoritative nameservers cho đến khi tìm thấy địa chỉ IP của tên miền cần tìm. DNS Recursor giúp người dùng không cần phải tự truy vấn từng máy chủ mà chỉ cần gửi yêu cầu tới DNS Recursor.
   1. **TLD Name Server ( Top level domain name server)** là nơi lưu trữ thông tin về các tên miền cấp cao nhất (Top-Level Domain) như .com, .org, .net, hoặc các tên miền quốc gia như .vn (Việt Nam), .jp (Nhật Bản). Sau khi nhận được yêu cầu từ Root Name Server, TLD nameserver sẽ trả về địa chỉ của Authoritative nameserver tương ứng. TLD Nameserver là bước trung gian để chuyển hướng từ cấp Root Name Server đến Authoritative nameserve.
   1. **Authoritative Name Server:** Server này có chứa thông tin chính xác về tên miền cụ thể. Nó là điểm dừng cuối trong truy vấn và phân giải địa chỉ IP cần thiết cung cấp cho **DNS Recursor**.
1. **Truy vấn phân cấp trong hệ thống DNS**

   DNS hoạt động theo cấu trúc phân cấp, từ gốc tới cụ thể:

- Khi người dùng nhập [www.google.com](http://www.google.com) vào thanh địa chỉ của trình duyệt và Enter Trình duyệt sẽ kiểm tra IP của  có được lưu trong bộ nhớ cache hay không. Nếu có sẽ truy cập ngay. Nếu không có trong Cache, trình duyệt sẽ gửi yêu cầu tới máy chủ DNS Resolver, sau đó:
1) **Root DNS Server**
- Resolver hỏi Root server: “Tôi muốn tìm [www.google.com](http://www.google.com)”
- Root server trả lời: “Tôi không biết chính xác, nhưng .com được quản lý bởi các server sau: ”
1) **TLD DNS Server (Top Level Domain - .com, .net, .org, ...)**
- DNS Resolver tiếp tục hỏi TLD server: “Tôi muốn tìm google.com”
- TLD server trả lời: “Google được quản lý tại Name Server này nè”
1) **Authoritative DNS Server**
- DNS Resolver hỏi tiếp Name Server của Google: “IP của www.google.com là gì?”
- Server trả lời: “Đây là IP của nó: 142.250.190.14”

  è **Trả kết quả và lưu cache**

- Resolver gửi IP về cho trình duyệt.
- IP được lưu lại ở nhiều lớp cache để tăng tốc cho các truy vấn sau.

**[ Trình duyệt → Cache → Resolver → Root → TLD → Authoritative → IP → Trình duyệt → Web server ]**

1. **Các loại bản ghi DNS**
- Bản ghi tên miền (DNS records) - là các thông tin cấu hình được lưu trữ trong hệ thống DNS (Domain Name System), dùng để chỉ định cách tên miền hoạt động, như: Trỏ tên miền về địa chỉ IP nào; xác định dịch vụ email; xác thực tên miền hoặc các chức năng khác
- Các bản ghi này bao gồm các tệp văn bản được viết bằng cú pháp DNS
- Mỗi bản ghi DNS chứa 4 trường chính: ***Name, Type, Data và TTL.***
  - ***Name***: Trường này cho phép thêm tiền tố (hay chính xác hơn là hậu tố, vì tên miền về mặt kỹ thuật được đọc từ phải sang trái) vào tên miền chính. (Nếu thêm bản ghi cho một domain phụ, chẳng hạn như shop.example.com, ta sẽ nhập “shop” vào trường này)
  - ***Type***: Loại bản ghi DNS xác định phần domain mà mỗi bản ghi sẽ thay đổi. Các loại bản ghi quan trọng nhất để giúp bạn thiết lập và vận hành được đề cập bên dưới.
  - ***Data*** : Trường dữ liệu chứa thông tin khác nhau tùy thuộc vào loại bản ghi đang tạo.
  - ***TTL*** (Time to Live): là thời gian tính bằng giây để mọi thay đổi đối với bản ghi DNS có hiệu lực. Với TTL là 3600, tất cả các thay đổi đối với bản ghi ví dụ này sẽ được làm mới sau mỗi 3600 giây (1 giờ).

  1. **Record SOA**
1. **Khái niệm: SOA (Start of Authority)** là bản ghi DNS xác định "vùng quyền quản lý" của tên miền và chứa thông tin quản trị của zone đó.

   Hiểu đơn giản r.SOA là nơi bắt đầu quản lý DNS cho tên miền này, và tại đây là thông tin cấu hình chính.”

1. **Vai trò - ứng dụng của Record SOA**
- Xác định **DNS server chính** cho một tên miền
- Quản lý **đồng bộ dữ liệu DNS giữa các máy chủ chính - phụ**
- Cung cấp **cấu hình cache & cập nhật** cho hệ thống DNS
- Luôn **là bản ghi đầu tiên** trong file zone của tên miền

1. **Cấu trúc thành phần của bản ghi SOA**
- Một bản ghi SOA có dạng như sau:

[tên miền] IN SOA [tên-server-dns] [địa-chỉ-email] (serial number; refresh number; retry number; expire number; time-to-live number)

- Ví dụ cú pháp:

vutruongan.com. IN  SOA  ns1.zonedns.com.  dnsadmin.vutruongan.com. (

`       	 `2025042601 ; Serial

`            `3600       ; Refresh

`            `1800       ; Retry

`            `1209600    ; Expire

`            `86400 )    ; Minimum TTL

`	`Trong đó: 

- vutruongan.com.  -  Là tên miền của tôi cần khai báo SOA
- ns1.zonedns.com. -  **Primary name server -NS:** giá trị DNS chính của tên miền hoặc máy chủ
- dnsadmin.vutruongan.com.  - Là email của quản trị DNS của tên miền  (viết dạng DNS, tức là  <dnsadmin@vutruongan.com>)
- **Serial**: [2025042601] -  Mã phiên bản cấu hình DNS (dạng: YYYYMMDDNN), giúp các DNS phụ biết khi nào cần cập nhật
- **Refresh**: [3600] - Sau 3600 giây (1 tiếng), các DNS phụ sẽ hỏi máy chính xem bản ghi có thay đổi không
- **Retry**: [1800] -  Nếu hỏi thất bại, sẽ thử lại sau 1800 giây (30 phút)
- **Expire**: [1209600] - Nếu sau 14 ngày mà không cập nhật được, DNS phụ sẽ ngừng phục vụ
- **Minimum TTL**: [86400] - Giá trị TTL mặc định cho các bản ghi DNS nếu không chỉ định rõ (1 ngày)

1. **Lưu ý khi sử dụng bản ghi SOA**
- Mỗi zone DNS chỉ có đúng 1 bản ghi SOA.
- Nếu cấu hình sai thông số SOA, DNS phụ có thể không đồng bộ → gây lỗi truy cập.

1. **Record A**
1. **Khái niệm**: Bản ghi A (Address) là loại bản ghi DNS dùng để liên kết một tên miền với một địa chỉ IP dạng IPv4.
1. **Vai trò và ứng dụng của bản ghi A**
- Truy cập website: Trỏ tên miền chính hoặc phụ đến máy chủ web (Apache, Nginx, v.v.).
- Trỏ subdomain (tên miền phụ), VD: mail.nhanhoa.com, api.nhanhoa.com, 
- Cấu hình với các hệ thống CDN, proxy, API, app mobile cũng yêu cầu bản ghi A để xác định máy chủ đích.
- Hạ tầng ảo hóa hoặc DNS nội bộ cũng sử dụng bản ghi A để định tuyến nội bộ.
1. **Cấu trúc thành phần trong bản ghi A**
- Cú pháp khai báo record A: **[domain\_name]  IN  A  [địa-chỉ-Ipv4].**

  Ví dụ: vutruongan.com. IN A 14.248.82.194 , Trong đó:

  1. vutruongan.com. - là tên miền đầy đủ muốn tạo, Dấu chấm ở cuối là cú pháp chuẩn DNS để chỉ rõ đây là tên miền gốc (root).
  1. IN - là viết tắt của **Internet**, chỉ loại hệ thống sử dụng (chuẩn phổ biến nhất trong DNS).
  1. A - Loại bản ghi
  1. 14.248.82.194 - Địa chỉ **IPv4** mà tên miền trỏ tới – chính là địa chỉ IP của máy chủ (server).
1. **Lưu ý khi sử dụng bản ghi A**
- **Không trùng với bản ghi CNAME** cho cùng tên miền.
- Không nên dùng bản ghi A cho nhiều subdomain nếu bạn định chuyển IP thường xuyên ~> nên dùng CNAME
- Có thể tạo **nhiều bản ghi A cho cùng tên miền** để load balancing.

  1. **Record AAAA**

Tương tự như bản ghi A, nhưng thay vì sử dụng IPv4 sẽ là địa chỉ IPv6.

1. **Record CNAME** 
1. **Khái niệm**: 
- Bản ghi **CNAME (Canonical name Record)** hay còn gọi là Bản ghi bí danh. Bản ghi CNAME cho phép một server có thể có nhiều tên. Nói cách khác bản ghi CNAME cho phép nhiều tên miền cùng trỏ đến một địa chỉ IP cho trước.
- Để có thể khai báo bản ghi CNAME, bắt buộc phải có bản ghi kiểu A để khai báo tên của máy. Tên miền được khai báo trong bản ghi kiểu A trỏ đến địa chỉ IP của máy được gọi là tên miền chính (canonical domain). Các tên miền khác muốn trỏ đến máy tính này phải được khai báo là bí danh của tên máy (alias domain).
  1. **Vai trò**
- Tạo bí danh (alias) cho một tên miền khác.
- Giúp nhiều subdomain trỏ về cùng một tên miền chính, mà không cần trỏ trực tiếp đến địa chỉ IP.
- Đơn giản hóa việc quản lý DNS: Khi tên miền chính thay đổi địa chỉ IP, chỉ cần cập nhật một nơi.
- Hỗ trợ trỏ domain phụ về dịch vụ bên ngoài như GitHub Pages, Blogger, dịch vụ email, CDN...
  1. **Cấu trúc thành phần trong bản ghi**
- Cấu trúc bản ghi CNAME:  **<subdomain>  IN  CNAME  <tên-miền-đích>.** 

  Ví dụ: shop.vutruongan.com. IN CNAME vutruongan.com.

Trong đó:

- shop.vutruongan.com. -  là tên miền phụ cần trỏ
- IN CNAME - tham số bản ghi
- vutruongan.com. – Tên miền chính mà subdomain sẽ trỏ đến

1. **Lưu ý khi sử dụng**
- **Không được dùng CNAME tại tên miền gốc**: Lý do domain gốc thường phải chứa các bản ghi khác như A, MX, TXT → CNAME gây xung đột (không thể tồn tại cùng các bản ghi khác).

  àchỉ dùng CNAME cho subdomain như www, blog, shop,…v.v.

- **Không dùng IP làm gì trị cho CNAME**: lý do CNAME chỉ trỏ tới tên miền, không trỏ tới IP
- **Nên kết thúc tên miền đích bằng dấu chấm (.)** trong cấu hình chuẩn để chỉ rõ là FQDN (tên miền đầy đủ).

  1. **Record MX**
1. **Khái niệm**: Bản ghi MX có tác dụng xác định, chuyển thư đến domain hoặc subdomain đích
1. **Vai trò của MX**
- Xác định máy chủ email (mail server) sẽ xử lý email gửi đến tên miền.
- Cho phép tên miền nhận email qua các hệ thống email chuyên dụng như Google Workspace, Microsoft 365, Zoho Mail…
- Hỗ trợ nhiều máy chủ dự phòng, đảm bảo ổn định và độ tin cậy của dịch vụ email.
1. **Cấu trúc, thành phần trong bản ghi MX**
- Cấu trúc: **<domain>  IN  MX  <priority>  <mail-server>**
- Ví dụ: vutruongan.com. IN MX 10 mail.vutruongan.com.  
  - vutruongan.com. - Là tên miền chính
  - IN  MX - Tham số bản ghi
  - 10 - Mức độ ưu tiên <**priority>**  (số càng nhỏ, ưu tiên càng cao)
  - mail.vutruongan.com.  - Là tên miền máy chủ xử lý email
1. **Lưu ý khi sử dụng bản ghi MX**
- **Mail server phải là một FQDN (tên miền đầy đủ)** – không được dùng địa chỉ IP.
- **Không sử dụng CNAME cho mail server** – tên trong bản ghi MX phải trỏ bằng bản ghi A hoặc AAAA. 
- Phải đảm bảo **mail server trỏ về đúng địa chỉ IP** (có bản ghi A tương ứng).
  1. **Record NS**
1. **Khái niệm** 
- **NS (Name Server) record** là một loại bản ghi DNS dùng để **xác định máy chủ tên miền (DNS server)** nào chịu trách nhiệm quản lý **các bản ghi DNS** của một tên miền **(NS sẽ cho biết “tên miền này đang được quản lý DNS ở đâu”)**
1. **Vai trò và ứng dụng của bản ghi NS**
- Chỉ định máy chủ DNS nào sẽ chịu trách nhiệm quản lý bản ghi DNS cho một tên miền.
- Nói cách khác: bản ghi NS giúp phân quyền điều khiển DNS cho tên miền của bạn.
- Cho phép tên miền hoạt động đúng trên Internet bằng cách trỏ đến DNS server lưu trữ các bản ghi A, MX, CNAME, TXT,...
1. **Cấu trúc, thành phần của bản ghi**
- Cấu trúc 1 bản ghi NS: [domain\_name] IN NS [DNS-Server\_name]
- Ví dụ: Khi tôi mua tên miền vutruongan.com dùng dịch vụ DNS của Nhân Hòa để quản lý các bản ghi, tôi cần trỏ tên miền về NS của họ như: ns1.zonedns.vn , ns2.zonedns.vn , ns3.zonedns.vn và (hoặc) ns4.zonedns.vn
- Cú pháp: vutruongan.com. IN NS ns1.zonedns.vn.

`          `vutruongan.com. IN NS ns2.zonedns.vn.

1. **Lưu ý khi sử dụng bản ghi**
- Một tên miền thường nên có ít nhất 2 bản ghi **NS** để đảm bảo hoạt động liên tục (dự phòng)
- Tên máy chủ NS **không được là địa chỉ IP**, mà phải là **tên miền đầy đủ**
- Khi thay đổi bản ghi NS, có thể mất thời gian để **DNS toàn cầu cập nhật (propagation)** – thường từ vài phút đến 24-48 giờ.
- Khi đã trỏ về NS khác, toàn bộ bản ghi DNS cần được **quản lý bên trong hệ thống đó**, không còn tác dụng nếu chỉnh ở nơi cũ.
- Bản ghi NS cấp cao (được quản lý ở nơi đăng ký tên miền) quyết định nơi quản lý toàn bộ DNS của tên miền.

  1. **Record PTR  (pointer)**
1. **Khái niệm:** PTR (Pointer) record **là bản ghi DNS dùng để** tra cứu ngược (Reverse DNS lookup) **–** tức là **từ địa chỉ IP suy ra tên miền.**
1. **Vai trò của PTR**
- **Xác thực IP trong hệ thống email**: Rất quan trọng để chống spam, nhiều máy chủ email yêu cầu IP gửi mail phải có PTR record hợp lệ.
- Giúp các dịch vụ **ghi lại log rõ ràng hơn** (ghi tên miền thay vì IP).
- **Tăng độ tin cậy** của hệ thống mạng, nhất là trong môi trường doanh nghiệp hoặc dịch vụ hosting.
1. **Cấu trúc, thành phần của bản ghi PTR**

<địa chỉ IP ngược dạng in-addr.arpa.> IN PTR <tên-miền>.

- Ví dụ: 194.82.248.14.in-addr.arpa. IN PTR vutruongan.com.

Trong đó: 194.82.248.14.in-addr.arpa.  - Tên miền tra cứu ngược tương ứng với địa chỉ IP 14.248.82.194

1. **Lưu ý khi sử dụng PTR**
- Để cấu hình bản ghi, cần liên hệ với đơn vị cung cấp IP cho bạn như các nhà cung cấp ISP , chứ không phải nhà cung cấp tên miền

  Ví dụ bạn cần cấu hình PTR của IP 14.248.82.194 về vutruongan.com

  Vì IP 14.248.82.194  do Viettel quản lý thì bạn cần liên hệ với ISP Viettel cung cấp IP 14.248.82.194 thay vì nhà cung cấp nhanhoa.com

- Nên có **bản ghi A tương ứng** cho tên miền dùng trong PTR.

  1. **Record SRV**
1. **Khái niệm**
- Bản ghi **SRV (Service Record)** là một loại bản ghi trong DNS dùng để chỉ định máy chủ cung cấp dịch vụ cụ thể cho một tên miền. Không giống như các bản ghi DNS khác (như A, CNAME hay MX), bản ghi SRV được sử dụng để xác định máy chủ lưu trữ dịch vụ dựa trên **tên dịch vụ, giao thức, độ ưu tiên,** và **cổng** mà dịch vụ đó đang sử dụng.
1. **Vai trò - ứng dụng của Record SRV**
- Bản ghi SRV rất quan trọng đối với các ứng dụng mạng, đặc biệt là các ứng dụng dựa trên IP như VoIP (thoại qua IP), XMPP (giao thức nhắn tin mở rộng), LDAP (dịch vụ thư mục), và nhiều dịch vụ khác.
- Bản ghi SRV giúp DNS không chỉ cung cấp địa chỉ IP mà còn thông tin về:
  - Tên dịch vụ (service)
  - Giao thức (TCP/UDP)
  - Tên miền cung cấp dịch vụ
  - Cổng mà dịch vụ chạy
  - Độ ưu tiên và trọng số để quyết định máy chủ nào sẽ được chọn nếu có nhiều máy chủ cung cấp dịch vụ.

1. **Cấu trúc thành phần bản ghi SRV**
- Bản ghi SRV có cấu trúc gồm các thành phần chính sau:
  - **Service**: Tên dịch vụ (ví dụ: \_sip cho dịch vụ VoIP, \_xmpp cho dịch vụ nhắn tin).
  - **Protocol**: Giao thức mà dịch vụ sử dụng, có thể là \_tcp hoặc \_udp.
  - **Priority**: Độ ưu tiên của máy chủ. Máy chủ có độ ưu tiên thấp hơn sẽ được chọn trước.
  - **Weight:** Trọng số để phân phối tải giữa các máy chủ có cùng độ ưu tiên.
  - **Port**: Cổng mà dịch vụ hoạt động (ví dụ: cổng 5060 cho SIP).
  - **Target:** Tên miền của máy chủ cung cấp dịch vụ.
- Cú pháp bản ghi SRV:

\_service.\_protocol.name. TTL IN SRV priority weight port target 

Trong đó:

|Thành phần|Mô tả|
| :-: | :-: |
|\_service|Dịch vụ đang sử dụng (ví dụ: \_sip, \_ldap)|
|\_protocol|Giao thức mạng (TCP hoặc UDP)|
|name|Tên miền chính (vd: vutruongan.com.)|
|TTL|Thời gian sống trong cache DNS|
|priority|Mức ưu tiên – số nhỏ hơn = ưu tiên cao hơn|
|weight|Trọng số – chia tải nếu có nhiều server cùng mức ưu tiên|
|port|Số cổng dịch vụ (vd: 5060 cho SIP, 389 cho LDAP)|
|target|Tên máy chủ (hostname) xử lý dịch vụ|

- Ví dụ: \_sip.\_tcp.vutruongan.com. 3600 IN SRV 10 20 5060 sipserver.vutruongan.com.
1. **Lưu ý khi sử dụng**
- Trỏ đến hostname: target trong bản ghi SRV phải là hostname, không phải địa chỉ IP. Hostname đó cần có bản ghi A/AAAA.
- Kiểm tra cổng: Phải đảm bảo **port** được mở và dịch vụ đang chạy tại server đích
- Không dùng chung với **CNAME:** target không được là bản ghi CNAME (phải là A/AAAA)

  1. **Record TXT**
1. **Khái niệm**
- Bản ghi TXT là bản ghi dùng để **lưu trữ dữ liệu văn bản (text)**, Cung cấp thông tin cho các máy chủ, dịch vụ (không phải con người đọc); Xác minh tên miền; Thiết lập bảo mật email (SPF, DKIM, DMARC).
1. **Vai trò ứng dụng của bản ghi TXT**
- **Vai trò chính của bản ghi TXT:**
  - Chứa các thông tin dạng text được dịch vụ bên ngoài (Google, Microsoft, mail server...) kiểm tra.
  - Đảm bảo an toàn khi gửi email, chống giả mạo, ngăn chặn thư rác
  - Giúp xác minh quyền sở hữu tên miền.
- **Ứng dụng:**

  |**Ứng dụng**|**Nội dung TXT**|
  | :-: | :-: |
  |**SPF** – chống giả mạo email|v=spf1 include:\_spf.google.com ~all|
  |**DKIM** – chữ ký số cho email|v=DKIM1; k=rsa; p=MIGfMA...|
  |**DMARC** – chính sách xác thực email|v=DMARC1; p=none; rua=mailto:admin@domain.com|
  |**Google Site Verification**|google-site-verification=abc123XYZ...|
  |**Microsoft 365 / AWS**|TXT đặc biệt để xác minh sở hữu tên miền|


1. **Cấu trúc thành phần bản ghi TXT**
- Cấu trúc một bản ghi TXT gồm:

  Tên\_miền. TTL IN  TXT "Nội\_dung\_text"

- Ví dụ 1: cấu hình SPF chống giả mạo email

vutruongan.com. 3600 IN TXT "v=spf1 include:\_spf.nhanhoa.com ~all"

trong đó:  "v=spf1 include:\_spf.nhanhoa.com ~all" → Nội dung bản ghi (chính là quy tắc SPF)

- Ví dụ 2 - xác minh Google Site:

vutruongan.com. 3600 IN TXT "google-site-verification=abc123456xyz"

- **Một số cấu trúc điển hình:**

|**Loại**|**Cấu trúc nội dung ví dụ**|**Giải thích**|
| :-: | :-: | :-: |
|**SPF**|v=spf1 include:\_spf.google.com ~all|Cho phép server Google gửi email cho domain|
|**DKIM**|v=DKIM1; k=rsa; p=MIGf...|Cung cấp public key để xác thực chữ ký email|
|**DMARC**|v=DMARC1; p=none; rua=mailto:dmarc@domain.com|Chính sách kiểm tra SPF, DKIM|
|**Xác minh dịch vụ**|google-site-verification=abc123...|Google kiểm tra quyền sở hữu domain|

1. **Lưu ý khi sử dụng bản ghi TXT**
- Phải dùng dấu ngoặc kép "..." quanh nội dung text.
- Không tự ý xuống dòng nếu chưa biết cách nối chuỗi (vì bản ghi DNS có giới hạn ký tự, thường là 255 ký tự mỗi chuỗi).
- Nội dung sai format có thể gây lỗi xác thực (ví dụ: mail bị vào spam).
- Một domain có thể có nhiều bản ghi TXT nhưng nên quản lý rõ ràng từng mục đích.
- SPF chỉ nên có một bản ghi duy nhất, nếu cần thêm nhiều IP, thì gom chung trong 1 bản ghi.

||
| :- |
|**Loại bản ghi**|**Chức năng**|**Giá trị bản ghi**|
| :- | :- | :- |
|**A**|Trỏ tên miền đến IPv4|example.com → 192.0.2.1|
|**AAAA**|Trỏ tên miền đến IPv6 (nếu có)|example.com → 2001:db8::1|
|**CNAME**|Alias - trỏ tên miền này đến tên miền khác|www.example.com → example.com|
|**MX**|Chỉ định máy chủ mail|mail.example.com |
|**NS**|Xác định máy chủ DNS ủy quyền|ns1.example.com|
|**PTR**|Phân giải ngược IP → tên miền|192\.0.2.1 → example.com|
|**TXT**|Lưu văn bản tùy ý, dùng trong SPF/DKIM, xác thực tên miền|v=spf1 include:\_spf.google.com ~all|
|**SRV**|Dùng cho các dịch vụ như SIP, XMPP|\_sip.\_tcp.example.com → 10 60 5060 sipserver.example.com|
|**SOA**|Thông tin gốc của miền (Start of Authority)|Chứa email admin, serial, refresh time...|
#
