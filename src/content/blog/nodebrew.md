---
title: nodebrewでバージョン管理
slug: nodebrew
pubDate: '2023-09-16'
image: /assets/js.png
category:
  - javascript
description: nodebrewでバージョン管理する方法を解説
---

`nodebrew`はNode.jsのバージョン管理を行うためのツール。

### nodebrewのインストール


```bash
brew install nodebrew
```

インストール後、環境変数を設定(zsh)

```bash
echo 'export PATH=$HOME/.nodebrew/current/bin:$PATH' >> ~/.zshrc
source ~/.zshrc
```

### Node.jsのバージョンをインストール

Node.jsをインストール。利用可能なバージョンをリスト表示するには

```bash
nodebrew ls-remote
```

特定のバージョン（ここでは20.5.1）をインストールするには、

```bash
nodebrew install v20.5.1
```

### バージョンを切り替える

インストールしたバージョンに切り替えるには、

```bash
nodebrew use v20.5.1
```

これでNode.jsのバージョンが切り替わる。

### インストール済みのバージョンを確認する

インストール済みのNode.jsのバージョンを表示するには、

```bash
nodebrew list
```
