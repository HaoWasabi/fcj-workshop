---
title: "Bản đề xuất"
date: 2026-07-01
weight: 2
chapter: false
pre: "<b>2.</b>"
---

# NeonFoodmap Website
## Tự động hóa quy trình CI/CD trên nền tảng AWS

### 1. Tổng quan dự án


Đề xuất này trình bày giải pháp triển khai hệ thống NeonFoodMap trên nền tảng Amazon Web Services (AWS) theo kiến trúc Cloud-Native, đáp ứng các yêu cầu về khả năng mở rộng, tính sẵn sàng cao, bảo mật và tự động hóa quy trình phát hành phần mềm. Mục tiêu của giải pháp là xây dựng một hạ tầng triển khai có khả năng tái sử dụng, hỗ trợ triển khai lặp lại, đồng thời chuẩn hóa quy trình vận hành theo định hướng DevOps trong môi trường Production.


NeonFoodMap là nền tảng website bản đồ ẩm thực, cho phép người dùng tìm kiếm, khám phá và đánh giá các địa điểm ăn uống theo thời gian thực. Hệ thống tích hợp các chức năng như tìm kiếm địa điểm (POI), định vị GPS, hiển thị lộ trình, đánh giá địa điểm và phát nội dung mô tả bằng công nghệ Text-to-Speech nhằm nâng cao trải nghiệm người dùng. Với đặc điểm xử lý dữ liệu theo thời gian thực và yêu cầu phục vụ nhiều người dùng đồng thời, hệ thống cần được triển khai trên một hạ tầng có khả năng mở rộng linh hoạt, đảm bảo tính sẵn sàng và dễ dàng bảo trì.


Đề xuất tập trung xây dựng kiến trúc triển khai sử dụng Docker và Amazon ECS Fargate, quản lý mã nguồn bằng GitHub, tự động hóa quy trình Build–Test–Deploy thông qua GitHub Actions và OpenID Connect (OIDC), lưu trữ Docker Image trên Amazon ECR, triển khai cơ sở dữ liệu Amazon RDS trong Private Subnet, quản lý tài nguyên tĩnh bằng Amazon S3 và giám sát hệ thống bằng Amazon CloudWatch. Giải pháp hướng tới việc hình thành một quy trình triển khai thống nhất, an toàn và có khả năng mở rộng cho các giai đoạn phát triển tiếp theo của dự án.


---


### 2. Phát biểu vấn đề
### Hiện trạng

Trước khi triển khai đề xuất, dự án NeonFoodMap Website mới chỉ tồn tại ở dạng mã nguồn ứng dụng (Frontend và Backend) hoạt động đơn lẻ, chưa được chuẩn hóa quy trình triển khai hay tích hợp lên hạ tầng đám mây. Cụ thể:

* **Chưa có hạ tầng tự động hóa:** Quy trình build và deploy ứng dụng đang thực hiện thủ công, chưa thiết lập luồng CI/CD tự động hóa trên môi trường Production.
* **Chưa ứng dụng mô hình Container hóa:** Ứng dụng chưa được đóng gói chuẩn hóa dưới dạng Docker Image để vận hành nhất quán giữa các môi trường.
* **Hạ tầng AWS chưa được thiết lập:** Hệ thống mạng VPC, cơ sở dữ liệu phân tán, các chính sách bảo mật IAM tối ưu cũng như các cơ chế giám sát (Monitoring/Logging) trên nền tảng AWS chưa được xây dựng và cấu hình đồng bộ.

### Mục tiêu

Đề xuất hướng tới các mục tiêu kỹ thuật sau:

- Tự động hóa quy trình Build, Test và Deploy.
- Loại bỏ việc sử dụng AWS Access Key trong GitHub thông qua OpenID Connect (OIDC).
- Chuẩn hóa quy trình triển khai ứng dụng theo mô hình Container.
- Đảm bảo tính sẵn sàng cao (High Availability) cho hệ thống.
- Hỗ trợ mở rộng tài nguyên linh hoạt theo nhu cầu tải.
- Thiết lập cơ chế giám sát, ghi log và cảnh báo tập trung.
- Chuẩn hóa quy trình triển khai theo mô hình DevOps và nâng cao khả năng tái sử dụng.

### Giải pháp

- Thiết kế kiến trúc hạ tầng AWS.
- Xây dựng quy trình CI/CD.
- Triển khai Backend và Frontend bằng Amazon ECS Fargate.
- Quản lý Docker Image.
- Cấu hình cơ sở dữ liệu.
- Quản lý Static Assets.
- Xây dựng hệ thống Logging và Monitoring.
- Hoàn thiện tài liệu triển khai theo từng Sprint.

### Lợi tức đầu tư
Việc chuẩn hóa và tự động hóa hệ thống mang lại những giá trị thiết thực:

- Tối ưu hóa chi phí vận hành (Cost Efficiency): Mô hình Serverless (ECS Fargate) và Serverless Storage giúp chỉ chi trả theo tài nguyên thực tế sử dụng, giảm thiểu lãng phí hạ tầng idle (nhàn rỗi).

- Tăng tốc độ phát triển (Time-to-Market): Quy trình CI/CD tự động giúp giảm thời gian release tính năng mới từ vài giờ/ngày xuống chỉ còn vài phút.

- Độ ổn định và sẵn sàng cao (High Availability): Hạ tầng tự động phục hồi và cân bằng tải giúp hệ thống đạt uptime cao, hạn chế tối đa thời gian gián đoạn dịch vụ (Downtime).

- Bảo mật và kiểm soát tốt hơn: Các tiêu chuẩn bảo mật của AWS kết hợp hệ thống giám sát chủ động giúp bảo vệ dữ liệu khách hàng và phát hiện sớm các lỗ hổng tiềm ẩn.


---


### 3. Kiến trúc giải pháp
![Sơ đồ kiến trúc 1](/images/2-Proposal/diagram1.png)

![Sơ đồ kiến trúc 2](/images/2-Proposal/diagram2.png)

### Danh sách dịch vụ AWS được sử dụng
Dưới đây là bảng liệt kê các dịch vụ AWS được sử dụng cho dự án:

| Dịch vụ AWS | Loại hình Dịch vụ | Vai trò & Chức năng trong Hệ thống |
| --- | --- | --- |
| **AWS IAM** | Identity & Access Management | Quản lý người dùng, nhóm, vai trò (Roles) và chính sách bảo mật, bắt buộc bật chính sách Force MFA cho tất cả tài khoản. |
| **VPC** | Networking | Cung cấp mạng riêng ảo (Virtual Private Cloud) với các dải CIDR blocks, Subnets công cộng và riêng tư, Route Tables, Internet Gateway và NAT Gateways. |
| **Amazon RDS** | Relational Database | Củng cố cơ sở dữ liệu quan hệ (RDS MySQL Multi-AZ) để lưu trữ và quản lý dữ liệu ứng dụng. |
| **Amazon S3** | Object Storage | Lưu trữ tệp tin với các bucket chuyên biệt (frontend, media, audio, logs), hỗ trợ cấu hình phiên bản (versioning), chính sách vòng đời (lifecycle) và mã hóa. |
| **Amazon ECR** | Container Registry | Kho lưu trữ các Docker Container Images cho cả Frontend và Backend. |
| **Amazon ECS** | Container Orchestration | Quản lý cụm cụm máy chủ ảo (Cluster) chạy ứng dụng theo dạng Fargate launch type. |
| **Application Load Balancer (ALB)** | Load Balancing | Phân phối lưu lượng truy cập HTTP/HTTPS internet vào các target groups và hỗ trợ cấu hình chuyển hướng, health checks. |
| **Amazon CloudWatch** | Monitoring & Observability | Thu thập log (CloudWatch Logs), theo dõi metrics và thiết lập các dashboard, báo động (alarms). |
| **Amazon SNS** | Push Notification Service | Gửi thông báo cảnh báo (ví dụ: billing alerts cho chi phí) tới quản trị viên. |
| **AWS CloudFront** | Content Delivery Network (CDN) | Phân phối nội dung toàn cầu, tăng tốc độ truy cập giao diện frontend và caching file âm thanh. |


----


### 4. Timeline & Milestones

| Giai đoạn | Thời gian | Hạng mục công việc chính |
| :--- | :--- | :--- | 
| **Tuần 1: Nghiên cứu & Thiết kế** | 22/06/2026 - 26/06/2026 | - Tìm hiểu AWS Foundation (Global Infrastructure, IAM, VPC, EC2, S3).<br><br>- Thiết kế kiến trúc hệ thống (Application, Database, Storage, Networking) và sơ đồ luồng dữ liệu. |
| **Tuần 2: Tìm hiểu Services & Thiết kế chi tiết** | 29/06/2026 - 03/07/2026 | - Tìm hiểu RDS và quy trình migrate database.<br><br>- Tìm hiểu ECS/ECR, CloudWatch, SQS, Athena, QuickSight, API Gateway và Load Balancer.<br><br>- Hoàn thiện sơ đồ kiến trúc triển khai. |
| **Tuần 3: Phát triển Front-end & Back-end** | 06/07/2026 - 10/07/2026 | - Phát triển Frontend (xây dựng giao diện, tích hợp API, Responsive UI).<br><br>- Phát triển Backend (Database Schema, RESTful API, Authentication/Authorization).<br><br>- Tạo IAM User, chính sách bảo mật và cài đặt Bill Alert. |
| **Tuần 4: Foundation & Infrastructure** | 13/07/2026 - 17/07/2026 | - Thiết lập VPC Multi-AZ.<br><br>- Cấp phát RDS MySQL.<br><br>- S3 Buckets + Lifecycle + IAM.<br><br>- Cấu hình IAM (CloudFormation).<br><br>- Thiết lập ECR + Docker. |
| **Tuần 5: CI/CD Pipeline & Deployment** | 20/07/2026 - 24/07/2026 | - Xây dựng CI/CD pipeline với GitHub Actions.<br><br>- Cấu hình ECS cluster + task definitions.<br><br>- Cấu hình ALB + Target Groups + Health Checks.<br><br>- Cấu hình Django trên AWS.<br><br>- Cấu hình React trên AWS. |
| **Tuần 6-7: Scaling, Monitoring & Go-Live** | 27/07/2026 - 07/08/2026 | - Cấu hình ECS Services + Auto-Scaling.<br><br>- Thiết lập CloudFront + CDN.<br><br>- Triển khai CloudWatch dashboard.<br><br>- Giám sát chi phí & Cài đặt cảnh báo (Cost Monitoring & Alerts).<br><br>- CloudWatch Logs + Log Insights.<br><br>- Kiểm thử toàn diện (End-to-End Testing).<br><br>- Hoàn thiện tài liệu triển khai. |
---


### 5.  Ngân sách dự kiến

Hệ thống tận dụng tối đa mô hình **AWS Free Tier** và **Serverless Pay-As-You-Go** (chỉ trả tiền cho tài nguyên thực tế sử dụng), giúp tối ưu hóa chi phí vận hành ở mức thấp nhất.

| Dịch vụ AWS | Mức sử dụng thực tế / giai đoạn | Chi phí thực tế ước tính (USD) | Vai trò & Chức năng trong Hệ thống |
| --- | --- | --- | --- |
| **Amazon RDS** | RDS MySQL Multi-AZ (chạy liên tục, có scale/migrate version) | **$10.00 - $15.00** | Củng cố cơ sở dữ liệu quan hệ để lưu trữ và quản lý dữ liệu ứng dụng. |
| **Amazon S3 & ECR** | Lưu trữ tệp tin (media, logs) và Docker Container Images | **$2.00 - $5.00** | Lưu trữ tệp tin chuyên biệt và kho lưu trữ Docker Container Images cho Frontend/Backend. |
| **Amazon ECS & NAT Gateway** | Chạy cụm container (Fargate) kết hợp NAT Gateways hoạt động liên tục | **$10.00 - $20.00** | Quản lý cụm cụm máy chủ ảo chạy ứng dụng và định tuyến mạng. |
| **Application Load Balancer (ALB)** | Phân phối lưu lượng HTTP/HTTPS internet | **$3.00 - $6.00** | Phân phối lưu lượng truy cập vào các target groups, hỗ trợ health checks. |
| **Amazon CloudWatch & SNS** | Thu thập log (CloudWatch Logs), metrics, dashboards và alarms | **$1.00 - $3.00** | Giám sát hệ thống, theo dõi metrics và gửi thông báo cảnh báo qua SNS. |
| **AWS CloudFront** | Phân phối nội dung CDN và caching | **$0.00 - $2.00** | Tăng tốc độ truy cập giao diện frontend và caching file âm thanh. |
| **TỔNG CHI PHÍ THỰC TẾ** | **Vận hành hệ thống & Testing** | **~$26.00 - +$51.00 USD / tháng** | *Bám sát theo các mốc log cost thực tế phát sinh trong quá trình chạy thử nghiệm và cấu hình tài nguyên (thực tế ghi nhận khoảng **31.52$ vào ngày 25/07/2026**).* |


---


### 6. Kết quả mong đợi


Sau khi hoàn thành quá trình triển khai, hệ thống dự kiến đạt được các kết quả sau:


- Hoàn thiện kiến trúc triển khai trên nền tảng AWS theo mô hình Cloud-Native.
- Quy trình CI/CD hoạt động tự động từ Build đến Deploy.
- Ứng dụng được triển khai bằng Amazon ECS Fargate.
- Docker Image được quản lý tập trung trên Amazon ECR.
- Cơ sở dữ liệu được triển khai an toàn trong Private Subnet.
- Hệ thống được giám sát thông qua cơ chế Logging, Monitoring và Alerting.
- Quy trình triển khai được chuẩn hóa, có khả năng mở rộng và tái sử dụng cho các dự án tương tự.