# Ruby における様々なメソッド

## メソッドとは？

* コードを分割して、共通で利用するための機能

* 大規模プログラムの開発で必要

## メソッドの働き

```
・長いコードを分割して整理
・コードに名前をつける
・コードを呼び出す
・コードを組み合わせる
```

## メソッドの定義
```
def say_hello()           //say_helloでメソッドを定義している
    puts "hello world"
end

say_hello()      //say_helloメソッドを呼び出している(()は省略可能)
```


### 九九の段
```
# 九九の表を作成してみよう

def multi(x, y)                 //multiメソッドの定義　引数は(x,y)
    return x * y
end
for step in 1..9                //for文で１の段から９の段まで繰り返す
    for num in 1..9              //for文で1から9を繰り返す
      print multi(step, num)     //multiメソッドを呼び出す
      if num < 9                 //numが９以下なら繰り返す
          print ", "
      end
    end
    puts ""　　　　　　　　　　　　//段区切りで改行
end
```


### RPG攻撃シーン
```
# RPGの攻撃シーン
def attack(enemy)
    puts "勇者は#{enemy}を倒した"
    hit = rand(1..10)
    if hit < 6
        puts "#{enemy}に、#{hit}ダメージを与えた！"
    else
        puts "#{enemy}にクリティカルダメージを与えた！"
    end
end
enemies = ["スライム","モンスター","ドラゴン"]
enemies.each do |enemy|
    attack(enemy)
end
```