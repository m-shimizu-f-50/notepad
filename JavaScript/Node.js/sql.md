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


### 値の更新・削除

###削除
####削除機能の手順
```
①削除ボタンを押しフォームをNode.jsに送信
②Node.jsからデータベースの中の値を削除
③Node.jsからリダイレクトする画面を表示
```
####①削除ボタンを押しフォームをNode.jsに送信
#####ルーティングを用意
メモを削除するためのルーティングを用意しましょう。データベースを変更する処理なのでpostを用います。
`app.js`
```
app.post('/delete',(req,res)=>{　データベースを変更するのでpostを使う
  //処理
});
```

#####削除ボタンを用意
削除ボタンを作りましょう。ボタンはフォームで作成します。ルーティングに合わせてフォームの`action属性`と`method属性`を指定しましょう。
`index.ejs`
```
<form action="/delete" method="post">  //ルーティングに合わせて指定する
  <input type="submit" value='削除'>
</form>
```

#####リダイレクトする
フォーム送信後には一覧画面を表示します。削除機能はデータベースを変更する処理なので、リダイレクトを用いて一覧画面を表示しましょう。
`app.js`
```
app.post('/delete',(req,res)=>{　データベースを変更するのでpostを使う
  //メモを削除する処理

  //一覧画面にリダイレクトする処理
  res.redirect('/index');  //URLを利用してリダイレクトする
});
```

####②Node.jsからデータベースの中の値を削除をする処理
削除にはDELETEクエリを使います。しかし現状では、サーバーはどのメモをWHEREに指定すれば良いか分からず削除できません。サーバーが削除するメモのidを受け取ることができれば、削除できるようになります。

#####URLで値を渡そう
idを受け渡すにはURLを利用します。リクエストするURLは/delete/3のようにidを含めるようにし、ルーティングのURLは/delete/:idのように指定します。これでURLに含まれたidを取得できるようになります。/:idの部分を`ルートパラメータ`と呼びます。

#####ルートパラメータを使う
ルーティングに合わせてフォームの送信先URLにメモのidを含める

`index.ejs`
```
<form action="/delete/<%=item.id%>" method="post">  //送信先URLにitemのidを含める
  <input type="submit" value='削除'>
</form>
```
`app.js`
```
app.post('/delete/:id',(req,res)=>{  //ルーティングURLにルートパラメータを指定する
  connection.query(
    'DELETE FROM items WHERE id=?',
    [req.params.id],  //[req.params.id]の値が？に入る
    (error,results)=>{
      res.redirect('/index');
    }
    );
});
```


###編集・更新
####編集・更新機能の手順
```
①編集ボタンを押しフォームをNode.jsに送信
②Node.jsからデータベースの中値から編集する値を取得
③Node.jsから編集画面を表示
④取得結果を表示
```

#####ルーティングを作成
一覧画面に編集ボタンを作成し、そのリンクのURLにメモのidを含めるようにします。ルーティングにはルートパラメータを使用してそのメモのidを受け取れるようにしましょう。idは選択されたメモ情報を取得するときに使います。

`index.ejs`
```
<li>
  ・・・・・
  <a href='/edit/<%=item.id%>'>編集</a>  //リンクで値のidを含んだ編集ボタンを作成する
</li>
```
受け取ったメモのidを利用して編集する値を取得
`app.js`
```
app.get('/edit/:id',(req,res)=>{  //ルートパラメータでidを受け取れるようにする
  connection.query(
    'SELECT* FROM items WHERE id=?',
    [req.params.id],  //[req.params.id]の値が？に入る
    (error,results)=>{
      res.redirect('edit.ejs',{item: result[0]});
      //配列resultsから1件目の要素を取り出し、edit.ejsにitemプロパティを渡す(0=1件目)
    }
  );
});
```

#####フォームに値を表示
value属性に値を指定すると、フォームに初期値を表示できます。取得したメモの内容をvalue属性に指定
`edit.ejs`
```
<form>
  <input value="<%=item.name%>" type="text">  //value属性に取得した値の内容を指定
</form>
```

####更新処理の手順
```
①編集ページから編集した値をフォームでNode.jsに送信
②Node.jsからデータベースの中の値を更新する
③リダイレクトでNode.jsから一覧画面を表示
```
#####更新に必要な情報
更新にはUPDATEクエリを使います。メモを更新するには、`フォームの値`と`値のid`が必要です。この2点を渡せるようにコードを書く

#####ルーティング
まずはルートパラメータを用いてidを渡します。更新のルーティングを用意。送信先URLには、事前にedit.ejsに渡していたitemのidを含めましょう。また、フォーム送信後には一覧画面へリダイレクトします。
`index.ejs`
```
<form action="/update/<%=item.id%>" method="post">  //送信先URLにitemのidを含める
  <input name="itemName" value="..." type="...">  //name属性を指定する
</form>
```

`app.js`
```
app.post('update/:id',(req,res)=>{
  connection.query(
    'UPDATE items SET name=? WHIRE id=?',
    [req.body.itemName,req.params.id],  //[req.params.id]の値が？(WHERE id=?)に入る
    (error,results)=>{                  //[req.body.itemName]の値が？(name id=?)に入る
      res.redirect('/index');
    }
  );
});
```

























