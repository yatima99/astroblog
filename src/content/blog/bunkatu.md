---
title: JavaScriptの分割代入
slug: js-bunkatu
pubDate: '2023-09-24'
image: /assets/js.png
category:
  - javascript
description: avaScriptで分割代入をスプレッド記法を組み合わせて使いこなす
---

## 配列の分割代入

配列の分割代入では、配列から特定の要素を取り出して新しい変数に代入することができる。スプレッド記法を組み合わせると、残りの要素を新しい配列として取得できる。

```javascript
const [first, second, ...rest] = [1, 2, 3, 4, 5];
console.log(first);  // 出力: 1
console.log(second); // 出力: 2
console.log(rest);   // 出力: [3, 4, 5]
```

この例では、配列の最初の2つの要素はそれぞれ`first`と`second`に代入され、残りの要素（`[3, 4, 5]`）は`rest`という新しい配列になる。

## オブジェクトの分割代入

オブジェクトでも同様のことができる。特定のプロパティを取り出しつつ、その他のプロパティは新しいオブジェクトとして取得できる。

```javascript
const { a, b, ...remaining } = { a: 1, b: 2, c: 3, d: 4 };
console.log(a);        // 出力: 1
console.log(b);        // 出力: 2
console.log(remaining); // 出力: { c: 3, d: 4 }
```

この例では、`a`と`b`プロパティはそれぞれ対応する変数に代入され、残りのプロパティ（`{ c: 3, d: 4 }`）は`remaining`という新しいオブジェクトになる。

## 配列とオブジェクトの複合使用

配列とオブジェクトの分割代入を一緒に使うことも可能。さらにスプレッド記法を組み合わせれば、より複雑なデータ構造を簡単に扱える。

```javascript
const [{ name, age }, ...otherInfo] = [
  { name: "Alice", age: 25 },
  { hobby: "Reading" },
  { hobby: "Swimming" }
];
console.log(name);      // 出力: Alice
console.log(age);       // 出力: 25
console.log(otherInfo); // 出力: [{ hobby: "Reading" }, { hobby: "Swimming" }]
```
