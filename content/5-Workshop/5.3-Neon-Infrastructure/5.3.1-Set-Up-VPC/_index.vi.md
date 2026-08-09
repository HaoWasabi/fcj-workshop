---
title : "Khởi tạo và cấu hình VPC"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.3.1. </b> "
---

### 5.3.1.1 Khởi tạo Amazon VPC

Bước đầu tiên là tạo VPC và subnet theo kiến trúc chuẩn.
1. Truy cập AWS Management Console.
2. Mở dịch vụ VPC.
3. Chọn Your VPCs → Create VPC.
4. Chọn tùy chọn VPC and more.
5. Cấu hình các thông số:
   - Name tag auto-generation: `NeonFood`
   - IPv4 CIDR block: `10.0.0.0/16`
   - Number of AZs: `2`
   - Number of Public Subnets: `2`
   - Number of Private Subnets: `4`
6. Nhấn Create VPC.

Kết quả mong đợi:

- Public subnet 1: `10.0.0.0/20`
- Public subnet 2: `10.0.16.0/20`
- Private subnet 1: `10.0.128.0/20`
- Private subnet 2: `10.0.144.0/20`
- Private subnet 3: `10.0.160.0/20`
- Private subnet 4: `10.0.176.0/20`

![Hình 1.](/images/5-Workshop/5.3-Neon-Infracstructure/image001.png)

![Hình 7.](/images/5-Workshop/5.3-Neon-Infracstructure/image007.png)

### 5.3.1.2. Tạo Elastic IP cho NAT Gateway

1. Mở VPC Console.
2. Chọn Elastic IP addresses.
3. Nhấn Allocate Elastic IP address.
4. Thêm tag `Name=EIP-NAT-AZ1a`.
5. Nhấn Allocate.

![Hình 9.](/images/5-Workshop/5.3-Neon-Infracstructure/image009.png)
![Hình 11.](/images/5-Workshop/5.3-Neon-Infracstructure/image011.png)


### 5.3.1.3. Tạo NAT Gateway

1. Vào VPC Console → NAT gateways.
2. Chọn Create NAT gateway.
3. Cấu hình:
   - Name: `NAT-Gateway-AZ1a`
   - Subnet: public subnet thuộc AZ 1a
   - Connectivity type: `Public`
   - Elastic IP: chọn địa chỉ EIP đã tạo
4. Nhấn Create NAT gateway.
5. Chờ trạng thái `Available`.

![Hình 13.](/images/5-Workshop/5.3-Neon-Infracstructure/image013.png)
![Hình 15.](/images/5-Workshop/5.3-Neon-Infracstructure/image015.png)

### 5.3.1.4. Cấu hình Route table cho private subnet

1. Mở Route tables.
2. Chọn route table gắn với private subnet thuộc AZ 1a.
3. Chọn Routes → Edit routes.
4. Thêm route mới:
   - Destination: `0.0.0.0/0`
   - Target: NAT Gateway vừa tạo
5. Nhấn Save changes.

![Hình 17.](/images/5-Workshop/5.3-Neon-Infracstructure/image017.png)
![Hình 19.](/images/5-Workshop/5.3-Neon-Infracstructure/image019.png)
![Hình 21.](/images/5-Workshop/5.3-Neon-Infracstructure/image021.png)
![Hình 23.](/images/5-Workshop/5.3-Neon-Infracstructure/image023.png)
![Hình 25.](/images/5-Workshop/5.3-Neon-Infracstructure/image025.png)


### 5.3.1.5. Kích hoạt Auto-assign Public IPv4 cho public subnet

1. Vào VPC Console → Subnets.
2. Chọn public subnet 1.
3. Chọn Actions → Edit subnet settings.
4. Tích chọn Enable auto-assign public IPv4 address.
5. Lưu lại.
6. Lặp lại tương tự với public subnet 2.

![Hình 27.](/images/5-Workshop/5.3-Neon-Infracstructure/image027.png)
![Hình 29.](/images/5-Workshop/5.3-Neon-Infracstructure/image029.png)
