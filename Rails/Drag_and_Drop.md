# ドラッグ＆ドロップで並び替えて順番を保存できるリストを作る
`Rails` `act_as_list` `sortable.js`

## Introduction
例えばお気に入りリストアプリなどを作ろうとすると、リストのアイテムを順番通りに整列させて、ドラッグ＆ドロップで並び替えて、その順番を保存したい場面があると思います。
jQuery UIのsortableを利用している記事が多かったのですが、モバイルでも利用する前提だと他のjsも使わなくてはいけなかったりと面倒だったのですが、Sortable.jsというライブラリを利用すればモバイルにも対応してくれていたので「Sortable.js」＋「acts_as_list」を使ってドラッグ＆ドロップで並び替えできるリストを実装してみました。


## 0. Model
`User(belongs_to)`と`favorite(has_many)`で紐付ける。(今回はお気に入りのストレッチなので`stretch(has_many)`も紐づける)

Userは一つのお気に入りリスト(belongs_to)を持ち、favoriteは一人のユーザーに対して複数のお気に入りストレッチを持ちます

## 1. favoriteモデルに順番の概念を与える
モデルに順番の概念を与えるために`acts_as_list`を使っています

[GitHub - swanandp/acts_as_list](https://github.com/brendon/acts_as_list)

```gem:gem
gem 'acts_as_list'
```
```
$ bundle install
```
`acts_as_list`がインストールできたら、favoriteにposition:integerのカラムを追加してください。