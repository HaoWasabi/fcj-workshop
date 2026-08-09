---
title: "Event 3"
date: 2026-08-01
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# Bài thu hoạch “Agent Forge - Deepdive”

### Mục Đích Của Sự Kiện

Workshop được tổ chức nhằm giúp người tham gia:

- Hiểu các khái niệm nền tảng về **Agentic AI** và AI Agent.
- Tìm hiểu cách xây dựng và triển khai AI Agent cho môi trường **production** bằng **Amazon Bedrock AgentCore**.
- Nắm vững kiến trúc, vòng đời và các thành phần của một hệ thống AI Agent.
- Thực hành sử dụng các công cụ, quy trình bảo mật và kỹ thuật cần thiết khi phát triển AI Agent.
- Khám phá các ứng dụng thực tế của AI Agent trong tự động hóa quy trình và giải quyết các bài toán doanh nghiệp.

### Danh Sách Diễn Giả

- **Nghia Tran** - Agentic AI Solution Architect
- **Anh Pham** - Cloud Consultant G-AsiaPacific Vietnam

### Định Dạng Workshop

Đây là **chuỗi workshop kéo dài 3 ngày**, được thiết kế theo lộ trình từ kiến thức nền tảng đến triển khai AI Agent trong môi trường production bằng Amazon Bedrock AgentCore.

- **Ngày 1 (01/08): AgentCore Foundations**  
  Tìm hiểu kiến trúc tổng quan của Amazon Bedrock AgentCore, bao gồm **Runtime**, **Gateway** và **Identity**, cùng các khái niệm nền tảng để xây dựng AI Agent.

- **Ngày 2 (08/08): Memory, Evaluations, Observability & Optimization**  
  Khám phá cách quản lý **Memory**, đánh giá hiệu quả của AI Agent (**Evaluations**), giám sát hệ thống (**Observability**) và tối ưu hiệu suất (**Optimization**).

- **Ngày 3 (15/08): DevOps, Policies & Production Best Practices**  
  Tìm hiểu quy trình **DevOps** cho AI Agent, xây dựng **Policies**, áp dụng các biện pháp bảo mật và các **best practices** để triển khai AI Agent trong môi trường production.

## Nội Dung Nổi Bật

### 1. Tổng Quan về Agentic AI

#### Agentic AI là gì?

**Agentic AI** là các hệ thống trí tuệ nhân tạo có khả năng **tự chủ thực hiện một mục tiêu** thay vì chỉ phản hồi từng câu lệnh của người dùng. Sau khi nhận được mục tiêu, Agentic AI có thể tự lập kế hoạch, lựa chọn công cụ, thực hiện các bước cần thiết và đánh giá kết quả để hoàn thành nhiệm vụ.

Các đặc điểm chính của Agentic AI:

- **Lập kế hoạch**: Chia nhỏ các tác vụ phức tạp thành các bước hành động cụ thể.
- **Ra quyết định**: Chọn hành động phù hợp dựa trên ngữ cảnh và mục tiêu.
- **Sử dụng công cụ**: Gọi API, tìm kiếm thông tin, truy cập cơ sở dữ liệu hoặc sử dụng các dịch vụ bên ngoài.
- **Tự thực thi**: Thực hiện nhiều bước liên tiếp với sự can thiệp tối thiểu của con người.
- **Đánh giá và điều chỉnh**: Kiểm tra kết quả sau mỗi bước và thay đổi kế hoạch khi cần thiết.

**Ví dụ:** Người dùng yêu cầu *"Triển khai ứng dụng lên AWS"*. Thay vì chỉ hướng dẫn từng bước, Agentic AI có thể tự build ứng dụng, tạo Docker image, đẩy image lên container registry, triển khai lên dịch vụ cloud, kiểm tra trạng thái hệ thống và báo cáo kết quả cuối cùng.

#### Các Cấp Độ Tự Chủ

Workshop phân loại các AI agent theo phổ tự chủ:

1. **Deterministic agents**: Hoạt động theo quy tắc cố định.  
   *Ví dụ:* Tự động format mã nguồn hoặc chạy workflow CI theo cấu hình có sẵn.

2. **Reactive agents**: Phản ứng với đầu vào mà không lập kế hoạch trước.  
   *Ví dụ:* GitHub Copilot sinh đoạn mã khi lập trình viên nhập yêu cầu.

3. **Goal-oriented agents**: Lập kế hoạch để đạt mục tiêu.  
   *Ví dụ:* AI nhận yêu cầu "Thêm chức năng thanh toán" rồi tự phân tích, viết mã, tạo API và kiểm thử.

4. **Learning agents**: Học từ kinh nghiệm và cải thiện.  
   *Ví dụ:* AI ghi nhớ các lỗi triển khai trước đây để lựa chọn cách khắc phục hiệu quả hơn ở các lần sau.

5. **Multi-agent systems**: Nhiều agent phối hợp với nhau.  
   *Ví dụ:* Coding Agent, Testing Agent, Security Agent và DevOps Agent cùng phối hợp để hoàn thành một dự án phần mềm.### 2. Amazon Bedrock AgentCore

#### Tổng Quan

**Amazon Bedrock AgentCore** là dịch vụ của AWS hỗ trợ xây dựng, triển khai và vận hành các AI Agent trong môi trường production. Dịch vụ cung cấp hạ tầng được quản lý hoàn toàn, giúp nhà phát triển tập trung vào logic của agent thay vì quản lý máy chủ hay hạ tầng.

Một số khả năng nổi bật của AgentCore:

- **Serverless Runtime**: Cung cấp môi trường thực thi mà không cần quản lý hạ tầng.
- **Tự động mở rộng**: Tự động mở rộng hoặc thu hẹp tài nguyên theo lưu lượng truy cập.
- **Bảo mật tích hợp**: Hỗ trợ xác thực, phân quyền và tích hợp với các dịch vụ bảo mật của AWS.
- **Quản lý vòng đời AI Agent**: Hỗ trợ quá trình phát triển, kiểm thử, triển khai và vận hành trong môi trường production.

#### Lợi Ích

- **Giảm chi phí vận hành** nhờ kiến trúc serverless và hạ tầng được AWS quản lý.
- **Tăng khả năng mở rộng** để đáp ứng khối lượng yêu cầu thay đổi theo thời gian.
- **Đảm bảo bảo mật** với các cơ chế xác thực và phân quyền tích hợp sẵn.
- **Thanh toán theo mức sử dụng**, chỉ trả chi phí cho tài nguyên hoặc lượt thực thi thực tế.
- **Rút ngắn thời gian phát triển** nhờ quy trình triển khai và kiểm thử nhanh chóng.### 3. Môi Trường Runtime

#### Mô Hình Thực Thi Agent

**Amazon Bedrock AgentCore Runtime** cung cấp môi trường thực thi (runtime) được quản lý hoàn toàn để chạy AI Agent trong môi trường production.

Các đặc điểm chính:

- **Serverless Runtime**: Agent được khởi chạy theo yêu cầu, không cần quản lý hoặc cấp phát máy chủ.
- **Firecracker MicroVM**: Mỗi lần thực thi agent diễn ra trong một **Firecracker MicroVM** được cô lập, giúp tăng cường bảo mật và đảm bảo môi trường thực thi nhất quán.
- **Tự động mở rộng**: Runtime tự động mở rộng hoặc thu hẹp tài nguyên theo số lượng yêu cầu.
- **Quản lý phiên làm việc**: Hỗ trợ duy trì trạng thái của agent trong suốt quá trình xử lý.

#### Quản Lý Bộ Nhớ

Runtime hỗ trợ nhiều cơ chế quản lý bộ nhớ để giúp AI Agent duy trì ngữ cảnh và thực hiện các tác vụ nhiều bước:

- **Session Memory**: Lưu trữ ngữ cảnh trong một phiên làm việc hoặc cuộc hội thoại.
- **Long-term Memory**: Lưu trữ thông tin lâu dài để tái sử dụng ở các phiên sau.
- **Context Management**: Quản lý và tối ưu lượng ngữ cảnh được truyền đến mô hình ngôn ngữ.

#### Xử Lý Dữ Liệu Streaming

AgentCore Runtime hỗ trợ phản hồi theo thời gian thực nhằm cải thiện trải nghiệm người dùng:

- **Streaming Response**: Trả kết quả từng phần ngay khi được tạo ra thay vì chờ hoàn thành toàn bộ.
- **Progress Updates**: Hiển thị trạng thái hoặc các bước xử lý của agent trong quá trình thực thi.
- **Giảm độ trễ cảm nhận**: Người dùng nhận được phản hồi sớm hơn đối với các tác vụ có thời gian xử lý dài.### 4. Danh Tính & Bảo Mật

#### Xác Thực & Phân Quyền

Amazon Bedrock AgentCore cung cấp các cơ chế xác thực và phân quyền giúp AI Agent truy cập tài nguyên một cách an toàn.

- **JSON Web Token (JWT)**: Xác thực người dùng hoặc ứng dụng bằng token.
- **Amazon Cognito**: Quản lý danh tính và xác thực người dùng.
- **AWS IAM**: Kiểm soát quyền truy cập của AI Agent đến các tài nguyên AWS theo nguyên tắc **least privilege**.
- **Service-to-Service Authentication**: Xác thực an toàn khi AI Agent giao tiếp với các dịch vụ hoặc API khác.

#### Bảo Mật Khi Gọi Công Cụ

Khi AI Agent sử dụng các công cụ hoặc API bên ngoài:

- Chỉ được cấp quyền truy cập đến các tài nguyên cần thiết (**Least Privilege**).
- Mỗi công cụ hoặc API có thể được áp dụng chính sách phân quyền riêng.
- Các hoạt động được ghi lại thông qua **AWS CloudTrail** để phục vụ kiểm tra và giám sát.
- Dữ liệu được mã hóa trong quá trình truyền bằng **HTTPS/TLS**.

#### Thực Hành Bảo Mật

Để triển khai AI Agent trong môi trường production, AWS khuyến nghị:

- Triển khai trong **Amazon VPC** khi cần cô lập mạng.
- Lưu trữ khóa API và thông tin nhạy cảm bằng **AWS Secrets Manager**.
- Áp dụng nguyên tắc **Least Privilege** cho mọi IAM Role và Policy.
- Theo dõi hoạt động bằng **Amazon CloudWatch** và **AWS CloudTrail** để phát hiện sự cố và hỗ trợ kiểm toán.### 5. Gateway & Middleware

#### AgentCore Gateway

**Amazon Bedrock AgentCore Gateway** là lớp trung gian giúp AI Agent kết nối với các công cụ, API và dịch vụ bên ngoài một cách an toàn và nhất quán.

Các chức năng chính:

- **Định tuyến yêu cầu**: Chuyển yêu cầu của AI Agent đến đúng API hoặc dịch vụ.
- **Quản lý API**: Hỗ trợ kết nối với nhiều loại dịch vụ và giao thức khác nhau.
- **Xác thực và phân quyền**: Kiểm soát quyền truy cập trước khi AI Agent gọi công cụ.
- **Giám sát**: Ghi nhận và theo dõi các yêu cầu giữa AI Agent và hệ thống bên ngoài.

#### Human-in-the-Loop (HITL)

Đối với các tác vụ quan trọng, AI Agent có thể yêu cầu sự phê duyệt của con người trước khi tiếp tục.

Ví dụ:

- Phê duyệt giao dịch tài chính.
- Xác nhận gửi email hoặc thông báo hàng loạt.
- Kiểm tra nội dung trước khi công khai.

#### Middleware

Middleware giúp AI Agent giao tiếp hiệu quả với các hệ thống khác.
Các chức năng phổ biến

- **Chuyển đổi dữ liệu** giữa AI Agent và API.
- **Caching** để giảm số lần gọi API và cải thiện hiệu suất.
- **Retry và Error Handling** khi dịch vụ bên ngoài gặp lỗi tạm thời.
- **Logging và Monitoring** để hỗ trợ giám sát và khắc phục sự cố.

### 6. Thực Hành

#### 6.1. Làm Quen Với Kiro IDE

Trong phần đầu của workshop, người tham gia thực hành cài đặt và cấu hình môi trường phát triển với **Kiro**. Đồng thời, tìm hiểu các tính năng hỗ trợ lập trình bằng AI và sử dụng **Steering** để định hướng cách AI sinh mã theo yêu cầu của dự án.

Các nội dung thực hành gồm:

- Cài đặt và thiết lập môi trường Kiro.
- Khám phá các tính năng hỗ trợ lập trình bằng AI.
- Thiết lập Steering để định hướng quá trình sinh mã.
- Thực hành tạo, chỉnh sửa và giải thích mã nguồn với sự hỗ trợ của AI.

#### 6.2. Thực Hành Xây Dựng Và Triển Khai AI Agent

Người tham gia thực hành theo hướng dẫn của workshop để khởi tạo và triển khai một AI Agent bằng **Amazon Bedrock AgentCore CLI**, đồng thời tìm hiểu quy trình triển khai Agent lên môi trường thực thi.

Các nội dung thực hành gồm:

- Khởi tạo dự án AI Agent bằng AgentCore CLI.
- Thực hành triển khai Agent lên Amazon Bedrock AgentCore.
- Kiểm thử quá trình tiếp nhận và xử lý yêu cầu của Agent.
- Quan sát quy trình hoạt động của Agent sau khi triển khai.

#### 6.3. Thực Hành Returns & Refunds Agent

Workshop hướng dẫn người tham gia xây dựng một AI Agent xử lý các yêu cầu hoàn trả và hoàn tiền nhằm minh họa quy trình giải quyết một bài toán nghiệp vụ.

Các nội dung thực hành gồm:

- Thực hành xây dựng Returns & Refunds Agent theo hướng dẫn.
- Tìm hiểu workflow xử lý yêu cầu hoàn trả.
- Kiểm thử quá trình hội thoại giữa người dùng và AI Agent.
- Quan sát cách Agent sử dụng các công cụ để xử lý yêu cầu.

#### 6.4. Thực Hành Persistent Memory

Người tham gia thực hành cấu hình **Persistent Memory** để AI Agent có thể lưu trữ và sử dụng lại thông tin giữa nhiều phiên làm việc.

Các nội dung thực hành gồm:

- Cấu hình Persistent Memory cho Agent.
- Lưu và truy xuất thông tin hội thoại.
- Kiểm tra khả năng ghi nhớ thông tin giữa các phiên làm việc.
- Quan sát ảnh hưởng của bộ nhớ đến chất lượng phản hồi của Agent.

#### 6.5. Thực Hành Kết Nối DynamoDB Và Knowledge Base

Workshop hướng dẫn kết nối AI Agent với các nguồn dữ liệu nhằm nâng cao khả năng truy xuất và trả lời thông tin.

Các nội dung thực hành gồm:

- Kết nối Agent với Amazon DynamoDB.
- Tích hợp Knowledge Base.
- Thực hành truy xuất dữ liệu phục vụ quá trình xử lý yêu cầu.
- Kiểm tra khả năng phản hồi của Agent dựa trên dữ liệu đã kết nối.

#### 6.6. Thực Hành Xây Dựng Giao Diện Web Chat

Người tham gia thực hành triển khai giao diện Web Chat để tương tác với AI Agent theo hướng dẫn của workshop.

Các nội dung thực hành gồm:

- Triển khai giao diện bằng Streamlit.
- Tích hợp Amazon Cognito để xác thực người dùng.
- Kết nối giao diện với AI Agent.
- Kiểm thử quá trình trao đổi giữa người dùng và AI Agent.

#### 6.7. Quan Sát Và Đánh Giá Hoạt Động Của Agent

Workshop giới thiệu các công cụ hỗ trợ theo dõi và đánh giá chất lượng hoạt động của AI Agent trên Amazon Bedrock AgentCore.

Các nội dung thực hành gồm:

- Quan sát Logs, Traces và GenAI Dashboard.
- Theo dõi quá trình xử lý yêu cầu của Agent.
- Thực hành sử dụng AgentCore Evaluations để đánh giá chất lượng phản hồi.
- Phân tích kết quả đánh giá để xác định các điểm cần cải thiện.

#### 6.8. Thực Hành Thiết Lập AgentCore Policies

Trong phần cuối của workshop, người tham gia thực hành cấu hình **AgentCore Policies** nhằm kiểm soát quyền truy cập của AI Agent đến các công cụ và nguồn dữ liệu.

Các nội dung thực hành gồm:

- Thiết lập AgentCore Policies.
- Cấu hình quyền truy cập của Agent đối với các công cụ.
- Kiểm tra hoạt động của Agent sau khi áp dụng chính sách.
- Tìm hiểu vai trò của chính sách bảo mật trong quá trình triển khai AI Agent.

## Những Gì Học Được

Sau khi tham gia workshop, tôi đã tiếp thu được nhiều kiến thức về Agentic AI và Amazon Bedrock AgentCore, bao gồm:

### Kiến Thức Chuyên Môn

- Hiểu khái niệm **Agentic AI** và sự khác biệt so với các ứng dụng AI truyền thống.
- Nắm được các cấp độ tự chủ của AI Agent, từ **Deterministic Agent** đến **Multi-Agent System**.
- Hiểu kiến trúc của **Amazon Bedrock AgentCore**, bao gồm Runtime, Gateway và Identity.
- Biết cách AI Agent lập kế hoạch, sử dụng công cụ và thực hiện nhiều bước để hoàn thành mục tiêu.
- Hiểu các cơ chế bảo mật như JWT, Amazon Cognito, IAM và nguyên tắc **Least Privilege**.

### Kiến Thức Triển Khai

- Hiểu quy trình xây dựng và triển khai AI Agent trong môi trường production.
- Biết cách tích hợp AI Agent với các API và công cụ bên ngoài.
- Tìm hiểu vai trò của Human-in-the-Loop trong các tác vụ yêu cầu sự phê duyệt của con người.
- Nắm được một số kỹ thuật Prompt Engineering và tối ưu workflow cho AI Agent.

### Bài Học Kinh Nghiệm

- Thiết kế AI Agent theo từng chức năng nhỏ trước khi xây dựng hệ thống phức tạp.
- Luôn ưu tiên bảo mật và phân quyền khi AI Agent truy cập tài nguyên.
- Theo dõi, đánh giá và tối ưu AI Agent dựa trên kết quả thực tế.
- Xây dựng AI Agent theo hướng dễ mở rộng và dễ bảo trì.

## Trải Nghiệm Trong Workshop

Tham gia **Ngày 1 của AWS FCAJ Agent Forge – Deep Dive** giúp tôi có cái nhìn tổng quan về cách xây dựng và vận hành AI Agent trong môi trường doanh nghiệp.

Thông qua phần trình bày của diễn giả và các nội dung minh họa, tôi hiểu rõ hơn về quy trình hoạt động của AI Agent, từ việc phân tích yêu cầu, lập kế hoạch, sử dụng công cụ đến hoàn thành mục tiêu. Workshop cũng giúp tôi tiếp cận kiến trúc **Amazon Bedrock AgentCore** và các thành phần quan trọng như Runtime, Gateway và Identity.

Bên cạnh phần lý thuyết, tôi được tìm hiểu các ví dụ thực tế về ứng dụng AI Agent trong tự động hóa quy trình, hỗ trợ khách hàng và phát triển phần mềm. Tôi cũng học được một số kỹ thuật Prompt Engineering, cách tối ưu workflow, cũng như các nguyên tắc bảo mật và triển khai AI Agent trong môi trường production.

Workshop mang lại nhiều kiến thức thực tiễn, giúp tôi hiểu rõ hơn về xu hướng phát triển của Agentic AI và tạo nền tảng để tiếp tục nghiên cứu các nội dung chuyên sâu trong những buổi workshop tiếp theo.

### Một số hình ảnh khi tham gia sự kiện
![Event Photo 1](/images/4-EventParticipated/image003.jpg)

![Event Photo 2](/images/4-EventParticipated/Event_4_8_pic1.jpg)


> **Đánh giá tổng thể:** Ngày 1 của **AWS FCAJ Agent Forge – Deep Dive** đã cung cấp nền tảng vững chắc về **Agentic AI** và **Amazon Bedrock AgentCore**, giúp người tham gia hiểu rõ từ các khái niệm cơ bản đến kiến trúc và cách triển khai AI Agent trong môi trường production. Workshop kết hợp giữa lý thuyết, ví dụ minh họa và các nội dung thực hành, đồng thời nhấn mạnh các yếu tố quan trọng như bảo mật, khả năng mở rộng, quản lý vòng đời và tích hợp công cụ. Đây là một chương trình hữu ích cho những ai muốn xây dựng các hệ thống AI Agent đáp ứng yêu cầu của môi trường doanh nghiệp.