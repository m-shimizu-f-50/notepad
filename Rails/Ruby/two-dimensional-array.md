# Ruby 二次元配列

二次元入れるは二つのindexで要素を指定する配列
```
landmap[1][2] = "城"

//インデックスの1,2に対応する要素を城を代入できる。
```
#### 用途
```
・RPGのマップ
・写真、イラストなどのイメージ
・ゲームの盤面
・表形式のデータ
・3D-CGの空間座標
```

```
team_c = ["勇者", "戦士", "魔法使い"]
team_d = ["盗賊", "忍者", "商人"]
team_e = ["スライム", "ドラゴン", "魔王"]
teams = [team_c, team_d, team_e]
p teams

p team[1]

p team[1][2]　　　　＊team配列１の中の配列２が呼び出される


//出力結果
[["勇者", "戦士", "魔法使い"], ["盗賊", "忍者", "商人"], ["スライム", "ドラゴン", "魔王"]]

["盗賊", "忍者", "商人"]

"商人"

配列の中に配列を格納することができる
```



## 二次元配列を基本操作する
```
team_c = ["勇者", "戦士", "魔法使い"]
team_d = ["盗賊", "忍者", "商人"]
team_e = ["スライム", "ドラゴン", "魔王"]
teams = [team_c, team_d, team_e]           //二次元配列
p teams

＊二次元配列を直接入力することができる
teams = [["勇者", "戦士", "魔法使い"], ["盗賊", "忍者", "商人"],["スライム", "ドラゴン", "魔王"] ]

teams[1][1]="魔道士"　　　　　　　//teams1の配列１を魔道士に更新する
p teams.length                 //teamsの配列の長さを出力する
p teams[0].length              //teams[0]の配列の長さを出力する
```

### 追加
```
teams = [["勇者", "戦士"], ["盗賊", "忍者", "商人"], ["スライム", "ドラゴン", "魔王"], ["魔法使い"]]

teams.push(["メタルモンスター","シルバーモンスター","ブラックモンスター"])
//pushメソッドで配列を追加する


teams[0].push("レッドドラゴン")
//配列の中に要素を追加する

```

### 削除
```
teams = [["勇者", "戦士"], ["盗賊", "忍者", "商人"], ["スライム", "ドラゴン", "魔王"], ["魔法使い"]]

teams.delete_at(1)
//index１の["盗賊", "忍者", "商人"]が削除される

teams[0].delete_at(1)
//teamsインデックス０の要素１（戦士）が削除される
```


## ループで配列を処理
```
team = ["勇者", "戦士", "魔法使い"]

team.each do |person|
    puts person
end

eachはteam配列から要素を１つずつ取り出しperson変数に格納してputsメソッドで要素を出力している

//出力結果
勇者
戦士
魔法使い




インデックスと一緒に表示(each_with_index)

team = ["勇者", "戦士", "魔法使い"]
team.each_with_index do |person, i|　　　　　　　　　//iにインデックスが格納される
    puts "#{i＋1}番目の#{person}が、スライムと戦った"   //インデックスは0から始まってしまうので＋1で1から始まるようにしている
end

//出力結果
１番目の勇者が、スライムと戦った
2番目の戦士が、スライムと戦った
3番目の魔法使いが、スライムと戦った
```



## mapメソッド
ある配列を別の配列に割り当てる
```
numbers =[3,1,4,1,5]
results =　[]
numbers.each do |item|　　　　　　　　//eachでnumbersをitem変数に格納
    results.push(item*10)　　　　　　//pushメソッドでitemの要素を10倍している
end

p results

//出力結果
[30, 10, 40, 10, 50]



＊mapメソッドでコンパクトにする

results２ = numbers.map do |item|
    item * 10
end
p results2

//出力結果
[30, 10, 40, 10, 50]


mapメソッドはnumbers配列からitem変数で要素を取り出して処理をされる
作成された新しい配列はresults2戻り値として新しく代入される

```

## 二次元配列をmapで作成する
```
配列をnewで作成
numbers = Array.new(10)
p numbers

配列の初期値を指定(1)
numbers = Array.new(10,1)
p numbers



二次元配列をmapで作成する
numbers3 = Array.new(4).map{Array.new(3, 1)}
p numbers3

//出力結果
[[1, 1, 1], [1, 1, 1], [1, 1, 1], [1, 1, 1]]

最初のArray.newで4つの要素の配列し、mapメソッドでその要素を一つずつ取り出して{Array.new(3, 1)}を実行する










