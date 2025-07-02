# 1. Tổng quan SSL/TLS

## 1.1. SSL là gì? TLS là gì? Sự khác nhau giữa SSL và TLS

### SSL (Secure Sockets Layer)
- **Định nghĩa:** Giao thức bảo mật được thiết kế để tạo ra một kênh liên lạc an toàn giữa máy khách và máy chủ qua Internet.
- **Chức năng chính:** 
  - Mã hóa dữ liệu truyền tải.
  - Xác thực danh tính của máy chủ (và tùy chọn là máy khách).
  - Đảm bảo tính toàn vẹn của dữ liệu.

### TLS (Transport Layer Security)
- **Định nghĩa:** Phiên bản kế nhiệm và nâng cấp của SSL (SSL 3.1) nhưng đổi tên để tránh vấn đề pháp lý.
- **Chức năng chính:** Tương tự SSL nhưng với:
  - Thuật toán mạnh hơn.
  - Hiệu suất tốt hơn.
  - Bảo vệ trước nhiều lỗ hổng hơn.

### So sánh SSL và TLS:
| Tiêu chí       | SSL               | TLS                          |
|----------------|-------------------|-------------------------------|
| Kế thừa        | Giao thức gốc     | Kế nhiệm SSL, phiên bản mới   |
| Thuật toán     | Yếu hơn           | Mạnh và hiện đại hơn          |
| Bảo mật        | Ít an toàn hơn    | An toàn hơn (loại bỏ lỗ hổng) |
| Tên gọi        | "SSL" phổ biến    | Thực tế hiện nay dùng TLS     |



## 1.2. Lịch sử và các phiên bản SSL/TLS

### SSL 2.0 (1995)
- Phiên bản đầu tiên từ Netscape.
- Lỗ hổng nghiêm trọng.
- Bị khai tử, không còn được dùng.

### SSL 3.0 (1996)
- Cải thiện từ SSL 2.0.
- Bị lỗi POODLE (2014) → Không còn an toàn.

### TLS 1.0 (1999)
- Tiếp nối SSL 3.0 → SSL 3.1.
- Có điểm yếu, đã bị ngừng hỗ trợ.

### TLS 1.1 (2006)
- Bảo vệ khỏi tấn công CBC.
- Đã ngừng hỗ trợ.

### TLS 1.2 (2008)
- Hỗ trợ AES-GCM, SHA-256.
- Phiên bản phổ biến nhất hiện nay.

### TLS 1.3 (2018)
- Mới nhất, an toàn nhất.
- Ưu điểm:
  - Giảm RTT (1-RTT hoặc 0-RTT).
  - Mã hóa phần lớn handshake.
  - Loại bỏ thuật toán cũ, renegotiation.


## 1.3. Vai trò của SSL/TLS trong bảo mật mạng

#### 1. HTTPS (HTTP Secure)
- Bảo vệ dữ liệu giữa trình duyệt và máy chủ.
- Mã hóa thông tin nhạy cảm: đăng nhập, thẻ tín dụng.
- Xác thực website.
- Ngăn phishing, MITM (Man-In-The-Middle).

#### 2. Email (SMTPS, IMAPS, POP3S)
- Mã hóa nội dung, xác thực khi gửi/nhận email.
- Bảo vệ thông tin người dùng.

#### 3. VPN (Virtual Private Network)
- Dùng TLS (OpenVPN, AnyConnect) để tạo đường hầm mã hóa.
- Cho phép truy cập mạng an toàn từ xa.

#### 4. Truyền file (FTPS, SFTP)
- **FTPS** dùng TLS để mã hóa FTP.
- **SFTP** dùng SSH, không dùng TLS nhưng có chức năng tương tự.

#### 5. API và ứng dụng đám mây
- Giao tiếp an toàn giữa hệ thống và dịch vụ bên ngoài.

#### 6. IoT (Internet of Things)
- Mã hóa kết nối thiết bị ↔ nền tảng điều khiển (cloud).


## 1.4. Lợi ích của SSL/TLS

-  **Bảo mật dữ liệu**: Mã hóa thông tin nhạy cảm.
-  **Toàn vẹn dữ liệu**: Dữ liệu không bị thay đổi khi truyền.
-  **Xác thực danh tính**: Thông qua chứng chỉ số.
-  **Tăng độ tin cậy**: Tạo niềm tin cho người dùng.
-  **Cải thiện SEO**: HTTPS là yếu tố xếp hạng trên Google.
-  **Tuân thủ quy định**: GDPR, HIPAA, PCI-DSS yêu cầu mã hóa.
-  **Chống tấn công**: Chống nghe lén, MITM, spoofing.
-  **Cải thiện hiệu suất (TLS 1.3)**: RTT thấp, kết nối nhanh hơn.

--- 


# 2. Cách thức hoạt động của SSL/TLS

## 2.1. Quy trình mã hóa và giải mã dữ liệu

SSL/TLS sử dụng kết hợp hai loại mã hóa chính: mã hóa bất đối xứng (asymmetric encryption) và mã hóa đối xứng (symmetric encryption).

### Mã hóa bất đối xứng (Asymmetric Cryptography)
- Sử dụng một cặp khóa: khóa công khai (public key) và khóa bí mật (private key).
- Dữ liệu được mã hóa bằng khóa công khai chỉ có thể được giải mã bằng khóa bí mật tương ứng (và ngược lại).
- **Ứng dụng trong SSL/TLS:** Chủ yếu dùng trong giai đoạn bắt tay (handshake) để trao đổi khóa phiên (session key) và xác thực danh tính.

![image](https://github.com/user-attachments/assets/d5fb5707-bf81-4ef2-8ada-9a72836ff00a)

### Mã hóa đối xứng (Symmetric-key cryptography)
- Sử dụng một khóa duy nhất (khóa phiên - session key) cho cả mã hóa và giải mã.
- Nhanh và hiệu quả hơn so với mã hóa bất đối xứng.
- **Ứng dụng trong SSL/TLS:** Dùng để mã hóa toàn bộ dữ liệu sau khi handshake hoàn tất.

![image](https://github.com/user-attachments/assets/2d12cdc1-9eec-4fc9-9702-4a37fdb757c0)

### Quy trình tổng thể
1. **Trao đổi khóa phiên:**  Sử dụng mã hóa bất đối xứng để thiết lập một khóa phiên đối xứng duy nhất giữa máy khách và máy chủ một cách an toàn (trong giai đoạn handshake).
2. **Mã hóa dữ liệu:** Dùng khóa phiên để mã hóa dữ liệu gửi đi.
3. **Giải mã dữ liệu:** Bên nhận dùng khóa phiên để giải mã.
4. **Đảm bảo toàn vẹn:** Dùng hash và MAC để kiểm tra dữ liệu không bị thay đổi.

## 2.2. Handshake SSL/TLS: Cách thiết lập kết nối an toàn

![image](https://github.com/user-attachments/assets/43e679df-7086-4db7-9588-4ba7c33a7fe3)

Dưới đây là quy trình TLS 1.2 handshake (TLS 1.3 có khác biệt nhưng tương tự về nguyên tắc):

### 1. ClientHello message
- Máy khách (trình duyệt) gửi thông điệp ClientHello tới máy chủ.
- Thông điệp này chứa:
    - Các phiên bản TLS mà máy khách hỗ trợ (ví dụ: TLS 1.0, 1.1, 1.2, 1.3).
    - Các bộ mã hóa (cipher suites) mà máy khách có thể sử dụng (ví dụ: AES-256-GCM, RSA, ECDSA).
    - Một chuỗi byte ngẫu nhiên (ClientRandom) dùng để tạo khóa phiên.
    - Các phần mở rộng (extensions) khác (ví dụ: Server Name Indication - SNI).

### 2. ServerHello message
- Máy chủ nhận ClientHello và chọn phiên bản TLS cao nhất mà cả hai bên đều hỗ trợ và là phiên bản an toàn.
- Chọn một bộ mã hóa từ danh sách của máy khách.
- Tạo một chuỗi byte ngẫu nhiên khác (ServerRandom).
- **Gửi thông điệp ServerHello** chứa phiên bản TLS đã chọn, bộ mã hóa đã chọn, và ServerRandom tới máy khách.

### 3. Client xác minh chứng chỉ (Authentication)
- Máy khách xác minh chứng chỉ SSL của máy chủ từ CA
- Chứng chỉ này chứa:
    - Khóa công khai của máy chủ.
    - Thông tin về chủ sở hữu chứng chỉ (tên miền, tổ chức).
    - Thông tin về Tổ chức cấp chứng chỉ (Certificate Authority - CA).
    - Chữ ký số của CA để xác minh tính hợp lệ của chứng chỉ.
    - Thời gian hiệu lực của chứng chỉ.
- Nếu xác thực không thành công, thì máy khách từ chối kết nối SSL và ném một ngoại lệ. Nếu xác thực thành công, chuyển sang bước **4**.

### 4. Client tạo session key
- Máy khách tạo một session key, mã hóa nó bằng khóa công khai của máy chủ và gửi đến máy chủ. Nếu máy chủ đã yêu cầu xác thực máy khách (chủ yếu là trong giao tiếp máy chủ với máy chủ), thì máy khách sẽ gửi chứng chỉ của chính mình đến máy chủ.

### 5. ServerHelloDone
- Máy chủ giải mã khóa phiên bằng khóa riêng của nó và gửi xác nhận đến máy khách được mã hóa bằng khóa phiên.



## 2.3. Vai trò của khóa công khai (public key) và khóa bí mật (private key)
Khóa công khai và khóa bí mật là nền tảng của mật mã học bất đối xứng và đóng vai trò trung tâm trong quá trình hoạt động của SSL/TLS:

### Khóa công khai (Public Key)
- **Tính chất:** Được công khai. Bất kỳ ai cũng có thể có được khóa công khai của một thực thể.

- **Vai trò:**
  - **Mã hóa khóa phiên**: Máy khách sử dụng khóa công khai của máy chủ (lấy từ chứng chỉ SSL/TLS) để mã hóa khóa phiên tiền mã hóa (pre-master secret) trước khi gửi cho máy chủ.
  - X**ác minh chữ ký số của CA**: Khóa công khai của Tổ chức cấp chứng chỉ (CA) được sử dụng để xác minh chữ ký số trên chứng chỉ của máy chủ, đảm bảo chứng chỉ đó là hợp lệ và được cấp bởi một CA đáng tin cậy.

  - **Xác thực danh tính của server:** Nếu máy khách có thể mã hóa dữ liệu bằng khóa công khai của máy chủ và máy chủ có thể giải mã thành công bằng khóa bí mật tương ứng, điều đó chứng minh rằng máy chủ thực sự là chủ sở hữu của cặp khóa đó, và do đó, danh tính của nó được xác thực.

### Khóa bí mật (Private Key)
- **Tính chất:** Chỉ chủ sở hữu nắm giữ.
- **Vai trò:**
  - **Giải mã khóa phiên**: Máy chủ sử dụng khóa bí mật của mình để giải mã khóa phiên tiền mã hóa (pre-master secret) được máy khách gửi đến. Đây là bước then chốt để cả hai bên có thể tính toán ra cùng một khóa phiên đối xứng.
  - **Tạo chữ ký số (đối với CA):** Tổ chức cấp chứng chỉ (CA) sử dụng khóa bí mật của mình để ký số lên chứng chỉ của máy chủ.
  - **Chứng minh quyền sở hữu tên miền hoặc máy chủ:** Việc chỉ máy chủ sở hữu khóa bí mật tương ứng với khóa công khai trên chứng chỉ là bằng chứng duy nhất cho thấy máy chủ đó là hợp pháp và có quyền sử dụng tên miền đó. Nếu khóa bí mật bị lộ, kẻ tấn công có thể giả mạo máy chủ.

### Mối quan hệ giữa khóa công khai và khóa bí mật:
- Chúng là một cặp duy nhất.
- Public key mã hóa → Private key giải mã.
- Private key tạo chữ ký → Public key xác minh.

---


# 3. Chứng chỉ SSL (SSL/TLS Certificate)

Chứng chỉ SSL/TLS là xương sống của bảo mật web, đóng vai trò quan trọng trong việc xác thực danh tính và thiết lập kênh truyền thông an toàn.

## 3.1. Khái niệm và mục đích

- **Khái niệm:** Chứng chỉ SSL/TLS (thường được gọi tắt là chứng chỉ SSL) là một tệp dữ liệu kỹ thuật số được phát hành bởi một bên thứ ba đáng tin cậy gọi là Tổ chức cấp chứng chỉ (Certificate Authority - CA). Chứng chỉ này liên kết một khóa công khai với danh tính của một thực thể (thường là một trang web hoặc máy chủ).
- **Mục đích chính:**
  - **Xác thực danh tính:** Chứng minh quá trình đang giao tiếp với máy chủ là chính xác  chứ không phải là kẻ mạo danh. Đây là mục đích quan trọng nhất.
  - **Cung cấp khóa công khai:** Chứa khóa công khai của máy chủ, cho phép máy khách mã hóa dữ liệu (như khóa phiên) để gửi an toàn đến máy chủ.
  - **Thiết lập kết nối an toàn:** Là thành phần không thể thiếu trong quá trình SSL/TLS handshake, giúp thiết lập kênh truyền thông được mã hóa và toàn vẹn.
  - **Tăng cường sự tin cậy:** Khi người dùng thấy biểu tượng khóa và "HTTPS" trên trình duyệt, họ biết rằng kết nối của họ an toàn và thông tin của họ được bảo vệ.

## 3.2. Các loại chứng chỉ SSL theo mức độ xác minh

Các loại chứng chỉ SSL/TLS được phân loại dựa trên mức độ xác minh danh tính của chủ thể chứng chỉ mà CA thực hiện. Mức độ xác minh càng cao thì độ tin cậy càng lớn.

- **DV (Domain Validation - Xác thực tên miền):**
  - **Mức độ xác minh:** Thấp nhất.
  - **Quy trình xác minh:** CA chỉ xác minh quyền sở hữu tên miền của người đăng ký. Điều này thường được thực hiện thông qua email xác nhận gửi đến địa chỉ email quản trị của tên miền, bằng cách tạo một bản ghi DNS cụ thể, hoặc đặt một tệp tin trên máy chủ web.
  - **Thời gian cấp phát:** Nhanh chóng, thường chỉ vài phút đến vài giờ.
  - **Độ tin cậy:** Phù hợp cho các blog cá nhân, trang web nhỏ, hoặc các trang web không xử lý thông tin nhạy cảm. Người dùng chỉ thấy "HTTPS" và biểu tượng khóa.
  
- **OV (Organization Validation - Xác thực tổ chức):**
  - **Mức độ xác minh:** Trung bình.
  - **Quy trình xác minh:** Ngoài việc xác minh quyền sở hữu tên miền, CA còn xác minh sự tồn tại hợp pháp của tổ chức/doanh nghiệp đăng ký chứng chỉ. Quá trình này bao gồm kiểm tra thông tin đăng ký kinh doanh, số điện thoại, địa chỉ vật lý, và các nguồn dữ liệu đáng tin cậy khác.
  - **Thời gian cấp phát:** Vài ngày làm việc, do có bước kiểm tra thủ công.
  - **Độ tin cậy:** Cao hơn DV, phù hợp cho các trang web thương mại điện tử, doanh nghiệp vừa và nhỏ, nơi cần thể hiện mức độ tin cậy nhất định.
  
- **EV (Extended Validation - Xác thực mở rộng):**
  - **Mức độ xác minh:** Cao nhất.
  - **Quy trình xác minh:** CA tiến hành một quy trình xác minh cực kỳ nghiêm ngặt và toàn diện, bao gồm xác minh vật lý, pháp lý và hoạt động của tổ chức. Quy trình này tuân thủ các nguyên tắc do Diễn đàn CA/Browser Forum đặt ra.
  - **Thời gian cấp phát:** Vài ngày đến vài tuần, do quy trình xác minh chặt chẽ.
  - **Độ tin cậy:** Cung cấp mức độ tin cậy cao nhất, phù hợp cho các ngân hàng, tổ chức tài chính, trang web thương mại điện tử lớn và các doanh nghiệp xử lý dữ liệu nhạy cảm.
  

## 3.3. Wildcard SSL và SAN (Subject Alternative Name) SSL

Đây là các loại chứng chỉ được phân loại dựa trên số lượng và cách quản lý tên miền/tên máy chủ mà chúng bảo vệ.

- **Wildcard SSL Certificate:**
  - **Mục đích:** Bảo vệ một tên miền chính và tất cả các tên miền phụ (subdomains) của nó ở một cấp độ.
  - **Cấu trúc:** Được phát hành cho `*.yourdomain.com`.
  - **Ví dụ:** Một chứng chỉ Wildcard cho `*.example.com` sẽ bảo mật `www.example.com`, `blog.example.com`, `mail.example.com`, v.v. Nó **không** bảo mật `sub.sub.example.com` (cấp độ sâu hơn) hoặc `example.com` (tên miền gốc, thường cần thêm trong SAN).
  - **Lợi ích:** Tiết kiệm chi phí và đơn giản hóa việc quản lý chứng chỉ khi có nhiều tên miền phụ.
- **SAN (Subject Alternative Name) SSL Certificate / Multi-Domain SSL Certificate:**
  - **Mục đích:** Bảo vệ nhiều tên miền khác nhau hoặc nhiều tên miền phụ ở các cấp độ khác nhau, bao gồm cả tên miền gốc.
  - **Cấu trúc:** Một chứng chỉ chứa một trường SAN cho phép liệt kê nhiều tên miền khác nhau.
  - **Ví dụ:** Một chứng chỉ SAN có thể bảo mật `yourdomain.com`, `www.yourdomain.com`, `blog.yourdomain.net`, `mail.yourdomain.org`, và thậm chí cả địa chỉ IP công cộng.
  - **Lợi ích:** Linh hoạt cao, lý tưởng cho các môi trường có nhiều trang web độc lập hoặc các dịch vụ cần bảo mật nhiều tên miền khác nhau trên cùng một máy chủ hoặc trên nhiều máy chủ. Cũng có thể được sử dụng để bảo mật tên miền chính và các tên miền phụ cụ thể.

## 3.4. Self-signed vs. CA-signed

- **CA-signed Certificate (Chứng chỉ được cấp bởi CA):**
  - **Khái niệm:** Là chứng chỉ được ký bởi một Tổ chức cấp chứng chỉ (CA) đáng tin cậy. Các CA này (ví dụ: DigiCert, Sectigo, Let's Encrypt) được các hệ điều hành và trình duyệt tin cậy một cách mặc định.
  - **Ưu điểm:**
    - **Được tin cậy phổ biến:** Trình duyệt và hệ điều hành sẽ tự động tin cậy chứng chỉ này, hiển thị "HTTPS" và biểu tượng khóa xanh mà không có cảnh báo.
    - **Xác thực danh tính:** Cung cấp xác thực danh tính đáng tin cậy cho người dùng.
  - **Nhược điểm:** Mất phí (trừ Let's Encrypt) và cần quá trình xác minh.
  - **Ứng dụng:** Mọi trang web/ứng dụng sản xuất (production) công khai.
- **Self-signed Certificate (Chứng chỉ tự ký):**
  - **Khái niệm:** Là chứng chỉ được tạo và ký bởi chính máy chủ hoặc ứng dụng sử dụng nó, chứ không phải bởi một CA độc lập.
  - **Ưu điểm:**
    - **Miễn phí và dễ tạo:** Không mất chi phí, có thể tạo ra ngay lập tức.
    - **Mã hóa dữ liệu:** Vẫn cung cấp khả năng mã hóa dữ liệu giữa máy khách và máy chủ.
  - **Nhược điểm:**
    - **Không được tin cậy:** Trình duyệt và hệ điều hành sẽ hiển thị cảnh báo bảo mật nghiêm trọng cho người dùng (ví dụ: "Kết nối  không an toàn" hoặc "Có thể là kẻ tấn công"), vì không có bên thứ ba đáng tin cậy nào xác minh danh tính.
    - **Không xác thực danh tính:** Không có giá trị trong việc xác thực danh tính của máy chủ cho người dùng cuối.
  - **Ứng dụng:** Chủ yếu được sử dụng cho môi trường phát triển, thử nghiệm, mạng nội bộ (intranet) nơi người dùng được hướng dẫn cách bỏ qua cảnh báo, hoặc trong các hệ thống nhúng/IoT nơi chỉ cần mã hóa nội bộ. **Tuyệt đối không nên sử dụng cho các trang web công khai.**

## 3.5. Mã hóa trong SSL/TLS

SSL/TLS không phải là một thuật toán mã hóa duy nhất mà là một bộ giao thức sử dụng nhiều thuật toán khác nhau cho các mục đích cụ thể.

- **Mã hóa đối xứng (Symmetric Encryption):**
  - **Định nghĩa:** Sử dụng cùng một khóa (khóa phiên) để mã hóa và giải mã dữ liệu.
  - **Ưu điểm:** Rất nhanh và hiệu quả cho việc mã hóa lượng lớn dữ liệu.
  - **Vai trò trong SSL/TLS:** Dùng để mã hóa dữ liệu thực tế được truyền tải sau khi quá trình handshake hoàn tất (ví dụ: nội dung trang web, thông tin đăng nhập).
  - **Các thuật toán phổ biến:**
    - **AES (Advanced Encryption Standard):** Tiêu chuẩn mã hóa mạnh nhất hiện nay, có các độ dài khóa 128, 192, 256 bit. Thường dùng với các chế độ hoạt động như AES-GCM (Galois/Counter Mode) hoặc AES-CBC (Cipher Block Chaining). AES-GCM được ưa chuộng hơn vì cung cấp cả tính bảo mật và tính toàn vẹn dữ liệu.
    - **3DES (Triple DES):** Phiên bản cũ hơn của DES, sử dụng ba lần DES. Mặc dù vẫn an toàn, nhưng chậm hơn AES và dần bị loại bỏ trong các phiên bản TLS mới hơn (đặc biệt là TLS 1.3).
    - **ChaCha20:** Một thuật toán mã hóa dòng hiện đại, thường được ghép nối với Poly1305 (để xác thực) tạo thành ChaCha20-Poly1305, được sử dụng trong TLS 1.3, đặc biệt hữu ích trên các thiết bị di động và phần cứng không có hỗ trợ tăng tốc AES.
- **Mã hóa bất đối xứng (Asymmetric Encryption - Public-key Encryption):**
  - **Định nghĩa:** Sử dụng một cặp khóa (khóa công khai và khóa bí mật) cho quá trình mã hóa và giải mã.
  - **Ưu điểm:** Cho phép trao đổi khóa an toàn và xác thực danh tính mà không cần trao đổi khóa bí mật.
  - **Vai trò trong SSL/TLS:** Chủ yếu dùng trong giai đoạn handshake để trao đổi khóa phiên đối xứng và xác thực danh tính.
  - **Các thuật toán phổ biến:**
    - **RSA (Rivest-Shamir-Adleman):** Thuật toán bất đối xứng phổ biến nhất. Được sử dụng để mã hóa khóa phiên tiền mã hóa (`pre-master secret`) và để ký số trên chứng chỉ.
    - **ECC (Elliptic Curve Cryptography):** Một phương pháp mật mã khóa công khai dựa trên lý thuyết đường cong Elliptic.
      - **Ưu điểm:** Cung cấp mức độ bảo mật tương đương với RSA nhưng với kích thước khóa nhỏ hơn đáng kể, dẫn đến hiệu suất cao hơn và tiêu thụ ít tài nguyên hơn.
      - **Ứng dụng:** Phổ biến trong TLS hiện đại, đặc biệt với các thiết bị di động và IoT.
- **Thuật toán trao đổi khóa (Key Exchange Algorithms):**
  - **Mục đích:** Là cách thức máy khách và máy chủ thống nhất một khóa phiên đối xứng một cách an toàn mà không bị bên thứ ba nghe lén.
  - **Các thuật toán phổ biến:**
    - **RSA (để trao đổi khóa):** Trong các phiên bản TLS cũ hơn (TLS 1.0, 1.1, 1.2), RSA có thể được sử dụng trực tiếp để mã hóa `pre-master secret`. Tuy nhiên, nhược điểm là nó **không cung cấp Perfect Forward Secrecy (PFS)**. Nếu khóa bí mật của máy chủ bị lộ sau này, tất cả các phiên đã ghi lại (nếu có) sử dụng RSA để trao đổi khóa đều có thể bị giải mã.
    - **Diffie-Hellman (DH) / Ephemeral Diffie-Hellman (DHE):**
      - Cho phép hai bên thống nhất một khóa bí mật chung qua một kênh công khai, ngay cả khi chưa từng gặp nhau trước đó.
      - **DHE (Ephemeral DH):** Tạo ra các khóa phiên DH mới cho mỗi phiên kết nối. Điều này đảm bảo **Perfect Forward Secrecy (PFS)**. Nếu khóa bí mật của máy chủ bị lộ, các phiên trước đó vẫn an toàn vì khóa phiên được tạo ra độc lập cho mỗi lần giao tiếp.
    - **Elliptic Curve Diffie-Hellman (ECDH) / Ephemeral Elliptic Curve Diffie-Hellman (ECDHE):**
      - Tương tự như DH/DHE nhưng sử dụng mật mã đường cong Elliptic.
      - **Ưu điểm:** Cung cấp cùng mức độ bảo mật với khóa nhỏ hơn nhiều, dẫn đến hiệu suất tốt hơn.
      - **Ứng dụng:** ECDHE là thuật toán trao đổi khóa được khuyến nghị và phổ biến nhất trong TLS hiện đại (đặc biệt là TLS 1.3), vì nó cung cấp PFS và hiệu suất cao.

## 3.6. Cấu trúc của chứng chỉ SSL/TLS

Một chứng chỉ SSL/TLS là một tệp nhị phân tuân thủ tiêu chuẩn X.509, chứa một số trường thông tin quan trọng. Các trường chính bao gồm:

- **Subject (Chủ thể):** Thông tin về thực thể mà chứng chỉ được cấp cho.
  - **Common Name (CN):** Tên miền chính mà chứng chỉ bảo vệ (ví dụ: `www.example.com`). Đây là trường truyền thống và vẫn còn phổ biến.
  - **Organizational Unit (OU):** Tên bộ phận hoặc đơn vị trong tổ chức (ví dụ: "IT Department").
  - **Organization (O):** Tên đầy đủ của tổ chức/doanh nghiệp (ví dụ: "Example Inc.").
  - **Locality (L):** Thành phố hoặc địa phương của tổ chức.
  - **State/Province (ST):** Bang hoặc tỉnh của tổ chức.
  - **Country (C):** Mã quốc gia hai chữ cái (ví dụ: "US", "VN").
- **Subject Alternative Name (SAN):**
  - Một trường mở rộng quan trọng cho phép chứng chỉ bảo vệ nhiều tên miền hoặc địa chỉ IP khác nhau.
  - Đây là cách hiện đại và được khuyến nghị để liệt kê các tên miền mà chứng chỉ bảo vệ (bao gồm cả tên miền chính, các tên miền phụ và các tên miền khác).
  - Ví dụ: một chứng chỉ cho `example.com` có thể có SAN cho `www.example.com`, `blog.example.com`, và `mail.example.com`.
- **Issuer (Người cấp phát):** Thông tin về Tổ chức cấp chứng chỉ (CA) đã ký và phát hành chứng chỉ này.
  - Bao gồm các trường tương tự như Subject (Common Name, Organization, Country, v.v.) của CA.
  - Quan trọng để trình duyệt xác minh chuỗi tin cậy (Certificate Chain) lên đến CA gốc.
- **Validity Period (Thời gian hiệu lực):**
  - **Not Before:** Ngày và giờ chứng chỉ bắt đầu có hiệu lực.
  - **Not After:** Ngày và giờ chứng chỉ hết hạn.
  - Chứng chỉ hết hạn sẽ không còn được trình duyệt tin cậy.
- **Public Key (Khóa công khai):**
  - Khóa công khai của chủ thể chứng chỉ. Đây là thành phần quan trọng nhất để thiết lập mã hóa bất đối xứng.
  - Bao gồm loại thuật toán khóa (ví dụ: RSA, ECC) và giá trị của khóa.
- **Signature Algorithm (Thuật toán chữ ký):** Thuật toán được CA sử dụng để ký số lên chứng chỉ (ví dụ: SHA256withRSA).
- **Signature (Chữ ký):** Chữ ký số của CA trên toàn bộ chứng chỉ. Trình duyệt sử dụng khóa công khai của CA để xác minh chữ ký này, đảm bảo tính toàn vẹn và không bị giả mạo của chứng chỉ.
- **Serial Number (Số seri):** Một số định danh duy nhất cho chứng chỉ.

## 3.7. Các phương pháp xác minh chứng chỉ SSL

Trình duyệt của người dùng (hoặc hệ điều hành) thực hiện một loạt các kiểm tra để xác minh tính hợp lệ và độ tin cậy của một chứng chỉ SSL/TLS khi thiết lập kết nối:

#### 1. **Xác minh chữ ký số của CA (Certificate Chain Validation):**
   - Trình duyệt kiểm tra chữ ký số của CA trên chứng chỉ máy chủ.
   - Nó sẽ tìm kiếm chứng chỉ của CA đó trong kho lưu trữ chứng chỉ tin cậy của hệ điều hành hoặc trình duyệt (Certificate Store/Trust Store).
   - Nếu chứng chỉ của CA không có trong kho lưu trữ, trình duyệt sẽ kiểm tra xem CA đó có được ký bởi một CA khác đáng tin cậy hơn hay không, tạo thành một "chuỗi tin cậy" (Certificate Chain) cho đến khi đạt được một "CA gốc" (Root CA) đã được tin cậy.
   - Nếu bất kỳ mắt xích nào trong chuỗi này bị lỗi hoặc không được tin cậy, chứng chỉ sẽ bị coi là không hợp lệ.
#### 2. **Kiểm tra ngày hết hạn (Validity Period Check):**
   - Trình duyệt kiểm tra xem ngày hiện tại có nằm trong khoảng "Not Before" và "Not After" của chứng chỉ hay không.
   - Nếu chứng chỉ đã hết hạn hoặc chưa có hiệu lực, nó sẽ bị từ chối.
#### 3. **So sánh tên miền (Domain Match):**
   - Trình duyệt so sánh tên miền mà người dùng đang truy cập với tên miền được ghi trong trường **Common Name (CN)** hoặc **Subject Alternative Name (SAN)** của chứng chỉ.
   - Nếu tên miền không khớp, trình duyệt sẽ cảnh báo rằng chứng chỉ không hợp lệ cho trang web này. (Ví dụ: truy cập `example.com` nhưng chứng chỉ lại cấp cho `anothersite.com`).
#### 4. **Kiểm tra thu hồi chứng chỉ (Revocation Check - CRL/OCSP):**
   - Trình duyệt kiểm tra xem chứng chỉ có bị CA thu hồi trước thời hạn hay không. Điều này có thể xảy ra nếu khóa bí mật bị lộ, hoặc thông tin trên chứng chỉ không còn chính xác.
   - Có hai phương pháp chính:
     - **CRL (Certificate Revocation List):** Trình duyệt tải xuống một danh sách các chứng chỉ đã bị thu hồi từ CA và kiểm tra xem chứng chỉ hiện tại có trong danh sách đó không.
     - **OCSP (Online Certificate Status Protocol):** Trình duyệt gửi yêu cầu trực tuyến đến CA (hoặc một máy chủ OCSP Responder) để hỏi trạng thái của một chứng chỉ cụ thể. OCSP nhanh hơn và hiệu quả hơn CRL.
   - OCSP Stapling là một cải tiến giúp máy chủ tự cung cấp phản hồi OCSP cho trình duyệt, giảm gánh nặng cho CA và tăng tốc độ xác minh.
#### 5. **Kiểm tra Trust Policy của CA:**
   - Trình duyệt cũng kiểm tra các chính sách và hạn chế được ghi trong chứng chỉ của CA, ví dụ như CA này chỉ được phép cấp chứng chỉ cho loại hình tổ chức nào, hoặc chỉ được cấp cho những mục đích sử dụng nhất định.


--- 
# 4. Nhà cung cấp chứng chỉ (CA – Certificate Authority)

Nhà cung cấp chứng chỉ (Certificate Authority – CA) là những tổ chức đáng tin cậy, đóng vai trò then chốt trong hệ thống bảo mật SSL/TLS. Họ là những "bên thứ ba" được tin cậy để phát hành và quản lý các chứng chỉ kỹ thuật số, đảm bảo rằng danh tính của các trang web và dịch vụ là chính xác.

## 4.1. Các CA uy tín

Trên thế giới có nhiều CA, nhưng một số cái tên nổi bật và được tin cậy rộng rãi bao gồm:

- **Sectigo (trước đây là Comodo CA):** Là một trong những nhà cung cấp chứng chỉ lớn nhất thế giới, cung cấp đa dạng các loại chứng chỉ từ DV, OV đến EV, Wildcard và SAN. Sectigo nổi tiếng với các giải pháp bảo mật toàn diện cho doanh nghiệp.
- **DigiCert:** Một CA hàng đầu khác, chuyên cung cấp các giải pháp chứng chỉ cao cấp cho các doanh nghiệp lớn và tổ chức chính phủ. DigiCert được biết đến với các chứng chỉ EV và các dịch vụ quản lý chứng chỉ tiên tiến. Họ cũng sở hữu Symantec Website Security (bao gồm các thương hiệu GeoTrust, Thawte, và RapidSSL).
- **GlobalSign:** Là một CA lâu đời và có uy tín, cung cấp một danh mục sản phẩm rộng lớn, từ chứng chỉ SSL/TLS cho đến các giải pháp nhận dạng kỹ thuật số và bảo mật IoT.
- **Let's Encrypt:** Một CA phi lợi nhuận, cung cấp chứng chỉ SSL/TLS miễn phí, tự động và mở. Đây là một sáng kiến đột phá nhằm phổ cập HTTPS.

## 4.2. Let's Encrypt – SSL miễn phí

Let's Encrypt đã thay đổi cuộc chơi trong thế giới SSL/TLS bằng cách cung cấp chứng chỉ hoàn toàn miễn phí.

- **Mục tiêu:** Phổ cập mã hóa trên toàn bộ Internet bằng cách loại bỏ rào cản về chi phí và sự phức tạp khi cài đặt chứng chỉ.
- **Cách thức hoạt động:** Let's Encrypt sử dụng giao thức **ACME (Automated Certificate Management Environment)** để tự động hóa hoàn toàn quá trình cấp và gia hạn chứng chỉ.
- **Loại chứng chỉ:** Chỉ cung cấp chứng chỉ **Domain Validation (DV)**. Điều này có nghĩa là họ chỉ xác minh quyền sở hữu tên miền, chứ không xác minh danh tính tổ chức.
- **Thời hạn:** Chứng chỉ Let's Encrypt có thời hạn **90 ngày**. Việc gia hạn cũng được tự động hóa qua ACME, khuyến khích các nhà quản trị hệ thống thiết lập tự động gia hạn.
- **Lợi ích:**
  - **Miễn phí:** Loại bỏ chi phí mua chứng chỉ.
  - **Tự động hóa:** Quá trình cấp và gia hạn diễn ra tự động, giảm thiểu công sức quản lý.
  - **Dễ sử dụng:** Các công cụ như Certbot giúp đơn giản hóa việc cài đặt.
  - **Thúc đẩy HTTPS:** Đã góp phần rất lớn vào việc tăng tỷ lệ sử dụng HTTPS trên Internet.
- **Hạn chế:**
  - **Chỉ DV:** Không cung cấp các chứng chỉ OV hay EV yêu cầu xác minh danh tính tổ chức.
  - **Thời hạn ngắn:** Yêu cầu gia hạn thường xuyên (mặc dù có thể tự động).
  - **Không có hỗ trợ kỹ thuật trực tiếp:** Hỗ trợ chủ yếu thông qua diễn đàn cộng đồng.

## 4.3. Cơ chế xác minh chứng chỉ (Domain Validation Methods)

Để cấp chứng chỉ DV, CA cần xác minh rằng người yêu cầu thực sự sở hữu hoặc có quyền kiểm soát tên miền mà họ đang yêu cầu chứng chỉ. Dưới đây là các phương pháp xác minh phổ biến:

### Xác minh DNS (DNS-01 Challenge):
  - **Cách thức:** CA sẽ yêu cầu tạo một bản ghi **TXT record** đặc biệt trong cấu hình DNS của tên miền của. Bản ghi này thường chứa một chuỗi mã hóa duy nhất do CA cung cấp.
  - **Ưu điểm:** Có thể tự động hóa hoàn toàn, không yêu cầu truy cập máy chủ web. Phù hợp cho các trang web đang chạy trên một máy chủ không thể truy cập HTTP hoặc khi muốn cấp chứng chỉ Wildcard.
  - **Nhược điểm:** Yêu cầu quyền truy cập vào cài đặt DNS của tên miền. Thời gian cập nhật bản ghi DNS có thể mất một chút thời gian.
### Xác minh Email (Email Validation):
  - **Cách thức:** CA gửi một email xác nhận đến một địa chỉ email quản trị cụ thể (ví dụ: `admin@yourdomain.com`, `hostmaster@yourdomain.com`, `webmaster@yourdomain.com`, hoặc địa chỉ email trong bản ghi WHOIS của tên miền).
  - **Ưu điểm:** Đơn giản và dễ thực hiện cho người dùng không am hiểu kỹ thuật.
  - **Nhược điểm:** Yêu cầu có quyền truy cập vào các tài khoản email quản trị đó. Có thể không tự động hóa được hoàn toàn cho tất cả các CA.
### Xác minh HTTP (HTTP-01 Challenge):
  - **Cách thức:** CA yêu cầu tạo một tệp tin đặc biệt với nội dung cụ thể và đặt nó vào một thư mục cụ thể (thường là `.well-known/acme-challenge/`) trên máy chủ web. CA sau đó sẽ truy cập URL đó để xác minh.
  - **Ưu điểm:** Dễ dàng tự động hóa với các công cụ như Certbot, không yêu cầu thay đổi DNS.
  - **Nhược điểm:** Yêu cầu máy chủ web phải có thể truy cập công khai qua HTTP. Không hoạt động được cho chứng chỉ Wildcard (vì Wildcard không gắn với một host cụ thể).
### CAA Record (Certificate Authority Authorization Record):
  - **Khái niệm:** Một loại bản ghi DNS cho phép chủ sở hữu tên miền chỉ định CA nào được phép cấp chứng chỉ cho tên miền của họ.
  - **Mục đích:** Tăng cường bảo mật bằng cách ngăn chặn các CA không được phép cấp chứng chỉ sai cho tên miền. Nếu một CA nhận được yêu cầu cấp chứng chỉ cho tên miền có bản ghi CAA không cho phép họ, họ phải từ chối yêu cầu đó.
  - **Vai trò trong xác minh:** Mặc dù CAA không phải là một phương pháp xác minh trực tiếp quyền sở hữu tên miền, nó là một kiểm tra bổ sung mà CA phải thực hiện trước khi cấp chứng chỉ. Nó giúp CA biết liệu họ có được phép cấp chứng chỉ cho tên miền đó hay không.

## 4.4. So sánh ưu/nhược điểm các loại chứng chỉ và CA

| Đặc điểm  | Chứng chỉ DV (Ví dụ: Let's Encrypt)    | Chứng chỉ OV (Ví dụ: Sectigo, GlobalSign) | Chứng chỉ EV (Ví dụ: DigiCert)             |
| ------| ------| ------- | --------- |
| **Giá thành** | Miễn phí| Có phí (trung bình) | Cao nhất |
| **Thời gian cấp** | Vài phút đến vài giờ  | Vài ngày làm việc  | Vài ngày đến vài tuần  |
| **Mức độ xác minh**   | Chỉ tên miền   | Tên miền + Tổ chức    | Tên miền + Tổ chức (xác minh mở rộng)   |
| **Hiển thị trên trình duyệt**  | HTTPS + Khóa. Không hiển thị tên tổ chức.  | HTTPS + Khóa. Hiển thị tên tổ chức khi nhấp vào khóa. | HTTPS + Khóa. (Trước đây là thanh địa chỉ xanh lá cây, nay ẩn sau biểu tượng khóa trên các trình duyệt hiện đại). |
| **Phù hợp cho**     | Blog cá nhân, trang web nhỏ, dự án phát triển, v.v.   | E-commerce, trang web doanh nghiệp, ứng dụng kinh doanh.   | Ngân hàng, tổ chức tài chính, các doanh nghiệp lớn xử lý dữ liệu nhạy cảm.     |
| **Bảo mật mã hóa**  | Giống nhau (đều sử dụng các thuật toán mã hóa mạnh)            | Giống nhau   | Giống nhau    |
| **Ưu điểm**   | Miễn phí, dễ cài đặt, tự động hóa cao.   | Mức độ tin cậy cao hơn DV, hiển thị tên tổ chức.    | Mức độ tin cậy cao nhất, tạo uy tín mạnh mẽ cho doanh nghiệp.  |
| **Nhược điểm**   | Không xác minh tổ chức, thời hạn ngắn, không hỗ trợ trực tiếp. | Có phí, quá trình xác minh thủ công hơn DV, không hiển thị trực tiếp tên tổ chức trên thanh địa chỉ như EV trước đây. | Chi phí cao, quá trình xác minh phức tạp và mất thời gian nhất.  |


**So sánh tổng quan các CA:**

- **Các CA thương mại (Sectigo, DigiCert, GlobalSign):**
  - **Ưu điểm:** Cung cấp đầy đủ các loại chứng chỉ (DV, OV, EV), hỗ trợ kỹ thuật chuyên nghiệp, có thể cung cấp các dịch vụ giá trị gia tăng khác (ví dụ: quét lỗ hổng bảo mật, bảo hiểm).
  - **Nhược điểm:** Có phí.
- **Let's Encrypt:**
  - **Ưu điểm:** Miễn phí, tự động hóa cao, đóng góp lớn vào việc phổ cập HTTPS.
  - **Nhược điểm:** Chỉ cung cấp DV, không có hỗ trợ trực tiếp, thời hạn chứng chỉ ngắn (90 ngày).

---

# 5. Thành phần kỹ thuật (cần làm rõ hơn)

## 5.1. Public Key – Private Key (Khóa công khai – Khóa bí mật)

- **Là một cặp khóa toán học được tạo ra đồng thời. Chúng có mối quan hệ đặc biệt:**
  - Thông tin được mã hóa bằng một khóa chỉ có thể được giải mã bằng khóa kia trong cùng cặp.
  - Dữ liệu được ký bằng một khóa có thể được xác minh bằng khóa kia.
- **Public Key (Khóa công khai):**
  - **Công khai:** Như tên gọi, khóa này được thiết kế để chia sẻ công khai. Nó được nhúng trong chứng chỉ SSL/TLS và được gửi cho bất kỳ ai muốn giao tiếp an toàn với máy chủ.
  - **Chức năng chính:**
    - **Mã hóa dữ liệu:** Bất kỳ ai muốn gửi dữ liệu bảo mật tới chủ sở hữu khóa bí mật đều sử dụng khóa công khai này để mã hóa dữ liệu.
    - **Xác minh chữ ký số:** Nếu chủ sở hữu khóa bí mật đã ký số vào một tài liệu, bất kỳ ai cũng có thể sử dụng khóa công khai tương ứng để xác minh rằng chữ ký đó là hợp lệ và tài liệu không bị thay đổi.
- **Private Key (Khóa bí mật):**
  - **Bí mật tuyệt đối:** Khóa này phải được giữ bí mật tuyệt đối bởi chủ sở hữu của nó. Nếu khóa bí mật bị lộ, toàn bộ hệ thống bảo mật  sẽ bị phá vỡ, cho phép kẻ tấn công giải mã lưu lượng truy cập, mạo danh máy chủ , và ký các chứng chỉ giả mạo.
  - **Chức năng chính:**
    - **Giải mã dữ liệu:** Chỉ chủ sở hữu khóa bí mật mới có thể giải mã dữ liệu đã được mã hóa bằng khóa công khai tương ứng.
    - **Tạo chữ ký số:** Chủ sở hữu sử dụng khóa bí mật để tạo chữ ký số trên các tài liệu hoặc chứng chỉ.
- **Ví dụ trong SSL/TLS:**
  - Khi yêu cầu chứng chỉ SSL/TLS, ta cần tạo một cặp khóa.
  - Khóa công khai  được đưa vào CSR và sau đó vào chứng chỉ SSL/TLS được cấp.
  - Khóa bí mật  được lưu trữ an toàn trên máy chủ  và được sử dụng để giải mã khóa phiên trong quá trình SSL/TLS handshake.

## 5.2. CSR (Certificate Signing Request)

- **Khái niệm:** CSR là một khối văn bản mã hóa được tạo ra trên máy chủ nơi dự định cài đặt chứng chỉ SSL/TLS. Nó chứa khóa công khai  và các thông tin định danh khác (như Common Name, Organization, Locality, v.v.) muốn đưa vào chứng chỉ.
- **Mục đích:** Khi muốn mua hoặc yêu cầu một chứng chỉ từ một CA, sẽ cần tạo một CSR và gửi nó cho CA. CA sẽ sử dụng thông tin trong CSR (đặc biệt là khóa công khai) để tạo ra chứng chỉ SSL/TLS cho.
- **Quy trình:**
  1. Tạo một cặp khóa (khóa công khai và khóa bí mật) trên máy chủ của mình.
  2. Từ khóa công khai và các thông tin chi tiết về tên miền/tổ chức, tạo CSR.
  3. Gửi CSR cho CA.
  4. CA xác minh danh tính  (tùy thuộc vào loại chứng chỉ DV/OV/EV).
  5. CA sử dụng khóa công khai  từ CSR và thông tin đã xác minh để tạo một chứng chỉ SSL/TLS, sau đó ký số vào chứng chỉ đó bằng khóa bí mật của chính CA.
  6. CA gửi chứng chỉ đã ký lại cho.
  7. Cài đặt chứng chỉ đó (cùng với khóa bí mật ) trên máy chủ.
- **Lưu ý quan trọng:** CSR chỉ chứa khóa công khai. Khóa bí mật **không bao giờ** được rời khỏi máy chủ.

## 5.3. PEM, DER, CRT, CER, PFX, P12 – Các định dạng file chứng chỉ

Các định dạng file này thường gây nhầm lẫn vì chúng đều liên quan đến chứng chỉ, nhưng chúng biểu diễn các thông tin khác nhau hoặc được mã hóa theo cách khác nhau.

- **PEM (Privacy Enhanced Mail):**
  - **Định dạng:** Định dạng phổ biến nhất cho chứng chỉ và khóa. Nó là định dạng văn bản base64 mã hóa, dễ đọc và sao chép.
  - **Nội dung:** Thường chứa chứng chỉ (kết thúc bằng `-----END CERTIFICATE-----`), khóa bí mật (kết thúc bằng `-----END PRIVATE KEY-----` hoặc tương tự), hoặc CSR (kết thúc bằng `-----END CERTIFICATE REQUEST-----`).
  - **Phổ biến:** Được sử dụng rộng rãi trên các hệ thống Unix/Linux, Apache, Nginx.
  - **Mở rộng file:** `.pem`, `.crt`, `.cer`, `.key`.
- **DER (Distinguished Encoding Rules):**
  - **Định dạng:** Định dạng nhị phân (binary) cho chứng chỉ và khóa. Không phải văn bản thuần túy.
  - **Nội dung:** Tương tự như PEM nhưng ở dạng nhị phân.
  - **Phổ biến:** Thường được sử dụng trên các hệ thống Windows, Java.
  - **Mở rộng file:** `.der`, `.cer` (trên Windows, `.cer` có thể là PEM hoặc DER).
- **CRT (Certificate):**
  - **Định dạng:** Thường là một tệp PEM hoặc DER chứa chứng chỉ cuối cùng.
  - **Mở rộng file:** `.crt`.
- **CER (Certificate):**
  - **Định dạng:** Tương tự như CRT, có thể là PEM hoặc DER.
  - **Mở rộng file:** `.cer`. Thường dùng trên Windows.
- **PFX (Personal Information Exchange) / PKCS#12 (.p12):**
  - **Định dạng:** Định dạng nhị phân được bảo vệ bằng mật khẩu, chứa **cả chứng chỉ (certificate) và khóa bí mật (private key)** cùng với chuỗi chứng chỉ (certificate chain) vào một tệp duy nhất.
  - **Mục đích:** Thường được sử dụng để di chuyển chứng chỉ và khóa bí mật giữa các máy chủ, đặc biệt là trên các hệ thống Windows (IIS) và Java keystores.
  - **Mở rộng file:** `.pfx`, `.p12`.
  - **Lưu ý:** Vì chứa khóa bí mật, tệp PFX/P12 cần được bảo vệ cực kỳ cẩn thận bằng mật khẩu mạnh.

**Tóm tắt về định dạng:**

- **Text-based (PEM):** Dễ đọc, dễ thao tác (copy/paste), được dùng cho chứng chỉ, khóa, CSR.
- **Binary (DER):** Hiệu quả hơn cho việc xử lý máy tính.
- **Container (PFX/P12):** Chứa nhiều thành phần (chứng chỉ, khóa bí mật, chain) trong một tệp, được bảo vệ bằng mật khẩu.

## 5.4. Chuỗi chứng chỉ (Certificate Chain)

- **Khái niệm:** Chuỗi chứng chỉ là một danh sách các chứng chỉ liên kết với nhau một cách phân cấp, bắt đầu từ chứng chỉ của thực thể (End-Entity Certificate) và kết thúc ở một chứng chỉ gốc được tin cậy (Root Certificate). Mục đích là để trình duyệt có thể xác minh rằng chứng chỉ máy chủ được ký bởi một CA đáng tin cậy.
- **Cấu trúc phân cấp:**
  1. **End-Entity Certificate (Chứng chỉ máy chủ/tên miền):** Chứng chỉ được cấp cho trang web hoặc máy chủ  (ví dụ: `yourdomain.com`). Nó được ký bởi một Intermediate CA.
  2. **Intermediate CA Certificate (Chứng chỉ CA trung gian):** Chứng chỉ này thuộc về một CA trung gian. Nó được sử dụng để ký chứng chỉ . Bản thân Intermediate CA Certificate lại được ký bởi một Root CA. Một chuỗi có thể có một hoặc nhiều Intermediate CA.
  3. **Root CA Certificate (Chứng chỉ CA gốc):** Đây là chứng chỉ cao nhất trong hệ thống phân cấp, tự ký và được cài đặt sẵn trong kho lưu trữ tin cậy (Trust Store) của các trình duyệt web và hệ điều hành (ví dụ: Google, Microsoft, Apple, Mozilla).
- **Mục đích của Intermediate CA:**
  - Root CA Certificates được giữ offline và cực kỳ an toàn vì nếu chúng bị lộ, toàn bộ hệ thống tin cậy sẽ sụp đổ.
  - Intermediate CA Certificates được sử dụng để thực hiện việc ký chứng chỉ hàng ngày, đóng vai trò "cầu nối" an toàn giữa Root CA và chứng chỉ End-Entity. Điều này giới hạn rủi ro nếu một CA trung gian bị xâm phạm.
- **Quá trình xác minh chuỗi:**
  - Khi trình duyệt nhận được chứng chỉ máy chủ, nó không thể trực tiếp xác minh bằng CA gốc (vì CA gốc không trực tiếp ký chứng chỉ máy chủ).
  - Trình duyệt sử dụng khóa công khai của Intermediate CA để xác minh chữ ký trên chứng chỉ máy chủ.
  - Sau đó, trình duyệt tìm kiếm chứng chỉ của Intermediate CA trong kho lưu trữ của nó. Nếu không có, máy chủ sẽ gửi kèm theo chứng chỉ Intermediate CA.
  - Tiếp theo, trình duyệt sử dụng khóa công khai của Root CA để xác minh chữ ký trên chứng chỉ Intermediate CA.
  - Nếu Root CA này nằm trong danh sách các CA gốc đáng tin cậy của trình duyệt, thì toàn bộ chuỗi được coi là hợp lệ và đáng tin cậy.
- **Ví dụ về chuỗi:** `YourWebsite.com` (End-Entity) ← `DigiCert TLS RSA SHA256 2020 CA1` (Intermediate CA) ← `DigiCert Global Root G2` (Root CA).

## 5.5. Intermediate CA và Root CA

- **Root CA (Root Certificate Authority - Tổ chức cấp chứng chỉ gốc):**
  - **Tính chất:** Là đỉnh của hệ thống phân cấp PKI (Public Key Infrastructure). Chứng chỉ của Root CA là "tự ký" (self-signed), nghĩa là nó tự ký vào chính nó.
  - **Độ tin cậy:** Các Root CA được kiểm tra và chấp thuận nghiêm ngặt bởi các nhà phát triển trình duyệt và hệ điều hành lớn (Microsoft, Google, Apple, Mozilla). Chứng chỉ của họ được cài đặt sẵn trong Trust Store của các thiết bị và phần mềm.
  - **Bảo mật:** Khóa bí mật của Root CA được bảo vệ cực kỳ nghiêm ngặt, thường được lưu trữ trong các mô-đun bảo mật phần cứng (Hardware Security Modules - HSM) ngoại tuyến, trong các phòng an toàn vật lý được bảo vệ cao. Chúng hiếm khi được sử dụng trực tiếp để ký các chứng chỉ End-Entity.
  - **Mục đích:** Để thiết lập điểm khởi đầu của sự tin cậy. Nếu một Root CA bị xâm phạm, hàng triệu chứng chỉ trên toàn cầu có thể bị ảnh hưởng.
- **Intermediate CA (Intermediate Certificate Authority - Tổ chức cấp chứng chỉ trung gian):**
  - **Tính chất:** Là các CA nằm giữa Root CA và End-Entity Certificate trong chuỗi tin cậy. Chúng được ký bởi Root CA (hoặc bởi một Intermediate CA khác đã được ký bởi Root CA).
  - **Mục đích:**
    - **Tăng cường bảo mật:** Giúp giảm thiểu rủi ro nếu một khóa bí mật CA bị xâm phạm. Nếu một Intermediate CA bị lộ, chỉ các chứng chỉ được cấp bởi CA đó bị ảnh hưởng, không phải toàn bộ hệ thống.
    - **Quản lý linh hoạt:** Cho phép Root CA ủy quyền việc cấp chứng chỉ hàng ngày cho các Intermediate CA mà không cần đưa khóa bí mật của Root CA ra môi trường hoạt động.
  - **Triển khai:** Khi cài đặt chứng chỉ SSL/TLS trên máy chủ, ngoài chứng chỉ của tên miền, cũng cần cài đặt các chứng chỉ Intermediate CA (thường được cung cấp cùng với chứng chỉ ) để trình duyệt có thể xây dựng chuỗi tin cậy hoàn chỉnh.

---
# CÀI ĐẶT SSL

## Cài đặt SSL trên APACHE
## Cài đặt SSL trên NGINX
## Cài đặt SSL trên IIS 
## Cài đặt IIS trên Tomcat
## Cài đặt IIS trên CPanel
## Cài đặt IIS trên Plesk
## Cài đặt IIS trên DirectAdmin
## SSL cho tên miền chính và subdomain
## Tự động gia hạn (Auto-renewal) với Let's Encrypt
