---
title: 複数のcommitを一つにまとめる方法
slug: git-rebase
pubDate: '2023-08-28'
image: /assets/git.png
category:
  - git
description: 複数のcommitを一つにまとめる方法について解説
---

commitが細かすぎて後から一つにまとめたいときに。

## git rebase を使用する
git rebase コマンドは、コミットを修正したり整理するために使用される。
複数のコミットを一つにまとめるには、git rebase -i を使用する。
```
# 最新のN個のコミットをまとめる場合
git rebase -i HEAD~N
```

これにより、エディタが開かれ、コミットのリストが表示される。これを編集して、まとめたいコミットを一つにまとめる。
```
pick 1234567 First commit
pick abcdefg Second commit
pick hijklmn Third commit

# ↓ まとめたいコミットを "squash" または "s" に変更
pick 1234567 First commit
squash abcdefg Second commit
squash hijklmn Third commit

```


## 注意点
- rebaseを行うと、commitのハッシュが変わるため、共有ブランチにpushしたcommitの歴史が変更される可能性があります。他の人が同じブランチで作業している場合、rebaseを行うとコンフリクトが発生することがある。
- git rebase を使うと、コミットの順序が変わる可能性がある。これは、後にrebaseされたコミットが、前のコミットの後に配置されるため。
