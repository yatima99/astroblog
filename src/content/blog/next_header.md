---
title: Next.jsでブラウザタブのタイトルを設定
slug: next header
pubDate: '2024-03-17'
image: /assets/ruby.jpg
category:
  - javascript
description: Next.jsでブラウザタブのタイトルを設定
---

ブラウザのタブタイトルを設定するには、**`next/head`**をインポートして、**`<Head>`**コンポーネントを使用し､コンポーネント内でタイトルを設定する。

```js

import Head from 'next/head';

...略...

return (
<>
	<Head>
		<title> Home </title>
	</Head>
{/* 以降のコンテンツ */}
	<Box>
	...
	</Box>
</>
);
```