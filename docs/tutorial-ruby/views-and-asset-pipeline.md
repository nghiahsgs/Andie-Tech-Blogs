---
sidebar_position: 17
---

# Bài 6: Views và Asset Pipeline

## ERB và Layouts

ERB (Embedded Ruby) là template engine mặc định trong Rails, cho phép bạn nhúng Ruby code vào HTML.

### Cú pháp cơ bản của ERB

```erb
<% # Ruby code không xuất ra output %>
<%= # Ruby code có xuất ra output %>
```

### Layouts

Layouts định nghĩa cấu trúc chung cho các trang trong ứng dụng.

```erb
<!-- app/views/layouts/application.html.erb -->
<!DOCTYPE html>
<html>
  <head>
    <title><%= yield(:title) %></title>
    <%= csrf_meta_tags %>
    <%= stylesheet_link_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>
    <%= javascript_pack_tag 'application', 'data-turbolinks-track': 'reload' %>
  </head>
  <body>
    <%= yield %>
  </body>
</html>
```

## Helpers và Partials

### View Helpers

Helpers là modules chứa các methods hỗ trợ trong views.

```ruby
# app/helpers/application_helper.rb
module ApplicationHelper
  def full_title(page_title = '')
    base_title = "My Rails App"
    page_title.empty? ? base_title : "#{page_title} | #{base_title}"
  end
end
```

Sử dụng trong view:

```erb
<title><%= full_title(yield(:title)) %></title>
```

### Partials

Partials cho phép bạn chia nhỏ views thành các phần có thể tái sử dụng.

```erb
<!-- app/views/shared/_error_messages.html.erb -->
<% if object.errors.any? %>
  <div id="error_explanation">
    <h2><%= pluralize(object.errors.count, "error") %> prohibited this <%= object.class.name.downcase %> from being saved:</h2>
    <ul>
    <% object.errors.full_messages.each do |msg| %>
      <li><%= msg %></li>
    <% end %>
    </ul>
  </div>
<% end %>
```

Sử dụng partial:

```erb
<%= render 'shared/error_messages', object: @user %>
```

## Asset Pipeline

Asset Pipeline là một framework để nén và fingerprint JavaScript, CSS, và image files.

### Cấu trúc thư mục

```
app/assets
|- images
|- javascripts
|- stylesheets
```

### Manifest Files

Manifest files chỉ định các assets cần được bao gồm.

```js
// app/assets/javascripts/application.js
//= require rails-ujs
//= require turbolinks
//= require_tree .
```

```css
/* app/assets/stylesheets/application.css */
/*
 *= require_self
 *= require_tree .
 */
```

### Asset Helpers

Rails cung cấp các helper methods để link đến assets.

```erb
<%= stylesheet_link_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>
<%= javascript_include_tag 'application', 'data-turbolinks-track': 'reload' %>
<%= image_tag 'logo.png', alt: 'Logo' %>
```

## Webpacker

Từ Rails 6, Webpacker được sử dụng mặc định để quản lý JavaScript.

### Cấu hình Webpacker

```yaml
# config/webpacker.yml
default: &default
  source_path: app/javascript
  source_entry_path: packs
  public_root_path: public
  public_output_path: packs
  cache_path: tmp/cache/webpacker
  ...
```

### Sử dụng Webpacker

```erb
<%= javascript_pack_tag 'application' %>
<%= stylesheet_pack_tag 'application' %>
```

## AJAX trong Rails

Rails hỗ trợ AJAX thông qua UJS (Unobtrusive JavaScript).

### Remote Forms

```erb
<%= form_with(model: @post, remote: true) do |form| %>
  <%= form.text_field :title %>
  <%= form.submit %>
<% end %>
```

### Responding to AJAX requests

```ruby
# app/controllers/posts_controller.rb
def create
  @post = Post.new(post_params)
  respond_to do |format|
    if @post.save
      format.html { redirect_to @post, notice: 'Post was successfully created.' }
      format.js   # Renders create.js.erb
      format.json { render :show, status: :created, location: @post }
    else
      format.html { render :new }
      format.json { render json: @post.errors, status: :unprocessable_entity }
    end
  end
end
```

```javascript
// app/views/posts/create.js.erb
$('#posts').append('<%= j render(@post) %>');
$('#post_title').val('');
```

## Responsive Design với Rails

Rails hỗ trợ responsive design thông qua CSS và JavaScript.

### Responsive CSS

```scss
// app/assets/stylesheets/responsive.scss
@media (max-width: 768px) {
  .container {
    width: 100%;
  }
}
```

### Responsive Images

```erb
<%= image_tag 'logo.png', srcset: { 'logo@2x.png' => '2x', 'logo@3x.png' => '3x' }, alt: 'Logo' %>
```

## Internationalization (I18n)

Rails cung cấp framework I18n để hỗ trợ đa ngôn ngữ.

### Cấu hình I18n

```ruby
# config/application.rb
config.i18n.default_locale = :en
config.i18n.available_locales = [:en, :vi]
```

### Sử dụng I18n trong Views

```erb
<h1><%= t('welcome.title') %></h1>
<p><%= t('welcome.message', name: current_user.name) %></p>
```

```yaml
# config/locales/en.yml
en:
  welcome:
    title: "Welcome to our site!"
    message: "Hello, %{name}!"

# config/locales/vi.yml
vi:
  welcome:
    title: "Chào mừng đến với trang web của chúng tôi!"
    message: "Xin chào, %{name}!"
```

## Kết luận

Views và Asset Pipeline là những phần quan trọng trong việc xây dựng giao diện người dùng trong Rails. Chúng cung cấp các công cụ mạnh mẽ để tạo ra các ứng dụng web động và responsive.

Trong thực tế, việc sử dụng hiệu quả các công cụ này đòi hỏi sự cân nhắc về hiệu suất và khả năng bảo trì. Việc tổ chức code tốt, sử dụng partials và helpers một cách thông minh, và tối ưu hóa assets có thể cải thiện đáng kể trải nghiệm người dùng và khả năng phát triển của ứng dụng.

Trong bài tiếp theo, chúng ta sẽ tìm hiểu về Controllers và Request Handling trong Rails, khám phá cách Rails xử lý các yêu cầu từ người dùng và chuẩn bị dữ liệu cho Views.