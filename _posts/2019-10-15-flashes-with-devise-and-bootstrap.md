---
layout: post
title: DeviseのflashにBootstrapを使う
tags: rails
requirements:
- rails 6.0.0
- devise 4.7.1
- bootstrap 4.3.1
date: 2019-10-15 23:39 +0900
---
Deviseのflash keyにはデフォルトで`notice`や`alert`が使われていますが、これを他のkeyで置き換えたい。

例えばDeviseとBootstrapを組み合わせて使うときにBootstrapのAlertには`.alert-notice`クラスはないので、それを置き換えて、Bootstrapで定義されている`.alert-success`クラスを使えるようにします。

## helperをつくる

まずは"devise_helper.rb"というファイルを新たに作成。

ここではDeviseで使われているkeyをBootstrapのAlertで使われているクラス名に置き換えています。

```ruby:app/helpers/devise_helper.rb
module DeviseHelper
  def bootstrap_alert(key)
    case key
    when "alert"
      "warning"
    when "notice"
      "success"
    when "error"
      "danger"
    end
  end
end
```

## view

Viewでは新しく"app/views/layouts/_flashes.html.erb"ファイルを作成し以下のようにします。

```html:app/views/layouts/_flashes.html.erb
<% flash.each do |key, value| %>
  <div class="alert alert-<%= bootstrap_alert(key) %>">
    <strong>
      <%= value %>
    </strong>
  </div>
<% end %>
```

あとは"app/views/layouts/application.html.erb"とかに`<%= render 'layouts/flashes' %>`を追加すればOKです。

###### 参考

- <https://github.com/plataformatec/devise/issues/2051#issuecomment-381444285>
