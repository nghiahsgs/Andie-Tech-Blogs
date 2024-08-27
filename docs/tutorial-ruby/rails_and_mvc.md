---
sidebar_position: 15
---

# Bài 4: Giới thiệu Rails và MVC

## Rails Framework Overview

Ruby on Rails, thường được gọi tắt là Rails, là một web application framework mạnh mẽ được viết bằng Ruby. Rails tuân theo hai nguyên tắc cốt lõi: "Convention over Configuration" (CoC) và "Don't Repeat Yourself" (DRY).

### Cài đặt Rails

```bash
gem install rails
```

### Tạo ứng dụng Rails mới

```bash
rails new my_app
cd my_app
```

## Model-View-Controller (MVC) Architecture

Rails sử dụng mô hình MVC để tổ chức code:

- **Model**: Xử lý dữ liệu và logic nghiệp vụ
- **View**: Hiển thị dữ liệu cho người dùng
- **Controller**: Điều phối tương tác giữa Model và View

### Model

Models trong Rails thường kế thừa từ `ApplicationRecord`:

```ruby
# app/models/user.rb
class User < ApplicationRecord
  validates :email, presence: true, uniqueness: true
  has_many :posts
end
```

### View

Views trong Rails sử dụng ERB (Embedded Ruby) mặc định:

```erb
<!-- app/views/users/index.html.erb -->
<h1>Users</h1>
<% @users.each do |user| %>
  <p><%= user.name %> (<%= user.email %>)</p>
<% end %>
```

### Controller

Controllers xử lý requests và chuẩn bị dữ liệu cho views:

```ruby
# app/controllers/users_controller.rb
class UsersController < ApplicationController
  def index
    @users = User.all
  end

  def show
    @user = User.find(params[:id])
  end
end
```

## Routing và RESTful Design

Rails sử dụng RESTful routing mặc định.

### Định nghĩa Routes

```ruby
# config/routes.rb
Rails.application.routes.draw do
  resources :users do
    resources :posts
  end
end
```

Lệnh này tạo ra các RESTful routes cho users và posts.

### Xem tất cả routes

```bash
rails routes
```

### Custom Routes

```ruby
get '/about', to: 'pages#about'
```

## Active Record Basics

Active Record là ORM (Object-Relational Mapping) của Rails, cung cấp interface giữa models và database.

### Tạo Model và Migration

```bash
rails generate model User name:string email:string
rails db:migrate
```

### CRUD Operations

```ruby
# Create
user = User.create(name: "John Doe", email: "john@example.com")

# Read
user = User.find(1)
users = User.where(name: "John Doe")

# Update
user.update(name: "Jane Doe")

# Destroy
user.destroy
```

### Validations

```ruby
class User < ApplicationRecord
  validates :name, presence: true
  validates :email, uniqueness: true, format: { with: /\A[\w+\-.]+@[a-z\d\-]+(\.[a-z\d\-]+)*\.[a-z]+\z/i }
end
```

### Callbacks

```ruby
class User < ApplicationRecord
  before_save :normalize_email
  after_create :send_welcome_email

  private

  def normalize_email
    self.email = email.downcase
  end

  def send_welcome_email
    UserMailer.welcome(self).deliver_later
  end
end
```

## Asset Pipeline và Webpacker

Rails cung cấp Asset Pipeline để quản lý static assets (CSS, JavaScript, images). Từ Rails 6, Webpacker được sử dụng mặc định cho JavaScript.

### Asset Pipeline

```ruby
# app/assets/stylesheets/application.css
/*
 *= require_tree .
 *= require_self
 */
```

### Webpacker

```bash
rails webpacker:install
```

```javascript
// app/javascript/packs/application.js
import 'core-js/stable'
import 'regenerator-runtime/runtime'
```

## Tích hợp với Front-end Frameworks

Rails có thể tích hợp dễ dàng với các front-end frameworks như React, Vue, hoặc Angular.

### Ví dụ với React

```bash
rails webpacker:install:react
```

```ruby
# app/views/layouts/application.html.erb
<%= javascript_pack_tag 'application' %>
```

```javascript
// app/javascript/components/HelloWorld.jsx
import React from 'react'

const HelloWorld = ({name}) => (
  <div>Hello {name}!</div>
)

export default HelloWorld
```

## Testing trong Rails

Rails cung cấp framework testing tích hợp.

### Unit Testing

```ruby
# test/models/user_test.rb
require 'test_helper'

class UserTest < ActiveSupport::TestCase
  test "should not save user without email" do
    user = User.new
    assert_not user.save, "Saved the user without an email"
  end
end
```

### Integration Testing

```ruby
# test/integration/user_flows_test.rb
require 'test_helper'

class UserFlowsTest < ActionDispatch::IntegrationTest
  test "can see the welcome page" do
    get "/"
    assert_select "h1", "Welcome#index"
  end
end
```

## Kết luận

Ruby on Rails là một framework mạnh mẽ cho phát triển web nhanh chóng và hiệu quả. Với cấu trúc MVC rõ ràng, routing linh hoạt, và ORM mạnh mẽ, Rails cho phép các nhà phát triển tập trung vào logic kinh doanh thay vì chi tiết triển khai.

Trong thực tế, việc sử dụng Rails hiệu quả đòi hỏi hiểu biết sâu sắc về các quy ước và "magic" của framework. Tuy nhiên, khi được sử dụng đúng cách, Rails có thể giúp bạn xây dựng các ứng dụng web phức tạp một cách nhanh chóng và dễ bảo trì.

Trong các bài tiếp theo, chúng ta sẽ đi sâu hơn vào các khía cạnh cụ thể của Rails, bao gồm Active Record nâng cao, testing, và các kỹ thuật tối ưu hóa hiệu suất.