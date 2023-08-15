---
title: ruby文字列操作
slug: ruby-string
pubDate: '2023-08-22'
image: /assets/ruby.jpg
category:
  - ruby
description: 文字列操作について解説
---

## 文字列操作で使うメソッド
```ruby
text = "Hello, Ruby!"
```
### 文字列の長さを取得
```ruby
length = text.length
puts length  # 出力: 13
```
### 文字列を大文字に変換
```ruby
uppercase_text = text.upcase
puts uppercase_text  # 出力: HELLO, RUBY!
```
### 文字列を小文字に変換
```ruby
lowercase_text = text.downcase
puts lowercase_text  # 出力: hello, ruby!
```
### 文字列を置換
```ruby
replaced_text = text.gsub("Hello", "Hi")
puts replaced_text  # 出力: Hi, Ruby!
```
### 文字列を分割
```ruby
words = text.split(", ")
puts words.inspect  # 出力: ["Hello", "Ruby!"]
```


