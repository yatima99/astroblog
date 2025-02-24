---
title: useEffectの依存配列について
slug: useEffectError
pubDate: '2024-12-22'
image: /assets/git.jpg
category:
  - react
description: useEffectのエラーについて
---

useEffectでエラーになったのでメモ。

## useEffectの基本的な仕組み
useEffectはReactコンポーネントの副作用を処理するためのフック。
副作用とは、データの取得や手動でのDOMの変更などの操作を指す。


```js
useEffect(() => {
  // 副作用を行うコード
  
  // クリーンアップ関数（オプション）
  return () => {
    // コンポーネントのアンマウント時やeffectの再実行前に実行されるコード
  };
}, [依存配列]); 
```

依存配列で指定された値が変更されると、useEffect内のコードが再実行される。
依存配列が空の場合は、コンポーネントのマウント時にのみ実行され、アンマウント時にクリーンアップ関数が実行される。

## 依存配列による無限ループエラー
### 典型的なパターン
```js
function Counter() {
  const [count, setCount] = useState(0);
  
  useEffect(() => {
    // この副作用はcount値を更新する
    setCount(count + 1);
  }, [count]); // countは依存配列に含まれている
  
  return <div>{count}</div>;
}
```

1. コンポーネントがレンダリングされる
2. useEffectが実行され、countが1に更新される
3. stateが変更されたため、コンポーネントが再レンダリングされる
4. 依存配列のcountが変わったため、useEffectが再実行される
5. 2〜4が無限に繰り返される
という無限ループが発生してしまう。

###  解決策
関数型更新を使用する
```js
function Counter() {
  const [count, setCount] = useState(0);
  
  useEffect(() => {
    // 前の状態を基に更新するため、count自体に依存しなくて良い
    setCount(prevCount => prevCount + 1);
  }, []); // 空の依存配列でも安全
  
  return <div>{count}</div>;
}
```
### ベストプラクティス

- 依存配列は常に完全に記述する: 
useEffectが使用するすべての外部値を依存配列に含めるべき。
- イミュータブルな更新を使用する: 
既存のオブジェクトを変更するのではなく、新しいオブジェクトを作成。
- 関数型更新を活用する: setState(prevState => ...) のパターンを使うことで、依存配列から値を除外できることがある。
- useCallbackやuseMemo: 
依存配列に関数やオブジェクトを含める場合は、これらのフックでメモ化する。
ESLintのルールを使用する: eslint-plugin-react-hooks を使用すると、依

## オブジェクトを依存配列にしたときのはまりポイント
依存配列として{name: 'Tanaka', age: 20}のようなオブジェクトを指定している場合、ageプロパティが変更されてもreactは依存配列の変更として検知してくれない。
なぜなら、useEffectは依存配列の比較を「浅い比較」（shallow comparison）で行うから。つまり、オブジェクトの参照が変わったかどうかだけを見て、その内部のプロパティ値が変わったかどうかは確認しない

### 解決策
- 個別のプロパティを依存配列に含める
```js
useEffect(() => {
  // 何らかの処理
}, [user.name, user.age]); // 個別のプロパティを依存配列に含める
```

- useState を使ったステート更新で新しいオブジェクトを作成する:

```js
Copyconst [user, setUser] = useState({ name: 'Tanaka', age: 20 });

// 年齢を更新する場合
setUser(prevUser => ({ ...prevUser, age: 21 })); // 新しいオブジェクトを作成
```

- useMemo を使って依存関係を管理する

```js
Copyconst memoizedUser = useMemo(() => {
  return { name: 'Tanaka', age: 20 };
}, [name, age]); // nameやageが変更されたときだけ新しいオブジェクトを作成

useEffect(() => {
  // 何らかの処理
}, [memoizedUser]);
```

### ポイント
Reactでは参照型のデータ（オブジェクトや配列）を扱う際は、イミュータブルな方法で更新することが推奨されている。
つまり、既存のオブジェクトを変更するのではなく、新しいオブジェクトを作成することで、比較が機能するようになる。