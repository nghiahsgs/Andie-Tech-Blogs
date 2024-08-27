---
sidebar_position: 3
---

# Sử dụng SonarQube trên Windows

## Phần 1: Cài đặt SonarQube

### Yêu cầu hệ thống
- Windows 10 hoặc mới hơn (64-bit)
- Java 11 hoặc mới hơn
- Ít nhất 4GB RAM (khuyến nghị 8GB)
- Ít nhất 2GB dung lượng ổ cứng trống

### Bước 1: Cài đặt Java
1. Tải OpenJDK 11 từ trang chính thức: https://jdk.java.net/java-se-ri/11
2. Giải nén file tải về vào thư mục, ví dụ: `C:\Program Files\Java\jdk-11`
3. Thiết lập biến môi trường JAVA_HOME:
   - Mở Control Panel > System > Advanced System Settings > Environment Variables
   - Thêm biến mới JAVA_HOME với giá trị là đường dẫn đến thư mục Java, ví dụ: `C:\Program Files\Java\jdk-11`
   - Thêm `%JAVA_HOME%\bin` vào biến PATH

### Bước 2: Tải và cài đặt SonarQube
1. Truy cập https://www.sonarqube.org/downloads/
2. Tải phiên bản Community Edition cho Windows
3. Giải nén file tải về vào thư mục, ví dụ: `C:\SonarQube`

### Bước 3: Cấu hình SonarQube ( bỏ qua bước này nếu mình chạy ở local)
1. Mở file `C:\SonarQube\conf\sonar.properties`
2. Bỏ comment và chỉnh sửa các dòng sau:
   ```
   sonar.jdbc.username=sonar
   sonar.jdbc.password=sonar
   sonar.jdbc.url=jdbc:h2:C:/SonarQube/data/sonarqube
   ```

### Bước 4: Khởi động SonarQube
1. Mở Command Prompt với quyền Administrator
2. Di chuyển đến thư mục `C:\SonarQube\bin\windows-x86-64`
3. Chạy lệnh: `StartSonar.bat`
4. Đợi cho đến khi thấy thông báo "SonarQube is up"

## Phần 2: Sử dụng SonarQube

### Truy cập giao diện web
1. Mở trình duyệt web và truy cập: http://localhost:9000
2. Đăng nhập với tài khoản mặc định:
   - Username: admin
   - Password: admin
3. Hệ thống sẽ yêu cầu đổi mật khẩu khi đăng nhập lần đầu

### Tạo dự án mới
1. Từ dashboard, chọn "Create new project"
2. Nhập tên dự án và mã dự án
3. Chọn phương thức phân tích (ví dụ: Locally)
4. Tạo token mới để xác thực
5. Chọn ngôn ngữ lập trình của dự án

### Phân tích mã nguồn
1. Tải và cài đặt SonarScanner từ https://docs.sonarqube.org/latest/analysis/scan/sonarscanner/
2. Giải nén vào thư mục, ví dụ: `C:\SonarScanner`
3. Thêm `C:\SonarScanner\bin` vào biến PATH
4. Mở Command Prompt trong thư mục dự án cần phân tích
5. Chạy lệnh:
   ```
   sonar-scanner.bat -D"sonar.projectKey=your-project-key" -D"sonar.sources=." -D"sonar.host.url=http://localhost:9000" -D"sonar.login=your-token"
   ```
6. Đợi quá trình phân tích hoàn tất

* Nếu bạn muốn phân tích file node_module thì dùng lệnh này ( bỏ node_module ra khỏi gitignore)
```
sonar-scanner.bat -D"sonar.projectKey=Node-module-4" -D"sonar.sources=node_modules" -D"sonar.host.url=http://localhost:9000" -D"sonar.token=your-token"
```


### Xem kết quả phân tích
1. Quay lại giao diện web SonarQube
2. Chọn dự án vừa phân tích
3. Xem các chỉ số về chất lượng mã nguồn, bao gồm:
   - Bugs và Vulnerabilities
   - Code Smells
   - Duplications
   - Test Coverage (nếu có)

### Cải thiện chất lượng mã nguồn
1. Xem chi tiết các vấn đề được phát hiện
2. Ưu tiên giải quyết các vấn đề nghiêm trọng (Blocker, Critical)
3. Refactor mã nguồn để giải quyết các vấn đề
4. Chạy phân tích lại để kiểm tra sự cải thiện

## Lưu ý quan trọng
- Đảm bảo luôn cập nhật SonarQube lên phiên bản mới nhất
- Tạo backup dữ liệu SonarQube định kỳ
- Cấu hình tường lửa để bảo vệ SonarQube nếu triển khai trên môi trường production