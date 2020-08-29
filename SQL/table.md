# テーブルの結合

## 内部結合(inner join)
お互いに一致している行が結合テーブルの対象となる

### 構文　inner join
```
select
  テーブル名１.列名,
  テーブル名２.列名・・・
from
  テーブル名１
inner join
  テーブル名２  //結合したいテーブル名
on テーブル名１.列名=テーブル名2.列名;　　//紐付ける
```
```
例）
select
  users.id,       //どこのテーブルのidなのか指定しておく(usersテーブル)
  users.last_name,
  users.first_name,
  prefectures.nmae
from
  users
inner join
  prefectures
on users.prefecture_id = prefectures.id;　　//共通する主キーと外部キーを紐づける
```
＊asを使ってコードをスッキリさせる
```
例）
select
  u.id,
  u.last_name,
  u.first_name,
  p.nmae
from
  users as u   //asをつけることで省略できる
inner join
  prefectures p　　//asは省略可能
on u.prefecture_id = p.id;
```

### 記述順序
```
1.select・・・取得行（カラム）の指定
2.from・・・対象テーブルの指定
３.結合処理
4.where・・・絞り込み条件の指定
5.group_by・・・グループ化の条件を指定
6.having・・・グループ化した後の絞り込み条件を指定
7.order by・・・並び替え条件を指定
8.limit・・・取得する行数の制限
```

###　実行順序
```
1.from・・・対象テーブルの指定
２.結合処理
3.where・・・絞り込み条件の指定
4.group_by・・・グループ化の条件を指定
5.having・・・グループ化した後の絞り込み条件を指定
6.select・・・取得行（カラム）の指定
7.order by・・・並び替え条件を指定
8.limit・・・取得する行数の制限
```

## 外部結合(outer join)
* 片方のテーブルの情報が全て出力される、テーブルの結合

* 外部結合欠落のあるデータを取り扱う結合(すべてのidが出力される＝値がないidはnullとなる)

###　構文
```
select
  テーブル名１.列名,
  テーブル名２.列名・・・
from
  テーブル名１
left outer join
  テーブル名２  //結合したいテーブル名
on テーブル名１.列名=テーブル名2.列名;　　//紐付ける
```
##### left outer join / right outer join
* left outer join・・・左側(from句で最初に書いたテーブル)をマスタとする

* right outer join・・・右側(from句で後に書いたテーブル)をマスタとする

```
例）
select
  u.last_name last_name,
  u.first_name user_id,
  o.user_id order_user_id,
  o.id order_id
from
  users u
left outer join
  orders o
on u.id= o.user_id
order by u.id;
```

＊複数のテーブルを結合することもできる






