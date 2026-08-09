---
title: "Giới thiệu"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# Bài toán Cloud & DevOps: Đưa hệ thống lên AWS Cloud, xây dựng luồng CI/CD tự động

Nhằm đáp ứng yêu cầu về khả năng triển khai linh hoạt, tự động hóa quy trình phát hành phần mềm và nâng cao tính sẵn sàng của hệ thống, đề tài thực hiện triển khai ứng dụng trên nền tảng Amazon Web Services (AWS) và xây dựng quy trình CI/CD (Continuous Integration/Continuous Deployment) theo mô hình tự động hóa hoàn toàn. Kiến trúc Cloud & DevOps của hệ thống được tổ chức thành ba luồng xử lý chính, bao gồm: luồng CI/CD, luồng xử lý yêu cầu người dùng và luồng giám sát – quản lý chi phí.

## Luồng CI/CD (Pipeline Flow)

Quy trình tích hợp và triển khai liên tục được thiết kế nhằm: tự động hóa toàn bộ quá trình từ khi lập trình viên cập nhật mã nguồn đến khi phiên bản mới của ứng dụng được triển khai trên môi trường vận hành. Cụ thể, khi Developer thực hiện thao tác push code lên kho mã nguồn GitHub, GitHub Actions sẽ được kích hoạt để khởi chạy pipeline. Pipeline sử dụng cơ chế xác thực OIDC (OpenID Connect) để liên kết với AWS Security Token Service (STS) và cấp phát thông tin xác thực tạm thời (temporary credentials), thay thế cho việc sử dụng Access Key/Secret Key tĩnh nhằm mục đích tăng cường mức độ an toàn bảo mật. Sau khi xác thực thành công, pipeline tiến hành build Docker image của ứng dụng và đẩy image lên Amazon Elastic Container Registry (ECR). Tiếp theo, dịch vụ Amazon ECS Fargate sẽ tự động pull image mới nhất từ ECR và thực hiện triển khai phiên bản mới của ứng dụng mà không cần can thiệp thủ công. 

- Luồng xử lý:
Developer → GitHub → GitHub Actions → OIDC Authentication → AWS STS → Amazon ECR → Amazon ECS Fargate.

## Luồng User (Request Flow)

Luồng xử lý yêu cầu được xây dựng theo mô hình phân phối nội dung và cân bằng tải nhằm đảm bảo hiệu năng truy cập và khả năng chịu lỗi của hệ thống.
Khi người dùng (User) gửi yêu cầu truy cập, yêu cầu trước tiên được tiếp nhận bởi Amazon CloudFront – mạng phân phối nội dung (CDN) của AWS. Đối với các tài nguyên tĩnh của giao diện người dùng, CloudFront sẽ truy xuất trực tiếp từ Amazon S3 Frontend Bucket. Đối với các yêu cầu động, CloudFront chuyển tiếp yêu cầu đến Application Load Balancer (ALB). ALB có nhiệm vụ phân phối lưu lượng truy cập đến các tác vụ ECS Fargate đang chạy trên hai Availability Zone (AZ) khác nhau nhằm đảm bảo tính sẵn sàng cao (High Availability). Các container ứng dụng kết nối đến Amazon RDS Primary đặt tại Zone A để thực hiện thao tác đọc/ghi dữ liệu. Đồng thời, dữ liệu được đồng bộ liên tục sang RDS Standby tại Zone B theo cơ chế Multi-AZ, giúp hệ thống có khả năng chuyển đổi dự phòng khi xảy ra sự cố.
Ngoài ra, các container ECS truy cập S3 Media Bucket thông qua VPC Endpoint, cho phép truyền dữ liệu nội bộ trong mạng AWS mà không cần đi qua Internet công cộng, từ đó tăng cường bảo mật và tối ưu chi phí truyền tải dữ liệu. 

- Luồng xử lý:
User → CloudFront → (S3 Frontend hoặc ALB) → ECS Fargate (2 AZ) → RDS Primary (Zone A) → RDS Standby (Zone B)

## Luồng Giám sát & Chi phí (Observability & Billing)

Để đảm bảo hệ thống vận hành ổn định và kiểm soát hiệu quả chi phí sử dụng tài nguyên đám mây, đề tài triển khai cơ chế giám sát và cảnh báo tự động dựa trên các dịch vụ quản trị của AWS. Toàn bộ nhật ký hệ thống (logs), bao gồm VPC Flow Logs và Application Logs, được thu thập và tập trung tại Amazon CloudWatch để phục vụ việc theo dõi, phân tích và xử lý sự cố. Hệ thống đồng thời được cấu hình Auto Scaling cho dịch vụ ECS, cho phép tự động mở rộng số lượng tác vụ khi mức sử dụng CPU hoặc Memory vượt quá 70%, qua đó duy trì hiệu năng trong điều kiện tải tăng cao.
Về quản lý chi phí, hệ thống sử dụng AWS Budgets với ngân sách được thiết lập ở mức 15 USD/tháng. Các ngưỡng cảnh báo được cấu hình tại 50%, 70% và 90% ngân sách. Khi chi phí đạt đến các ngưỡng này hoặc xuất hiện dấu hiệu gia tăng chi phí bất thường, AWS Budgets sẽ tự động kích hoạt Amazon SNS (Simple Notification Service) để gửi email cảnh báo đến quản trị viên, hỗ trợ phát hiện sớm rủi ro và đưa ra biện pháp xử lý kịp thời. 

- Luồng giám sát và cảnh báo:
CloudWatch (Logs & Metrics) → Auto Scaling → AWS Budgets → Amazon SNS → Email Alert

## Kiến trúc hệ thống

### Kiến trúc tổng thể

![](/images/2-Proposal/diagram1.png)

Kiến trúc hệ thống được chia thành năm lớp chính:

#### CI/CD Layer

Bao gồm:

- GitHub Repository
- GitHub Actions
- Docker Build
- AWS STS
- Amazon ECR

Đây là lớp chịu trách nhiệm tự động hóa toàn bộ quy trình triển khai. Sau mỗi lần Push lên nhánh chính:

1. Source Code được Build.
2. Docker Image được tạo.
3. GitHub sử dụng OIDC để xác thực.
4. AWS STS cấp Temporary Credential.
5. Image được Push lên Amazon ECR.
6. ECS Service thực hiện Rolling Deployment.


#### Presentation Layer

Bao gồm:

- Amazon CloudFront
- Amazon S3 Static Website

Frontend được lưu trữ trên Amazon S3 và phân phối thông qua CloudFront nhằm:

- Giảm độ trễ
- Tăng tốc truy cập
- Giảm tải Backend


#### Application Layer

Bao gồm:

- Application Load Balancer
- Amazon ECS Cluster
- Backend Service
- Frontend Service

Ứng dụng được triển khai trên ECS Fargate giúp loại bỏ việc quản lý EC2. Các Service hoạt động độc lập giúp:

- Mở rộng riêng từng thành phần
- Rolling Update
- Tự động khởi động lại khi lỗi


#### Data Layer

Bao gồm:

- Amazon RDS MySQL
- Multi-AZ Deployment

Database được triển khai trong Private Database Subnet.

Việc sử dụng Multi-AZ giúp:

- Tăng khả năng chịu lỗi
- Tự động Failover
- Giảm thời gian gián đoạn dịch vụ


#### Monitoring Layer

Bao gồm:

- Amazon CloudWatch
- Amazon SNS

CloudWatch thu thập:

- ECS Logs
- Container Logs
- Metrics
- Application Logs

SNS chịu trách nhiệm gửi Email Alert khi phát hiện bất thường.


### Kiến trúc triển khai ECS

![](/images/2-Proposal/diagram2.png)

Hệ thống sử dụng một ECS Cluster gồm hai dịch vụ:

#### Backend Service

Triển khai:

- Django REST API
- Docker Container
- ECS Fargate

Backend chịu trách nhiệm:

- Authentication
- Business Logic
- Database Access
- Upload Media


#### Frontend Service

Triển khai:

- React Application
- Docker Container
- ECS Fargate

Frontend giao tiếp với Backend thông qua ALB.


#### Service Discovery

AWS Cloud Map được sử dụng để quản lý Service Discovery giữa các Container trong ECS Cluster.


#### Load Balancing

Application Load Balancer tiếp nhận HTTP Request/HTTPS Request, sau đó định tuyến đến Backend Service.


## Thành phần AWS sử dụng

| Dịch vụ                 | Mục đích                                                                                                                                                                                                                                                         |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IAM                     | Quản lý danh tính (Identity) và phân quyền truy cập tài nguyên AWS. Tạo User, Group, Role và Policy để kiểm soát người dùng hoặc dịch vụ nào được phép thực hiện các thao tác cụ thể theo nguyên tắc **Least Privilege**.                                        |
| STS                     | Cấp **Temporary Security Credentials** (Access Key, Secret Key, Session Token) có thời hạn ngắn cho người dùng hoặc ứng dụng. Được sử dụng trong GitHub Actions (OIDC), Assume Role và các kịch bản truy cập AWS an toàn mà không cần lưu khóa truy cập dài hạn. |
| VPC                     | Xây dựng mạng riêng ảo trên AWS, cho phép định nghĩa dải địa chỉ IP, bảng định tuyến, ACL và Security Group để cô lập và bảo vệ hạ tầng triển khai ứng dụng.                                                                                                     |
| Public / Private Subnet | Phân chia hạ tầng thành các vùng mạng có mức độ truy cập khác nhau. Public Subnet chứa các tài nguyên cần truy cập từ Internet (ALB, NAT Gateway), trong khi Private Subnet chứa các tài nguyên nội bộ (ECS, RDS) nhằm tăng cường bảo mật.                       |
| NAT Gateway             | Cho phép các tài nguyên trong Private Subnet truy cập Internet theo chiều ra (Outbound) để tải package, cập nhật hệ thống hoặc pull Docker Image mà không cho phép kết nối trực tiếp từ Internet vào.                                                            |
| Internet Gateway        | Kết nối VPC với Internet, cho phép các tài nguyên trong Public Subnet gửi và nhận lưu lượng mạng từ bên ngoài, phục vụ việc truy cập website và các dịch vụ công khai.                                                                                           |
| ECS Fargate             | Nền tảng chạy container không cần quản lý máy chủ. Tự động cung cấp hạ tầng, triển khai và mở rộng ứng dụng container dựa trên Docker, giúp giảm chi phí vận hành và quản trị.                                                                                   |
| ECR                     | Kho lưu trữ Docker Image riêng trên AWS. Lưu trữ, quản lý phiên bản và cung cấp Image cho ECS khi triển khai hoặc cập nhật ứng dụng thông qua quy trình CI/CD.                                                                                                   |
| RDS MySQL               | Dịch vụ cơ sở dữ liệu quan hệ được quản lý hoàn toàn. Lưu trữ dữ liệu ứng dụng, hỗ trợ sao lưu tự động, Multi-AZ, giám sát và khả năng mở rộng nhằm đảm bảo tính sẵn sàng và độ tin cậy của hệ thống.                                                            |
| S3                      | Dịch vụ lưu trữ đối tượng dùng để lưu trữ website tĩnh (React), hình ảnh, tệp người dùng, log hệ thống, file sao lưu và các tài nguyên tĩnh khác với độ bền và khả năng mở rộng cao.                                                                             |
| CloudFront              | Mạng phân phối nội dung (CDN) giúp phân phối nội dung từ S3 hoặc ALB thông qua các Edge Location trên toàn cầu, giảm độ trễ, tăng tốc độ tải và hỗ trợ HTTPS, cache cũng như bảo vệ ứng dụng.                                                                    |
| ALB                     | Cân bằng tải HTTP/HTTPS cho nhiều container hoặc máy chủ. Thực hiện định tuyến yêu cầu theo URL, Host Header hoặc Path đến các ECS Service tương ứng, đồng thời tích hợp Health Check và SSL/TLS.                                                                |
| CloudWatch              | Giám sát tài nguyên AWS và ứng dụng thông qua Metrics, Logs, Dashboard và Alarm. Thu thập log từ ECS, theo dõi hiệu năng hệ thống, phát hiện sự cố và hỗ trợ phân tích, khắc phục lỗi.                                                                           |
| SNS                     | Dịch vụ gửi thông báo theo mô hình Publish/Subscribe. Tự động gửi Email, SMS hoặc kích hoạt các dịch vụ khác khi CloudWatch Alarm hoặc các sự kiện AWS được kích hoạt.                                                                                           |
| Secrets Manager         | Lưu trữ và quản lý an toàn các thông tin nhạy cảm như mật khẩu cơ sở dữ liệu, API Key và Access Token. Hỗ trợ mã hóa bằng KMS, kiểm soát quyền truy cập và tự động xoay vòng (Secret Rotation) để tăng cường bảo mật.                                            |