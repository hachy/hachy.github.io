---
layout: post
title: Rails アプリに Webpacker を導入する
tags: rails
requirements:
- Rails 5.1.4
- webpacker 3.0.2
---

既存の Rails アプリに Webpacker を導入したいと思います。

Webpacker は JavaScript だけを管理して css や画像は従来通り Sprockets 等にまかせる、といった使い方をすることもできます。

しかし今回は JavaScript/Css/Image すべてを Webpacker で管理することを目標にやっていきます。

## Webpacker をインストール

Gemfile に `webpacker` を追加してインストールします。

```ruby:Gemfile
gem 'webpacker'
```

```shell
bundle
bin/rails webpacker:install
```

## ディレクトリ構成

ディレクトリ構成は以下のようにしてみました。

```yml
app/javascript:
  ├── packs:
  │   └── application.js
  └── javascripts:
  │   └── application.js
  └── stylesheets:
  │   └── application.scss
  └── images:
      └── logo.svg
```

`app/javascript` の部分は変えられます。

`config/webpacker.yml` の `source_path: app/javascript` を自分の好きなように変えてみましょう。

個人的に `app/javascript` 配下に css 等があるのは違和感しかないですが、今回はデフォルトのままでいきます。

## 使い方

`app/javascript/packs/application.js` をエントリーファイルとして Javascript や CSS ファイルをインポートします。

```javascript:app/javascript/packs/application.js
import '../javascripts/application';
import '../stylesheets/application';

require.context('../images', true, /\.(png|jpg|jpeg|svg)$/);

console.log('Hello World from Webpacker');
```

views の head タグの中を変更します。

`stylesheet_link_tag` `javascript_include_tag` は削除します。
（削除せずに併用することもできます）

```erb:app/views/layouts/application.html.erb
<%= javascript_pack_tag 'application' %>
<%= stylesheet_pack_tag 'application' %>
```

`bin/rails s` でサーバをたちあげてみましょう。

コンソールで `Hello World from Webpacker` とでているのが確認できるはずです。

ちなみに画像は `asset_pack_path` helper を使います。

```erb
<img src="<%= asset_pack_path 'images/logo.svg' %>" />
```

## ライブラリをYarnでインストール

例として `rails-ujs` と `jQuery` をいれてみます。

### rails-ujs

Asset pipeline を使っている場合は `//= require rails-ujs` でしたが、
Webpacker を使う場合は `yarn add` して以下のようにします。

```shell
yarn add rails-ujs
```

```javascript:app/javascript/javascripts/application.js
import Rails from 'rails-ujs';

Rails.start();
```

### jQuery

jQuery は Yarn でインストールしたら `config/webpack/environment.js` を以下のように編集すると使えるようになります。

```shell
yarn add jquery
```

```javascript:config/webpack/environment.js
const { environment } = require('@rails/webpacker');
const webpack = require('webpack');

environment.plugins.set(
  'Provide',
  new webpack.ProvidePlugin({
    $: 'jquery',
    jQuery: 'jquery',
    jquery: 'jquery',
  }),
);

module.exports = environment;
```

その他のライブラリも `yarn add` で追加できます。

以上 Webpacker 導入の備忘録でした。


参考

- <https://github.com/rails/webpacker>
- <https://webpack.js.org/guides/dependency-management/#context-module-api>
- <https://github.com/rails/webpacker/issues/705>
- <http://blog.tai2.net/webpacker3.html>
- <https://github.com/rails/rails/tree/master/actionview/app/assets/javascripts>
- <https://github.com/rails/webpacker/blob/master/docs/webpack.md>
