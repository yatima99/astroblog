---
title: javascriptのthisについて
slug: js-this
pubDate: '2023-09-27'
image: /assets/js.png
category:
  - javascript
description: javascriptのthisについて解説
---

一言でいうと**現在実行中の関数やメソッドが「何に対して」作用しているのかを示す変数**のこと。
いくつかのパターンに分類して解説する。

## グローバルコンテキストでの`this`

グローバルスコープ（どのオブジェクトにも属さない場所）で`this`を使うと、ブラウザ環境では`window`オブジェクトを、Node.js環境では`global`オブジェクトを指す。

```javascript
console.log(this);  // ブラウザ: window, Node.js: global
```

## 通常の関数呼び出し

通常の関数呼び出しでは、thisはグローバルオブジェクト（ブラウザではwindow、Node.jsではglobal）を指す。

```js
function myFunction() {
  console.log(this);
}
myFunction(); // ブラウザではwindow, Node.jsではglobal
```

## オブジェクトメソッドとしての呼び出し

メソッドが所属するオブジェクトを指す。

```javascript
const obj = {
  showThis: function() {
    console.log(this);
  }
};

obj.showThis();  // obj
```

## コンストラクタ関数と`this`

`new`演算子を使って関数をコンストラクタとして呼び出す場合、`this`は新しく生成されるインスタンスを指す。
pythonにおける`self`にあたる。

```javascript
function Person(name) {
  this.name = name;
}

const alice = new Person("Alice");
console.log(alice.name);  // "Alice"
```

## アロー関数と`this`

アロー関数内の`this`は、アロー関数が定義されたスコープのthisの値を引継ぎ、それを関数内で使用する。
一般的な関数と違い、アロー関数は自身のthisを持たない。

```javascript
const obj = {
  name: "Alice",
  showThis: function() {
    const arrowFunc = () => {
      console.log(this);
    };
    arrowFunc();  // obj
  }
};

obj.showThis();
```

## `call`、 `apply`、`bind`メソッドによる指定

`call`メソッドは、指定した`this`値とともに関数を実行する。このメソッドの第一引数は関数内で`this`として参照されるオブジェクトで、それ以降の引数は関数へ渡される。

```javascript
function greet(greeting, kutouten) {
  console.log(greeting + ', ' + this.name + kutouten);
}

const person = { name: 'Alice' };

greet.call(person, 'Hello', '!');  // 出力: "Hello, Alice!"
```

`apply`メソッドも`call`と同様に、指定した`this`値で関数を実行するが、`apply`は関数に渡す引数を配列として受け取る。

`bind`メソッドは強制的にオブジェクトと結びつける。

```js
function greet(greeting, kutouten) {
  console.log(greeting + ', ' + this.name + kutouten);
}

const person = { name: 'Alice' };

const greetAlice = greet.bind(person);
greetAlice('Hello', '!');  // 出力: "Hello, Alice!"
```

