---
sidebar_position: 306
---
# Bài 6: Monitoring và Logging với CloudWatch và CloudTrail

## 1. Amazon CloudWatch

Amazon CloudWatch là một dịch vụ monitoring và observability toàn diện cho AWS cloud resources và các ứng dụng chạy trên AWS.

### 1.1 CloudWatch Metrics

- **Định nghĩa**: Data về performance của các systems và resources của bạn.
- **Namespace**: Container cho các metrics liên quan.
- **Dimensions**: Name/value pairs để xác định một metric cụ thể.
- **Resolution**: Standard (60 seconds) hoặc High Resolution (1 second).
- **Retention**: Tự động lưu trữ data points dựa trên độ phân giải của metric.

### 1.2 CloudWatch Alarms

- Trigger actions dựa trên metric values.
- Có thể send notifications tới SNS topics.
- Hỗ trợ các trạng thái: OK, ALARM, INSUFFICIENT_DATA.
- Có thể configure để perform EC2 actions (reboot, stop, terminate, recover).

### 1.3 CloudWatch Logs

- **Log Groups**: Collections của log streams thường từ cùng một application.
- **Log Streams**: Sequences của log events từ cùng một source.
- **Metric Filters**: Extract metric values từ log data.
- **Retention Settings**: Tự động delete logs sau một thời gian nhất định.

### 1.4 CloudWatch Dashboards

- Customizable home pages trong CloudWatch console để monitor resources.
- Có thể include graphs từ different regions và accounts.
- Hỗ trợ automatic refresh.

### 1.5 CloudWatch Events

- Deliver near real-time stream của system events.
- Có thể trigger Lambda functions, SNS topics, và các AWS services khác.

### 1.6 CloudWatch Insights

- **CloudWatch Logs Insights**: Analyze log data và trả lời queries.
- **Container Insights**: Collect, aggregate, summarize metrics và logs từ containerized applications.
- **Lambda Insights**: Detailed performance metrics cho Lambda functions.

## 2. AWS CloudTrail

AWS CloudTrail là một service cung cấp governance, compliance, operational auditing, và risk auditing của AWS account của bạn.

### 2.1 Key Concepts

- **Trail**: Configuration cho cách CloudTrail delivers log files tới một S3 bucket.
- **Event**: Record của một activity trong AWS account.
- **Management Events**: Operations thực hiện trên resources trong AWS account.
- **Data Events**: Resource operations thực hiện trên hoặc trong một resource.

### 2.2 Trail Configuration

- **Multi-region trail**: Log events từ all regions.
- **Organization trail**: Log events cho tất cả AWS accounts trong một organization.
- **Log file integrity validation**: Verify rằng logs không bị thay đổi sau khi được delivered.

### 2.3 CloudTrail Events

- **Who**: AWS account, IAM user/role, federated user, AWS service.
- **What**: API calls, console sign-ins, service events.
- **When**: Event time và date.
- **Where**: Source IP address, region, resource ARNs.

### 2.4 CloudTrail Insights

- Automatically detect unusual API activity trong AWS account.
- Use machine learning để analyze normal management event patterns.

### 2.5 Integration với các Services khác

- **Amazon S3**: Store và archive log files.
- **CloudWatch Logs**: Monitor CloudTrail events trong near real-time.
- **Amazon EventBridge**: Trigger workflows dựa trên CloudTrail events.
- **AWS Config**: Provide a history của configuration changes tới AWS resources.

## 3. Monitoring Best Practices

1. **Implement Comprehensive Monitoring**:
   - Monitor all layers của application stack.
   - Use both CloudWatch và third-party monitoring tools nếu cần.

2. **Set Up Proactive Notifications**:
   - Configure alarms cho critical metrics.
   - Use SNS để deliver notifications tới appropriate channels.

3. **Centralize Logs**:
   - Aggregate logs từ tất cả sources vào CloudWatch Logs.
   - Use log retention policies để manage storage costs.

4. **Use CloudWatch Dashboards**:
   - Create custom dashboards cho different stakeholders.
   - Include the most relevant metrics cho each audience.

5. **Implement Detailed Monitoring**:
   - Enable detailed monitoring cho critical resources.
   - Use custom metrics khi built-in metrics không đủ.

6. **Automate Responses**:
   - Use CloudWatch alarms để trigger auto scaling.
   - Implement auto-remediation với AWS Lambda.

7. **Audit API Activity**:
   - Enable CloudTrail trong tất cả regions.
   - Use CloudTrail Insights để detect unusual activity.

8. **Implement Log Analysis**:
   - Use CloudWatch Logs Insights để analyze log data.
   - Consider exporting logs tới a data warehouse cho long-term analysis.

9. **Monitor Costs**:
   - Use AWS Cost Explorer và AWS Budgets.
   - Set up alerts cho unexpected spikes trong usage hoặc costs.

10. **Regular Review và Optimization**:
    - Regularly review monitoring setup và alarms.
    - Adjust thresholds và metrics based on changing needs.

## 4. Integrating CloudWatch và CloudTrail

1. **Security Monitoring**:
   - Use CloudTrail để log API calls.
   - Create CloudWatch alarms based on specific CloudTrail events.

2. **Operational Insights**:
   - Analyze CloudTrail logs với CloudWatch Logs Insights.
   - Create custom metrics từ CloudTrail events sử dụng metric filters.

3. **Compliance và Auditing**:
   - Use CloudTrail cho detailed audit logs.
   - Create CloudWatch dashboards để visualize compliance-related metrics.

4. **Automated Response**:
   - Trigger Lambda functions based on CloudTrail events via CloudWatch Events.
   - Implement automated remediation cho security hoặc compliance issues.

Hiểu sâu về Monitoring và Logging trên AWS, đặc biệt là CloudWatch và CloudTrail, là cực kỳ quan trọng cho việc duy trì, troubleshoot, và bảo mật các ứng dụng và infrastructure trên AWS. Những kiến thức này là nền tảng cho kỳ thi SAA-C03 và sẽ giúp bạn trong việc thiết kế các giải pháp có khả năng observable và auditable trên AWS.