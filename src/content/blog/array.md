---
title: javascriptの配列操作
slug: js-array
pubDate: '2023-09-26'
image: /assets/js.png
category:
  - javascript
description: 複数のcommitを一つにまとめる方法について解説
---


## 要素の追加・削除

### `push()`、`pop()`

push: 配列の最後に一つ以上の要素を追加します。
pop: 配列の最後の要素を取り除き、その要素を返します。

```javascript
let arr = [1, 2, 3];
arr.push(4);
console.log(arr); // [1, 2, 3, 4]
arr.pop()
console.log(arr); // [1,2,3]
```

### `shift()`、`unshift()`

shift(): 配列の最初の要素を取り除き、その要素を返します。
unshift(): 配列の最初に一つ以上の要素を追加します。

`push()`、`pop()`が配列の最後の要素への操作であるのに対して、
`shift()`, `unshift()`は配列の最初の要素への操作。

```javascript
let arr = [1, 2, 3];
let firstElement = arr.shift();
console.log(firstElement); // 1
console.log(arr); // [2, 3]

arr.unshift(0);
console.log(arr); // [0, 2, 3]
```

### （余談）データ構造との関連

push()とpop()のみを使用することで、配列はスタックの動作をする。
一方、push()とshift()を組み合わせることで、配列はキューの動作をする。

## スライス

### `splice()`

配列から要素を追加または削除します。
splice(開始位置,削除する要素数,追加要素1,追加要素2……)

```javascript
let arr = [1, 2, 3, 4];
arr.splice(1, 2, 'a', 'b');
console.log(arr); // [1, 'a', 'b', 4]
```

### `slice()`

配列の一部を抽出して新しい配列を作成します。

```javascript
let arr = [1, 2, 3, 4];
let newArr = arr.slice(1, 3);
console.log(newArr); // [2, 3]
```

## 高階関数

### `map()`

配列のすべての要素に関数を適用した新しい配列を作成。

```javascript
let arr = [1, 2, 3];
let doubled = arr.map(x => x * 2);
console.log(doubled); // [2, 4, 6]
```

### `filter()`

配列の要素のうち、指定した関数のテストを通過する要素からなる新しい配列を作成します。

```javascript
let arr = [1, 2, 3, 4];
let even = arr.filter(x => x % 2 === 0);
console.log(even); // [2, 4]
```

### `reduce()`

配列の各要素を1回ずつ見ていって関数を適用し、最後に1つの値を返す

```javascript
let arr = [1, 2, 3, 4];
let sum = arr.reduce((acc, x) => acc + x, 0);
console.log(sum); // 10
```

## 検索

### `find()`

配列の中から指定した関数に一致する**最初の**要素を返します。一致する要素がない場合は `undefined` を返します。

```javascript
const array = [5, 12, 8, 130, 44];
const found = array.find(element => element > 10);
console.log(found); // 12
```

### `findIndex()`

配列の中から指定した関数に一致する**最初の**要素のインデックスを返します。一致する要素がない場合は `-1` を返します。

```javascript
const array = [5, 12, 8, 130, 44];
const index = array.findIndex(element => element > 10);
console.log(index); // 1
```

どちらのメソッドも、条件に一致する要素が見つかった時点で検索を終了する。これにより、大きな配列で条件に一致する要素が早い段階で見つかる場合、検索処理が効率的になる。
複数の検索結果がほしい場合は`filter()`メソッドを使用する。
