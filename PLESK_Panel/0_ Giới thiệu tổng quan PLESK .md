> # PLESK

# 1. Giới thiệu tổng quan về PLESK 

- Plesk là một bảng điều khiển quản trị web hosting (hosting control panel) phổ biến, giúp người dùng quản lý các máy chủ và trang web một cách dễ dàng thông qua giao diện đồ họa thân thiện. Nó hỗ trợ cả hệ điều hành Linux và Windows, cung cấp các công cụ để quản lý tài khoản hosting, tên miền, email, cơ sở dữ liệu, và bảo mật.

- Plesk tự giới thiệu là một nền tảng lưu trữ trang web toàn diện, mang lại tính kỹ thuật, tùy chọn bảo mật và công cụ tự động hóa, tất cả trong một bảng điều khiển. Control Panel này xếp vị trí thứ hai trong danh sách các bảng điều khiển máy chủ phổ biến nhất. Nó được triển khai trên hơn 370.000 máy chủ, hỗ trợ hơn 12 triệu trang web.

# 2 Các phiên bản của Plesk

Plesk có hai phiên bản chính là **Plesk Onyx** và **Plesk Obsidian**, trong đó Plesk Obsidian là phiên bản mới nhất và được cập nhật thường xuyên. Ngoài ra, Plesk còn có các phiên bản theo dạng gói như Web Admin Edition, Web Pro Edition, Web Host Edition, và Developer Pack, dành cho các đối tượng người dùng khác nhau. 

## Plesk Onyx 

Là một phiên bảnquản lý trang web cơ bản và đầu tiên của Plesk, thiết kế để quản lý máy chủ và các dịch vụ lưu trữ web một cách hiệu quả hơn, đồng thời tập trung vào việc hỗ trợ tốt hơn cho các nhà phát triển. Dưới đây là các tính năng nổi bật của Plesk Onyx:
- Tích hợp Git: Hỗ trợ quản lý mã nguồn với Git trực tiếp từ bảng điều khiển. Người dùng có thể dễ dàng triển khai ứng dụng từ các kho lưu trữ GitHub hoặc Bitbucket.
- Docker Integration: Onyx cung cấp khả năng tích hợp với Docker, cho phép người dùng dễ dàng triển khai các container Docker trực tiếp từ giao diện của Plesk.
- WordPress Toolkit: Công cụ này giúp dễ dàng cài đặt, quản lý và bảo mật WordPress. Tuy nhiên, phiên bản này chỉ hỗ trợ các tính năng cơ bản, như cài đặt và cập nhật tự động WordPress, themes và plugins.
- Tường lửa: Tính năng bảo mật bao gồm quản lý tường lửa mạnh mẽ, cùng với bảo vệ chống brute-force và DDoS để đảm bảo an toàn cho các ứng dụng web.

## Plesk Obsidian

Plesk Obsidian, ra mắt năm 2019, là bản nâng cấp toàn diện so với Onyx, tập trung vào việc cải thiện bảo mật, hiệu suất và trải nghiệm người dùng. Đây là phiên bản hiện đại nhất với nhiều tính năng mới và tối ưu hóa vượt trội:

- **Fail2Ban** tích hợp: Obsidian hỗ trợ Fail2Ban, một công cụ bảo vệ máy chủ khỏi các cuộc tấn công brute-force bằng cách tự động chặn địa chỉ IP có hành vi nghi ngờ.
- SSL It!: Hỗ trợ cài đặt chứng chỉ SSL một cách tự động và liên tục đảm bảo mọi tên miền luôn được bảo mật. Không chỉ cài đặt SSL, mà còn tự động gia hạn và cài đặt trên toàn hệ thống.
- Advisor Security: Đây là công cụ tư vấn bảo mật mới, giúp quản trị viên tối ưu hóa hệ thống bảo mật bằng cách cung cấp các đề xuất và mẹo nâng cao.
- WordPress Toolkit cải tiến: WordPress Toolkit trong Obsidian cung cấp các tính năng mở rộng như staging, cloning (nhân bản), smart updates (cập nhật thông minh) và các công cụ quản lý bảo mật mạnh mẽ hơn.
- SEO Toolkit: Tính năng giúp tối ưu hóa các trang web cho công cụ tìm kiếm. Nó cung cấp các công cụ để phân tích, theo dõi và cải thiện SEO.
- Improved File Manager: Quản lý file được cải tiến với khả năng kéo và thả trực quan, hỗ trợ zip/unzip, chỉnh sửa mã nguồn và nhiều tính năng thuận tiện cho người dùng không quen thuộc với dòng lệnh.

# 3. Plesk phù hợp với ai?
**Plesk** được xây dựng để phục vụ nhiều đối tượng người dùng khác nhau, với **các cấp độ quyền hạn** và nhu cầu khác nhau: 3 cấp độ chính

- **Quản trị viên máy chủ (Administrator)**: Có toàn quyền kiểm soát máy chủ, quản lý tài nguyên, cấu hình dịch vụ, tạo gói reseller và gói hosting, cũng như giám sát toàn bộ hệ thống.
- **Đại lý (Reseller)**: Được phép tạo và quản lý các gói hosting riêng để cung cấp cho khách hàng của mình, nhưng không có quyền truy cập vào cấu hình máy chủ gốc.
- **Khách hàng (Customer)**: Là người dùng cuối, được cấp một gói hosting để quản lý website, email, cơ sở dữ liệu và các ứng dụng của riêng họ một cách dễ dàng.

# 4. Ưu và nhược điểm của Plesk

### Ưu điểm

* **Đa nền tảng:** Khả năng hoạt động trên cả hai nền tảng hệ điều hành là Windows và Linux, tạo điều kiện thuận lợi cho sự linh hoạt và đa dạng của người sử dụng.
* **Độ ổn định và tin cậy cao:** Phần mềm này được đánh giá cao về độ ổn định và tin cậy, giúp người dùng tự tin trong quá trình quản lý hosting và website.
* **Tính đầy đủ của các tính năng:** Cung cấp một loạt các tính năng từ cơ bản đến nâng cao, hỗ trợ người dùng trong quản lý và tối ưu hóa hoạt động của họ trên internet.
* **Giao diện thân thiện:** Giao diện đơn giản và thân thiện với người dùng giúp người mới sử dụng và người có kinh nghiệm tận dụng toàn bộ tiềm năng của hệ thống quản lý hosting này.
* **Tính linh hoạt và tiện ích cao:** Có khả năng tích hợp thiết kế website, thanh toán tự động và giao diện storefront SaaS, tạo ra một trải nghiệm quản lý hosting toàn diện.
* **Dễ dàng tạo và quản lý hosting:** Cho phép người dùng tạo và quản lý nhiều hosting cùng một lúc dựa trên cấu hình định sẵn.
* **Quản lý tài khoản FTP linh hoạt:** Cho phép tạo nhiều tài khoản FTP và linh hoạt trong cấu trúc web.

### Nhược điểm

Mặc dù Plesk mang lại nhiều ưu điểm, nhưng cũng tồn tại một số hạn chế:

* **Chi phí cao:** Plesk không phải là giải pháp miễn phí, và chi phí sử dụng có thể khá đắt, đặc biệt với các gói có nhiều tính năng cao cấp hoặc sử dụng cho mục đích thương mại.
* **Sự phức tạp khi cài đặt ban đầu**: Đối với những người không có kinh nghiệm, việc cài đặt và thiết lập ban đầu của Plesk trên server có thể phức tạp và mất thời gian.
* **Tài nguyên hệ thống:** Plesk yêu cầu một lượng tài nguyên hệ thống khá lớn. Điều này có thể gây ảnh hưởng đến hiệu suất khi chạy trên các máy chủ có cấu hình thấp hoặc bị giới hạn tài nguyên.
* **Cần cập nhật liên tục**: Để duy trì tính bảo mật và ổn định, Plesk website hosting cần được cập nhật thường xuyên. Quá trình này đôi khi có thể gây gián đoạn ngắn trong dịch vụ, đặc biệt là khi chạy các website đang hoạt động.
* **Hỗ trợ không toàn diện cho một số nền tảng:** Mặc dù Plesk hỗ trợ nhiều hệ điều hành, một số tính năng có thể bị giới hạn hoặc không hoạt động tốt trên các phiên bản hệ điều hành hoặc phần cứng cụ thể.

