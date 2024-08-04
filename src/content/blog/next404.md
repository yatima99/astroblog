---
title: Next.jsのカスタム404ページ
slug: next 404page
pubDate: '2024-03-17'
image: /assets/ruby.jpg
category:
  - javascript
description: Next.jsのカスタム404ページの設定
---

Next.jsプロジェクトのpagesフォルダ下に404.tsxというページを新規に作成｡

```ts
import { Card, CardContent, Container } from '@mui/material'

const NotFound = () => {
  return (
    <Container maxWidth="sm">
      <Card sx={{ p: 3, mt: 8, backgroundColor: '#EEEEEE' }}>
        <CardContent sx={{ lineHeight: 2 }}>
          お探しになったページは存在しません｡
        </CardContent>
      </Card>
    </Container>
  )
}

export default NotFound
```

などとすると特に設定することなく404ページがオリジナルのものと置き換わる｡