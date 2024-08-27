---
sidebar_position: 307
---

# Bài 7: Security và Compliance trên AWS

## 1. Shared Responsibility Model

AWS operates on a shared responsibility model, phân chia trách nhiệm bảo mật giữa AWS và khách hàng.

### 1.1 AWS Responsibilities (Security "of" the Cloud)

- Physical security of data centers
- Hardware và network infrastructure
- Virtualization infrastructure
- Software for managed services

### 1.2 Customer Responsibilities (Security "in" the Cloud)

- Network traffic protection
- Operating system security
- Application security
- Identity và access management
- Client-side và server-side encryption
- Network và firewall configuration

## 2. Identity và Access Management (IAM)

IAM là service quản lý quyền truy cập vào AWS resources.

### 2.1 Key Components

- **Users**: Entities representing people hoặc applications
- **Groups**: Collections of users
- **Roles**: Sets of permissions for specific use cases
- **Policies**: Documents defining permissions

### 2.2 Best Practices

- Implement least privilege principle
- Use IAM roles for applications và AWS services
- Regularly rotate credentials
- Enable MFA for all users, especially root account
- Use policy conditions for extra security

## 3. Data Protection

### 3.1 Encryption at Rest

- **AWS Key Management Service (KMS)**: Manage encryption keys
- **AWS CloudHSM**: Hardware security modules for regulatory compliance
- **S3 Encryption**: Server-side và client-side encryption options

### 3.2 Encryption in Transit

- **SSL/TLS**: For data in transit
- **VPN**: For secure communication with on-premises networks
- **AWS Certificate Manager**: Provision, manage, và deploy SSL/TLS certificates

### 3.3 Data Backup và Recovery

- **AWS Backup**: Centralized backup service
- **S3 Versioning**: Keep multiple versions of objects
- **RDS Automated Backups và Snapshots**: For database recovery

## 4. Network Security

### 4.1 Amazon VPC

- **Security Groups**: Stateful firewall for EC2 instances
- **Network ACLs**: Stateless firewall for subnets
- **Flow Logs**: Capture network traffic information

### 4.2 AWS WAF (Web Application Firewall)

- Protects web applications from common exploits
- Integrates with CloudFront, Application Load Balancer, và API Gateway

### 4.3 AWS Shield

- **Standard**: Automatic protection against DDoS attacks
- **Advanced**: 24/7 access to AWS DDoS response team và financial protection

## 5. Monitoring và Logging

### 5.1 AWS CloudTrail

- Logs API calls for your account
- Useful for security analysis, resource change tracking, và compliance auditing

### 5.2 Amazon CloudWatch

- Monitoring service for AWS resources và applications
- Set alarms và automate responses to security events

### 5.3 Amazon GuardDuty

- Intelligent threat detection service
- Uses machine learning to identify unexpected và potentially unauthorized activity

## 6. Compliance và Governance

### 6.1 AWS Config

- Assesses, audits, và evaluates configurations of AWS resources
- Helps maintain compliance with internal policies và regulatory standards

### 6.2 AWS Artifact

- On-demand access to AWS security và compliance reports
- Provides access to AWS ISO certifications, PCI reports, và SOC reports

### 6.3 Amazon Inspector

- Automated security assessment service
- Helps improve security và compliance of applications deployed on AWS

## 7. Security Best Practices

1. **Implement a Strong Identity Foundation**
   - Centralize identity management
   - Implement principle of least privilege

2. **Enable Traceability**
   - Turn on CloudTrail in all regions
   - Use CloudWatch to set up alerts for suspicious activities

3. **Apply Security at All Layers**
   - Use security groups, NACLs, và WAF
   - Implement data encryption at rest và in transit

4. **Automate Security Best Practices**
   - Use AWS Config rules to automatically check resource configurations
   - Implement automated responses to security events with Lambda

5. **Protect Data in Transit và at Rest**
   - Use SSL/TLS for all communications
   - Implement server-side encryption for data at rest

6. **Keep People Away from Data**
   - Use IAM roles và temporary credentials
   - Implement strict access controls

7. **Prepare for Security Events**
   - Have an incident response plan
   - Regularly conduct security drills

## 8. Compliance Programs và Frameworks

AWS supports numerous compliance programs, bao gồm:

- HIPAA
- PCI DSS
- SOC 1, 2, và 3
- ISO 27001
- FedRAMP
- GDPR

Customers can leverage AWS compliance programs to build và maintain their own compliant applications.

## 9. AWS Security Hub

- Comprehensive view of security alerts và security posture across AWS accounts
- Aggregates, organizes, và prioritizes security alerts từ multiple AWS services

## 10. Future of Security on AWS

- Increasing use of AI và machine learning for threat detection
- Greater emphasis on automation in security responses
- Continued focus on compliance với evolving global regulations

Hiểu sâu về Security và Compliance trên AWS là cực kỳ quan trọng cho việc thiết kế, triển khai và duy trì các ứng dụng và infrastructure an toàn trên cloud. Những kiến thức này là nền tảng cho kỳ thi SAA-C03 và sẽ giúp bạn trong việc xây dựng các giải pháp bảo mật toàn diện trên AWS.