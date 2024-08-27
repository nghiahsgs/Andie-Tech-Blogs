---
sidebar_position: 21
---

# Bài 9: Performance Optimization và Caching

## Tầm quan trọng của Optimization

Tối ưu hóa hiệu suất là một phần quan trọng trong việc phát triển ứng dụng Rails, đặc biệt khi ứng dụng phát triển và số lượng người dùng tăng lên.

## Database Optimization

### Indexing

Indexing là một trong những cách hiệu quả nhất để cải thiện hiệu suất truy vấn.

```ruby
class AddIndexToUsersEmail < ActiveRecord::Migration[6.1]
  def change
    add_index :users, :email
  end
end
```

### Query Optimization

Sử dụng các phương pháp như eager loading để tránh N+1 queries:

```ruby
# Thay vì
@posts = Post.all
@posts.each do |post|
  puts post.user.name
end

# Sử dụng
@posts = Post.includes(:user)
@posts.each do |post|
  puts post.user.name
end
```

### Explain Queries

Sử dụng `explain` để phân tích và tối ưu các truy vấn phức tạp:

```ruby
User.where(status: 'active').explain
```

## Caching Strategies

Rails cung cấp nhiều cách để implement caching.

### Page Caching

Page caching lưu trữ toàn bộ output của một request.

```ruby
class WelcomeController < ApplicationController
  caches_page :index
  
  def index
    # ...
  end
end
```

### Action Caching

Action caching tương tự như page caching nhưng vẫn chạy qua before filters.

```ruby
class ProductsController < ApplicationController
  caches_action :index, :show
  
  def index
    @products = Product.all
  end
end
```

### Fragment Caching

Fragment caching cho phép bạn cache một phần của view.

```erb
<% cache @product do %>
  <h1><%= @product.name %></h1>
  <p><%= @product.description %></p>
<% end %>
```

### Russian Doll Caching

Russian Doll Caching là kỹ thuật nested fragment caching.

```erb
<% cache @post do %>
  <%= @post.title %>
  <% @post.comments.each do |comment| %>
    <% cache comment do %>
      <%= comment.body %>
    <% end %>
  <% end %>
<% end %>
```

### Low-Level Caching

Sử dụng Rails.cache để lưu trữ bất kỳ dữ liệu nào.

```ruby
Rails.cache.fetch("user_#{user.id}", expires_in: 12.hours) do
  user.calculate_expensive_stat
end
```

## Background Jobs

Sử dụng background jobs để xử lý các tác vụ nặng và không đồng bộ.

```ruby
class HardWorker
  include Sidekiq::Worker
  
  def perform(user_id)
    user = User.find(user_id)
    user.process_large_data
  end
end

HardWorker.perform_async(user.id)
```

## Rack Mini Profiler

Rack Mini Profiler là một gem hữu ích để phân tích hiệu suất ứng dụng Rails.

```ruby
# Gemfile
gem 'rack-mini-profiler'
```

## Bullet

Bullet giúp phát hiện và sửa các N+1 queries.

```ruby
# Gemfile
gem 'bullet', group: 'development'

# config/environments/development.rb
config.after_initialize do
  Bullet.enable = true
  Bullet.alert = true
  Bullet.bullet_logger = true
  Bullet.console = true
end
```

## Memcached hoặc Redis

Sử dụng Memcached hoặc Redis làm backend cho caching để cải thiện hiệu suất.

```ruby
# config/environments/production.rb
config.cache_store = :mem_cache_store
# hoặc
config.cache_store = :redis_cache_store, { url: ENV['REDIS_URL'] }
```

## Asset Pipeline Optimization

Tối ưu hóa Asset Pipeline để giảm thời gian tải trang.

```ruby
# config/environments/production.rb
config.assets.js_compressor = :uglifier
config.assets.css_compressor = :sass
config.assets.compile = false
```

## Content Delivery Network (CDN)

Sử dụng CDN để phân phối assets nhanh hơn.

```ruby
# config/environments/production.rb
config.action_controller.asset_host = 'http://assets.example.com'
```

## Database Connection Pooling

Cấu hình connection pooling để quản lý kết nối database hiệu quả.

```yaml
# config/database.yml
production:
  adapter: postgresql
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
```

## Query Caching

Rails tự động cache các truy vấn trong một request.

```ruby
User.where(name: 'John').to_a
# Truy vấn database lần đầu
User.where(name: 'John').to_a
# Sử dụng cache, không truy vấn database
```

## Monitoring và Profiling

Sử dụng các công cụ như New Relic hoặc Scout để monitor và profile ứng dụng trong production.

## Best Practices

1. Luôn đo lường trước và sau khi tối ưu.
2. Tập trung vào các bottlenecks lớn nhất trước.
3. Cẩn thận với premature optimization.
4. Sử dụng các công cụ profiling để xác định vấn đề thực sự.
5. Cân nhắc giữa performance và maintainability của code.

## Kết luận

Tối ưu hóa hiệu suất là một quá trình liên tục trong phát triển ứng dụng Rails. Bằng cách áp dụng các kỹ thuật caching, tối ưu hóa database, và sử dụng các công cụ phù hợp, bạn có thể cải thiện đáng kể hiệu suất của ứng dụng.

Trong thực tế, việc tối ưu hóa đòi hỏi sự cân bằng giữa hiệu suất, tính bảo trì, và thời gian phát triển. Điều quan trọng là phải hiểu rõ về ứng dụng của bạn, các patterns sử dụng, và nhu cầu của người dùng để áp dụng các kỹ thuật tối ưu hóa một cách hiệu quả nhất.

Trong bài cuối cùng, chúng ta sẽ tìm hiểu về Deployment và Best Practices trong Rails, khám phá cách đưa ứng dụng Rails của bạn lên production một cách an toàn và hiệu quả.