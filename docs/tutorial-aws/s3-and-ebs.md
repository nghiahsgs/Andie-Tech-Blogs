---
sidebar_position: 302
---

# Bài 2: Tối ưu hóa Storage với Amazon S3 và EBS

## 1. Amazon Simple Storage Service (S3)

Amazon S3 là dịch vụ lưu trữ object có khả năng mở rộng, độ bền cao và tính sẵn sàng cao. S3 được sử dụng rộng rãi cho nhiều mục đích như backup, lưu trữ, content distribution, và data lakes.

### 1.1 S3 Storage Classes

S3 cung cấp nhiều storage class để tối ưu chi phí:

- **S3 Standard**: Lưu trữ dữ liệu truy cập thường xuyên.
- **S3 Intelligent-Tiering**: Tự động di chuyển dữ liệu giữa hai tier dựa trên patterns truy cập.
- **S3 Standard-Infrequent Access (S3 Standard-IA)**: Cho dữ liệu truy cập ít nhưng cần truy cập nhanh khi cần.
- **S3 One Zone-Infrequent Access (S3 One Zone-IA)**: Tương tự S3 Standard-IA nhưng lưu trữ trong một AZ duy nhất.
- **S3 Glacier Instant Retrieval**: Cho dữ liệu archive cần truy cập tức thì.
- **S3 Glacier Flexible Retrieval**: Cho dữ liệu archive với thời gian truy xuất linh hoạt từ vài phút đến vài giờ.
- **S3 Glacier Deep Archive**: Cho dữ liệu archive truy cập rất hiếm, với thời gian truy xuất trong vòng 12 giờ.

### 1.2 S3 Features

- **Versioning**: Lưu trữ nhiều phiên bản của một object.
- **Lifecycle Management**: Tự động chuyển objects giữa các storage class hoặc xóa objects cũ.
- **Encryption**: Hỗ trợ server-side encryption (SSE) với nhiều options.
- **Access Control**: Sử dụng bucket policies, ACLs, và IAM policies để quản lý access.
- **Static Website Hosting**: Host static websites trực tiếp từ S3 buckets.
- **Cross-Region Replication (CRR)**: Tự động replicate data giữa các regions.
- **Transfer Acceleration**: Tăng tốc độ upload/download sử dụng CloudFront's edge locations.

### 1.3 S3 Performance Optimization

- **S3 Transfer Acceleration**: Sử dụng CloudFront's edge locations để tăng tốc độ transfer.
- **Multipart Upload**: Chia file lớn thành nhiều phần nhỏ để upload song song.
- **S3 Select**: Truy xuất subset của data từ một object để giảm lượng data transferred.
- **Prefix Naming**: Sử dụng prefix naming hiệu quả để cải thiện performance cho high request rates.

### 1.4 S3 Security

- **IAM Policies**: Quản lý access ở level user/role.
- **Bucket Policies**: JSON-based policies áp dụng cho toàn bộ bucket.
- **Access Control Lists (ACLs)**: Quản lý access ở level object.
- **Encryption in Transit**: Sử dụng HTTPS/TLS.
- **Encryption at Rest**: Sử dụng SSE-S3, SSE-KMS, hoặc SSE-C.
- **Versioning và MFA Delete**: Bảo vệ chống xóa hoặc overwrite không mong muốn.

## 2. Amazon Elastic Block Store (EBS)

Amazon EBS cung cấp block-level storage volumes cho use với EC2 instances. EBS volumes tồn tại độc lập với vòng đời của instance và có thể được attach/detach từ instances.

### 2.1 EBS Volume Types

- **General Purpose SSD (gp2 và gp3)**: 
  - Cân bằng giữa price và performance.
  - Phù hợp cho hầu hết workloads.

- **Provisioned IOPS SSD (io1 và io2)**: 
  - Cho I/O-intensive workloads như large databases.
  - Cung cấp highest performance và lowest latency.

- **Throughput Optimized HDD (st1)**: 
  - Cho workloads truy cập thường xuyên với large, sequential I/Os.
  - Ví dụ: big data, data warehouses, log processing.

- **Cold HDD (sc1)**: 
  - Lowest cost storage cho infrequently accessed workloads.
  - Ví dụ: Colder data requiring fewer scans per day.

### 2.2 EBS Features

- **Snapshots**: Point-in-time backups của EBS volumes, stored in S3.
- **Encryption**: AES-256 encryption sử dụng AWS KMS.
- **Elastic Volumes**: Dynamically increase capacity, tune performance, và change volume type.
- **Multi-Attach**: Attach một io1/io2 volume tới multiple EC2 instances trong cùng AZ.

### 2.3 EBS Performance

- **IOPS (Input/Output Operations Per Second)**: Measure of how fast we can read/write to the volume.
- **Throughput**: Measure of how much data can be read/written in a given time period.
- **Latency**: Time it takes for an I/O operation to complete.

Tips để tối ưu performance:
- Chọn đúng volume type cho workload.
- Sử dụng RAID configurations nếu cần higher IOPS hoặc throughput.
- Sử dụng EBS-optimized instances.

### 2.4 EBS vs Instance Store

- **EBS**: 
  - Persistent storage.
  - Can be detached và reattached to different instances.
  - Supports snapshots và encryption.

- **Instance Store**: 
  - Ephemeral storage.
  - Highest I/O performance.
  - Data lost when instance stops (not just reboots).

### 2.5 EBS Security

- **Encryption**: Sử dụng AWS KMS để encrypt data at rest và in transit giữa EBS và EC2.
- **Access Control**: Sử dụng IAM policies để control who can manage EBS volumes và snapshots.
- **Snapshots**: Có thể được shared safely bằng cách mã hóa với custom KMS keys.

## 3. Best Practices

1. **Chọn đúng storage solution**: S3 cho object storage, EBS cho block storage.
2. **Implement proper backup strategies**: Sử dụng S3 versioning và EBS snapshots.
3. **Optimize cost**: Sử dụng S3 Intelligent-Tiering và lifecycle policies; chọn đúng EBS volume type.
4. **Secure your data**: Encrypt data at rest và in transit, implement least privilege access.
5. **Monitor performance**: Sử dụng Amazon CloudWatch để monitor storage metrics.
6. **Plan for disaster recovery**: Implement cross-region replication cho S3; có kế hoạch restore EBS từ snapshots.

Hiểu sâu về S3 và EBS sẽ giúp bạn thiết kế các giải pháp storage hiệu quả, an toàn và tiết kiệm chi phí trên AWS. Những kiến thức này rất quan trọng cho kỳ thi SAA-C03.