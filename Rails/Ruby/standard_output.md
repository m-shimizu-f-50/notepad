# 標準入力とは
もともとはLINUXなどのUnix系OSで用意されていた仕組みです。
標準入力に対応するようにプログラムを作っておけば、プログラム実行時に、ファイルを読み込んだり、キーボードからデータを読み込んだり、パラメータを指定したりというように、入力先を切り替えることができます。

## gets
標準入力から一行入力
```
line = gets
puts line
```

## gets.to_i
標準入力から一行読み込み、整数に変換
```
line = gets.to.i
puts line

//入力された数字が出力される
```
```
例題）
標準入力で2つの整数が2行で与えられます。
1つ目の数値から２つ目の数値までを、1ずつ増加させながら、1行ずつ順番に出力するプログラムを作成してください。たとえば、3と5という数値が与えられた場合、次のように出力します。

3
4
5
```
```
num1 = gets.to_i
num2 = gets.to_i
for i in num1..num2
    puts i
end
```

## chomp　メソッド
文字列の末尾の改行コードを取り除きます
```
line = gets.chomp
puts = "#{line}は、スライムを攻撃した"       //改行コードを取り除いた状態で出力される
```

## printメソッド
引数の改行なしで出力する
```
print "hello world"
```


## 標準入力から２次元配列
```
while line = gets　　　　　　　　//標準入力から１行ずつデータをline変数に読み込んで
    line.chomp!               //末尾の改行コードを取り除く
    p line　　　　　　　　　　　 //pメソッドで出力
end



＊標準入力から２次元配列
enemy_img = []
while line = gets
    line.chomp!
    enemy_img.push(line.split(","))　　　　　　　//push(line.split(","))で二次元配列にしている
end

enemy_img.each do |line|
    line.each do |dot|
        if dot.to_i ==1
        //標準入力から読み込んだデータは文字列になるので.to_iで整数に変換する
          文字列だとif文がうまく判定できないため
            print "#"
        else
            print " "
        end
    end
    puts ""
end


```