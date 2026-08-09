---
title: "Bản đề xuất"
date: 2026-07-01
weight: 2
chapter: false
pre: "<b>2. </b>"
---

# NeonFoodmap Website
## Tự động hóa quy trình CI/CD trên nền tảng AWS

### 1. Tổng quan dự án

NeonFoodMap là nền tảng website bản đồ ẩm thực, cho phép người dùng tìm kiếm, khám phá và đánh giá các địa điểm ăn uống theo thời gian thực. Hệ thống tích hợp các chức năng như tìm kiếm địa điểm (POI), định vị GPS, hiển thị lộ trình, đánh giá địa điểm và phát nội dung mô tả bằng công nghệ Text-to-Speech nhằm nâng cao trải nghiệm người dùng. Với đặc điểm xử lý dữ liệu theo thời gian thực và yêu cầu phục vụ nhiều người dùng đồng thời, hệ thống cần được triển khai trên một hạ tầng có khả năng mở rộng linh hoạt, đảm bảo tính sẵn sàng và dễ dàng bảo trì.

Đề xuất này trình bày giải pháp triển khai hệ thống NeonFoodMap trên nền tảng Amazon Web Services (AWS) theo kiến trúc Cloud-Native, đáp ứng các yêu cầu về khả năng mở rộng, tính sẵn sàng cao, bảo mật và tự động hóa quy trình phát hành phần mềm. Mục tiêu của giải pháp là xây dựng một hạ tầng triển khai có khả năng tái sử dụng, hỗ trợ triển khai lặp lại, đồng thời chuẩn hóa quy trình vận hành theo định hướng DevOps trong môi trường Production.

---


### 2. Phát biểu vấn đề
#### Hiện trạng

Trước khi triển khai đề xuất, dự án NeonFoodMap Website mới chỉ tồn tại ở dạng mã nguồn ứng dụng (Frontend và Backend) hoạt động đơn lẻ, chưa được chuẩn hóa quy trình triển khai hay tích hợp lên hạ tầng đám mây. Cụ thể:

* **Chưa có hạ tầng tự động hóa:** Quy trình build và deploy ứng dụng đang thực hiện thủ công, chưa thiết lập luồng CI/CD tự động hóa trên môi trường Production.
* **Chưa ứng dụng mô hình Container hóa:** Ứng dụng chưa được đóng gói chuẩn hóa dưới dạng Docker Image để vận hành nhất quán giữa các môi trường.
* **Hạ tầng AWS chưa được thiết lập:** Hệ thống mạng VPC, cơ sở dữ liệu phân tán, các chính sách bảo mật IAM tối ưu cũng như các cơ chế giám sát (Monitoring/Logging) trên nền tảng AWS chưa được xây dựng và cấu hình đồng bộ.

#### Mục tiêu

Đề xuất hướng tới các mục tiêu kỹ thuật sau:

- Tự động hóa quy trình Build, Test và Deploy.
- Loại bỏ việc sử dụng AWS Access Key trong GitHub thông qua OpenID Connect (OIDC).
- Chuẩn hóa quy trình triển khai ứng dụng theo mô hình Container.
- Đảm bảo tính sẵn sàng cao (High Availability) cho hệ thống.
- Hỗ trợ mở rộng tài nguyên linh hoạt theo nhu cầu tải.
- Thiết lập cơ chế giám sát, ghi log và cảnh báo tập trung.
- Chuẩn hóa quy trình triển khai theo mô hình DevOps và nâng cao khả năng tái sử dụng.

#### Giải pháp

- Thiết kế kiến trúc hạ tầng AWS.
- Xây dựng quy trình CI/CD.
- Triển khai Backend và Frontend bằng Amazon ECS Fargate.
- Quản lý Docker Image.
- Cấu hình cơ sở dữ liệu.
- Quản lý Static Assets.
- Xây dựng hệ thống Logging và Monitoring.
- Hoàn thiện tài liệu triển khai theo từng Sprint.

#### Lợi tức đầu tư
Việc chuẩn hóa và tự động hóa hệ thống mang lại những giá trị thiết thực:

- Tối ưu hóa chi phí vận hành (Cost Efficiency): Mô hình Serverless (ECS Fargate) và Serverless Storage giúp chỉ chi trả theo tài nguyên thực tế sử dụng, giảm thiểu lãng phí hạ tầng idle (nhàn rỗi).

- Tăng tốc độ phát triển (Time-to-Market): Quy trình CI/CD tự động giúp giảm thời gian release tính năng mới từ vài giờ/ngày xuống chỉ còn vài phút.

- Độ ổn định và sẵn sàng cao (High Availability): Hạ tầng tự động phục hồi và cân bằng tải giúp hệ thống đạt uptime cao, hạn chế tối đa thời gian gián đoạn dịch vụ (Downtime).

- Bảo mật và kiểm soát tốt hơn: Các tiêu chuẩn bảo mật của AWS kết hợp hệ thống giám sát chủ động giúp bảo vệ dữ liệu khách hàng và phát hiện sớm các lỗ hổng tiềm ẩn.


---


### 3. Sơ đồ kiến trúc 

#### Kiến trúc tổng thể
![Sơ đồ kiến trúc 1](/images/2-Proposal/diagram1.png)

Kiến trúc triển khai được xây dựng trên hai Availability Zone để cải thiện tính sẵn sàng:

- **Phân phối frontend:** Nội dung tĩnh được lưu trên Amazon S3 Static Website và phân phối qua Amazon CloudFront để tăng tốc độ truy cập cho người dùng.
- **Xử lý API:** CloudFront chuyển các yêu cầu API đến ALB. ALB định tuyến lưu lượng đến các ECS Fargate task trong private subnet và Auto Scaling Group phân bổ trên hai Availability Zone.
- **Cơ sở dữ liệu:** Amazon RDS MySQL triển khai Multi-AZ, gồm primary database và standby database đồng bộ nhằm nâng cao khả năng chịu lỗi.
- **CI/CD:** Đẩy mã nguồn lên GitHub. GitHub Actions dùng OIDC để xác thực với AWS STS, build container image và push lên Amazon ECR; ECS sau đó pull image và triển khai phiên bản mới.
- **Bảo mật và hạ tầng:** AWS IAM quản lý quyền truy cập, AWS Secrets Manager lưu trữ thông tin nhạy cảm, và AWS CloudFormation chuẩn hóa việc cung cấp và thay đổi hạ tầng.
- **Quan sát hệ thống:** Amazon CloudWatch thu thập log và metric; log có thể được lưu trữ lâu dài trên S3. Amazon SNS gửi cảnh báo đến email.

#### Kiến trúc kết nối dịch vụ
![Sơ đồ kiến trúc 2](/images/2-Proposal/diagram2.png)

- Người dùng truy cập ứng dụng qua Internet Gateway và Application Load Balancer (ALB).
- Frontend và backend được đóng gói thành container, vận hành bằng Amazon ECS Fargate trong ECS Cluster.
- AWS Cloud Map được sử dụng để quản lý Service Discovery giữa các Container trong ECS Cluster, giúp các dịch vụ giao tiếp nội bộ mà không cần phải cập nhật lại IP khi cập nhật Task Revision mới.

#### Các thành phần kiến trúc

| Dịch vụ AWS | Loại hình Dịch vụ | Vai trò & Chức năng trong Hệ thống |
| --- | --- | --- |
| **AWS IAM** | Identity & Access Management | Quản lý người dùng, nhóm, vai trò (Roles) và chính sách bảo mật, bắt buộc bật chính sách Force MFA cho tất cả tài khoản. |
| **VPC** | Networking | Cung cấp mạng riêng ảo (Virtual Private Cloud) với các dải CIDR blocks, Subnets công cộng và riêng tư, Route Tables, Internet Gateway và NAT Gateways. |
| **Amazon RDS** | Relational Database | Củng cố cơ sở dữ liệu quan hệ (RDS MySQL Multi-AZ) để lưu trữ và quản lý dữ liệu ứng dụng. |
| **Amazon S3** | Object Storage | Lưu trữ tệp tin với các bucket chuyên biệt (frontend, media, audio, logs), hỗ trợ cấu hình phiên bản (versioning), chính sách vòng đời (lifecycle) và mã hóa. |
| **Amazon ECR** | Container Registry | Kho lưu trữ các Docker Container Images cho cả Frontend và Backend. |
| **Amazon ECS** | Container Orchestration | Quản lý cụm cụm máy chủ ảo (Cluster) chạy ứng dụng theo dạng Fargate launch type. |
| **Application Load Balancer** | Load Balancing | Phân phối lưu lượng truy cập HTTP/HTTPS internet vào các target groups và hỗ trợ cấu hình chuyển hướng, health checks. |
| **Amazon CloudWatch** | Monitoring & Observability | Thu thập log (CloudWatch Logs), theo dõi metrics và thiết lập các dashboard, báo động (alarms). |
| **Amazon SNS** | Push Notification Service | Gửi thông báo cảnh báo (ví dụ: billing alerts cho chi phí) tới quản trị viên. |
| **AWS CloudFront** | Content Delivery Network (CDN) | Phân phối nội dung từ S3 và chuyển API request đến ALB |

#### AWS Well-Architected Framework

| Trụ cột | Giải pháp áp dụng |
| --- | --- |
| Operational Excellence | GitHub Actions CI/CD, CloudFormation, CloudWatch. |
| Security | IAM Least Privilege, Secrets Manager, KMS, Private Subnets |
| Reliability | Application Load Balancer, ECS Auto Scaling, RDS Multi-AZ, VPC Endpoint for S3. |
| Performance Efficiency | CloudFront, ECS Fargate AutoScaling, RDS Optimization. |
| Cost Optimization | ECS Fargate Auto Scaling, S3 Lifecycle. |
| Sustainability | Scale theo nhu cầu, tắt môi trường dev ngoài giờ |

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

| Dịch vụ | Cấu hình theo kiến trúc hiện tại | Chi phí/tháng | Chi phí ước tính tối đa/tháng |
| --- | --- | --- | --- |
| Amazon ECS (Fargate) | Chạy backend container trên ECS; Production dùng 2 tasks trên 2 AZ với Auto Scaling, ví dụ 0,5 vCPU và 1 GB RAM cho mỗi task. | $9,86 | ~$20–35 |
| Amazon RDS MySQL | Production dùng Multi-AZ: Primary ở AZ A và Standby ở AZ B. | $11,78 | ~$50 |
| NAT Gateway, ALB và Amazon VPC | Hai NAT Gateway và ALB cho Production; dashboard hiện gộp một phần chi phí vào [EC2 – Other](https://thuy0an.github.io/aws-learning-journey-fcaj/vi/2-proposal/) và [VPC](https://thuy0an.github.io/aws-learning-journey-fcaj/vi/2-proposal/) . | $32,80 | ~$82–84 |
| Amazon CloudFront | Phân phối static web từ S3 và định tuyến API; giả định khoảng 100 GB truyền dữ liệu. | $0.00 (Free Tier cho 1 TB) | $0.00 (Free Tier cho 1 TB) |
| Amazon S3 | Lưu static web, media và logs; giả định khoảng 50 GB. | ~$2 | ~$2 |
| Amazon CloudWatch và SNS | Flow Logs, container logs, metrics, alarms và gửi email cảnh báo. | $5,61 | ~$5–6 |
| AWS Secrets Manager và Amazon ECR | Lưu secrets và container images. | ~$2 | ~$3 |
| Tổng chi phí/tháng | | $64,05 | ~$166–184 |

Ngoài ra đã áp dụng một số biện pháp tối ưu chi phí khác như:
- Cấu hình **AWS Budgets** và cảnh báo qua SNS ở các ngưỡng 50%, 80% và 100% ngân sách tháng.
- Theo dõi chi phí NAT Gateway, ECS Fargate, RDS và CloudWatch là các nhóm chi phí chính.
- Chỉ duy trì số lượng ECS task cần thiết; sử dụng Auto Scaling để tránh cấp phát tài nguyên nhàn rỗi.
- Xóa hoặc dừng các tài nguyên không còn sử dụng trong môi trường staging sau khi hoàn tất kiểm thử.
- Sử dụng CloudFront cache cho static web và media để giảm lưu lượng đến origin; cân nhắc S3 Lifecycle khi dung lượng log hoặc media tăng lên.

---

### 6. Đánh giá rủi ro
#### Ma trận rủi ro
| Rủi ro | Khả năng | Ảnh hưởng |
| --- | --- | --- |
| Chi phí AWS vượt dự báo | Trung bình | Trung bình |
| ECS task hoặc container gặp lỗi | Trung bình | Trung bình |
| Sự cố cơ sở dữ liệu | Thấp | Cao |
| Lộ thông tin nhạy cảm | Thấp | Rất cao |
| Lưu lượng tăng đột biến | Trung bình | Trung bình |
| Log hoặc cảnh báo không đầy đủ | Trung bình | Trung bình |
| Lỗi trong quá trình triển khai phiên bản mới | Trung bình | Trung bình |

#### Kế hoạch dự phòng và ứng phó
- Xử lý cảnh báo chi phí ngay khi chạm ngưỡng ngân sách; xác định dịch vụ phát sinh và dừng hoặc điều chỉnh tài nguyên không cần thiết.
- Khi API hoặc container lỗi, kiểm tra CloudWatch Logs, trạng thái ALB health check và ECS task definition trước khi rollback hoặc triển khai bản sửa.
- Khi có sự cố dữ liệu, ưu tiên bảo vệ dữ liệu, đánh giá ảnh hưởng và thực hiện khôi phục theo quy trình backup/restore đã kiểm thử.
- Khi phát hiện dấu hiệu lộ thông tin xác thực, thu hồi hoặc xoay vòng secret, kiểm tra IAM permissions và rà soát lịch sử triển khai.


### 7. Kết quả mong đợi


Sau khi hoàn thành quá trình triển khai, hệ thống dự kiến đạt được các kết quả sau:

- **Cải tiến kỹ thuật:** Số hóa việc thuyết minh và quản lý POI, thay thế quy trình cung cấp thông tin thủ công bằng nền tảng đa phương tiện có thể giám sát, mở rộng và triển khai tự động trên AWS.
- **Giá trị dài hạn:** Hình thành nền tảng nội dung và dữ liệu có thể tái sử dụng cho các khu vực du lịch khác; đồng thời tạo cơ sở để mở rộng phân tích hành vi người dùng, nội dung đa ngôn ngữ và hợp tác với các hộ kinh doanh địa phương trong tương lai.

### 8. Tài liệu tham khảo

[1]: [AWS Well-Architected Framework](https://aws.amazon.com/vi/architecture/well-architected/)

[2]: [The First Cloud Journey](https://cloudjourney.awsstudygroup.com/)

[3]: [AWS Documentation](https://docs.aws.amazon.com/)
