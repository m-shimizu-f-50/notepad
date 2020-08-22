# 配列を操作するメソッド

## pushメソッド
pushメソッドとは、配列の最後に新しい要素を追加するメソッドです。
pushメソッドの後の()の中に追加したい要素を入力します。
以下の例では、pushメソッドの引数「4」が配列の最後に追加されています。
```
const number = [1,2,3];
console.log(numbers);
numbers.push(4); 配列に新しい要素を追加する
console.log(numbers);
```
```
[1,2,3]

[1,2,3,4]
```

##forEachメソッド
forEachメソッドは配列の中の要素を1つずつ取り出して、全ての要素に繰り返し同じ処理を行うメソッドです。
以下の例では、配列numbersの要素が順番にすべて出力されています。
次のスライドで、実際にどのような処理がされているか詳しく確認しましょう。

```
const number = [1,2,3];
numbers.forEach((number)=>{　配列の中の要素を一つずつ取り出して同じ処理をする
  console.log(number);　　　　引数に入っている関数はコールバック関数と呼びます
});
```
```
1
2
3
```
forEachメソッドの引数には、アロー関数が入っています。
配列内の要素が1つずつ順番にアロー関数の引数に代入され、処理が繰り返し実行されます

## findメソッド
findメソッドとは、コールバック関数の処理部分に記述した条件式に合う1つ目の要素を配列の中から取り出すメソッドです。
以下の例では、配列numbersの中から、3より大きい1つ目の要素である5が
foundNumberに代入されコンソールに出力されています。

```
const numbers = [1,3,5,7];
const foundNumber = numbers.find((number){
  return number >3;
});

console.log(foundNumber);
```
```
5  条件に合う要素だけ呼び出される
```
配列numbersの要素が1つずつ引数numberに代入されて処理が進みます。
コールバック関数の中は { return 条件 } と書くことで、条件に合う要素が戻り値となります。findメソッドは条件に合う要素が見つかった時に終了するので、条件に合う最初の1つの要素しか取り出せません。


## filterメソッド
filterメソッドとは記述した条件に合う要素のみを取り出して新しい配列を作成するメソッドです。
以下の例では配列numbersから「3より大きい数字」をすべて取り出しています。

```
const number =[1,3,5,7];
const filtredNumbers = numbers.filter((number)=>{
  return number >3; 要素の中で３より大きい数を取り出す
});

console.log(filterNumber);
```
```
[5,7]  条件に合うものが全て出されて新しい配列が定数filterNumbersに代入される
```

## mapメソッド
mapメソッドとは、配列内のすべての要素に処理を行い、その戻り値から新しい配列を作成するメソッドです。
以下の例では配列numbersの全ての要素を2倍した要素を持つ、新しい配列を作成しています。

```
const numbers = [1,2,3];
const doubledNumbers = numbers.map((number)=>{
  return number *2;
});

console.log(doubledNumbers);
```
```
[2,4,6]  配列numbersに全て数値が2倍され、定数doubledNumbersに代入された
```
配列numbersの要素が1つずつ引数numberに代入されています。その後、mapメソッド内の 「要素を2倍する処理」 をした配列が新しく作られ、定数doubledNumbersに配列として代入されています。
コールバック関数の中の処理は { return 値 } と書きます。














