---
sidebar_position: 5
---

# Hướng dẫn Export Dữ liệu Security từ SonarQube sử dụng API

## Giới thiệu

Tài liệu này hướng dẫn cách sử dụng API của SonarQube để export dữ liệu security, cụ thể là thông tin về Security Hotspots. Script Python được cung cấp để thực hiện việc này và tạo ra một báo cáo chi tiết.

## Yêu cầu

- Python 3.x
- Thư viện `requests`
- Quyền truy cập vào SonarQube server và API token hoặc thông tin đăng nhập

## Cài đặt

1. Đảm bảo Python đã được cài đặt trên hệ thống của bạn.
2. Cài đặt thư viện requests bằng lệnh:
   ```
   pip install requests
   ```



## Full Source Code

Dưới đây là toàn bộ source code của script để export dữ liệu security từ SonarQube:

```python
import requests
import json
from requests.auth import HTTPBasicAuth
from datetime import datetime
import io

# Configuration
sonarqube_url = "http://localhost:9000"
project_key = "Test-FE"
# project_key = "Node-module-4"
username = "admin"
password = "261997"

file_report = "security_hotspots_report.txt"
# file_report = "security_hotspots_node_module_report.txt"

# API endpoint for Security Hotspots
url = f"{sonarqube_url}/api/hotspots/search"

# Parameters
params = {
    "projectKey": project_key,
    "ps": 500  # Number of results per page, maximum 500
}

# Use Basic Authentication
auth = HTTPBasicAuth(username, password)

# Send request
response = requests.get(url, params=params, auth=auth)
data = response.json()

hotspots = data['hotspots']

# Use StringIO to create a buffer for writing
output = io.StringIO()

output.write(f"Total number of Security Hotspots: {len(hotspots)}\n")
output.write("\nList of Security Hotspots:\n\n")

for index, hotspot in enumerate(hotspots, 1):
    output.write(f"Hotspot {index}:\n")
    output.write(f"  File: {hotspot['component']}\n")
    output.write(f"  Line: {hotspot['line']}\n")
    output.write(f"  Vulnerability Probability: {hotspot['vulnerabilityProbability']}\n")
    output.write(f"  Security Category: {hotspot['securityCategory']}\n")
    output.write(f"  Status: {hotspot['status']}\n")
    output.write(f"  Description: {hotspot['message']}\n")
    
    # Convert dates
    creation_date = datetime.strptime(hotspot['creationDate'], "%Y-%m-%dT%H:%M:%S%z")
    update_date = datetime.strptime(hotspot['updateDate'], "%Y-%m-%dT%H:%M:%S%z")
    
    output.write(f"  Creation Date: {creation_date.strftime('%d/%m/%Y %H:%M:%S')}\n")
    output.write(f"  Update Date: {update_date.strftime('%d/%m/%Y %H:%M:%S')}\n")
    output.write(f"  Author: {hotspot['author']}\n")
    output.write(f"  Rule: {hotspot['ruleKey']}\n")
    output.write("---\n")

# Statistics
vulnerability_counts = {}
category_counts = {}
file_counts = {}

for hotspot in hotspots:
    vulnerability_counts[hotspot['vulnerabilityProbability']] = vulnerability_counts.get(hotspot['vulnerabilityProbability'], 0) + 1
    category_counts[hotspot['securityCategory']] = category_counts.get(hotspot['securityCategory'], 0) + 1
    file_counts[hotspot['component']] = file_counts.get(hotspot['component'], 0) + 1

output.write("\nStatistics:\n")
output.write("\nVulnerability Probability:\n")
for prob, count in vulnerability_counts.items():
    output.write(f"  {prob}: {count}\n")

output.write("\nSecurity Categories:\n")
for category, count in category_counts.items():
    output.write(f"  {category}: {count}\n")

output.write("\nFiles with the most issues:\n")
sorted_files = sorted(file_counts.items(), key=lambda x: x[1], reverse=True)
for file, count in sorted_files[:5]:  # Display top 5 files
    output.write(f"  {file}: {count}\n")

# Write content to file
with open(file_report, 'w', encoding='utf-8') as file:
    file.write(output.getvalue())

# Print to console
print(output.getvalue())

print(f"\nThe report has been written to the file '{file_report}'")
```


## Cấu hình

Trước khi chạy script, cần cấu hình các thông tin sau:

```python
sonarqube_url = "http://localhost:9000"  # URL của SonarQube server
project_key = "Test-FE"  # Key của project cần export dữ liệu
username = "admin"  # Username để truy cập API
password = "261997"  # Password để truy cập API
file_report = "security_hotspots_report.txt"  # Tên file báo cáo output
```

## Chạy Script

1. Lưu code vào một file Python, ví dụ `sonarqube_security_export.py`.
2. Chạy script bằng lệnh:
   ```
   python sonarqube_security_export.py
   ```

## Giải thích Code

### 1. Import các thư viện cần thiết

```python
import requests
import json
from requests.auth import HTTPBasicAuth
from datetime import datetime
import io
```

### 2. Cấu hình và thiết lập kết nối

```python
url = f"{sonarqube_url}/api/hotspots/search"
params = {
    "projectKey": project_key,
    "ps": 500  # Số lượng kết quả tối đa trên mỗi trang
}
auth = HTTPBasicAuth(username, password)
```

### 3. Gửi request và nhận dữ liệu

```python
response = requests.get(url, params=params, auth=auth)
data = response.json()
hotspots = data['hotspots']
```

### 4. Xử lý và format dữ liệu

Script sẽ lặp qua từng hotspot và trích xuất các thông tin quan trọng như:
- Tên file
- Số dòng
- Mức độ nguy hiểm
- Danh mục bảo mật
- Trạng thái
- Mô tả
- Ngày tạo và cập nhật
- Tác giả
- Rule key

### 5. Tạo thống kê

Script tạo các thống kê về:
- Số lượng hotspot theo mức độ nguy hiểm
- Số lượng hotspot theo danh mục bảo mật
- Các file có nhiều vấn đề nhất

### 6. Xuất báo cáo

Báo cáo được xuất ra file văn bản và cũng được in ra console.

## Kết quả

Sau khi chạy script, bạn sẽ nhận được:
1. Một file báo cáo text chứa chi tiết về tất cả Security Hotspots và các thống kê.
2. Thông tin tổng quan được in ra console.

## Tùy chỉnh

- Để thay đổi số lượng kết quả trên mỗi trang, điều chỉnh tham số `ps` trong `params`.
- Để export dữ liệu cho nhiều project, bạn có thể sửa script để lặp qua danh sách các project key.

## Lưu ý

- Đảm bảo bạn có quyền truy cập vào API của SonarQube.
- Script này chỉ lấy 500 kết quả đầu tiên. Nếu project của bạn có nhiều hơn 500 Security Hotspots, bạn cần sửa script để hỗ trợ phân trang.
- Luôn bảo vệ thông tin đăng nhập và API token của bạn.