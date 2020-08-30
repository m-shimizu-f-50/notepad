# ビュー

## ビューとは？
* データそのものを保存するのではなく、データを取り出す`SELECT文だけを保存`する
* データベースの利便性を高める道具
* SQL観点から見ると、テーブルと同じもの
* ビューを使うと、必要なデータが複数のテーブルにまたがる場合などの複雑な集約を行いやすくなる
* よく使うselect文はビューにして使い回すことができる


## テーブルとビューの違いとは？
* テーブル

　実際のデータの保存

* ビュー

　ビューの中にはselect文が保存される

  　ビュー自体はデータを持たない

## 構文

```
create view ビュー名(<ビューの列名1>,<ビューの列名2>,...)
as
<select文>
```

```
例）都道府県ごとのユーザー数
create view prefecture_user_counts(name,count)　//下記のselect文をprefecture_user_countsというviewにする
as
select
  p.name name
  count(*) count
from
  users u
inner join
  prefectures p
  on u.prefecture_id = p.id
group by
  u.prefecture_id;
```

＊viewを使うにはselect文のfrom句にテーブルの代わりにビューを指定する
```
select
  name
  count
from
  prefecture_user_counts;
```
上の記述で都道県ごとのユーザー数が出力できる


## ビューの削除

### 構文(drop view)
```
drop view ビュー名
```
```
例）
drop view prefecture_user_counts;
```
