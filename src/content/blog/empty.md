---
title: 空値判定メソッドいろいろ
slug: rails-empty
pubDate: '2023-09-20'
image: /assets/rails.png
category:
  - rails
description: railsのオブジェクトの中身を判定する便利なメソッドについて解説
---

Rails において、`nil?` 、 `empty?`、 `blank?`、 `present?` はオブジェクトの中身を判定する便利なメソッドだが、使い分けが難しいのでまとめる。

## `nil?`メソッド

レシーバが`nil`であるとき`true`を返す。

```ruby
nil.nil?      # => true
"abc".nil?    # => false
0.nil?        # => false
[].nil?       # => false
```

## `empty?` メソッド

レシーバが`""`, `[]`, `{}`のとき、`true`を返す。
（空のとき`true`になる）

```ruby
"".empty?     # => true
[].empty?     # => true
{}.empty?     # => true
"abc".empty?  # => false
[1, 2, 3].empty?  # => false
```

## `blank?`メソッド

`blank?` メソッドは、レシーバが
`false`、 `nil`に加えて、空っぽいオブジェクト(`""`, `" "`, `[]`, `{}`)
が`true`になる

```ruby
nil.blank?    # => true
"".blank?     # => true
"  ".blank?   # => true
[].blank?     # => true
{}.blank?     # => true
false.blank?  # => true
```

## `present?` メソッド

`blank?` メソッドの真偽反転版で、
オブジェクトが偽または空でない値を持っている場合にtrueを返す。

```ruby
nil.present?    # => false
"".present?     # => false
"  ".present?   # => false
"abc".present?  # => true
[1, 2, 3].present?  # => true
```
