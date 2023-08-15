---
title: gitコマンド集
slug: git-commands
pubDate: '2023-08-12'
image: /assets/git.png
category:
  - ruby
description: よく使うgitコマンドを羅列
---

Gitでよく使うコマンド集｡

### 1. リポジトリの作成
~~~
$ git init
~~~
現在のディレクトリに.gitという隠しディレクトリが作成され、Gitで管理できるようになります。

### 2. ブランチの作成､切り替え
~~~
$ git switch -c <ブランチ名>         # -cオプションで作成と切り替えを同時に行う
~~~


### 3. ファイルの追加､コミット
~~~
$ git add .　　　　#すべてのファイルを追加
$ git commit -m "コミットメッセージ"
~~~

#### 3.1 直前のコミットを取り消したいとき
~~~
$ git reset --soft HEAD^　
~~~
- `--soft`はコミットだけを取り消すオプション。（`--hard`はコミットを取り消した上に変更内容も取り消される。）
- HEAD^は直前のコミットを表す
- n個前のコミットを取り消す場合は`HEAD~n`(3つ前を取り消す例：`HEAD~3`)

#### 3.2 コミットの上書き
~~~
$ git commit --amend 
~~~
コミットメッセージを変えたいときに`git commit --amend -m "コミットメッセージ"`としてよく使う｡

### 4. ブランチのマージ
~~~
$ git switch main
$ git merge <ブランチ名>
~~~

### 5. リモートリポジトリ(GitHubなど)へのプッシュ
~~~
$ git remote add origin <リモートリポジトリのURL>
$ git push -u origin main
~~~

### 6. ブランチの削除
~~~
$ git branch -d <ブランチ名>
~~~

### 今の状況がわからなくなったとき

`$ git status`でプロジェクトファイルのステージング状態を確認したり､
`$ git log`でコミット履歴を確認｡

