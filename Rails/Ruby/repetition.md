# Ruby 繰り返し処理
## for文
```
for カウンタ変数 in 繰り返す範囲 do   # doは省略可能
    繰り返し処理
end
```

```
例）HTMLで１歳から１０歳までのプルダウンリスト
puts "<select name='age'>"
for age in 1..10
  puts "<option>#{age}歳</option"
end

puts "</select>"
```

## while文
条件式が真である限り、処理を繰り返します。
```
# whileによるループ処理

# カウンタ変数を初期化
while 条件式 do    # doは省略可能
    #繰り返し処理
    #カウンタ変数を更新
end
```

```
例）
i=1
while i <=10      //1から10を繰り返す
  puts i    #繰り返し処理
  i=i+1            #カウンタ変数を更新
end
```


## if文(条件分岐)

```
if 条件式
  条件式が成り立ったときの処理
else
  条件式が成立しなかったときの処理
end
```

```
number = 1
if number == 1
  puts "数字は1です"
else
  puts "数字は1ではありません"
end
```

[実行結果]
```
数字は1です
```


### 複数の条件式
```
if 条件式
  条件式が成り立ったときの処理
elsif 条件式2
  条件式2が成立したときの処理
else
  条件式がどれも成立しなかったときの処理
end
```