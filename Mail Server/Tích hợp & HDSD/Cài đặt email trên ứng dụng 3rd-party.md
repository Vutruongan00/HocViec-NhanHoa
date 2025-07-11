># Hướng dẫn sử dụng

# 1. Cài đặt email trên ứng dụng 3rd-party

## 1.1 Outlook

### Outlook Desktop

<img width="784" height="477" alt="image" src="https://github.com/user-attachments/assets/07f52e3f-d831-44ad-a13c-28286f932430" />

<img width="960" height="624" alt="image" src="https://github.com/user-attachments/assets/c731de90-f0a7-4a26-92b8-3d61fe2e07ef" />

<img width="490" height="548" alt="image" src="https://github.com/user-attachments/assets/c7534bb3-aa98-4dee-8bd8-17fdc7602b58" />

<img width="494" height="550" alt="image" src="https://github.com/user-attachments/assets/87b30f33-0aa4-49cc-8c2e-b6eeb18273d3" />

<img width="1406" height="743" alt="image" src="https://github.com/user-attachments/assets/f2c505ad-a146-4632-9f37-46e08a15ca4e" />


### Outlook Mobile

<img width="359" height="342" alt="image" src="https://github.com/user-attachments/assets/e2a738df-daba-45b9-b80a-aff21eb53af6" />

<img width="357" height="306" alt="image" src="https://github.com/user-attachments/assets/c853df85-e1e1-493c-9c6f-b215d01e1148" />

<img width="711" height="705" alt="image" src="https://github.com/user-attachments/assets/7518a9e5-3595-4dd1-a0bb-b8484ff674e3" />

<img width="421" height="537" alt="image" src="https://github.com/user-attachments/assets/1815250b-2439-4fe4-ba08-685ea89719f4" />

<img width="472" height="427" alt="image" src="https://github.com/user-attachments/assets/9b6b427c-c4eb-4e20-89d0-f4407992acd3" />

## 1.2 Mozilla Thunderbird 
- **Mở Thunderbird --> Setting --> Account Setting** 

<img width="581" height="341" alt="image" src="https://github.com/user-attachments/assets/a9ab795c-069f-4629-9366-8634f3b7356c" />

- **Nhập thông tin tài khoản --> Continue (Tự động cấu hình IMAP/POP và SMTP)** 

<img width="665" height="516" alt="image" src="https://github.com/user-attachments/assets/ea0c684a-1047-435a-a0b9-36aa85eda5b8" />

- Hoặc chọn **Configure Manually** để cấu hình thủ công: **--> Re-test --> Done**

<img width="946" height="671" alt="image" src="https://github.com/user-attachments/assets/830919aa-3177-4305-b98a-4e14556f8517" />

- Xong rồi! truy cập vào mail thôi

<img width="1045" height="501" alt="image" src="https://github.com/user-attachments/assets/89c466eb-6572-4e6f-8390-c4d17fee43ec" />

## 1.3 Apple Mail
- Truy cập App **Setting (Cài đặt) --> Ứng dụng --> Mail --> Các tài khoản Mail**
  
<img width="391" height="422" alt="image" src="https://github.com/user-attachments/assets/5a244016-a914-4b13-9392-1c5801212def" />

<img width="414" height="448" alt="image" src="https://github.com/user-attachments/assets/c7c8c84d-4b9b-4ae0-9dea-0edeb48f8d91" />

- **--> Thêm tài khoản --> Khác --> Thêm tài khoản Mail** description

<img width="383" height="352" alt="image" src="https://github.com/user-attachments/assets/462a6c8e-25dd-4eaf-b071-fd751163d0c0" />

<img width="390" height="554" alt="image" src="https://github.com/user-attachments/assets/1a32c84e-8d17-416e-a6d9-005fb822246b" />

<img width="387" height="488" alt="image" src="https://github.com/user-attachments/assets/733dbb18-7308-4b5e-9df4-b6e4629b4a72" />

- **Nhập thông tin tài khoản:**

<img width="396" height="362" alt="image" src="https://github.com/user-attachments/assets/d17f0a39-78d2-4bfb-8dd0-2f2568a5d538" />

- **Nhập thông tin cấu hình Máy chủ --> Chờ xác minh**

<img width="396" height="706" alt="image" src="https://github.com/user-attachments/assets/02bcdd20-17a2-4e17-adef-5c5cbc318f4f" />

- Xác minh thành công --> truy cập **Apple Mail** sử dụng thôi.... hẹ hẹ!

# 2. Autodiscover

1. **Tạo Record DNS:**

- Tạo bản ghi **CNAME** hoặc **A record** cho autodiscover.mail.antvpro.io.vn trỏ về IP máy chủ Zimbra
```
autodiscover IN CNAME mail.example.com
```
<img width="750" height="382" alt="image" src="https://github.com/user-attachments/assets/e2e19213-1ea9-49b3-b470-048e5a810a96" />

> #### Lưu ý: 
> subdomain `autodiscover.antvpro.io.vn` **phải có SSL** bên trong bằng cách sử dụng Chứng chỉ Wildcard hoặc SAN để ghi

- Tạo **record SRV** với các giá trị sau: dịch vụ, giao thức, mức độ ưu tiên, trọng số, cổng và tên máy chủ.

```
_autodiscover._tcp IN SRV 0 0 443 mail.example.com
```
<img width="1189" height="503" alt="image" src="https://github.com/user-attachments/assets/ab3417f1-7042-45a6-a1d3-7a6ed0daaf92" />

2. **Kiểm tra Autodiscover bằng DNS**
    - Trên Windows:
    ```bash!
    nslookup -q=srv _autodiscover._tcp.example.com
    ```
   <img width="572" height="221" alt="image" src="https://github.com/user-attachments/assets/f5b40a15-9009-48d5-8b7e-b4dd7ecaf746" />


    - Trên Linux:
    ```
    dig _autodiscover._tcp.example.com SRV
    ```
    
3. **Đăng nhập trên Outlook**
- Giờ thì đăng nhập vào các ứng dụng Mail chỉ cần nhập địa chỉ email và mật khẩu là được

> **Lưu ý:**  Chỉ đăng nhập được trên Outlook Desktop

- Để đăng nhập vào được Mobile App thì cần phải bật chức năng này trong Zimbra:

<img width="756" height="409" alt="image" src="https://github.com/user-attachments/assets/03233e88-de66-4f48-95c1-0654a7fde7f1" />

- Tham khảo cách bật: https://wiki.zimbra.com/wiki/Autodiscover

> Lưu ý: 
> - tính năng này chỉ khả dụng trên **Zimbra 8x**, và **9x, 10x Zimbra Network Edition**
> - **không hỗ trợ** trên các phiên bản **Zimbra 9x, 10x Open Source Edition**
