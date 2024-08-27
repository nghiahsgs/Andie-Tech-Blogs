---
sidebar_position: 19
---

# Bài 8: Testing trong Ruby và Rails

## Tầm quan trọng của Testing

Testing là một phần không thể thiếu trong quá trình phát triển phần mềm. Nó giúp đảm bảo rằng code hoạt động như mong đợi, giảm thiểu bugs, và tạo điều kiện cho việc refactor và mở rộng code trong tương lai.

## Test-Driven Development (TDD)

TDD là một phương pháp phát triển phần mềm trong đó bạn viết test trước khi viết code thực tế.

1. Viết một test mô tả chức năng mong muốn
2. Chạy test và xem nó fail
3. Viết code đủ để pass test
4. Refactor code nếu cần
5. Lặp lại quá trình

## RSpec - Testing Framework phổ biến trong Rails

RSpec là một testing framework BDD (Behavior-Driven Development) cho Ruby.

### Cài đặt RSpec

Thêm vào Gemfile:

```ruby
group :development, :test do
  gem 'rspec-rails'
end
```

Sau đó chạy:

```bash
rails generate rspec:install
```

### Ví dụ về Model Spec

```ruby
# spec/models/user_spec.rb
require 'rails_helper'

RSpec.describe User, type: :model do
  it "is valid with valid attributes" do
    user = User.new(name: "John Doe", email: "john@example.com")
    expect(user).to be_valid
  end

  it "is not valid without a name" do
    user = User.new(email: "john@example.com")
    expect(user).to_not be_valid
  end

  it "is not valid without an email" do
    user = User.new(name: "John Doe")
    expect(user).to_not be_valid
  end

  it "is not valid with a duplicate email" do
    User.create(name: "John Doe", email: "john@example.com")
    user = User.new(name: "Jane Doe", email: "john@example.com")
    expect(user).to_not be_valid
  end
end
```

### Controller Spec

```ruby
# spec/controllers/users_controller_spec.rb
require 'rails_helper'

RSpec.describe UsersController, type: :controller do
  describe "GET #index" do
    it "returns a success response" do
      get :index
      expect(response).to be_successful
    end
  end

  describe "POST #create" do
    context "with valid params" do
      it "creates a new User" do
        expect {
          post :create, params: { user: { name: "John", email: "john@example.com" } }
        }.to change(User, :count).by(1)
      end
    end
  end
end
```

## Factories với FactoryBot

FactoryBot giúp tạo ra test data một cách dễ dàng và linh hoạt.

### Cài đặt FactoryBot

Thêm vào Gemfile:

```ruby
group :development, :test do
  gem 'factory_bot_rails'
end
```

### Định nghĩa Factory

```ruby
# spec/factories/users.rb
FactoryBot.define do
  factory :user do
    name { "John Doe" }
    sequence(:email) { |n| "user#{n}@example.com" }
    password { "password123" }

    trait :admin do
      admin { true }
    end
  end
end
```

### Sử dụng Factory trong tests

```ruby
RSpec.describe User, type: :model do
  it "has a valid factory" do
    expect(FactoryBot.build(:user)).to be_valid
  end

  it "can create an admin user" do
    admin = FactoryBot.create(:user, :admin)
    expect(admin.admin).to be true
  end
end
```

## System Tests với Capybara

System tests cho phép bạn test ứng dụng từ góc độ của người dùng.

### Cài đặt Capybara

Capybara đã được tích hợp sẵn trong Rails 5.1+. Bạn chỉ cần thêm webdrivers:

```ruby
group :test do
  gem 'webdrivers'
end
```

### Viết System Test

```ruby
# spec/system/user_signs_up_spec.rb
require 'rails_helper'

RSpec.describe "User signs up", type: :system do
  before do
    driven_by(:rack_test)
  end

  it "allows a user to sign up" do
    visit new_user_registration_path
    fill_in "Name", with: "John Doe"
    fill_in "Email", with: "john@example.com"
    fill_in "Password", with: "password123"
    fill_in "Password confirmation", with: "password123"
    click_button "Sign up"

    expect(page).to have_content("Welcome! You have signed up successfully.")
  end
end
```

## Mocking và Stubbing với RSpec Mocks

Mocking và stubbing cho phép bạn isolate các phần của code để test.

```ruby
RSpec.describe UsersController, type: :controller do
  describe "GET #show" do
    it "returns user details" do
      user = instance_double("User", id: 1, name: "John Doe")
      allow(User).to receive(:find).with("1").and_return(user)

      get :show, params: { id: "1" }
      expect(response).to be_successful
    end
  end
end
```

## Test Coverage với SimpleCov

SimpleCov giúp bạn theo dõi test coverage của ứng dụng.

Thêm vào Gemfile:

```ruby
group :test do
  gem 'simplecov', require: false
end
```

Thêm vào đầu file `spec/rails_helper.rb`:

```ruby
require 'simplecov'
SimpleCov.start 'rails'
```

## Continuous Integration (CI)

Sử dụng CI tools như Jenkins, Travis CI, hoặc GitHub Actions để tự động chạy tests mỗi khi có thay đổi trong code.

## Best Practices trong Testing

1. Viết tests có ý nghĩa và mô tả rõ ràng hành vi mong muốn.
2. Giữ cho mỗi test độc lập với các tests khác.
3. Sử dụng factories để tạo test data.
4. Tránh over-testing - tập trung vào các chức năng quan trọng.
5. Chạy tests thường xuyên, không chỉ trước khi deploy.
6. Duy trì test suite nhanh để khuyến khích việc chạy tests thường xuyên.

## Kết luận

Testing là một kỹ năng quan trọng trong phát triển Rails. Nó không chỉ giúp đảm bảo chất lượng code mà còn cung cấp documentation sống về cách ứng dụng hoạt động. Với các công cụ như RSpec, FactoryBot, và Capybara, Rails cung cấp một hệ sinh thái testing mạnh mẽ và linh hoạt.

Trong thực tế, việc duy trì một test suite toàn diện đòi hỏi thời gian và công sức, nhưng lợi ích lâu dài của nó là rất lớn. Tests tốt giúp bạn tự tin hơn khi thay đổi code, giảm thời gian debug, và cải thiện chất lượng tổng thể của ứng dụng.

Trong bài tiếp theo, chúng ta sẽ tìm hiểu về Performance Optimization và Caching trong Rails, khám phá cách làm cho ứng dụng Rails của bạn nhanh hơn và có khả năng mở rộng tốt hơn.