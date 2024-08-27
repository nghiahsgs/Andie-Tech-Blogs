---
sidebar_position: 13
---

# Bài 2: Lập trình hướng đối tượng trong Ruby

Lập trình hướng đối tượng (OOP) là một trong những điểm mạnh của Ruby. Hiểu và áp dụng tốt OOP sẽ giúp bạn viết code Ruby hiệu quả và dễ bảo trì hơn.

## Classes và Objects

Trong Ruby, mọi thứ đều là object. Classes là bản thiết kế để tạo ra các objects.

```ruby
class Cat
  def initialize(name)
    @name = name
  end

  def speak
    puts "#{@name} says Meow!"
  end
end

kitty = Cat.new("Whiskers")
kitty.speak  # Output: Whiskers says Meow!
```

### Accessor Methods

Ruby cung cấp các shorthand để tạo getter và setter methods:

```ruby
class Cat
  attr_reader :name        # Tạo getter
  attr_writer :age         # Tạo setter
  attr_accessor :color     # Tạo cả getter và setter

  def initialize(name, age, color)
    @name = name
    @age = age
    @color = color
  end
end

kitty = Cat.new("Whiskers", 3, "white")
puts kitty.name            # Whiskers
kitty.age = 4              # Sử dụng setter
puts kitty.color           # white
kitty.color = "black"      # Sử dụng setter
```

## Inheritance (Kế thừa)

Kế thừa cho phép một class kế thừa các thuộc tính và phương thức từ một class khác.

```ruby
class Animal
  def speak
    puts "Animal makes a sound"
  end
end

class Dog < Animal
  def speak
    puts "Dog barks"
  end
end

class Cat < Animal
  def speak
    super
    puts "Cat meows"
  end
end

Dog.new.speak  # Dog barks
Animal.new.speak  # Animal makes a sound
Cat.new.speak   # Cat meows
```

## Modules và Mixins

Modules là một cách để nhóm các methods, classes, và constants. Chúng cung cấp namespace và hỗ trợ mixin functionality.

```ruby
module Swimmable
  def swim
    puts "#{self.class} is swimming"
  end
end

class Fish
  include Swimmable
end

class Duck
  include Swimmable
end

Fish.new.swim  # Fish is swimming
Duck.new.swim  # Duck is swimming
```

### Namespace

```ruby
module Animals
  class Dog
    def speak
      puts "Woof!"
    end
  end
end

dog = Animals::Dog.new
dog.speak  # Woof!
```

## Duck Typing

Ruby sử dụng duck typing: "Nếu nó đi như vịt và kêu như vịt, thì nó là vịt". Điều này cho phép tính linh hoạt cao trong code.

```ruby
def make_sound(animal)
  animal.speak
end

class Dog
  def speak
    puts "Woof!"
  end
end

class Cat
  def speak
    puts "Meow!"
  end
end

make_sound(Dog.new)  # Woof!
make_sound(Cat.new)  # Meow!
```

## Blocks, Procs, và Lambdas

Đây là các cách khác nhau để đóng gói và truyền các đoạn code trong Ruby.

### Blocks

Blocks là đoạn code được đặt trong `do...end` hoặc `{}`. Chúng thường được sử dụng với các iterators.

```ruby
[1, 2, 3].each do |num|
  puts num * 2
end

# Hoặc
[1, 2, 3].each { |num| puts num * 2 }
```

### Procs

Procs là objects của blocks, cho phép bạn lưu trữ và tái sử dụng blocks.

```ruby
double = Proc.new { |x| x * 2 }
puts double.call(5)  # 10

[1, 2, 3].map(&double)  # [2, 4, 6]
```

### Lambdas

Lambdas tương tự như Procs nhưng có một số khác biệt nhỏ về cách xử lý arguments và return.

```ruby
say_hello = lambda { |name| puts "Hello, #{name}!" }
say_hello.call("Ruby")  # Hello, Ruby!

# Hoặc sử dụng cú pháp rút gọn
multiply = ->(x, y) { x * y }
puts multiply.call(3, 4)  # 12
```

## Kết luận

Lập trình hướng đối tượng trong Ruby cung cấp một cách mạnh mẽ và linh hoạt để tổ chức code. Bằng cách sử dụng classes, modules, và các tính năng như duck typing, bạn có thể tạo ra code dễ đọc, dễ bảo trì và có khả năng tái sử dụng cao.

Trong thực tế, việc sử dụng OOP hiệu quả đòi hỏi sự cân nhắc cẩn thận về thiết kế và tổ chức code. Hãy nhớ rằng, mục tiêu cuối cùng là tạo ra code clean, DRY (Don't Repeat Yourself), và dễ hiểu.

Trong các bài tiếp theo, chúng ta sẽ đi sâu hơn vào các khía cạnh nâng cao của Ruby, bao gồm metaprogramming và các kỹ thuật tối ưu hóa hiệu suất.