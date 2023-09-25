---
title: rubyハッシュ
slug: ruby-hash
pubDate: '2023-08-22'
image: /assets/ruby.jpg
category:
  - ruby
description: ハッシュについて解説
---

ハッシュの基本

```ruby
person = {
  "name" => "Alice",
  "age" => 10,
  "city" => "Wonderland"
}
```

### ハッシュのキーを取得
```ruby
keys = person.keys
puts keys.inspect  # 出力: ["name", "age", "city"]
```
### ハッシュの値を取得
```ruby
age = person["age"]
puts age  # 出力: 10
```
### ハッシュにキーと値を追加
```ruby
person["occupation"] = "Adventurer"
puts person.inspect
# 出力: {"name"=>"Alice", "age"=>10, "city"=>"Wonderland", "occupation"=>"Adventurer"}
```
### ハッシュからキーと値を削除
```ruby
person.delete("city")
puts person.inspect
# 出力: {"name"=>"Alice", "age"=>10, "occupation"=>"Adventurer"}

```
