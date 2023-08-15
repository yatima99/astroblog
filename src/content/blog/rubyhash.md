---
title: rubyハッシュ
slug: ruby-hash
pubDate: '2023-08-22'
image: /assets/ruby.jpg
category:
  - ruby
description: ハッシュについて解説
---

ハッシュ関連のメソッド

```ruby
person = {
  "name" => "Alice",
  "age" => 30,
  "city" => "Wonderland"
}

# ハッシュのキーを取得
keys = person.keys
puts keys.inspect  # 出力: ["name", "age", "city"]

# ハッシュの値を取得
age = person["age"]
puts age  # 出力: 30

# ハッシュにキーと値を追加
person["occupation"] = "Adventurer"
puts person.inspect
# 出力: {"name"=>"Alice", "age"=>30, "city"=>"Wonderland", "occupation"=>"Adventurer"}

# ハッシュからキーと値を削除
person.delete("city")
puts person.inspect
# 出力: {"name"=>"Alice", "age"=>30, "occupation"=>"Adventurer"}

```
