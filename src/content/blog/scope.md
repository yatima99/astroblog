---
title: Rails ルーティングのscopeメソッドについて
slug: rails-scope
pubDate: '2023-09-12'
image: /assets/rails.png
category:
  - rails
description: Rails ルーティングのscopeメソッドについて解説
---

Ruby on Rails（Rails）において、ルーティングは非常に重要な概念です。ルーティングはクライアントからのリクエストを適切なコントローラーとアクションに振り分ける役割を果たします。Railsでは、このルーティングの設定を`config/routes.rb`ファイルで行います。この記事では、特に`scope`メソッドを使ったルーティングのカスタマイズに焦点を当てます。

## `scope`メソッドとは？

`scope`メソッドはルーティングのパスやURLのプレフィックスを共通化するためのメソッドです。これによって、コードがDRY（Don't Repeat Yourself）になり、管理も容易になります。

## 基本的な使い方

### パスのプレフィックスを追加する

以下の例では、`admin`というパスのプレフィックスを複数のルートに追加しています。

```ruby
# config/routes.rb

Rails.application.routes.draw do
  scope :admin do
    resources :users
    resources :products
  end
end
```

この設定により、以下のようなURLパターンが生成されます。

- `/admin/users`
- `/admin/products`

### コントローラの名前空間を指定する

`module`オプションを使用すると、特定の名前空間にあるコントローラを指定できます。

```ruby
# config/routes.rb

Rails.application.routes.draw do
  scope module: 'admin' do
    resources :users
    resources :products
  end
end
```

この場合、`Admin::UsersController`や`Admin::ProductsController`といったコントローラが対象になります。

### `path`と`as`オプション

`path`と`as`オプションを使うと、URLと名前付きルートにそれぞれ別のプレフィックスを指定できます。

```ruby
# config/routes.rb

Rails.application.routes.draw do
  scope path: 'admin', as: 'admin' do
    resources :users  # admin_users_path
    resources :products  # admin_products_path
  end
end
```

## ネストした`scope`

`scope`はネストさせることも可能です。

```ruby
# config/routes.rb

Rails.application.routes.draw do
  scope :admin do
    scope :dashboard do
      resources :stats
    end
  end
end
```

この設定により、`/admin/dashboard/stats`といったURLが生成されます。

## まとめ

`scope`メソッドを使うと、ルーティングの設定が一層柔軟になります。特定のグループのルートに対して一括でパスや名前空間を設定できるので、メンテナンスも容易になるでしょう。このメソッドを上手く利用することで、Railsアプリケーションのルーティングを効率的に管理できます。