---
sidebar_position: 14
---

# Bài 3: Ruby nâng cao

## Metaprogramming

Metaprogramming là khả năng viết chương trình có thể tạo ra hoặc sửa đổi code trong thời gian chạy. Đây là một trong những tính năng mạnh mẽ nhất của Ruby.

### method_missing

`method_missing` là một hook method được gọi khi một object nhận một message không tương ứng với bất kỳ method nào đã định nghĩa.

```ruby
class DynamicMethod
  def method_missing(name, *args)
    if name.to_s =~ /^find_by_(.+)$/
      # Tạo động method dựa trên tên được gọi
      define_singleton_method(name) do |value|
        puts "Finding by #{$1} with value #{value}"
      end
      send(name, *args)
    else
      super
    end
  end
end

obj = DynamicMethod.new
obj.find_by_name("Ruby")  # Output: Finding by name with value Ruby
obj.find_by_age(30)       # Output: Finding by age with value 30
```

### define_method

`define_method` cho phép bạn định nghĩa methods một cách động trong runtime.

```ruby
class Greeter
  ['hello', 'hi', 'hey'].each do |greeting|
    define_method "#{greeting}_world" do
      "#{greeting.capitalize}, World!"
    end
  end
end

greeter = Greeter.new
puts greeter.hello_world  # Hello, World!
puts greeter.hi_world     # Hi, World!
```

## Exception Handling

Xử lý ngoại lệ là một phần quan trọng của việc viết code robust.

```ruby
begin
  # Code có thể gây ra exception
  result = 10 / 0
rescue ZeroDivisionError => e
  puts "Caught exception: #{e.message}"
rescue StandardError => e
  puts "Caught a different exception: #{e.message}"
ensure
  puts "This will always run"
end
```

### Custom Exceptions

Tạo exception tùy chỉnh bằng cách kế thừa từ `StandardError`.

```ruby
class CustomError < StandardError
  def initialize(msg="My custom error")
    super
  end
end

begin
  raise CustomError.new("Something went wrong")
rescue CustomError => e
  puts e.message
end
```

## File I/O

Ruby cung cấp nhiều phương thức để làm việc với files.

### Đọc file

```ruby
File.open('example.txt', 'r') do |file|
  file.each_line do |line|
    puts line
  end
end

# Hoặc đọc toàn bộ file vào một string
content = File.read('example.txt')
```

### Ghi file

```ruby
File.open('output.txt', 'w') do |file|
  file.puts "Writing to a file."
  file.write "Another line.\n"
end
```

## Regular Expressions

Regular expressions (regex) trong Ruby rất mạnh mẽ và linh hoạt.

```ruby
text = "Ruby is awesome"

# Kiểm tra match
puts text.match?(/Ruby/)  # true

# Tìm và thay thế
new_text = text.gsub(/awesome/, 'amazing')
puts new_text  # Ruby is amazing

# Tách string
"a,b,c".split(/,/)  # ["a", "b", "c"]

# Capture groups
"Ruby 2.7.0".match(/(\w+)\s(\d+\.\d+\.\d+)/) do |m|
  puts "Language: #{m[1]}, Version: #{m[2]}"
end
```

## Garbage Collection và Memory Management

Ruby sử dụng garbage collection tự động để quản lý bộ nhớ.

### Garbage Collection cơ bản

```ruby
GC.start  # Kích hoạt garbage collection thủ công

# Kiểm tra số lượng objects trước và sau GC
before = ObjectSpace.count_objects
GC.start
after = ObjectSpace.count_objects
puts "Objects freed: #{before[:TOTAL] - after[:TOTAL]}"
```

### Weak References

Weak references cho phép bạn giữ tham chiếu đến một object mà không ngăn nó bị garbage collected.

```ruby
require 'weakref'

obj = Object.new
weak = WeakRef.new(obj)

puts weak.weakref_alive?  # true

obj = nil
GC.start

puts weak.weakref_alive?  # false
```

## Tối ưu hóa hiệu suất

### Profiling

Ruby cung cấp công cụ profiling để phân tích hiệu suất chương trình.

```ruby
require 'profile'

def slow_method
  sleep(1)
end

10.times { slow_method }
```

Chạy script với `ruby -rprofile script.rb` sẽ in ra thông tin profiling.

### Benchmark

Module `Benchmark` giúp đo thời gian thực thi của code.

```ruby
require 'benchmark'

n = 1_000_000
Benchmark.bm do |x|
  x.report("for:")   { for i in 1..n; a = "1"; end }
  x.report("times:") { n.times do   ; a = "1"; end }
  x.report("upto:")  { 1.upto(n) do ; a = "1"; end }
end
```

## Kết luận

Ruby nâng cao mở ra một thế giới mới với các khả năng mạnh mẽ như metaprogramming, xử lý ngoại lệ tinh vi, và quản lý bộ nhớ hiệu quả. Tuy nhiên, với sức mạnh lớn đi kèm trách nhiệm lớn. Khi sử dụng các kỹ thuật nâng cao này, luôn cân nhắc về tính dễ đọc và bảo trì của code.

Trong thực tế, việc sử dụng metaprogramming nên được giới hạn cho những trường hợp thực sự cần thiết, và nên được document cẩn thận. Xử lý ngoại lệ nên được thiết kế để bắt và xử lý các lỗi một cách graceful. Khi làm việc với file I/O, hãy luôn nhớ đóng file sau khi sử dụng (hoặc sử dụng block form để Ruby tự động xử lý việc này).

Trong các bài tiếp theo, chúng ta sẽ khám phá Ruby on Rails và cách áp dụng những kiến thức Ruby nâng cao này trong phát triển web.