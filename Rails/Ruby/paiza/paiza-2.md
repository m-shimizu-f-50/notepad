# Ruby 文字列の出力

例題１）

整数nが1行目、続く行でn個の文字列が与えられるので、n個の文字列をそのまま出力してください。

```
入力例1
3
AB
CD
EF

出力例1
AB
CD
EF

入力例2
3
1
2
3

出力例2
1
2
3
```

## コード

```
num = gets.chomp.to_i
(0...num).each do
    puts gets.chomp
end
```

## 解説

```
num = gets.chomp.to_i
```
取得した１行目の値(例１では３)をnum変数に代入する

```
(0...num).each do
```

`0...num`回を繰り返し取り出す