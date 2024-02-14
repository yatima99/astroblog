---
title: Amazon ECS Execを使ってコンテナ内部に入る
slug: ecs-exec
pubDate: '2024-01-25'
image: /assets/aws.png
category:
  - aws
description: Amazon ECS Execを使ってコンテナ内部に入る方法について解説
---

[https://www.skyarch.net/blog/amazon-ecs-execを使ってコンテナ内部でコマンド実行してみ/](https://www.skyarch.net/blog/amazon-ecs-exec%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%A6%E3%82%B3%E3%83%B3%E3%83%86%E3%83%8A%E5%86%85%E9%83%A8%E3%81%A7%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E5%AE%9F%E8%A1%8C%E3%81%97%E3%81%A6%E3%81%BF/)

これを参考にやるだけ｡

自分の場合､aws cliのバージョンが古いと実行できないことに気づかず詰まった。バージョンアップしましょう｡

あと、enable ecs-execを有効にしたあとサービスの再起動が必要。（記事にちゃんと書いてあるけど）

ユーザー登録の際の認証メール送信がうまくいかず､DBに未認証ユーザーだけ作成されるという現象に困っていたが､これで外部からユーザーを消去することができるように｡

