---
title: pluckとmapの違い
slug: ruby-pluck
pubDate: '2023-08-28'
image: /assets/ruby.jpg
category:
  - ruby
description: pluckとmap の違いについて解説
---
pluckとmapはどちらも、配列やActive Recordのコレクションなどの要素を処理するためのメソッド。それぞれの特徴と使い分けの条件について書いていく。

## map
map メソッドは、配列やコレクションの各要素に対してブロックを評価し、その評価結果を新しい配列として返す。

```ruby
numbers = [1, 2, 3, 4, 5]
squared_numbers = numbers.map { |number| number ** 2 }
# squared_numbers => [1, 4, 9, 16, 25]
```
Active Recordでは、クエリ結果を取得した際に map メソッドを使って、クエリ結果の各レコードに対して必要なデータを抽出することが可能。

## pluck
pluck メソッドは、Active Recordのコレクション（クエリ結果など）から指定したカラムの値だけを抽出して新しい配列として返す。このメソッドはデータベースクエリを効率的に実行し、指定されたカラムだけを取得することができる。

```ruby
users = User.pluck(:name)
# users => ["Alice", "Bob", "Charlie"]
```
pluck メソッドは非常に効率的で、必要なカラムのみを取得するためにデータベースへのクエリが最小限に抑えられるので、大量のデータを処理する際に有効。
一方で、メソッド実行時に毎回SQLが発行されるというデメリットがある。

## 使い分け
- 特定のカラムのみ利用する場合はメモリ節約の観点からpluckを利用
- インスタンス化されたActive Recordモデルから特定のカラムを取り出す場合はmapを利用