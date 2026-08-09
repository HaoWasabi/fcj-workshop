---
title: "Event 4"
date: 2026-08-08
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

# Bài thu hoạch “Agent Forge - Deepdive (Day 2)”

### Mục Đích Của Sự Kiện

AWS FCAJ Agent Forge – Deepdive (Ngày 2) là buổi học thứ hai trong chuỗi workshop chuyên sâu do cộng đồng First Cloud AI Journey (FCAJ) phối hợp tổ chức. Chương trình thuộc cấp độ nâng cao (L300 – Advanced), được xây dựng dựa trên tài liệu và kinh nghiệm thực tế của các kỹ sư AWS.

Workshop được tổ chức nhằm giúp người tham gia:
- Có thêm kiến thức quản lý bộ nhớ, đánh giá chất lượng phản hồi, giám sát hệ thống và tối ưu hiệu suất.
- Thực hành phát triển một AI Agent từ mức cơ bản thành hệ thống Agentic AI có khả năng vận hành trong môi trường doanh nghiệp.

### Danh Sách Diễn Giả

- Anh **Hiếu** - Co-head cộng đồng FCAJ, Solution Architect tại AWS Việt Nam.
- Anh **Hải Anh** - Cloud Consultant tại Chiase Pacific, trực tiếp hướng dẫn phần thực hành lab.
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

#### Agent Memory

Agent Memory giúp Agent vượt qua giới hạn của Context Window, duy trì ngữ cảnh hội thoại và cá nhân hóa trải nghiệm của người dùng.

**Short-term Memory** là bộ nhớ ngắn hạn, lưu trữ đồng bộ toàn bộ lịch sử hội thoại dưới dạng các tin nhắn thô. Nhờ đó, Agent có thể hiểu mạch trao đổi hiện tại và phản hồi nhất quán. Hệ thống cũng hỗ trợ cơ chế rẽ nhánh (branching), tương tự như cách Git tạo nhánh trong quá trình phát triển phần mềm.

**Long-term Memory** là bộ nhớ dài hạn, hoạt động bất đồng bộ. Hệ thống trích xuất những thông tin quan trọng từ hội thoại và lưu dưới dạng vector để có thể truy xuất trong các phiên sau. Bốn chiến lược lưu trữ chính gồm:

- **Summary:** tóm tắt và nén nội dung hội thoại.
- **User Preference:** lưu trữ sở thích của người dùng.
- **Semantic:** lưu trữ tri thức chuyên ngành.
- **Episodic:** lưu lại các quyết định hoặc sự kiện đã diễn ra.

**Namespace** được sử dụng như một cấu trúc thư mục phân cấp để cô lập dữ liệu theo strategy, actor hoặc session. Khi kết hợp semantic search và similarity ranking, Agent có thể tìm đúng thông tin cần thiết, giảm lượng token sử dụng và cải thiện thời gian phản hồi.

#### Khả năng quan sát hệ thống

Workshop nhấn mạnh nguyên tắc: *“You cannot fix what you cannot see”* — không thể khắc phục vấn đề nếu không quan sát được vấn đề đó. Hệ thống Observability sử dụng chuẩn OpenTelemetry để thu thập ba nhóm dữ liệu chính:

- **Logs:** ghi lại chi tiết về request, lỗi kết nối, lỗi hệ thống hoặc log từ terminal.
- **Traces:** theo dõi toàn bộ hành trình của một request, từ khi người dùng gửi prompt đến khi Agent trả về phản hồi, bao gồm các tool call.
- **Metrics:** đo lường các chỉ số như mức tiêu thụ token, tỷ lệ lỗi và độ trễ phản hồi.

Những dữ liệu này giúp đội ngũ phát triển xác định nguyên nhân gây chậm trễ, tối ưu chi phí token và cải thiện trải nghiệm người dùng.

#### Hệ thống đánh giá Agent

Một rủi ro phổ biến của AI Agent là hiện tượng **hallucination**, tức đưa ra thông tin không chính xác nhưng thể hiện như sự thật. Để hạn chế rủi ro này, hệ thống cung cấp 13 evaluator tích hợp sẵn, chẳng hạn như **correctness** và **helpfulness**.

Các evaluator được áp dụng ở ba cấp độ:

- **Session level:** đánh giá kết quả của toàn bộ phiên làm việc.
- **Trace level:** đánh giá độ chính xác của phản hồi.
- **Span level:** đánh giá từng bước xử lý, chẳng hạn như việc gọi tool hoặc truyền tham số.

Hệ thống hỗ trợ hai hình thức đánh giá. **On-demand** phù hợp với giai đoạn phát triển và thử nghiệm; **Online** được sử dụng để theo dõi chất lượng Agent theo thời gian thực trong môi trường production. Kết quả đánh giá tự động vẫn cần được chuyên gia lĩnh vực kiểm chứng để bảo đảm tính chính xác.

## Những Gì Học Được

### Kiến Thức Chuyên Môn

- Hiểu rõ sự khác biệt giữa Short-term Memory và Long-term Memory, đặc biệt là cơ chế xử lý đồng bộ và bất đồng bộ.
- Nắm được ba trụ cột của Observability là Logs, Traces và Metrics, cùng vai trò của chuẩn OpenTelemetry trong việc theo dõi sức khỏe hệ thống.
- Hiểu cách các evaluator tự động đánh giá phản hồi của Agent theo tiêu chí chuẩn hóa thay vì dựa hoàn toàn vào cảm nhận chủ quan.
- Biết thêm về Cedar Policy và cơ chế sandbox, qua đó nhận thức rõ vai trò của bảo mật khi Agent thực hiện tác vụ hoặc thử nghiệm mã nguồn.

### Bài Học Kinh Nghiệm

- Thiết kế AI Agent theo từng chức năng nhỏ trước khi xây dựng hệ thống phức tạp.
- Luôn ưu tiên bảo mật và phân quyền khi AI Agent truy cập tài nguyên.
- Theo dõi, đánh giá và tối ưu AI Agent dựa trên kết quả thực tế.
- Xây dựng AI Agent theo hướng dễ mở rộng và dễ bảo trì.

## Trải Nghiệm Trong Workshop

Tham gia **Ngày 2 của AWS FCAJ Agent Forge – Deep Dive** giúp tôi có cái nhìn tổng quan về cách xây dựng và vận hành AI Agent trong môi trường doanh nghiệp.

Thông qua phần trình bày của diễn giả và các nội dung thực hành, tôi hiểu rõ hơn cách để tạo ra một AI Agent hiệu quả bằng việc cung cấp cho hệ thống cơ chế lưu trữ tri thức, giám sát, đánh giá chất lượng và bảo mật chặt chẽ.

### Một số hình ảnh khi tham gia sự kiện
![Event Photo 1](/images/4-EventParticipated/image004.jpg)

> **Đánh giá tổng thể:** Ngày 2 của **AWS FCAJ Agent Forge – Deep Dive** đã cung cấp nền tảng vững chắc về **Agentic AI** và **Amazon Bedrock AgentCore**, giúp người tham gia hiểu rõ từ các khái niệm cơ bản đến kiến trúc và cách triển khai AI Agent trong môi trường production. Workshop kết hợp giữa lý thuyết, ví dụ minh họa và các nội dung thực hành, đồng thời nhấn mạnh các yếu tố quan trọng như bảo mật, khả năng mở rộng, quản lý vòng đời và tích hợp công cụ. Đây là một chương trình hữu ích cho những ai muốn xây dựng các hệ thống AI Agent đáp ứng yêu cầu của môi trường doanh nghiệp.