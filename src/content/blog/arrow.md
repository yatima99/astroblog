---
title: アロー関数について
slug: js-arrow
pubDate: '2023-09-25'
image: /assets/js.png
category:
  - javascript
description: JavaScriptのアロー関数について解説
---
## 通常の関数

最も基本的な関数の定義方法

```js
function sayHello(name) {
  console.log("Hello, " + name + "!");
}
```

## 関数式

関数式は、関数を変数に代入する方法で関数を定義。

```js
const sayGoodbye = function(name) {
  console.log("Goodbye, " + name + "!");
};
```

## アロー関数

アロー関数は、ES6（ECMAScript 2015）で導入された。
短い関数を簡潔に記述するケースや、`this` キーワードのスコープに関連する問題を解決するのに有用。
最も単純な形では、アロー関数は以下のようになる。

```javascript
const add = (a, b) => a + b;
```

この関数は、`a` と `b` の2つの引数を受け取り、それらを足して返します。

複数の命令を含む関数の場合は、中括弧 `{}` と `return` ステートメントを使用します。

```javascript
const calculate = (a, b) => {
  const sum = a + b;
  const diff = a - b;
  return { sum, diff };
};
```

### 引数が一つの場合

引数が一つしかない場合は、括弧を省略できる。

```javascript
const double = x => x * 2;
```

### 引数がない場合

引数がない場合、空の括弧 `()` を使う。

```javascript
const getRandom = () => Math.random();
```

### `this` の挙動

アロー関数の特徴の一つは、`this` キーワードに対する挙動です。アロー関数内の `this` は、関数が定義された時点での `this` の値を「覚え」ます。一方で、通常の関数では `this` の値が動的に変わることがあります。

```javascript
const obj = {
  value: 42,
  createArrowFn: function() {
    return () => this.value;
  },
};

const arrowFn = obj.createArrowFn();
console.log(arrowFn());  // 42
```

この例では、`createArrowFn` メソッドがアロー関数を返しています。このアロー関数は `createArrowFn` が呼び出された時点での `this.value`、つまり `obj.value` を返します。

### 制限と注意点

- `arguments` オブジェクトがありません。
- `yield` キーワードが使えないので、ジェネレータ関数として使えません。
- `super` キーワードも同様に使えません。
- コンストラクタとして使用できません。すなわち、`new` キーワードを使ってインスタンスを生成することはできません。

以上がアロー関数の基本的な特性と使い方です。短縮形としての利点、`this` の挙動、そしていくつかの制限点に注意を払いながら使用すると、非常に便利な機能です。