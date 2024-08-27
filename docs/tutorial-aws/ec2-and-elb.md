---
sidebar_position: 301
---

# Bài 1: Hiểu sâu về Amazon EC2 và Elastic Load Balancing

## 1. Amazon Elastic Compute Cloud (EC2)

Amazon EC2 là dịch vụ cung cấp máy chủ ảo trên đám mây, cho phép bạn có thể tạo và quản lý các máy chủ (gọi là instances) một cách linh hoạt.

### 1.1 Loại Instance

EC2 cung cấp nhiều loại instance khác nhau, mỗi loại được tối ưu hóa cho các use case cụ thể:

- **General Purpose (T3, M5, M6g)**: Cân bằng giữa tính toán, bộ nhớ và mạng. Phù hợp cho nhiều ứng dụng đa dạng.
- **Compute Optimized (C5, C6g)**: Tối ưu cho các ứng dụng đòi hỏi CPU cao như scientific modeling, batch processing, và game servers.
- **Memory Optimized (R5, R6g, X1)**: Cho các ứng dụng xử lý large datasets trong bộ nhớ như in-memory databases.
- **Storage Optimized (I3, D2)**: Cho các workloads đòi hỏi high, sequential read/write access như data warehousing.
- **Accelerated Computing (P3, G4)**: Sử dụng hardware accelerators như GPUs cho machine learning và đồ họa.

### 1.2 Pricing Models

EC2 cung cấp nhiều tùy chọn pricing để tối ưu chi phí:

- **On-Demand**: Trả tiền theo giờ hoặc giây, không cần cam kết dài hạn.
- **Reserved Instances (RI)**: Cam kết sử dụng 1 hoặc 3 năm để được giảm giá đáng kể (lên đến 72%).
- **Spot Instances**: Sử dụng capacity dư thừa của AWS với giá thấp hơn (giảm đến 90%), nhưng có thể bị interrupt.
- **Savings Plans**: Cam kết sử dụng một lượng compute nhất định (tính bằng $/giờ) để được giảm giá.

### 1.3 Tính năng quan trọng

- **AMI (Amazon Machine Image)**: Template chứa cấu hình software (OS, application server, applications) để launch EC2 instances.
- **Security Groups**: Hoạt động như firewall ảo, kiểm soát inbound và outbound traffic.
- **Elastic IP**: Địa chỉ IPv4 tĩnh được thiết kế cho dynamic cloud computing.
- **Placement Groups**: Ảnh hưởng đến cách EC2 instances được đặt trên underlying hardware.
- **User Data**: Scripts có thể chạy khi instance khởi động, dùng để cấu hình instance.

### 1.4 Auto Scaling

Auto Scaling cho phép tự động điều chỉnh số lượng EC2 instances dựa trên các điều kiện định trước:

- **Launch Configuration/Template**: Định nghĩa cấu hình cho instances mới.
- **Auto Scaling Group**: Nhóm các instances được quản lý cùng nhau.
- **Scaling Policies**: Định nghĩa khi nào và cách thức scale up hoặc down.

## 2. Elastic Load Balancing (ELB)

ELB tự động phân phối incoming traffic giữa nhiều targets, như EC2 instances, containers, và IP addresses.

### 2.1 Các loại Load Balancer

- **Application Load Balancer (ALB)**:
  - Layer 7 (HTTP/HTTPS) load balancing
  - Hỗ trợ path-based routing, host-based routing
  - Tích hợp tốt với các container services như ECS
  - Hỗ trợ WebSocket và HTTP/2 protocols

- **Network Load Balancer (NLB)**:
  - Layer 4 (TCP/UDP) load balancing
  - Có thể xử lý hàng triệu requests mỗi giây với độ trễ cực thấp
  - Hỗ trợ static IP và elastic IP
  - Tốt cho các ứng dụng cần hiệu suất cực cao

- **Classic Load Balancer (CLB)**:
  - Legacy load balancer, hỗ trợ cả Layer 4 và Layer 7
  - Không được khuyến nghị cho các ứng dụng mới

### 2.2 Tính năng chính của ELB

- **Health Checks**: Kiểm tra sức khỏe của các targets và chỉ route traffic đến healthy targets.
- **SSL/TLS Termination**: Có thể handle SSL/TLS encryption và decryption.
- **Sticky Sessions**: Có thể route requests từ một user đến cùng một backend instance.
- **Cross-Zone Load Balancing**: Phân phối traffic đều giữa các Availability Zones.

### 2.3 Integration với Auto Scaling

Kết hợp ELB với Auto Scaling cho phép:
- Tự động đăng ký các EC2 instances mới với load balancer.
- Phân phối traffic đến các instances mới được tạo ra.
- Ngừng gửi traffic đến các instances bị terminate.

### 2.4 Monitoring và Logging

- **CloudWatch Metrics**: ELB tự động gửi metrics đến CloudWatch.
- **Access Logs**: Có thể enable access logs để ghi lại tất cả requests.
- **Request Tracing**: ALB hỗ trợ request tracing để debug và analyze traffic patterns.

## 3. Best Practices

1. **Sử dụng nhiều Availability Zones**: Để tăng tính sẵn sàng và fault tolerance.
2. **Implement Auto Scaling**: Để tự động điều chỉnh capacity theo demand.
3. **Sử dụng ALB cho microservices**: ALB hỗ trợ tốt cho kiến trúc microservices với path-based routing.
4. **Enable Cross-Zone Load Balancing**: Để đảm bảo traffic được phân phối đều.
5. **Sử dụng Target Groups**: Organize và manage các backend resources hiệu quả.
6. **Implement proper security measures**: Sử dụng security groups, NACLs, và SSL/TLS encryption.
7. **Monitor và log**: Sử dụng CloudWatch và access logs để hiểu traffic patterns và troubleshoot issues.

Hiểu sâu về EC2 và ELB là nền tảng quan trọng cho việc thiết kế và triển khai các ứng dụng scalable và highly available trên AWS. Nắm vững các concepts này sẽ giúp bạn rất nhiều trong kỳ thi SAA-C03.