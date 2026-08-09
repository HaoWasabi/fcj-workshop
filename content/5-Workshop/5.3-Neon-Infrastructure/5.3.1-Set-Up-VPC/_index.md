---
title : "Initialize and Configure VPC"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.3.1. </b> "
---

### 5.3.1.1 Create Amazon VPC

The first step is to create a VPC and subnets following the standard architecture.

1. Go to the AWS Management Console.
2. Open the VPC service.
3. Select Your VPCs → Create VPC.
4. Choose the **VPC and more** option.
5. Configure the following settings:
   - Name tag auto-generation: `NeonFood`
   - IPv4 CIDR block: `10.0.0.0/16`
   - Number of AZs: `2`
   - Number of Public Subnets: `2`
   - Number of Private Subnets: `4`
6. Click **Create VPC**.

Expected results:

- Public subnet 1: `10.0.0.0/20`
- Public subnet 2: `10.0.16.0/20`
- Private subnet 1: `10.0.128.0/20`
- Private subnet 2: `10.0.144.0/20`
- Private subnet 3: `10.0.160.0/20`
- Private subnet 4: `10.0.176.0/20`

![Figure 1.](/images/5-Workshop/5.3-Neon-Infracstructure/image001.png)

![Figure 7.](/images/5-Workshop/5.3-Neon-Infracstructure/image007.png)

### 5.3.1.2. Create Elastic IP for NAT Gateway

1. Open the VPC Console.
2. Select **Elastic IP addresses**.
3. Click **Allocate Elastic IP address**.
4. Add a tag `Name=EIP-NAT-AZ1a`.
5. Click **Allocate**.

![Figure 9.](/images/5-Workshop/5.3-Neon-Infracstructure/image009.png)
![Figure 11.](/images/5-Workshop/5.3-Neon-Infracstructure/image011.png)

### 5.3.1.3. Create NAT Gateway

1. Go to VPC Console → **NAT gateways**.
2. Click **Create NAT gateway**.
3. Configure:
   - Name: `NAT-Gateway-AZ1a`
   - Subnet: public subnet in AZ 1a
   - Connectivity type: `Public`
   - Elastic IP: select the EIP you created earlier
4. Click **Create NAT gateway**.
5. Wait until the status shows `Available`.

![Figure 13.](/images/5-Workshop/5.3-Neon-Infracstructure/image013.png)
![Figure 15.](/images/5-Workshop/5.3-Neon-Infracstructure/image015.png)

### 5.3.1.4. Configure Route Table for Private Subnet

1. Open **Route tables**.
2. Select the route table associated with the private subnet in AZ 1a.
3. Choose **Routes** → **Edit routes**.
4. Add a new route:
   - Destination: `0.0.0.0/0`
   - Target: the NAT Gateway you just created
5. Click **Save changes**.

![Figure 17.](/images/5-Workshop/5.3-Neon-Infracstructure/image017.png)
![Figure 19.](/images/5-Workshop/5.3-Neon-Infracstructure/image019.png)
![Figure 21.](/images/5-Workshop/5.3-Neon-Infracstructure/image021.png)
![Figure 23.](/images/5-Workshop/5.3-Neon-Infracstructure/image023.png)
![Figure 25.](/images/5-Workshop/5.3-Neon-Infracstructure/image025.png)

### 5.3.1.5. Enable Auto-assign Public IPv4 for Public Subnets

1. Go to VPC Console → **Subnets**.
2. Select public subnet 1.
3. Choose **Actions** → **Edit subnet settings**.
4. Check **Enable auto-assign public IPv4 address**.
5. Save.
6. Repeat the same steps for public subnet 2.

![Figure 27.](/images/5-Workshop/5.3-Neon-Infracstructure/image027.png)
![Figure 29.](/images/5-Workshop/5.3-Neon-Infracstructure/image029.png)
