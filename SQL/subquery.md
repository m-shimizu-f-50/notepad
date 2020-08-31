# サブクエリ

## サブクエリとは
あるお問い合わせの結果に基づいて、異なる問い合わせを行う仕組み

## 構文：サブクエリ
```
select
  列名, ...
from
  テーブル名
where
  列名  演算子（select 列名 from テーブル名２...);
```




### not in 演算子
* ある値がセット内で含まれて`いない`かどうか
* 例）idが1か2か３でない行を取得

```
select * from products where id not in (1,2,3);
```
例）2017年12月に、商品を購入していないユーザーを出力
```
select
  id,
  last_name
from
  users
where id not in(
  select
    user_id
  from
    orders
  where
    order_time >='2017-12-01 00:00:00'
    and order_time <'2018-01-01 00:00:00'
  );
```





