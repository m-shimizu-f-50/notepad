#Node.js

## Node.jsとは？
Node.jsは本来クライアント側の言語である`JavaScriptをサーバーサイドで動かす`ための仕組みなのじゃ。

### Node.jsのフレームワーク
Node.jsには便利な機能を簡単に使えるようにまとめたパッケージというものがあります。`Express`もパッケージの１つです。

パッケージを用意するには`npm (Node Package Manager)`というシステムを使います。npmにはパッケージを共有・取得する機能があります。

npm installコマンドを使って、インターネットから自分のアプリケーションにパッケージをインストールする
```
$ npm install express
```

Expressを使うには、インストールしたパッケージの読み込みと、実際に使用するための準備が必要です。
`app.js`
```
//定型分
const express = require('express'); expressの読み込み
const app = express(); expressを使用するための準備
```

### listenメソッド
listenメソッドを用意してapp.jsファイルを実行するとサーバーを起動することができます。ファイルを実行するには「node ファイル名」とします。

`app.js`
```
app.listen(3000); licallhost:3000でアクセス可能なサーバーを起動する
```
`ターミナル`
```
$node app.js
```

## ルーティング
/topにリクエストが来た時に、トップ画面を表示することができます。URLに対応する処理を実行することをルーティングといいます。

`app.js`
```
app.get('/top',(req,res)=>{
  //トップ画面を表示
  });
```
ルーティングの処理では、`req(リクエストの略)・res(レスポンスの略)`の２つの引数を受け取ります。reqやresには、リクエスト・レスポンスに関する情報が入っています。
```
app.get('/top',(req,res)=>{
  res.render('top.ejs'); 指定したファイルを画面に表示させる
});
```

## CSSを適用
1.Expressでは、CSSや画像などのファイルがどこに置かれているかを指定する必要があります。今回は例でpublicというフォルダにこれらのファイルを置いていきましょう。
`app.js`
```
app.use(express.static('public')); publicフォルダを読み込めるようにする
```
2.publicフォルダの中にcssファイルを作成します。CSSを適用したい場合は、 EJSファイルでpublicフォルダ内のパスを指定します。
`top.ejs`
```
<link rel = 'stylesheet' href = '/css/style.css'> public内のパスを指定してCSS読み込み
```

## 画像を使う
画像ファイルはiamgesフォルダの中に置く

`top.ejs`
```
<img src = '/images/top.png'
```

## EJSを使って値を表示する
EJSは、HTMLとJavaScriptのコード両方を記述できるNode.jsのパッケージ
### EJSをインストール
`ターミナル`
```
$ npm install ejs
```
## JavaScriptを記述
JavaScriptのコードを記述するには、<% %>または<%= %>で囲みます。
<% %>で囲んだ場合はブラウザに何も表示されないので、変数の定義などに用います。変数の値などをブラウザに表示したい場合は<%= %>を用います。

例）
`index.ejs`
```
<% items.forEach((item)=>{  forEachでitemを繰り返し処理する
  <li>
    <span><%=item.id%><span>
    <span><%=item.name%><span>
  </li>
<%});%>
```













