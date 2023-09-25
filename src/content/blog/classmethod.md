---
title: クラスメソッド、インスタンスメソッドの使い分け
slug: ruby-classmethod
pubDate: '2023-09-22'
image: /assets/ruby.jpg
category:
  - ruby
description: クラスメソッド、インスタンスメソッドの使い分けについて解説
---
Rubyでクラスを設計するとき、メソッドをクラスメソッドかインスタンスメソッドのどちらで実装すべきか曖昧だったのでまとめる。

### インスタンスメソッド

インスタンスメソッドは、クラスのインスタンス（オブジェクト）に対して呼び出されるメソッド。
これらのメソッドは、そのクラスのインスタンスが持っているデータや状態に対して操作を行う。

例：

```ruby
class Dog
  def initialize(name)
    @name = name
  end

  def bark
    puts "#{@name} says WanWan!"
  end
end

pochi = Dog.new("Pochi")
pochi.bark  # Output: "Pochi says WanWan!"
```

### クラスメソッド

クラスメソッドは、クラス自体に対して呼び出されるメソッド。インスタンスに依存しないような動作をする場合や、クラスレベルでの設定を行う場合に用いられる。

例：

```ruby
class Dog
  @@total_dogs = 0

  def initialize(name)
    @name = name
    @@total_dogs += 1
  end

  def self.total_dogs
    @@total_dogs
  end
end

puts Dog.total_dogs  # Output: 0

fido = Dog.new("Fido")
puts Dog.total_dogs  # Output: 1
```

### 使い分け

- **インスタンスメソッド**
    オブジェクトの状態に依存する動作を行う場合に使用。たとえば、特定のユーザーの名前を取得するメソッド、アカウントの残高を更新するメソッドなど。

- **クラスメソッド**
   クラス全体に関わる設定や、インスタンスに依存しない動作を行いたい場合に使用。たとえば、全てのユーザーの数を知りたい、特定の設定をクラスレベルで適用したい、といったケースに用いる。

基本的にはインスタンスメソッドとして実装するのが一般的。
