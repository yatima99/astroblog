---
title: nilガードについて
slug: ruby-nil
pubDate: '2023-09-17'
image: /assets/ruby.jpg
category:
  - ruby
description: nilガードについて解説
---

## nilガードが必要な理由
例えば、以下のように出生年月日から年齢を計算する関数があったとする。

```ruby
def calculate_age(birthdate)
  current_year = Time.now.year
  age = current_year - birthdate.year
end
```

ここでbirthdateがnilであると、nil.yearが呼び出されてNoMethodErrorエラーが発生する。
nilをチェックすることでこのようなエラーを防ぐのがnilガード。

## 様々なnilガード

if文でnilガードを書くこともできるが、rubyではnilガードする便利なメソッドが提供されている。

### '||'演算子

nilかどうかではなく、nilまたは偽と評価される値（falseや0）をガードしたい場合、||演算子を使ってデフォルト値を設定できる。

```ruby
name = params[:name] || "Guest"
```

### '&.'演算子（通称ぼっち演算子）

&.演算子を使うと、呼び出すオブジェクトがnilの場合にはnilを返す。

```ruby
age = current_year - birthdate&.year
```
