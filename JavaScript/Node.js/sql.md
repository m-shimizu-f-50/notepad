#Node.jsにMysqlを接続

##mysqlパッケージをインストール
`ターミナル`
```
npn install myspl
```
## Mysqlに接続
mysqlパッケージを読み込み、createConnectionメソッドを用います。データベースに接続するための情報を定数connectionに代入しましょう。これでMySQLへの接続が完了です
`app.js`
```
const myspl = repuire('mysql');  //mysqlパッケージの読み込み
const connection =myspl.createConnection({
  //データベース名、パスワードなど
})
```

##一覧画面を表示するまでの流れ
connection.query('クエリ', クエリ実行後の処理)と書くことで、Node.jsからデータベースに対してクエリを実行することができます。
`app.js`
```
connection.query(
  'SELECT*FROM items',　//クエリ
  ()=>{
    //一覧画面の表示
  }
)
```
クエリ実行後の処理は2つの引数を取ることができます。
第1引数のerrorにはクエリが失敗したときのエラー情報が、第2引数のresultsにはクエリの実行結果（ここでは取得したメモ情報）が入ります。

```
connection.query(
  'SELECT*FROM items',
  (error,result)=>{
    console.log(results);
    res.render('index.ejs');
  }
);
```
###取得した値の表示
EJSはrenderメソッドから値を受け取ることができます。renderメソッドの第2引数に{プロパティ : 値}と書くことで、EJS側に値を渡すことができます。今回はデータベースから取得した値を使いましょう。
`app.js`
```
app.get('/index',(req,res)=>{
  connection.query('SELECT*FROM items',
    (error,result)=>{
      res.render('index.ejs'{items:results}) //オブジェクトを渡す
    }
  );
};
```
`index.ejs`
```
<% items.forEach((item)=>{%>  //オブジェクトのプロパティで値を使える
```
####getとpostとは
Webでは、サーバーへリクエストするときに、どんな処理をしたいかをメソッドで伝えるようにルールで決まっています。

>get
画面を表示したいときはGETを使う

>post
データベースを変更したいときはPOSTを使う


##フォームを使ったリクエスト
フォームを作るときはHTMLの<form>タグを用います。 action属性には送信先のURL、method属性には「post」か「get」をルーティングに合わせて指定します。
`new.ejs`
```
<form action='/create' method='post'> //ルーティングに合わせて指定する
  <input type='text'>
  <input type='submit' value='作成する'>
</form>
```
`app.js`
```
app.post('/create',(req,res)=>{
  ・・・・
});
```
###一覧画面を表示しよう
`app.js`
```
app.post('/create',(req,res)=>{
  //メモを追加する処理

  //一覧画面を表示する処理
  connection.query(
    'SELECT*FROM items',
    (error,result)=>{
      res.render('index.ejs'{items:results})
    }
  );
});
```

###フォームの値を受け取る
input要素にname属性を指定すると、右の図のようなオブジェクトの形で情報がサーバーに送信されます。よってサーバー側ではreq.body.name属性の値でフォームの値を取得できます。
`new.ejs`
```
<form action='/create' method='post'>
  <input type='text' name='itemName'>  //name属性を指定する
  <input type='submit' value='作成する'>
</form>
```

`app.js`
```
app.post('/create',(req,res)=>{ //res.body.itemNameに入力したものが入る
  ・・・・
});
```

＊フォームの値を受け取るにはapp.jsで下図のソースコードを追加する必要があります。
`app.js`
```
app.use(express.urlencoded({extended:false}));
//フォームの値を受け取るための定型文

app.post('/create',(req,res)=>{
  console.log(req.body.itemNmae); //フォームの値を取得する
});
```

###データを追加する
SELECTの時と同様にqueryメソッドを使うことで　INSERTを実行することができます。itemsテーブルのidにはAUTO INCREMENTを設定しているので、idの値を指定する必要はありません。

フォームからの値をクエリに使うときは、VALUESに「?」を含めます。次
に「connection.query()」の第2引数に渡したい配列を指定します。この配列の要素が「?」の部分に入り、実行されます。

`app.js`
```
app.post('/create',(req,res)=>{
  connection.query(
    'INSERT INTO item s(name)VALUE(?),
    [ req.body.itemName], //配列の要素が？に入る
    (error,result)=>{
      res.redirect('/index'); //URLを指定してリダイレクトする
      );
    }
  );
});
```
＊リダイレクトとは

サーバーは「次はこのURLにリクエストしてね」というレスポンスを返すことができます。このレスポンスを受け取ったブラウザは指定されたURLに自動的にリクエストします。このような別のURLに再度リクエストさせる仕組みをリダイレクトと言います。

リダイレクトを用いると、追加処理後に/indexにリクエストして一覧画面を表示することができます。これによりメモ作成後にリロードしても、追加処理が実行されないのでメモが増えません。













