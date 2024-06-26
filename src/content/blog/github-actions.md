---
title: Githubに秘密情報をうっかりpushしてしまった時の対処法[体験談]
slug: github-actions
pubDate: '2024-02-26'
image: /assets/git.png
category:
  - git
description: Githubに秘密情報をうっかりpushしてしまった時の対処法
---
AWSのECSへのデプロイを自動化するためにgithub Actionsを使ってみようと思い､

ECSのタスクをjson形式でコピーし､

```json
{
    "family": "task-definition-backend",
    "containerDefinitions": [
        {
            "name": "rails",
            "image": "xxxxxxx.dkr.ecr.ap-northeast-1.amazonaws.com/rails",
            "cpu": 0,
            "portMappings": [
                {
                    "name": "rails-3000-tcp",
                    "containerPort": 3000,
                    "hostPort": 3000,
                    "protocol": "tcp",
                    "appProtocol": "http"
                }
            ],
            "essential": true,
            "environment": [
                {
                    "name": "RAILS_LOG_TO_STDOUT",
                    "value": "true"
                },
                {
                    "name": "RAILS_MASTER_KEY",
                    "value": "xxxxxxxxxxxxxx"
                },
                {
                    "name": "AWS_ACCESS_KEY_ID",
                    "value": "xxxxxxxxxxxxxxxxx"
                },
                {
                    "name": "AWS_SECRET_ACCESS_KEY",
                    "value": "xxxxxxxxxxxxxxx"
                }
            ],
          .............以下省略..............
}
```

このように､環境変数のmasterkeyやAWSアクセスキーをそのまま記載した状態でgithubにpushしてしまった｡

次の日､AWSから警告メールが来てミスに気づく｡

とりあえず

- AWSのアクセスキーを無効化する
- リポジトリをprivateに変更する

を実施した｡

次に､

[リポジトリからの機微なデータの削除 - GitHub Docs](https://docs.github.com/ja/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository)

を参考にリポジトリから機密情報を削除した｡

自分は[git filter-repo](https://docs.github.com/ja/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository#using-git-filter-repo)の手法を用いた｡

プルリクから削除するのにサポートに連絡する必要があり身構えたが､実際はAIアシスタントでポチポチ回答するだけで削除申請が通り簡単だった｡

再発防止のため､機密情報のリアルタイム感知を行うGitGuardianをインストールした｡

本題はこのような機密情報をtask-definition.jsonに記載せずGithub ActionsでCDを行う方法だが､

git secretに機密情報を登録し､cd.ymlに
```yml
    - name: Prepare task definition
      run: |
        sed -i 's|${RAILS_MASTER_KEY}|'${{ secrets.RAILS_MASTER_KEY }}'|g' ./rails/task-definition.json
        sed -i 's|${S3_AWS_ACCESS_KEY_ID}|'${{ secrets.S3_AWS_ACCESS_KEY_ID }}'|g' ./rails/task-definition.json
        sed -i 's|${MAILER_SENDER}|'${{ secrets.MAILER_SENDER }}'|g' ./rails/task-definition.json
        sed -i 's|${S3_AWS_SECRET_ACCESS_KEY}|'${{ secrets.S3_AWS_SECRET_ACCESS_KEY }}'|g' ./rails/task-definition.json
      shell: bash
```
のようにsedコマンドで注入することで､機密情報をgithubにさらすことなく自動デプロイができるようになった｡

[追記]
アクセスキーが流出したことをAWSが感知すると､IAMユーザーに「AWSCompromisedKeyQuarantineV2」ポリシーが自動的に付与され､S3バケットなどのアクセス権限を失うようになっているよう｡
これで急にアプリのアップロード機能が動作しなくなり焦った｡
アクセスキーを変更するだけではダメで､IAMユーザー自体を作り直し､アクセスキーを再発行するのが正解でした｡
