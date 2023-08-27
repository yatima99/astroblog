---
title: ruby基本文法
slug: ruby
pubDate: '2023-08-22'
image: /assets/ruby.jpg
category:
  - ruby
description: rubyの基本文法について
---

Rubyの基本的な文法をさらっと。


## 1. 変数とデータ型
Rubyは動的型付け言語なので、データ型を指定せずに変数を宣言できる。

```ruby
name = "Alice"
name.class #=> String

bool = true
bool.class #=> TrueClass

age = 20
age.class #=> Int

height = 155.6
height.class #=> Float
```

## 2. 文字列
ダブルクォーテーションまたはシングルクォーテーションで文字列を作成。
ダブルクォーテーション内では変数の値を展開できるという違いがある。

```ruby
single_quoted = 'シングルクォーテーション'
double_quoted = "ダブルクォーテーション"

name = "Bob"
greeting = "Hello, #{name}!"
```

文字列同士で結合できる。
```ruby
first_name = '太郎'
last_name = '田中'

name = last_name + first_name 
puts "Hello, #{name}!"
```


## 3. 数値演算

```ruby
x = 10
y = 5
sum = x + y
difference = x - y
product = x * y
quotient = x / y
```

## 4. ブロック


## 5. 条件文
if文。他の言語のelse ifとちがい、elsifを用いる。

```ruby
if age >= 18
  puts "成年です"
elsif age == 17
　puts "永遠の17歳です"
else
  puts "未成年です"
end
```

## 6. ループ
他言語と違い、for文は使わずにメソッドを用いることが多い。
```ruby
5.times do
  puts "hello"
end
```
```ruby
fruits = ["りんご", "バナナ", "オレンジ"]
fruits.each do |fruit|
  puts fruit
end
```
while文
```ruby
i = 0
while i < 10 do
  puts i
  i += 1
end
```

## 7. 配列
複数の値をまとめて扱うためのデータ構造。

```ruby
fruits = ["りんご", "バナナ", "オレンジ"]
puts fruits[0] # "りんご"が表示される

fruits.push("グレープ") # 配列に要素を追加
fruits.pop("バナナ") # 配列から要素を削除
```

## 8. メソッド
関数のようなもので再利用可能。

```ruby
def area(x, y)
  x * y
end

puts(5, 6) 
```
returnを省略できる。

## 9. クラスとオブジェクト

```ruby
class Person
  def initialize(name, age)
    @name = name
    @age = age
  end

  def introduce
    puts "私は#{@name}で、#{@age}歳です。"
  end
end

person = Person.new("David", 25)
person.introduce()
```






