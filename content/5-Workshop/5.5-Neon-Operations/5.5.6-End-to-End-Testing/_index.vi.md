---
title : "Kiểm thử End-to-End"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.5.6. </b> "
---

Sau khi hoàn thành phần này, hệ thống sẽ được xác nhận:
- Toàn bộ luồng người dùng từ **Frontend** qua **ALB** đến **Backend (ECS)** hoạt động đúng
- Các dịch vụ lưu trữ và CDN (**S3, CloudFront, RDS MySQL**) phục vụ đúng dữ liệu
- Không có lỗi, liên kết hỏng hoặc sự cố dữ liệu ở các tính năng cốt lõi


### 5.5.6.1. Các kịch bản kiểm thử

#### Kịch bản 1. Luồng đăng ký → đăng nhập

| Mục | Nội dung |
|-----|----------|
| **Mục tiêu** | Đảm bảo luồng tạo tài khoản và đăng nhập thành công |
| **Kết quả mong đợi** | Người dùng tạo tài khoản thành công, đăng nhập được và hệ thống cập nhật đúng trạng thái |

**Quy trình thực hiện:**

1. Truy cập web qua CloudFront.
2. Vào trang **Settings**, chọn **"Đăng nhập tài khoản khác"** → nhấn **Tạo tài khoản**.
3. Điền đầy đủ thông tin vào form đăng ký.
4. Sử dụng tài khoản vừa tạo để thực hiện **Đăng nhập**.

**Trường hợp ngoại lệ:**
- TH1: Không mở được cổng giao dịch do sự cố hệ thống.
- TH2: Điền sai thông tin, form báo lỗi validation.
- TH3: Đăng nhập thất bại do mật khẩu không chính xác.

![Hình 1. Màn hình đăng ký](/images/5-Workshop/5.5-Neon-Operations/image018.png)

![Hình 2. Màn hình đăng nhập](/images/5-Workshop/5.5-Neon-Operations/image020.png)


#### Kịch bản 2. Duyệt điểm đến (Browse POIs) và xem mô tả

| Mục | Nội dung |
|-----|----------|
| **Mục tiêu** | Kiểm thử khả năng tải danh sách POI và lấy thông tin mô tả từ backend |
| **Kết quả mong đợi** | Dữ liệu địa điểm hiển thị đầy đủ, hình ảnh và mô tả tải nhanh, không bị lỗi kết nối API |

**Quy trình thực hiện:**

1. Tại trang chủ, lướt xem các POI trên bản đồ.
2. Chọn một POI để xem thông tin chi tiết.
3. Kiểm tra hình ảnh, tên địa điểm và đoạn mô tả hiển thị đúng.

**Trường hợp ngoại lệ:**
- TH1: Mất địa điểm khi reload trang (hiếm gặp, do cache chưa load kịp).

![Hình 3. Màn hình trang chính với danh sách POI](/images/5-Workshop/5.5-Neon-Operations/image022.png)


#### Kịch bản 3. Phát âm thanh thuyết minh qua CloudFront

| Mục | Nội dung |
|-----|----------|
| **Mục tiêu** | Đảm bảo các tệp âm thanh phát mượt mà thông qua CDN CloudFront |
| **Kết quả mong đợi** | Âm thanh phát trực tuyến nhanh, ổn định, không bị giật lag, đúng tệp yêu cầu |

**Quy trình thực hiện:**

1. Tại trang chủ, chọn một POI để nghe thuyết minh.
2. Nhấn nút **Phát âm thanh** — âm thanh tự động phát.
3. Kiểm tra nút dừng/tiếp tục hoạt động đúng.

**Trường hợp ngoại lệ:**
- TH1: Tệp đứng hình trong khoảng thời gian ngắn khi hệ thống đang generate TTS (trường hợp tệp âm thanh gốc bị xóa).
- TH2: Phát nhầm giọng đọc cho ngôn ngữ (đã được fix).

![Hình 4. Màn hình POI đang phát thuyết minh](/images/5-Workshop/5.5-Neon-Operations/image024.png)
![Hình 5. Màn hình POI đang dừng thuyết minh](/images/5-Workshop/5.5-Neon-Operations/image026.png)


#### Kịch bản 4. Đặt tour và theo dõi hành trình

| Mục | Nội dung |
|-----|----------|
| **Mục tiêu** | Kiểm thử tính năng đặt tour và cập nhật trạng thái hành trình |
| **Kết quả mong đợi** | Hệ thống tự phát âm thuyết minh và cập nhật trạng thái khi người dùng di chuyển đến địa điểm trong tour |

**Quy trình thực hiện:**

1. Vào trang **Tour**, chọn một tour (cần mở khóa nếu là Tour Premium).
2. Nhấn **Bắt đầu hành trình**.
3. Di chuyển đến các địa điểm trong tour được chỉ định.
4. Kiểm tra hệ thống tự phát audio và cập nhật trạng thái hành trình.

**Trường hợp ngoại lệ:**
- TH1: Không thể bắt đầu tour do chưa mở khóa các Tour Premium.

![Hình 6. Màn hình Tour Premium chưa mở khóa](/images/5-Workshop/5.5-Neon-Operations/image028.png)

![Hình 7. Màn hình Tour Premium miễn phí](/images/5-Workshop/5.5-Neon-Operations/image030.png)

![Hình 8. Màn hình Tour đang thuyết minh](/images/5-Workshop/5.5-Neon-Operations/image032.png)


#### Kịch bản 5. Tích hợp thanh toán (Sandbox)

| Mục | Nội dung |
|-----|----------|
| **Mục tiêu** | Kiểm thử cổng thanh toán với môi trường sandbox |
| **Kết quả mong đợi** | Thanh toán thành công, trạng thái đơn hàng được cập nhật đúng |

**Quy trình thực hiện:**

1. Chọn một dịch vụ hoặc đơn hàng trên hệ thống.
2. Chọn phương thức thanh toán qua **PayPal** hoặc thẻ sandbox.
3. Điền thông tin và nhấn **Xác nhận thanh toán**.
4. Kiểm tra trạng thái đơn hàng được cập nhật.

**Trường hợp ngoại lệ:**
- TH1: Không thể thanh toán do cung cấp không đủ thông tin.
- TH2: Không thể thanh toán do thông tin sai.
- TH3: Không thể thanh toán do không đủ số dư.
- TH4: Không thể thanh toán do thiết bị đang offline.

![Hình 9. Màn hình khi mở thanh toán](/images/5-Workshop/5.5-Neon-Operations/image034.png)

![Hình 10. Màn hình khi chọn thanh toán bằng Paypal](/images/5-Workshop/5.5-Neon-Operations/image036.png)

![Hình 11. Màn hình khi chọn thanh toán bằng thẻ](/images/5-Workshop/5.5-Neon-Operations/image038.png)

![Hình 12. Màn hình khi thanh toán thành công](/images/5-Workshop/5.5-Neon-Operations/image040.png)


#### Kịch bản 6. Kiểm thử xử lý lỗi (Invalid data & Timeouts)

| Mục | Nội dung |
|-----|----------|
| **Mục tiêu** | Đảm bảo cơ chế xử lý ngoại lệ thân thiện với người dùng |
| **Kết quả mong đợi** | Giao diện hiển thị thông báo lỗi rõ ràng, không làm sập ứng dụng |

**Quy trình thực hiện:**

1. Nhập dữ liệu không hợp lệ vào các form đăng ký hoặc đặt chỗ.
2. Mô phỏng sự cố độ trễ mạng hoặc timeout.
3. Kiểm tra thông báo lỗi hiển thị đúng và hệ thống vẫn hoạt động bình thường.


#### Kịch bản 7. Kiểm thử giao diện đa nền tảng (Mobile Responsiveness)

| Mục | Nội dung |
|-----|----------|
| **Mục tiêu** | Đảm bảo hoạt động nhất quán trên Desktop và Mobile |
| **Kết quả mong đợi** | Giao diện co giãn linh hoạt, không bị tràn viền hay che khuất thành phần quan trọng |

**Quy trình thực hiện:**

1. Dùng **Developer Tools** trên trình duyệt (hoặc thiết bị thực tế) để giả lập các kích thước màn hình.
2. Kiểm tra UI, UX trên cả mobile và desktop.

![Hình 13. Giao diện trên điện thoại](/images/5-Workshop/5.5-Neon-Operations/image042.png)
![Hình 14. Giao diện trên máy tính](/images/5-Workshop/5.5-Neon-Operations/image044.png)


### 5.5.6.2. Tổng kết kiểm thử

Hầu hết các kịch bản kiểm thử đều hoạt động đúng như kỳ vọng. Các trường hợp ngoại lệ đã được ghi nhận và phần lớn đã được xử lý hoặc có phương án dự phòng.

| Kịch bản | Kết quả |
|----------|---------|
| Đăng ký → Đăng nhập | Passed |
| Browse POIs + Xem mô tả | Passed |
| Phát audio qua CloudFront | Passed |
| Đặt tour + Theo dõi hành trình | Passed |
| Thanh toán sandbox | Passed |
| Xử lý lỗi và timeout | Passed |
| Mobile responsiveness | Passed |
