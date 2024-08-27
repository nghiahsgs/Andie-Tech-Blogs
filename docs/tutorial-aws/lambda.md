---
sidebar_position: 305
---
# Bài 5: Serverless Architecture với Lambda và API Gateway

## 1. AWS Lambda

AWS Lambda là một compute service cho phép bạn chạy code mà không cần quản lý servers. Lambda tự động scale ứng dụng của bạn bằng cách chạy code để đáp ứng từng trigger.

### 1.1 Key Features

- **Event-driven**: Chạy code để đáp ứng events từ các AWS services khác.
- **Stateless**: Mỗi invocation là độc lập.
- **Short-lived executions**: Giới hạn thời gian chạy tối đa là 15 phút.
- **Supports multiple languages**: Bao gồm Node.js, Python, Java, C#, Go, và Ruby.
- **Pay-per-use**: Chỉ trả tiền cho compute time bạn consume.

### 1.2 Lambda Function Components

- **Handler**: Entry point cho Lambda function.
- **Event object**: Chứa data cho function xử lý.
- **Context object**: Cung cấp methods và properties về invocation, function, và execution environment.

### 1.3 Triggers và Integrations

Lambda có thể được trigger bởi nhiều AWS services, bao gồm:

- Amazon S3
- Amazon DynamoDB
- Amazon Kinesis
- Amazon SNS
- Amazon CloudWatch Events
- AWS CloudTrail
- Amazon API Gateway

### 1.4 Deployment và Versioning

- **Deployment Packages**: Zip file chứa function code và dependencies.
- **Versions**: Immutable snapshots của function code và configuration.
- **Aliases**: Pointers tới specific versions, useful cho staging và production environments.

### 1.5 Monitoring và Logging

- CloudWatch Metrics: Tự động cung cấp metrics như invocations, duration, và errors.
- CloudWatch Logs: Lambda tự động logs function invocations.
- X-Ray: Để trace và analyze serverless applications.

## 2. Amazon API Gateway

Amazon API Gateway là một fully managed service để developers tạo, publish, maintain, monitor, và secure APIs ở bất kỳ scale nào.

### 2.1 Key Features

- **Multiple Endpoint Types**: HTTP, REST, và WebSocket APIs.
- **Authentication và Authorization**: Integrate với AWS Cognito và Lambda authorizers.
- **API Versioning**: Maintain multiple versions của APIs.
- **Request/Response Transformations**: Modify requests và responses on-the-fly.
- **API Keys và Usage Plans**: Manage và monitor third-party developers.
- **CloudWatch Integration**: Monitor API usage và performance.

### 2.2 API Types

- **HTTP API**: Thiết kế cho low-latency, cost-effective integrations với AWS services, HTTP endpoints.
- **REST API**: Cung cấp API management features như usage plans, API keys, publishing, và monetization.
- **WebSocket API**: Xây dựng real-time two-way communication applications.

### 2.3 Integration Types

- **Lambda Function**: Invoke Lambda function để xử lý request.
- **HTTP Endpoint**: Integrate với HTTP endpoints.
- **AWS Service**: Directly integrate với AWS services.
- **Mock**: Return static responses mà không gửi request tới backend.

### 2.4 Stages và Deployments

- **Stages**: Represent different environments (e.g., dev, test, prod).
- **Deployments**: Snapshot của API resources và methods.
- **Canary Releases**: Test new API versions bằng cách routing một phần traffic.

### 2.5 Security

- **HTTPS enforcement**: Bảo mật communication giữa clients và API Gateway.
- **AWS WAF integration**: Protect APIs từ common web exploits.
- **Resource Policies**: Control access tới APIs.

## 3. Serverless Application Model (SAM)

AWS SAM là một open-source framework để build serverless applications trên AWS.

### 3.1 Key Components

- **SAM Template**: YAML file định nghĩa serverless application's AWS resources.
- **SAM CLI**: Command line tool để build, test, và deploy serverless applications.

### 3.2 Benefits

- **Single Deployment Configuration**: Define Lambda functions, API Gateway APIs, và DynamoDB tables trong một file.
- **Local Testing**: Test serverless applications locally.
- **Built-in Best Practices**: Bao gồm tracing, permissions management, và monitoring.

## 4. Serverless Architecture Patterns

### 4.1 API Backend

- Use API Gateway và Lambda để create scalable, serverless API backends.

### 4.2 Data Processing

- Use Lambda với S3, Kinesis, hoặc DynamoDB streams để process data in real-time.

### 4.3 Web Application

- Combine API Gateway, Lambda, và Amazon S3 để host serverless web applications.

### 4.4 Alexa Skills

- Use Lambda để power Alexa skills.

### 4.5 ChatOps

- Integrate Lambda với Slack hoặc other chat platforms để tự động hóa tasks.

## 5. Best Practices

1. **Keep Functions Small và Focused**: Mỗi function nên làm một task cụ thể.

2. **Minimize Cold Starts**: 
   - Keep functions warm với scheduled events.
   - Use Provisioned Concurrency cho latency-sensitive applications.

3. **Optimize Function Performance**:
   - Minimize dependencies.
   - Reuse execution context.
   - Use environment variables cho configuration.

4. **Handle Errors Gracefully**: 
   - Implement proper error handling và logging.
   - Use Dead Letter Queues để capture failed event processing.

5. **Secure Your Serverless Applications**:
   - Follow principle of least privilege khi setting up IAM roles.
   - Encrypt environment variables.
   - Validate và sanitize all inputs.

6. **Monitor và Log**:
   - Use CloudWatch Logs và Metrics.
   - Implement distributed tracing với X-Ray.

7. **Design for Scalability**:
   - Use asynchronous processing khi có thể.
   - Implement backoff strategies cho rate-limited APIs.

8. **Cost Optimization**:
   - Optimize function memory settings.
   - Use caching trong API Gateway để reduce Lambda invocations.

Hiểu sâu về Serverless Architecture, đặc biệt là Lambda và API Gateway, là cực kỳ quan trọng cho việc thiết kế và triển khai các ứng dụng hiện đại, có khả năng mở rộng cao trên AWS. Những kiến thức này là nền tảng cho kỳ thi SAA-C03 và sẽ giúp bạn trong việc xây dựng các giải pháp serverless hiệu quả và tiết kiệm chi phí.