## **Môi trường và công cụ sử dụng**
- Hệ điều hành: Windows Server 2022
- Dịch vụ web: Internet Information Services (IIS)
- Trình duyệt: Microsoft Edge
## **Cài đặt IIS trên Windows Server**
### **Cách 1**: Dùng Powershell với quyền admin để cài đặt IIS
- Chạy lệnh sau để cài đặt IIS: 

  *Install-WindowsFeature -Name Web-Server -IncludeManagementTools*

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.001.png)

- Kiểm tra việc cài đặt:  *Get-WindowsFeature -Name Web-Server*

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.002.png)
*cài đặt thành công!*

### **Cách 2**: Cài đặt IIS bằng **Server Manager** trên Windows Server 2022
- ` `Chạy Server Manager và nhấp vào Add roles and features.

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.003.png)

- Nhấn Next

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.004.png)

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.005.png)

- Chọn host bạn muốn thêm các service

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.006.png)

- Tích vào hộp Web Server (IIS)

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.007.png)

- Nhấn Next cho đến khi hiện hiện nút Install à click Install để hoàn tất cài đặt

![image](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.008.png)

- Chạy trình duyệt web và truy cập vào localhost, sau đó bạn có thể xác minh IIS có đang chạy bình thường không.

![](image/Aspose.Words.27640d2c-70dd-404f-aa6d-cdf0d15b0720.009.png) 
*Ok!*

