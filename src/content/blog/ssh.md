---
title: ssh接続とは
slug: ssh
pubDate: '2023-10-07'
image: /assets/git.png
category:
  - git
description: ssh接続について解説
---

## SSHとは？

SSH（Secure SHell）は、安全なリモート接続をするためのプロトコル。
SSHを使用すると、信頼できないネットワークを介し、安全に他のコンピュータに接続してコマンドを実行したり、ファイルを転送したりすることができる。

従来は`telnet`というプロトコルが使用されていたが、データが暗号化されないという問題があった。それを改善したのがSSH。

## SSHの特徴

1. **データの暗号化**：通信内容が第三者に盗み見られることがない。
2. **公開鍵認証**：公開鍵と秘密鍵の２種類の鍵をもちいる「公開鍵認証」によってクライアント-サーバー間で安全な認証を行う

## HTTPS接続との比較

**パスワードレス認証**: 公開鍵をリモートサーバに登録し、対応する秘密鍵をローカルマシンに持っていれば、パスワード入力なしでリポジトリへのアクセスや変更が可能となる。
**セキュリティ**: 秘密鍵が安全に保管されている限り、SSHの公開鍵認証は非常に安全。

## SSH接続の方法

Githubにssh接続する例。

### 公開鍵と秘密鍵の生成

```bash
$cd ~/.ssh
```
で、鍵を入れるフォルダに移動

```bash
$ssh-keygen -t rsa
```
で鍵を生成。(何度か質問されるが全てEnterで良い)
id_rsaとid_rsa.pubのキーペアが生成される。

### 公開鍵をGithubに登録する
id_rsa.pubの中身をコピーし、
https://github.com/settings/ssh
で鍵を登録。

### SSH接続の確認

```bash
ssh -T git@github.com
```
で
**Hi [account名]! You've successfully authenticated, but GitHub does not provide shell access.**
と返ってきたら接続成功。

