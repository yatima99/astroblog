---
title: Railsにおけるポリモーフィック関連の基礎
slug: rails-polimofic
pubDate: '2023-09-15'
image: /assets/rails.png
category:
  - rails
description: Railsにおけるポリモーフィック関連の基礎について解説
---

Ruby on Railsにおいて、「ポリモーフィック関連」は複数のモデルが同一のモデルに関連するケースでよく使用される。

## ポリモーフィック関連とは？

ポリモーフィック関連を使うことで、一つのモデルが複数の異なるモデルと関連を持つことができる。

## 実装例

`Comment`モデルが`Article`と`Product`に関連する例。

### モデルの設定

まずはモデルの設定。`Comment`モデルには`commentable`という名前でポリモーフィック関連を定義。

```ruby
# app/models/comment.rb
class Comment < ApplicationRecord
  belongs_to :commentable, polymorphic: true
end
```

`Article`と`Product`モデルでは、`has_many`を用いて`Comment`との関連を定義。

```ruby
# app/models/article.rb
class Article < ApplicationRecord
  has_many :comments, as: :commentable
end

# app/models/product.rb
class Product < ApplicationRecord
  has_many :comments, as: :commentable
end
```

### データベースの設定

データベースでも少し特別な設定が必要。`comments`テーブルには、`commentable_type`と`commentable_id`という2つのカラムを追加。

```ruby
# db/migrate/xxxx_create_comments.rb
class CreateComments < ActiveRecord::Migration[6.0]
  def change
    create_table :comments do |t|
      t.text :content
      t.references :commentable, polymorphic: true, null: false
      t.timestamps
    end
  end
end
```

### データの操作

データの作成や取得は以下のように行える。

```ruby
# コメントを作成
article = Article.find(1)
comment_for_article = article.comments.create(content: 'Great article!')

product = Product.find(1)
comment_for_product = product.comments.create(content: 'Awesome product!')

# コメントを取得
article_comments = article.comments
product_comments = product.comments
```

## まとめ

Railsのポリモーフィック関連は、一つのモデル（この例では`Comment`）が複数の異なるモデル（`Article`、`Product`）と関連を持つことを容易にする。
これによってコードがDRY（Don't Repeat Yourself）になり、アプリケーションのメンテナンスが容易になる。