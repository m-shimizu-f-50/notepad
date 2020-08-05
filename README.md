# Mst's notepad

ここはMstのメモようリポジトリです。

## GFM（GitHub Flavored Markdown）

https://help.github.com/articles/github-flavored-markdown/

## Grip コミットする前にMarkdownの表示をブラウザでチェック（自分のパソコン上）
手順

①pipをMacのパソコンの中に入れる

  自分のパソコン上にpipをインストールする

`
$sudo easy_install pip
`

  パスワードを要求される（自分のパソコンに入るためのパスワード）

②pipを使ってgrepというパッケージをインストールします。

`$pip install grip`

③ブラウザで表示したいリポジトリに移動する（Mac上）

④

`$brew install grip`

(インストールにかなり時間がかかる)

⑤移したいファイルを指定する

例`$grip README.md`

ターミナル上
>1. Serving Flask app "grip.app" (lazy loading)
>2. Environment: production
>3. WARNING: Do not use the development server in a production environment.
>4. Use a production WSGI server instead.
>5. Debug mode: off
>6. Running on http://localhost:6419/ (Press CTRL+C to quit)

⑥ローカルパスhttp://localhost:6419/にアクセスするとプレビュー結果を見ることが出来ます！


