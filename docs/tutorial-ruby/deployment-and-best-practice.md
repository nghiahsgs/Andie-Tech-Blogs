---
sidebar_position: 22
---

# Bài 10: Deployment và Best Practices

## Deployment Strategies

### Heroku

Heroku là một platform phổ biến để deploy ứng dụng Rails.

1. Cài đặt Heroku CLI
2. Tạo một Heroku app: `heroku create`
3. Push code lên Heroku: `git push heroku main`
4. Chạy migrations: `heroku run rails db:migrate`

### Amazon Web Services (AWS)

AWS cung cấp nhiều dịch vụ để host ứng dụng Rails:

1. EC2 để host application servers
2. RDS cho database
3. S3 cho file storage
4. CloudFront cho CDN

### Docker

Docker cho phép đóng gói ứng dụng và dependencies vào containers:

```dockerfile
# Dockerfile
FROM ruby:2.7
RUN apt-get update -qq && apt-get install -y nodejs postgresql-client
WORKDIR /myapp
COPY Gemfile /myapp/Gemfile
COPY Gemfile.lock /myapp/Gemfile.lock
RUN bundle install
COPY . /myapp

CMD ["rails", "server", "-b", "0.0.0.0"]
```

## Continuous Integration/Continuous Deployment (CI/CD)

Sử dụng CI/CD tools như Jenkins, Travis CI, hoặc GitHub Actions để tự động hóa quá trình testing và deployment.

Ví dụ với GitHub Actions:

```yaml
# .github/workflows/ci.yml
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Set up Ruby
      uses: ruby/setup-ruby@v1
      with:
        ruby-version: 2.7
    - name: Run tests
      run: |
        bundle install
        bundle exec rails db:setup
        bundle exec rspec
```

## Security Best Practices

### Bảo vệ khỏi SQL Injection

Sử dụng parameterized queries:

```ruby
User.where("name = ?", params[:name])
```

### Cross-Site Scripting (XSS) Prevention

Rails tự động escapes output. Đảm bảo không vô tình bypass tính năng này:

```erb
<%= @user.name %>  <!-- Safe -->
<%== @user.name %> <!-- Unsafe, bypasses escaping -->
```

### Cross-Site Request Forgery (CSRF) Protection

Rails có CSRF protection built-in. Đảm bảo nó được bật:

```ruby
class ApplicationController < ActionController::Base
  protect_from_forgery with: :exception
end
```

### Secure Headers

Sử dụng gem `secure_headers` để thiết lập các security headers:

```ruby
# config/initializers/secure_headers.rb
SecureHeaders::Configuration.default do |config|
  config.x_frame_options = "DENY"
  config.x_content_type_options = "nosniff"
  config.x_xss_protection = "1; mode=block"
  config.hsts = "max-age=31536000; includeSubDomains"
end
```

## Performance Monitoring

Sử dụng các tools như New Relic, Datadog, hoặc Skylight để monitor performance trong production.

## Error Tracking

Sử dụng Bugsnag, Sentry, hoặc Airbrake để track và notify errors trong production.

## Logging

Cấu hình logging phù hợp cho production environment:

```ruby
# config/environments/production.rb
config.log_level = :info
```

## Database Backups

Thiết lập regular backups cho database. Với Heroku:

```bash
heroku pg:backups:schedule DATABASE_URL --at '02:00 America/Los_Angeles'
```

## Scaling

### Horizontal Scaling

Thêm nhiều application servers để handle nhiều requests hơn.

### Vertical Scaling

Nâng cấp resources (CPU, RAM) cho servers hiện tại.

## Code Quality

### Rubocop

Sử dụng Rubocop để enforce coding standards:

```ruby
# .rubocop.yml
AllCops:
  NewCops: enable
  TargetRubyVersion: 2.7
```

### Code Reviews

Thực hiện code reviews thường xuyên để đảm bảo chất lượng code và chia sẻ kiến thức.

## Documentation

Duy trì documentation cập nhật, bao gồm README, API docs, và inline comments.

## Versioning

Sử dụng semantic versioning cho ứng dụng của bạn.

## Environment Variables

Sử dụng environment variables cho các cấu hình nhạy cảm:

```ruby
database_url = ENV['DATABASE_URL']
```

## Caching in Production

Cấu hình caching cho production environment:

```ruby
# config/environments/production.rb
config.action_controller.perform_caching = true
config.cache_store = :mem_cache_store
```

## Content Delivery Network (CDN)

Sử dụng CDN để serve static assets:

```ruby
# config/environments/production.rb
config.action_controller.asset_host = 'http://assets.example.com'
```

## Regular Updates

Giữ Rails và các gems cập nhật để có các tính năng mới nhất và các bản vá bảo mật.

## Monitoring và Alerting

Thiết lập monitoring và alerting để nhanh chóng phát hiện và respond to issues.

## Kết luận

Deployment và maintaining một ứng dụng Rails trong production là một quá trình phức tạp đòi hỏi sự chú ý đến nhiều khía cạnh khác nhau. Từ việc chọn strategy deployment phù hợp, đảm bảo bảo mật, monitoring performance, đến việc duy trì code quality và documentation, mỗi khía cạnh đều đóng vai trò quan trọng trong việc đảm bảo ứng dụng của bạn chạy smoothly và có khả năng scale.

Trong thực tế, việc áp dụng các best practices này nên được thực hiện từ sớm trong quá trình phát triển và được duy trì liên tục. Điều này không chỉ giúp ứng dụng của bạn sẵn sàng cho production mà còn giúp team phát triển làm việc hiệu quả hơn và giảm thiểu các issues trong tương lai.

Với series 10 bài học này, bạn đã có một cái nhìn tổng quan về Ruby on Rails, từ cơ bản đến nâng cao. Hy vọng rằng những kiến thức này sẽ giúp bạn trở thành một Rails developer giỏi hơn và xây dựng được những ứng dụng web mạnh mẽ và hiệu quả.