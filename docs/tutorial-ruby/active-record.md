---
sidebar_position: 16
---

# Bài 5: Active Record Nâng cao

Active Record là ORM (Object-Relational Mapping) mạnh mẽ của Rails. Trong bài này, chúng ta sẽ đi sâu vào các tính năng nâng cao của Active Record.

## Associations

Associations cho phép bạn định nghĩa mối quan hệ giữa các models.

### Types of Associations

1. `belongs_to`
2. `has_one`
3. `has_many`
4. `has_many :through`
5. `has_one :through`
6. `has_and_belongs_to_many`

```ruby
class User < ApplicationRecord
  has_many :posts
  has_many :comments
  has_many :commented_posts, through: :comments, source: :post
end

class Post < ApplicationRecord
  belongs_to :user
  has_many :comments
  has_many :tags
  has_many :categories, through: :tags
end

class Comment < ApplicationRecord
  belongs_to :user
  belongs_to :post
end
```

### Polymorphic Associations

```ruby
class Image < ApplicationRecord
  belongs_to :imageable, polymorphic: true
end

class User < ApplicationRecord
  has_one :image, as: :imageable
end

class Product < ApplicationRecord
  has_one :image, as: :imageable
end
```

## Callbacks

Callbacks cho phép bạn hook vào lifecycle của Active Record objects.

```ruby
class User < ApplicationRecord
  before_save :normalize_email
  after_create :send_welcome_email
  before_destroy :ensure_not_admin

  private

  def normalize_email
    self.email = email.downcase
  end

  def send_welcome_email
    UserMailer.welcome(self).deliver_later
  end

  def ensure_not_admin
    throw :abort if admin?
  end
end
```

## Validations

Validations đảm bảo chỉ có dữ liệu hợp lệ được lưu vào database.

```ruby
class User < ApplicationRecord
  validates :email, presence: true, uniqueness: true,
                    format: { with: /\A[\w+\-.]+@[a-z\d\-]+(\.[a-z\d\-]+)*\.[a-z]+\z/i }
  validates :name, length: { minimum: 2, maximum: 50 }
  validates :terms_of_service, acceptance: true
  validate :password_complexity

  private

  def password_complexity
    return if password.blank? || password =~ /^(?=.*?[A-Z])(?=.*?[a-z])(?=.*?[0-9])(?=.*?[#?!@$%^&*-]).{8,70}$/
    errors.add :password, 'Complexity requirement not met. Please use: 1 uppercase, 1 lowercase, 1 digit and 1 special character'
  end
end
```

## Query Interface

Active Record cung cấp một interface mạnh mẽ để truy vấn database.

```ruby
# Chaining methods
users = User.where(active: true).order(created_at: :desc).limit(10)

# Eager loading
posts = Post.includes(:comments, :tags).where(published: true)

# Joining tables
users = User.joins(:posts).where(posts: { published: true })

# Custom SQL
User.find_by_sql("SELECT * FROM users WHERE active = 't'")
```

### Scopes

Scopes cho phép bạn định nghĩa các truy vấn thường xuyên sử dụng.

```ruby
class Post < ApplicationRecord
  scope :published, -> { where(published: true) }
  scope :recent, ->(days) { where("created_at > ?", days.days.ago) }

  # Equivalent to:
  # def self.published
  #   where(published: true)
  # end
end

Post.published.recent(7)
```

## N+1 Query Problem và Eager Loading

N+1 query là một vấn đề hiệu suất phổ biến. Eager loading giúp giải quyết vấn đề này.

```ruby
# N+1 problem
users = User.all
users.each do |user|
  puts user.posts.count  # Mỗi vòng lặp gây ra một query mới
end

# Solution with eager loading
users = User.includes(:posts)
users.each do |user|
  puts user.posts.size  # Không có thêm query nào được thực hiện
end
```

## Query Optimization

### Indexing

Indexes cải thiện hiệu suất truy vấn đáng kể.

```ruby
class AddIndexToUsersEmail < ActiveRecord::Migration[6.1]
  def change
    add_index :users, :email, unique: true
  end
end
```

### Counter Cache

Counter cache giúp tránh các truy vấn không cần thiết để đếm associations.

```ruby
class Comment < ApplicationRecord
  belongs_to :post, counter_cache: true
end

class Post < ApplicationRecord
  has_many :comments
end
```

## Advanced Techniques

### Single Table Inheritance (STI)

STI cho phép bạn sử dụng một bảng duy nhất cho nhiều models có liên quan.

```ruby
class Vehicle < ApplicationRecord
end

class Car < Vehicle
end

class Truck < Vehicle
end
```

### Overriding Accessors

Bạn có thể override các accessor methods mặc định.

```ruby
class User < ApplicationRecord
  def name
    "#{first_name} #{last_name}"
  end

  def name=(full_name)
    split = full_name.split(' ', 2)
    self.first_name = split.first
    self.last_name = split.last
  end
end
```

## Kết luận

Active Record là một công cụ mạnh mẽ trong hệ sinh thái Rails. Nó cung cấp một interface trực quan để tương tác với database, đồng thời cung cấp các tính năng nâng cao để xử lý các trường hợp phức tạp.

Trong thực tế, việc sử dụng Active Record hiệu quả đòi hỏi sự cân nhắc cẩn thận về hiệu suất và thiết kế database. Các kỹ thuật như eager loading, indexing, và caching nên được áp dụng một cách thông minh để đảm bảo ứng dụng của bạn có thể mở rộng quy mô.

Trong bài tiếp theo, chúng ta sẽ tìm hiểu về Views và Asset Pipeline trong Rails, khám phá cách Rails xử lý phần presentation của ứng dụng web.