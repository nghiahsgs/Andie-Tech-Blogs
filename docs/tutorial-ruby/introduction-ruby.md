---
sidebar_position: 12
---

# Bài 1: Giới thiệu và Cài đặt Ruby

## Giới thiệu về Ruby

Ruby là một ngôn ngữ lập trình động, hướng đối tượng, được tạo ra bởi Yukihiro Matsumoto (thường được gọi là Matz) vào năm 1995. Với triết lý "tối ưu hóa sự hạnh phúc của lập trình viên", Ruby đã trở thành một trong những ngôn ngữ được yêu thích nhất trong cộng đồng phát triển phần mềm.

### Đặc điểm nổi bật:

1. **Cú pháp rõ ràng và dễ đọc**: Ruby được thiết kế để có cú pháp tự nhiên, gần gũi với ngôn ngữ tiếng Anh.
2. **Linh hoạt**: Mọi thứ trong Ruby đều là đối tượng, cho phép bạn mở rộng và sửa đổi các lớp có sẵn.
3. **Metaprogramming**: Ruby có khả năng mạnh mẽ trong việc viết code tự sinh code.
4. **Cộng đồng lớn mạnh**: Với RubyGems, bạn có thể dễ dàng tìm thấy và sử dụng các thư viện do cộng đồng phát triển.

### Ứng dụng phổ biến:

- Phát triển web với Ruby on Rails
- Scripting và automation
- DevOps và quản lý cấu hình (với Chef, Puppet)
- Phân tích dữ liệu và Machine Learning

## Cài đặt Ruby

Để bắt đầu với Ruby, bạn cần cài đặt nó trên máy tính của mình. Dưới đây là hướng dẫn cho các hệ điều hành phổ biến:

### Windows:

1. Truy cập https://rubyinstaller.org/
2. Tải xuống phiên bản Ruby+Devkit mới nhất
3. Chạy trình cài đặt và làm theo hướng dẫn

### macOS:

macOS đã có sẵn Ruby, nhưng tôi khuyên bạn nên sử dụng một version manager như rbenv:

```bash
# Cài Homebrew nếu chưa có
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Cài rbenv và ruby-build
brew install rbenv

# Thêm rbenv vào shell của bạn
echo 'eval "$(rbenv init -)"' >> ~/.zshrc  # hoặc ~/.bash_profile nếu bạn dùng bash

# Cài đặt Ruby (ví dụ version 3.2.2)
rbenv install 3.2.2
rbenv global 3.2.2
```

### Linux (Ubuntu/Debian):

```bash
sudo apt-get update
sudo apt-get install rbenv
rbenv install 3.2.2
rbenv global 3.2.2
```

## Kiểm tra cài đặt

Sau khi cài đặt, mở terminal và chạy:

```bash
ruby -v
```

Nếu bạn thấy version của Ruby, chúc mừng! Bạn đã cài đặt thành công.

## Chương trình Ruby đầu tiên

Hãy tạo file `hello.rb` với nội dung sau:

```ruby
puts "Hello, Ruby World!"
```

Chạy chương trình bằng lệnh:

```bash
ruby hello.rb
```

## Cú pháp cơ bản

### Biến

Ruby sử dụng dynamic typing. Bạn không cần khai báo kiểu dữ liệu:

```ruby
name = "Ruby"
age = 28
is_awesome = true
```

### Kiểu dữ liệu cơ bản

- Strings: `"Hello"`, `'World'`
- Numbers: `42`, `3.14`
- Booleans: `true`, `false`
- Arrays: `[1, 2, 3]`
- Hashes: `{ "key" => "value", another_key: "another value" }`

### Toán tử

Ruby hỗ trợ các toán tử thông thường: `+`, `-`, `*`, `/`, `%`, `**` (lũy thừa)

### Cấu trúc điều khiển

```ruby
# If-else
if condition
  # code
elsif another_condition
  # code
else
  # code
end

# Loop
5.times { puts "Ruby is fun!" }

# Iteration
[1, 2, 3].each do |number|
  puts number * 2
end
```

## Lời kết

Ruby là một ngôn ngữ mạnh mẽ và linh hoạt. Với cú pháp rõ ràng và cộng đồng hỗ trợ tích cực, Ruby là lựa chọn tuyệt vời cho cả người mới bắt đầu và lập trình viên có kinh nghiệm. Trong các bài tiếp theo, chúng ta sẽ đi sâu hơn vào các tính năng nâng cao của Ruby và cách áp dụng chúng trong thực tế.