---
sidebar_position: 5
---

# Hướng dẫn sử dụng Retire.js

Retire.js là một công cụ quét các thư viện JavaScript để phát hiện các phiên bản có lỗ hổng bảo mật hoặc đã lỗi thời. Dưới đây là hướng dẫn cách sử dụng Retire.js để bảo vệ dự án của bạn.

## Cài đặt

1. Đảm bảo bạn đã cài đặt Node.js trên máy tính.
2. Cài đặt Retire.js toàn cục bằng npm:

```
npm install -g retire
```

## Sử dụng cơ bản

1. Quét một thư mục:

```
retire path/to/your/project
```

2. Quét một file cụ thể:

```
retire path/to/your/file.js
```

3. Quét một URL:

```
retire https://example.com
```

## Tùy chọn hữu ích

- `--outputformat`: Chọn định dạng đầu ra (text, json, jsonsimple)
- `--outputpath`: Chỉ định đường dẫn file đầu ra
- `--ignore`: Bỏ qua các thư viện cụ thể
- `--severity`: Chỉ hiển thị các vấn đề với mức độ nghiêm trọng nhất định

Ví dụ:

```
retire --outputformat json --outputpath results.json --severity high
```

## Tích hợp với CI/CD

Bạn có thể tích hợp Retire.js vào quy trình CI/CD bằng cách thêm nó vào script npm:

```json
{
  "scripts": {
    "security-check": "retire --severity high --outputformat json --outputpath security-results.json"
  }
}
```

Sau đó, chạy lệnh `npm run security-check` trong pipeline CI/CD của bạn.

## Cập nhật cơ sở dữ liệu

Retire.js sử dụng một cơ sở dữ liệu các lỗ hổng bảo mật đã biết. Để cập nhật cơ sở dữ liệu này:

```
retire --update
```

## Kết luận

Sử dụng Retire.js thường xuyên giúp bạn phát hiện và giải quyết các vấn đề bảo mật tiềm ẩn trong các thư viện JavaScript của dự án. Hãy tích hợp nó vào quy trình phát triển để đảm bảo an toàn cho ứng dụng của bạn.