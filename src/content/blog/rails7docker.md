---
title: dockerでrails7環境構築
slug: docker-rails7
pubDate: '2023-08-22'
image: /assets/docker.png
category:
  - docker
description: Node.js製のpython-shellモジュールを使うことで、JavaScriptからPythonへのデータの送受信が可能に。Pythonスクリプトを実行し、結果を受け取る方法も解説します。
---

### 環境

- Ruby: 3.1.0
- Ruby on Rails: 7.0.4
- MySQL: 8.0

### 1. プロジェクトフォルダを作成､移動
~~~sh
$ mkdir my-rails-app
$ cd my-rails-app
~~~
以下のファイルを作成します｡
- Dockerfile
- docker-compose.yml
- Gemfile
- Gemfile.lock

~~~sh
$ touch Dockerfile docker-compose.yml Gemfile Gemfile.lock
~~~

### 2. Dockerfileの中身を記述
```Dockerfile
FROM ruby:3.1.0

RUN mkdir /app
WORKDIR /app

COPY Gemfile /app/Gemfile
COPY Gemfile.lock /app/Gemfile.lock

RUN bundle install
COPY . /app

EXPOSE 3000

CMD ["rails", "server", "-b", "0.0.0.0"]
```
なお､Rails 7 では Webpacker が標準では組み込まれなくなったので yarn や Node.js のインストールは不要です。

### 3. Gemfileの中身を記述
~~~Gemfile:Gemfile
source 'https://rubygems.org'
git_source(:github) { |repo| "https://github.com/#{repo}.git" }

ruby '3.1.0'

gem 'rails', '~> 7.0.4'
gem 'mysql2'
~~~
Gemfile.lockは空のままで大丈夫です｡

### 4 .docker-compose.ymlに以下の内容を記述
~~~yaml:docker-compose.yaml
version: '3.9'
services:
  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: myrootpassword
      MYSQL_DATABASE: myrailsapp_development
      MYSQL_USER: myrailsapp
      MYSQL_PASSWORD: myrailsapppassword
    volumes:
      - db-data:/var/lib/mysql
    ports:
      - "3306:3306"
  web:
    build: .
    command: bash -c "rm -f tmp/pids/server.pid && rails server -b '0.0.0.0'"
    volumes:
      - .:/app
    ports:
      - "3000:3000"
    depends_on:
      - db

volumes:
  db-data:
~~~
### 5. Railsアプリケーションを作成
~~~sh
$ docker-compose run web rails new . \
--force --no-deps --database=mysql --skip-javascript
~~~
--skip-javascriptオプションを追加して、JavaScriptの設定をスキップしています。

### 6. 生成されたconfig/database.ymlを編集し、データベース設定を変更
~~~yaml:config/database.yml
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: myrailsapp
  password: myrailsapppassword
  host: db

development:
  <<: *default
  database: myrailsapp_development

test:
  <<: *default
  database: myrailsapp_test

production:
  <<: *default
  database: myrailsapp_production
  username: <%= ENV['MYRAILSAPP_DATABASE_USER'] %>
  password: <%= ENV['MYRAILSAPP_DATABASE_PASSWORD'] %>
  host: <%= ENV['MYRAILSAPP_DATABASE_HOST'] %>
~~~





### 7. RailsアプリとMysqlデータベースのコンテナをビルド＆実行
~~~sh
$ docker-compose up --build
~~~

## 動作確認
http://localhost:3000/ に移動し､起動を確認します。

