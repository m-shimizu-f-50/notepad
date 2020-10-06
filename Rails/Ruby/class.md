# クラス
オブジェクトの設計図

## オブジェクトとは？

* 多くのプログラミング言語はオブジェクト指向の文法

* オブジェクト：変数とメソッドをセットにした物

* クラスから実体化（インスタンス化）


## 簡単なクラスの作成
```
class Player　　　　　　　　　　//クラスの定義
    def walk()
        puts "勇者は荒野を歩いていた"
    end
end

player1 = Player.new()　　　　//オブジェクトを生成
player1.walk()              //オブジェクトのメソッドを呼び出す
```

### 解説
```
class Player

end
```
* クラスでクラス名を指定する（Playerクラス）

＊クラス名は大文字から始まる

```
def walk()
    puts "勇者は荒野を歩いていた"
end
```

* その中にクラスの持つメソッドを記述します

```
player1 = Player.new()
player1.walk()
```

* `Player.new()`newメソッドを呼び出すとオブジェクトを作ることができます。それを`player1`変数に代入します
* `player1 = Player.new()`をすることで`player1.walk()`したときにオブジェクトのメソッドを呼び出すことができる



## インスタンス変数を用いたクラス

### インスタンスとは？
```
インスタンス変数は、実体化したインスタンスが持つ変数です。インスタンス変数は、インスタンスがある限りデータが保持されます。

インスタンス変数は、変数名の先頭に「@」を付けます。
```



```
class Player

    attr_accessor :job          ///アクセサ
    def initialize(job)         ///初期化メソッド
        @job =job
    end

    def walk()
        puts "#{@job}は荒野を歩いていた"
    end
end

player1 = Player.new("戦士")　　　　　//player1オブジェクトはずっと戦士という値を持っている
player1.walk()



player1.job = "勇者"            //job変数を書き換えている
player1.walk()

```
[実行結果]

```
戦士は荒野を歩いていた
勇者は荒野を歩いていた
```
＊アクセサを用いることでPlayerオブジェクトのインスタンス変数である変数を書き換えることができる



```
# RPGの敵クラスを作る
class Enemy
    attr_accessor :name
    def initialize(name)
        @name = name
    end

    def attack()
        puts "#{@name}は勇者を攻撃した"
    end
end


enemies = []
enemies.push(Enemy.new("スライム"))
enemies.push(Enemy.new("モンスター"))
enemies.push(Enemy.new("魔王"))

enemies.each do |enemy|
    enemy.attack()
end
```
[実行結果]
```
スライムは勇者を攻撃した
モンスターは勇者を攻撃した
魔王は勇者を攻撃した
```


## クラスで、引数と戻り値のあるメソッドを作る
```
# クラスで、引数と戻り値のあるメソッドを作る

class Item
    @@tax = 1.08        #クラス変数(消費税)
    def initialize(price,quantity)
        @price = price
        @quantity = quantity
    end

    def total()          #合計金額を計算するメソッド
        (@price*@quantity*@@tax).round  #rundメソッドで四捨五入
    end
end



orenge = Item.new(100,5)
puts "合計金額は#{orenge.total}円です"

# ↓ 同じ意味です(分解して分かりやすく修正)変数に代入するやり方

apple = Item.new(120,10)
total = apple.total()
puts "合計金額は#{total}円です"
```

[実行結果]
```
合計金額は540円です
合計金額は1296円です
```

### クラス変数とは？
クラス変数を宣言したクラスのインスタンス全てで共有して利用できる変数。

クラス変数は、変数名の先頭に「@@」を付ける。



