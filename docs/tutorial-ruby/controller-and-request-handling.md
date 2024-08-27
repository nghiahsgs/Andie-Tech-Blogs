---
sidebar_position: 18
---

# Bài 7: Controllers và Request Handling

## Vai trò của Controllers

Controllers trong Rails đóng vai trò trung gian giữa Models và Views. Chúng xử lý requests, tương tác với Models, và chuẩn bị dữ liệu cho Views.

## Cấu trúc cơ bản của một Controller

```ruby
class UsersController < ApplicationController
  def index
    @users = User.all
  end

  def show
    @user = User.find(params[:id])
  end

  def new
    @user = User.new
  end

  def create
    @user = User.new(user_params)
    if @user.save
      redirect_to @user, notice: 'User was successfully created.'
    else
      render :new
    end
  end

  private

  def user_params
    params.require(:user).permit(:name, :email, :password)
  end
end
```

## Filters và Callbacks

Filters cho phép bạn chạy code trước, sau hoặc xung quanh các actions của controller.

```ruby
class ApplicationController < ActionController::Base
  before_action :require_login
  after_action :log_action
  around_action :wrap_in_transaction, only: [:create, :update, :destroy]

  private

  def require_login
    unless current_user
      flash[:error] = "You must be logged in to access this section"
      redirect_to login_url
    end
  end

  def log_action
    Rails.logger.info "Action completed: #{action_name}"
  end

  def wrap_in_transaction
    ActiveRecord::Base.transaction do
      yield
    end
  end
end
```

## Strong Parameters

Strong Parameters là một tính năng bảo mật giúp kiểm soát dữ liệu được phép truyền vào trong mass assignment.

```ruby
class UsersController < ApplicationController
  def create
    @user = User.new(user_params)
    # ...
  end

  private

  def user_params
    params.require(:user).permit(:name, :email, :password)
  end
end
```

## Session và Cookies

Rails cung cấp các cơ chế để lưu trữ dữ liệu giữa các requests.

```ruby
class SessionsController < ApplicationController
  def create
    user = User.authenticate(params[:email], params[:password])
    if user
      session[:user_id] = user.id
      redirect_to root_url, notice: "Logged in!"
    else
      flash.now.alert = "Invalid email or password"
      render "new"
    end
  end

  def destroy
    session[:user_id] = nil
    redirect_to root_url, notice: "Logged out!"
  end
end
```

## API development với Rails

Rails cung cấp các công cụ mạnh mẽ để xây dựng APIs.

```ruby
class Api::V1::UsersController < ApplicationController
  skip_before_action :verify_authenticity_token
  before_action :authenticate_token

  def index
    @users = User.all
    render json: @users
  end

  def show
    @user = User.find(params[:id])
    render json: @user
  end

  private

  def authenticate_token
    unless valid_token?
      render json: { error: 'Invalid token' }, status: :unauthorized
    end
  end

  def valid_token?
    request.headers['Authorization'] == 'Bearer valid_token'
  end
end
```

## Request và Response Cycle

Hiểu rõ về chu trình request-response rất quan trọng trong Rails.

1. Request đến server
2. Router xác định controller và action
3. Controller xử lý request
4. Controller tương tác với Model (nếu cần)
5. Controller chuẩn bị dữ liệu cho View
6. View render kết quả
7. Response được gửi lại cho client

## Exception Handling

Xử lý ngoại lệ một cách graceful là rất quan trọng trong Rails.

```ruby
class ApplicationController < ActionController::Base
  rescue_from ActiveRecord::RecordNotFound, with: :record_not_found
  rescue_from User::NotAuthorized, with: :user_not_authorized

  private

  def record_not_found
    render plain: "404 Not Found", status: 404
  end

  def user_not_authorized
    flash[:alert] = "You are not authorized to perform this action."
    redirect_to(request.referrer || root_path)
  end
end
```

## Caching trong Controllers

Caching có thể cải thiện đáng kể hiệu suất của ứng dụng.

```ruby
class ProductsController < ApplicationController
  def index
    @products = Rails.cache.fetch("products", expires_in: 12.hours) do
      Product.all.to_a
    end
  end
end
```

## Concerns

Concerns cho phép bạn chia sẻ logic giữa các controllers.

```ruby
# app/controllers/concerns/authenticatable.rb
module Authenticatable
  extend ActiveSupport::Concern

  included do
    before_action :require_login
  end

  private

  def require_login
    unless current_user
      redirect_to login_path, alert: "Please log in first"
    end
  end
end

class UsersController < ApplicationController
  include Authenticatable
  # ...
end
```

## Testing Controllers

Testing controllers là một phần quan trọng trong phát triển Rails.

```ruby
# test/controllers/users_controller_test.rb
require 'test_helper'

class UsersControllerTest < ActionDispatch::IntegrationTest
  test "should get index" do
    get users_url
    assert_response :success
  end

  test "should create user" do
    assert_difference('User.count') do
      post users_url, params: { user: { name: 'John', email: 'john@example.com' } }
    end

    assert_redirected_to user_url(User.last)
  end
end
```

## Kết luận

Controllers đóng vai trò trung tâm trong ứng dụng Rails, xử lý requests, tương tác với models, và chuẩn bị dữ liệu cho views. Hiểu rõ về cách hoạt động của controllers và các kỹ thuật xử lý requests sẽ giúp bạn xây dựng các ứng dụng Rails mạnh mẽ và linh hoạt.

Trong thực tế, việc giữ cho controllers "gầy" (thin) bằng cách di chuyển logic phức tạp vào models hoặc service objects là một practice tốt. Điều này giúp code dễ đọc, dễ bảo trì và dễ test hơn.

Trong bài tiếp theo, chúng ta sẽ đi sâu vào việc testing trong Ruby on Rails, một khía cạnh quan trọng để đảm bảo chất lượng và độ tin cậy của ứng dụng.