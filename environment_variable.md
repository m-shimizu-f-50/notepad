# 環境変数化の設定[Ruby on Rails]

## 環境変数とは？

環境変数とはAWSのパスワードや、APIの秘密鍵などGithubにあげたくない機密情報の第三者への漏洩を防ぐためにサーバーなどのOSに保存しアプリに渡す仕組みです。

なお環境変数は開発環境、本番環境にそれぞれ設定しなければなりません。


## Dotenvを用いた設定方法

Dotenvは.envファイルに記述する設定方法です。
なお、RailsではDotenv-railsというgemが用意されているのでこちらを使用していきます。

### 1.Gemをインストール

```
    gem 'dotenv-rails'
```

```
    bundle install
```

### 2.envファイルを作成

環境変数を設定したいアプリのappファイルやconfigファイル、Gemfileなどが置いてあるルートディレクトリに「.env」というファイルを作成してください

### 3.環境変数を設定

作成した.envファイルに環境変数を記述します。

.env
```
    DB_USERNAME="RDSの接続ユーザ名"
    DB_PASSWORD="RDSのパスワード"
    DB_HOST="RDSのエンドポイント名"
    DB_DATABASE="アプリケーション名"

    S3_ACCESS_KEY_ID = '******'
    S3_SECRET_ACCESS_KEY = '*********'

    MAIL_NAME= "*********"
    MAIL_PASSWORD= "*********"
```

### 4.gitignoreを編集

作成した.envファイルがgithubにアップされないように.gitignoreファイルに記述します

【注意】
* gitignoreファイルの最終行に以下を追記して、保存してください。
* gitignoreファイルも、アプリケーション直下にあります。

.gitignore
```
    #下記を追記
    /.env
```
これで環境変数をgithubにアップすることなく使用することができます。


### 5.環境変数の呼び出し

config/database.yml
```
    #設定した環境変数の呼び出し
    production:
      <<: *default
      database: <%= ENV['DB_DATABASE'] %>
      adapter: mysql2
      encoding: utf8mb4
      charset: utf8mb4
      collation: utf8mb4_general_ci
      host: <%= ENV['DB_HOST'] %>
      username: <%= ENV['DB_USERNAME'] %>
      password: <%= ENV['DB_PASSWORD'] %>
```


config/enviroments/development.rb
```
config.action_mailer.smtp_settings = {
    port:                 587,
    address:              'smtp.gmail.com',
    domain:               'gmail.com',
    user_name:            ENV['MAIL_NAME'],
    password:             ENV['MAIL_PASSWORD'],
    authentication:       'plain',
    enable_starttls_auto: true
  }
```

以上になります。
通常.envを読みこませるにはコマンドが必要ですが、dotenv-railsというgemを導入することにより自動的に読み込んでくれます。







