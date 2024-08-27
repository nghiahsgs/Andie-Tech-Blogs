---
sidebar_position: 303
---

# Bài 3: Networking trên AWS với VPC

## 1. Amazon Virtual Private Cloud (VPC) Overview

Amazon VPC là dịch vụ networking cho phép bạn tạo một mạng ảo riêng biệt trong môi trường AWS cloud. VPC cho phép bạn kiểm soát đầy đủ môi trường mạng ảo của mình, bao gồm việc lựa chọn dải IP, tạo subnet, cấu hình route table và network gateways.

### 1.1 VPC Components

- **Subnet**: Một dải IP trong VPC, có thể là public hoặc private.
- **Route Table**: Tập hợp các rules để xác định traffic sẽ được điều hướng như thế nào.
- **Internet Gateway**: Cho phép giao tiếp giữa VPC và internet.
- **NAT Gateway/Instance**: Cho phép các instances trong private subnet truy cập internet.
- **Network ACL (NACL)**: Firewall stateless ở level subnet.
- **Security Group**: Firewall stateful ở level instance.
- **VPC Endpoints**: Cho phép kết nối private với các dịch vụ AWS mà không cần internet gateway.

## 2. Thiết kế VPC

### 2.1 IP Addressing

- Chọn một CIDR block cho VPC (ví dụ: 10.0.0.0/16).
- Chia CIDR block thành các subnets nhỏ hơn cho mỗi Availability Zone.

### 2.2 Subnets

- **Public Subnets**: Có route đến Internet Gateway.
- **Private Subnets**: Không có direct route đến Internet Gateway.
- **Database Subnets**: Thường là private và có thể có additional security controls.

### 2.3 Availability Zones

- Spread subnets across multiple AZs để tăng high availability.
- Thông thường, mỗi tier của application (web, app, database) sẽ có subnet riêng trong mỗi AZ.

## 3. Routing and Internet Connectivity

### 3.1 Route Tables

- Main Route Table: Mặc định cho tất cả subnets.
- Custom Route Tables: Có thể tạo và liên kết với specific subnets.

### 3.2 Internet Gateway

- Attach một Internet Gateway vào VPC để cho phép internet connectivity.
- Cấu hình route trong public subnets để gửi traffic đến Internet Gateway.

### 3.3 NAT Gateway/Instance

- Cho phép instances trong private subnets truy cập internet.
- NAT Gateway là một managed service, trong khi NAT Instance là một EC2 instance bạn phải quản lý.

## 4. Security

### 4.1 Network ACLs

- Stateless firewall ở level subnet.
- Rules được áp dụng theo thứ tự số.
- Có thể ALLOW hoặc DENY traffic.

### 4.2 Security Groups

- Stateful firewall ở level instance.
- Chỉ có ALLOW rules.
- Có thể reference other security groups.

### 4.3 Flow Logs

- Capture thông tin về IP traffic đi vào và ra khỏi network interfaces trong VPC.
- Có thể được tạo ở level VPC, subnet, hoặc network interface.

## 5. VPC Connectivity Options

### 5.1 VPC Peering

- Kết nối hai VPCs với nhau sử dụng private IP addresses.
- VPCs có thể ở different regions hoặc accounts.

### 5.2 AWS Transit Gateway

- Network transit hub để kết nối VPCs và on-premises networks.

### 5.3 AWS Direct Connect

- Dedicated network connection từ on-premises tới AWS.

### 5.4 VPN

- Site-to-Site VPN: Kết nối on-premises network với AWS.
- Client VPN: Cho phép remote users truy cập vào VPC.

## 6. VPC Endpoints

### 6.1 Interface Endpoints

- Powered by AWS PrivateLink.
- Cho phép kết nối tới services sử dụng private IP addresses.

### 6.2 Gateway Endpoints

- Chỉ available cho S3 và DynamoDB.
- Được chỉ định trong route table.

## 7. Advanced VPC Features

### 7.1 Elastic Network Interfaces (ENIs)

- Virtual network cards bạn có thể attach/detach từ EC2 instances.

### 7.2 Elastic IP Addresses

- Static, public IPv4 addresses bạn có thể allocate cho account của mình.

### 7.3 VPC Flow Logs

- Capture thông tin về IP traffic trong VPC.

## 8. Best Practices

1. **Plan your IP address space carefully**: Sử dụng CIDR blocks lớn cho VPC để có room cho growth.

2. **Use multiple AZs**: Spread resources across AZs để tăng availability.

3. **Use private subnets**: Đặt resources không cần direct internet access vào private subnets.

4. **Implement least privilege**: Sử dụng NACLs và Security Groups để control access.

5. **Use VPC endpoints**: Giảm data transfer costs và tăng security bằng cách sử dụng VPC endpoints.

6. **Monitor your VPC**: Sử dụng VPC Flow Logs và CloudWatch để monitor traffic.

7. **Use transit gateway cho complex networks**: Nếu bạn có nhiều VPCs và on-premises networks, consider sử dụng AWS Transit Gateway.

8. **Automate network creation**: Sử dụng AWS CloudFormation hoặc Terraform để automate VPC và network resource creation.

Hiểu sâu về VPC và networking trên AWS là cực kỳ quan trọng cho việc thiết kế và triển khai các ứng dụng an toàn, có khả năng mở rộng trên cloud. Những kiến thức này là nền tảng cho kỳ thi SAA-C03 và sẽ giúp bạn trong việc thiết kế các giải pháp AWS phức tạp.