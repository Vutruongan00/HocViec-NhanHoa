# **NHỮNG VIỆC ĐƯỢC TRIỂN KHAI**

[1. **Triển khai site demo1 html basic trên web server IIS**](#triển-khai-site-demo1-html-basic-trên-web-server-iis)
  
[2. **Triển khai site demo2 ASP classic trên IIS**](#triển-khai-site-demo2-asp-classic-trên-iis)
  
[3. **Triển khai site demo3 .net (3.5, 4.x) trên IIS**](#triển-khai-site-demo3-net-35-4x-trên-iis)
  
[4. **Triển khai site demo 4 .php trên IIS**](#triển-khai-site-demo-4-php-trên-iis)
   
# **ĐÃ HOÀN THÀNH**
## 1. **Triển khai site demo1 html basic trên web server IIS**
### 1.1 **Mục tiêu bài thực hành**
- Hiểu cách cài đặt và cấu hình IIS trên Windows Server.
- Biết cách tạo và triển khai một website tĩnh HTML lên IIS.
- Quản lý và kiểm tra hoạt động của website trên trình duyệt.
### 1.2. **Các bước triển khai**
#### **Tạo một thư mục chứa dự án HTML**:  C:\WebServer\Demo1-html
- Tạo file index.html chứa nội dung HTML đơn giản

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.010.png)

#### **Thêm site mới trên IIS**
- Mở IIS Manager
- Chuột phải vào mục Sites > Add Website

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.011.png)

- Nhập thông tin: à nhấn OK
  - Đặt tên site name, chọn đến Application Pool, (chọn .NET v4.5 hoặc DefaultAppPool), sau đó OK
  - Physical Path: chọn đến đường dẫn thư mục vừa tạo có chứa file web config: C:\WebServer\Demo1-html
  - IP Address: Để mặc định hoặc gõ đúng cái địa chỉ IP của localhost (ở đây là 127.0.0.1), Port 80 là mặc định, 
  - Hostname: tùy ý.  

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.012.png)

 nhấn OK là đã tạo thành công site

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.013.png)

- Để trang web chạy được cần chỉnh sửa file hosts: Vào đường dẫn: C:\Windows\System32\drivers\etc , mở file hosts lên và làm như sau:
  - 127.0.0.1: IP localhost 
  - truongan.demo1.com:  Host name chúng ta đã thêm trong quá trình tạo Site), Lưu ý bỏ dấu # trước dòng
  - Lưu lại là OK

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.014.png)

- Mở trình duyệt gõ Host name chúng ta đã tạo, thì sẽ chạy được project Web của chùng ta

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.015.png)

Thành công tạo Site demo1 html basic trên WebServer IIS

////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

## 2. **Triển khai site demo2 ASP classic trên IIS**
### **Bước 1: Kích hoạt ASP Classic trong IIS**
- Mở Server Manager.
- Vào Manage → Add Roles and Features.

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.016.png)

- Nhấn Next cho đến phần Roles, mở rộng Web Server (IIS) → Web Server → Application Development.
- Tích chọn mục “**ASP**”.

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.017.png)

- Nhấn Next → Install để **kích hoạt ASP Classic trong IIS:**

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.018.png)

### Bước 2: Tạo thư mục chứa source code ASP Classic** 

   (tương tự như ở Phần I)

- Tạo file source code đơn giản default.asp

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.019.png)

### **Bước 3: Tạo site mới trong IIS**
- Các bước Add Website tương tự như ở Phần I

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.020.png)

- Lưu ý khi tạo xong cần kiểu tra Default Document trong trong IIS như hình 

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.021.png)

- Mở Default Document lên nếu không thấy chứa file source code vừa tạo thì add thêm hoặc cấu hình lại site để đảm bảm trang web hoạt động được. Nếu đầy đủ rồi thì ok

![](Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.022.png)

### **Bước 4: Cấu hình file hosts**

   Tương tự như bài trước cần cấu hình file hosts để chỉnh sửa **hostname** có thể truy cập được như lúc cấu hình site

   ![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.023.png)

- Kết quả: 

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.024.png)

////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

## 3. **Triển khai site demo3 .net (3.5, 4.x) trên IIS**
### **Bước 1: Cài đặt ASP.NET trên IIS và các thành phần cần thiết**
- Mở Server Manager → chọn "Add Roles and Features".
- Chọn các mục sau: và Install (như hình dưới)
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

![C:\Users\DELL\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState\media\b85bef83-bbd7-4723-b572-0be41da5bcb5.png](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.025.png)

- Cài đặt xong khởi động lại Server


### **Bước 2: Chuẩn bị mã nguồn website (tạp thư mục chứa site)**

   (Tương tự như các phần trước)

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.026.png)

### **Bước 3: Tạo website mới trong IIS**

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.027.png)

** Kết quả:** 

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.028.png)

////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////


## **4. Triển khai site demo 4 .php trên IIS**

### **Bước 1: Cài đặt PHP cho IIS**
- Tải PHP từ <https://windows.php.net/download/>, lựa chọn phiên bản phù hợp (ở đây mình chọn bản 8.2.28)

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.029.png)

- Giải nén vào thư mục, ví dụ: **C:\PHP**

### **Bước 2: Cấu hình IIS Handler Mapping** 

   (Do Web Platform Installer (Web PI) đã dừng hoạt động từ năm 2022 nên ta cần cấu hình IIS Handler Mapping để chạy file **.php** thủ công trên IIS)

- Mở IIS Manager **à** chọn site muốn cấu hình **à** chọn **Handler Mappings**

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.030.png)

- Điền thông tin Mapping:

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.031.png)

### **Bước 3: Cấu hình thêm cho FastCGI**
- Quay lại giao diện chính của site à Mở **FastCGI Settings**

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.032.png)

- Click đúp vào dòng  php-cgi.exe và thêm các biến sau:

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.033.png)

- Sau khi cấu hình xong **Restart IIS** để hoàn tất
### **Bước 4: Tạo thư mục chứa site (file mã nguồn)**
- Tương tự như các phần trước, tạo 1 file **index.php** trong thư mục vừa tạo

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.034.png)

### **Bước 5: Tạo website mới trong IIS**

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.035.png)

### **Bước 6: Cấu hình file hosts**

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.036.png)

- **Truy cập web site è kết quả:**

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.037.png)
