---
sidebar_position: 6
---

# Bài 4: Hướng dẫn sử dụng npm audit để kiểm tra bảo mật dự án Node.js

npm audit là một công cụ tích hợp sẵn trong npm (Node Package Manager) giúp kiểm tra các lỗ hổng bảo mật trong các dependencies của dự án Node.js. Dưới đây là hướng dẫn chi tiết về cách sử dụng npm audit.

## 1. Kiểm tra cơ bản

Để chạy kiểm tra bảo mật cơ bản, hãy mở terminal trong thư mục gốc của dự án và chạy lệnh:

```
npm audit
```

Lệnh này sẽ quét `package-lock.json` (hoặc `npm-shrinkwrap.json`) và báo cáo về các lỗ hổng bảo mật đã biết.

## 2. Xem báo cáo chi tiết

Để xem báo cáo chi tiết hơn, sử dụng:

```
npm audit --json
```

Kết quả sẽ được hiển thị dưới dạng JSON, giúp bạn dễ dàng phân tích hoặc tích hợp vào các công cụ khác.

## 3. Tự động khắc phục vấn đề

npm audit có thể tự động cập nhật các dependencies để khắc phục các lỗ hổng bảo mật. Sử dụng lệnh:

```
npm audit fix
```

Lưu ý: Hãy cẩn thận khi sử dụng lệnh này vì nó có thể cập nhật các dependencies lên phiên bản mới nhất, có thể gây ra vấn đề tương thích.

## 4. Chỉ khắc phục các vấn đề nghiêm trọng

Để chỉ khắc phục các lỗ hổng nghiêm trọng:

```
npm audit fix --only=prod
```

## 5. Tạo báo cáo và lưu vào file

Để tạo báo cáo và lưu vào file:

```
npm audit --json > npm-audit-report.json
```

## 6. Thiết lập mức độ nghiêm trọng

Bạn có thể thiết lập mức độ nghiêm trọng tối thiểu để báo cáo:

```
npm audit --audit-level=moderate
```

Các mức độ bao gồm: low, moderate, high, critical

## 7. Tích hợp vào quy trình CI/CD

Thêm npm audit vào script trong `package.json`:

```json
{
  "scripts": {
    "security-check": "npm audit --audit-level=high"
  }
}
```

Sau đó, chạy `npm run security-check` trong pipeline CI/CD của bạn.

## 8. Xử lý kết quả npm audit

- Nếu npm audit báo cáo lỗ hổng, hãy đánh giá mức độ nghiêm trọng và tác động.
- Cập nhật các dependencies bị ảnh hưởng nếu có thể.
- Nếu không thể cập nhật ngay, hãy tìm cách giảm thiểu rủi ro và lên kế hoạch cập nhật trong tương lai.

## 9. Thực hiện định kỳ

Nên chạy npm audit định kỳ (ví dụ: hàng tuần) để đảm bảo dự án luôn được cập nhật về mặt bảo mật.

## Kết luận

npm audit là một công cụ mạnh mẽ giúp duy trì bảo mật cho dự án Node.js của bạn. Bằng cách sử dụng nó thường xuyên và kết hợp vào quy trình phát triển, bạn có thể phát hiện và giải quyết các lỗ hổng bảo mật tiềm ẩn một cách hiệu quả.