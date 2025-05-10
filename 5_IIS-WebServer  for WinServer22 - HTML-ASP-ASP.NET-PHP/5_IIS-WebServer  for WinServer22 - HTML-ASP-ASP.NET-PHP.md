**BÁO CÁO KẾT QUẢ CÔNG VIỆC**

1. **NHỮNG VIỆC ĐƯỢC TRIỂN KHAI**
1. **Triển khai site demo1 html basic trên web server IIS**
1. **Triển khai site demo2 ASP classic trên IIS**
1. **Triển khai site demo3 .net (3.5, 4.x) trên IIS**
1. **Triển khai site demo 4 .php trên IIS**
1. **ĐÃ HOÀN THÀNH**
- **Môi trường và công cụ sử dụng**
- Hệ điều hành: Windows Server 2022
- Dịch vụ web: Internet Information Services (IIS)
- Trình duyệt: Microsoft Edge
- **Cài đặt IIS trên Windows Server**
- **Cách 1**: Dùng Powershell với quyền admin để cài đặt IIS
- Chạy lệnh sau để cài đặt IIS: 

  *Install-WindowsFeature -Name Web-Server -IncludeManagementTools*

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.001.png)

- Kiểm tra việc cài đặt:  *Get-WindowsFeature -Name Web-Server*

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.002.png)
*cài đặt thành công!*

- **Cách 2**: Cài đặt IIS bằng **Server Manager** trên Windows Server 2022
- ` `Chạy Server Manager và nhấp vào Add roles and features.

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.003.png)

- Nhấn Next

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.004.png)

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.005.png)

- Chọn host bạn muốn thêm các service

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.006.png)

- Tích vào hộp Web Server (IIS)

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.007.png)

- Nhấn Next cho đến khi hiện hiện nút Install à click Install để hoàn tất cài đặt

![image](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.008.png)

- Chạy trình duyệt web và truy cập vào localhost, sau đó bạn có thể xác minh IIS có đang chạy bình thường không.

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.009.png) 
*Ok!*




1. **Triển khai site demo1 html basic trên web server IIS**
1. **Mục tiêu bài thực hành**
- Hiểu cách cài đặt và cấu hình IIS trên Windows Server.
- Biết cách tạo và triển khai một website tĩnh HTML lên IIS.
- Quản lý và kiểm tra hoạt động của website trên trình duyệt.
1. **Các bước triển khai**
1. **Tạo một thư mục chứa dự án HTML**: C:\WebServer\Demo1-html
- Tạo file index.html chứa nội dung HTML đơn giản

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.010.png)

1. **Thêm site mới trên IIS**
- Mở IIS Manager
- Chuột phải vào mục Sites > Add Website

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.011.png)

- Nhập thông tin: à nhấn OK
  - Đặt tên site name, chọn đến Application Pool, (chọn .NET v4.5 hoặc DefaultAppPool), sau đó OK
  - Physical Path: chọn đến đường dẫn thư mục vừa tạo có chứa file web config: C:\WebServer\Demo1-html
  - IP Address: Để mặc định hoặc gõ đúng cái địa chỉ IP của localhost (ở đây là 127.0.0.1), Port 80 là mặc định, 
  - Hostname: tùy ý.  

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.012.png)

è nhấn OK là đã tạo thành công site

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.013.png)

- Để trang web chạy được cần chỉnh sửa file hosts: Vào đường dẫn: C:\Windows\System32\drivers\etc , mở file hosts lên và làm như sau:
  - 127.0.0.1: IP localhost 
  - truongan.demo1.com:  Host name chúng ta đã thêm trong quá trình tạo Site), Lưu ý bỏ dấu # trước dòng
  - Lưu lại là OK

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.014.png)

- Mở trình duyệt gõ Host name chúng ta đã tạo, thì sẽ chạy được project Web của chùng ta

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.015.png)

èThành công tạo Site demo1 html basic trên WebServer IIS

1. **Triển khai site demo2 ASP classic trên IIS**
1. ` `**Bước 1: Kích hoạt ASP Classic trong IIS**
- Mở Server Manager.
- Vào Manage → Add Roles and Features.

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.016.png)

- Nhấn Next cho đến phần Roles, mở rộng Web Server (IIS) → Web Server → Application Development.
- Tích chọn mục “**ASP**”.

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.017.png)

- Nhấn Next → Install để **kích hoạt ASP Classic trong IIS:**

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.018.png)

1. **Bước 2: Tạo thư mục chứa source code ASP Classic** 

   (tương tự như ở Phần I)

- Tạo file source code đơn giản default.asp

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.019.png)

1. **Bước 3: Tạo site mới trong IIS**
- Các bước Add Website tương tự như ở Phần I

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.020.png)

- Lưu ý khi tạo xong cần kiểu tra Default Document trong trong IIS như hình 

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.021.png)

- Mở Default Document lên nếu không thấy chứa file source code vừa tạo thì add thêm hoặc cấu hình lại site để đảm bảm trang web hoạt động được. Nếu đầy đủ rồi thì ok

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.022.png)

1. **Bước 4: Cấu hình file hosts**

   Tương tự như bài trước cần cấu hình file hosts để chỉnh sửa **hostname** có thể truy cập được như lúc cấu hình site

   ![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.023.png)

- Kết quả: 

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.024.png)


1. **Triển khai site demo3 .net (3.5, 4.x) trên IIS**
1. **Bước 1: Cài đặt ASP.NET trên IIS và các thành phần cần thiết**
- Mở Server Manager → chọn "Add Roles and Features".
- Chọn các mục sau: à Install 
  - **Web Server (IIS)**
    - .NET Framework 3.5 Features
    - .NET Framework 4.8 Features
- Trong phần **Web Server Role (IIS) → Role Services**, tích chọn:
  - **Application Development:**
    - .NET Extensibility 3.5
    - .NET Extensibility 4.8
    - ASP.NET 3.5
    - ASP.NET 4.8
    - ISAPI Extensions
    - ISAPI Filters
  - **Common HTTP Features:**
    - Static Content
    - Default Document
    - Directory Browsing
    - HTTP Errors
    - HTTP Redirection
  - **Security:**
    - Request Filtering
    - Windows Authentication (nếu cần)
  - **Management Tools:**
    - IIS Management Console

![C:\Users\DELL\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState\media\b85bef83-bbd7-4723-b572-0be41da5bcb5.png](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.025.png)

- Cài đặt xong khởi động lại Server


1. **Bước 2: Chuẩn bị mã nguồn website (tạp thư mục chứa site)**

   (Tương tự như các phần trước)

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.026.png)

1. **Bước 3: Tạo website mới trong IIS**

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.027.png)

**è Kết quả:** 

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.028.png)

**IV.	Triển khai site demo 4 .php trên IIS**

1. **Bước 1: Cài đặt PHP cho IIS**
- Tải PHP từ <https://windows.php.net/download/>, lựa chọn phiên bản phù hợp (ở đây mình chọn bản 8.2.28)

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.029.png)

- Giải nén vào thư mục, ví dụ: **C:\PHP**

1. **Bước 2: Cấu hình IIS Handler Mapping** 

   (Do Web Platform Installer (Web PI) đã dừng hoạt động từ năm 2022 nên ta cần cấu hình IIS Handler Mapping để chạy file **.php** thủ công trên IIS)

- Mở IIS Manager **à** chọn site muốn cấu hình **à** chọn **Handler Mappings**

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.030.png)

- Điền thông tin Mapping:

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.031.png)

1. **Bước 3: Cấu hình thêm cho FastCGI**
- Quay lại giao diện chính của site à Mở **FastCGI Settings**

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.032.png)

- Click đúp vào dòng  php-cgi.exe và thêm các biến sau:

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.033.png)

- Sau khi cấu hình xong **Restart IIS** để hoàn tất
1. **Bước 4: Tạo thư mục chứa site (file mã nguồn)**
- Tương tự như các phần trước, tạo 1 file **index.php** trong thư mục vừa tạo

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.034.png)

1. **Bước 5: Tạo website mới trong IIS**

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.035.png)

1. **Bước 6: Cấu hình file hosts**

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.036.png)

- **Truy cập web site è kết quả:**

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.037.png)
