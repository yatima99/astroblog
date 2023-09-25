---
title: クロージャとは
slug: js-closure
pubDate: '2023-09-23'
image: /assets/js.png
category:
  - javascript
description: JavaScriptのクロージャについて解説
---


クロージャは、**関数が外側のスコープの変数にアクセスできる**という性質のこと。
これにより、**関数に状態をもたせる**ができる。

### 例 1: 基本的なクロージャ

```javascript
function outer() {
  let outerVariable = 'outer function';

  function inner() {
    console.log(outerVariable);
  }

  return inner;
}

const myClosure = outer();
myClosure();  // "outer function" が出力される
```

`inner()` 関数が外側のスコープの`outerVariable`にアクセスできている。

### 例 2: クロージャを使ったカウンタ

```javascript
function createCounter() {
  let count = 0;

  return function() {
    count++;
    return count;
  };
}

const counter = createCounter();

console.log(counter());  // 1
console.log(counter());  // 2
console.log(counter());  // 3
```

`createCounter`関数が返す関数（無名関数）は、外部の`count`変数を「覚えて」いる。
これにより、`createCounter`関数に状態を持たせることができる。

## クロージャのメリット

**データのプライバシーとカプセル化**: クロージャを使用すると、外部から直接アクセスできない「プライベート変数」を作成できる。これによりグローバル変数を減らし、コードの複雑性を減らすことができる。

