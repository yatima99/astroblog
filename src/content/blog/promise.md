---
title: 非同期処理について
slug: js-promise
pubDate: '2023-09-28'
image: /assets/js.png
category:
  - javascript
description: JavaScriptの非同期処理について解説
---


## 1. 非同期処理とは

JavaScriptの非同期処理は、特定のコードの実行が完了するのを待たずに、次のコードを実行できる特性を指します。これにより、時間がかかる処理（例：サーバーからデータを取得する）を行っている間も、アプリケーションはユーザーの操作に応答し続けることができます。

### 単一スレッドとイベントループ

JavaScriptは単一スレッドの言語であり、一度に一つのタスクしか処理できません。
しかし、非同期処理によって、待機中のタスクが完了するのを待つことなく、次のタスクに移行することができます。
JavaScriptが非同期タスクを管理する仕組みを「イベントループ」と呼びます。

### イベントループと非同期処理

イベントループは、メッセージキューと呼ばれるタスクの待ち行列を持っており、実行可能なタスクを順番に処理していきます。非同期処理は、特定のタスクが終了した時点でコールバック関数をメッセージキューに追加することで、そのタスクの完了を通知します。イベントループは、メッセージキューにタスクがある限り、それを取り出して処理を続けます。

## 2. コールバック

コールバックは、他の関数に引数として渡される関数です。この関数は、特定のイベントが発生した時や、特定のタスクが完了した後に、「コールバック」として実行されます。

### 基本的な使用例

以下は、`setTimeout`を使用したコールバック関数の基本的な例です。

```javascript
setTimeout(function() {
  console.log('1秒後に表示');
}, 1000);
```

この例では、無名関数が`setTimeout`にコールバック関数として渡され、3秒後に実行されます。

### コールバック地獄

非同期処理でコールバック関数を多用すると、コールバック関数のネストが深くなることで、コードの可読性が低下する。
これは俗に「コールバック地獄」と呼ばれている。

```javascript
firstFunction(params, function() {
  secondFunction(params, function() {
    thirdFunction(params, function() {
      // さらにネストが深くなる…
    });
  });
});
```

コールバック地獄を解決するため、javascriptはこれから紹介するPromise, さらにはasync/awaitが導入されました。

## 3. Promise

JavaScriptの`Promise`は、非同期処理の結果を表現するオブジェクトです。
Promiseは、以下の3つの状態を持ちます。

`pending`: 非同期処理が実行中（まだ完了していない）状態
`fulfilled`: 非同期処理が正常に完了した状態
`rejected`: 非同期処理が異常終了した状態


### Promiseの生成

Promiseオブジェクトは、`new Promise`構文を用いて生成されます。

```javascript
let promise = new Promise((resolve, reject) => {
  let success = true; // 成功・失敗を表すフラグ
  if(success) {
    resolve('処理が成功しました。');
  } else {
    reject('処理が失敗しました。');
  }
});
```

Promiseの引数`resolve`、`reject`は

### thenメソッドとcatchメソッド

Promiseオブジェクトが生成されると、`then`メソッド、`catch`メソッドを用いて、非同期処理の完了時のコールバック関数を登録することができます。

```javascript
promise.then((message) => {
  console.log(message); // 成功時のメッセージ
}).catch((message) => {
  console.log(message); // 失敗時のメッセージ
});
```

### Promiseチェーン

`then`メソッドは新しいPromiseオブジェクトを返すため、`then`や`catch`を連鎖させることができます。これにより、複数の非同期処理を順番に実行することが可能です。

```javascript
firstFunction()
  .then((result) => secondFunction(result))
  .then((result) => thirdFunction(result))
  .catch((error) => console.error(error));
```

### Promise.all

`Promise.all`メソッドを使用すると、複数のPromiseが全てfulfilledになるのを待つことができます。

```javascript
Promise.all([promise1, promise2, promise3])
  .then((results) => console.log(results))
  .catch((error) => console.error(error));
```

## 4. async/await

`async/await`は`Promise`をより直感的に書けるようにしたシンタックスシュガーで、
非同期処理を同期処理のように直列的に書くことができるので可読性が高いです。

### async関数

`async`キーワードは、関数の前に置くことで、その関数は非同期関数となります。非同期関数は、必ず`Promise`オブジェクトを返します。

```javascript
async function myFunction() {
  return 'Hello, async/await!';
}

myFunction().then(console.log); // 'Hello, async/await!'
```

### await式

`await`キーワードは、`async`関数内で使用されます。`await`は、`Promise`の解決を待ち、その結果を返します。これにより、非同期処理の結果を同期的に扱うことができます。

```javascript
async function fetchData() {
  let response = await fetch('https://api.example.com/data');
  let data = await response.json();
  console.log(data);
}
```

`await`式を使用することで、`fetchData`関数内のコードは、`fetch`の処理が完了するまで次の行に移動せず、その後にJSONデータのパースを行います。これにより、コードは直感的に読み取れ、理解しやすくなります。

### エラーハンドリング

`async/await`を使用する際のエラーハンドリングは、`try/catch`構文を使用します。

```javascript
async function fetchData() {
  try {
    let response = await fetch('https://api.example.com/data');
    let data = await response.json();
    console.log(data);
  } catch (error) {
    console.error('データの取得に失敗:', error);
  }
}
```

## まとめ

以上、javascriptの非同期処理について、コールバック関数、promise、async/awaitについて解説しました。