# rails generate migrationコマンドまとめ

## 基本コマンド

```
# マイグレーションファイル作成コマンド
$ rails generate migration クラス名

# モデル作成
$ rails generate model モデル名
```
クラス名は何でもOKだけど「アクション+テーブル名」とかが慣例っぽいです。分かりやすければ良いかな。これで /db/migrate/タイムスタンプ_クラス名.rb というファイルが作れる。ここに、スキーマの変更点を記載すればOK。

モデルの新規作成はmodel モデル名。命名規則は[モデル名とテーブル名の規約](https://railsdoc.com/model#)を参照。

generateは g と略すことができる

## テーブルを作る

```
$ rails g model モデル名 フィールド:型:(unique|index) 以降必要なだけ
```

またはモデルテーブルを作り直接マイグレーションファイルに必要なカラムを記述して`＄rails g migration`をする

`$rail g maigration`した後にマイグレーションファイルを修正したい場合は、直接修正したいマイグレーションファイルでカラムを追加・削除をして`$rails g migration:reset`コマンドをする

## 指定できる型

* string: 文字型
* text: 長い文字列
* integer: 整数(数字)
* float: 浮動小数
* decimal: 精度の高い少数
* datetime: 日時
* timestamp: タイムスタンプ
* time: 時間
* date: 日時
* binary: バイナリデータ
* boolean: Boolean

## マイグレーションファイルを実行する

```
 実行
$ rake db:migrate
```


























