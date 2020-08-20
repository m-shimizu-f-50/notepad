# SQLでよく陥るエラー

## 初心者によく出るエラー

①DB選択していない
```
use mydb; //ここが記述されていない
select * from users;
```
エラー文
```
Error Code 1046 No database selected.
```
シンタックスエラー　SQLの構文が間違っているというエラーが出ています

②スペルミス

エラー文
```
you have an error in your SQL syntax
```

③全角文字にしている（あ）

大文字で入力してしまって実行した場合

エラー文
```
ERROR 1064: You have an error in your SQL syntax. Check the manual that corresponds to your MySQL server version for the right syntax to use near 'select * from users' at line1
```
