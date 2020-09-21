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