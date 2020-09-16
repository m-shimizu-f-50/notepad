# Rails でよくあるエラー


## はじめに
Webアプリケーションを作成して行く段階でエラーが出るのはつきもの！
そのなかで、いかにエラーとしっかり向き合うかが重要かと個人的には思っております！

## エラー文

# undefined method 'id' for nil:NilClass

```
例）
def update
  @tweet = Tweet.find(params[:id])
  if @tweet.update(tweet_params)
    ...
end
```

上のようなコードがあったとしましょう！

このとき、`undefined method 'update' for nil:NilClass`とエラーが出たと仮定します！

この意味としては、NilClassのnilオブジェクトに対して、`update`メソッドは定義されていないよ！ってことです。

つまり、@tweetがnilになっているということです！

findメソッドは、見つからなかった場合にnilオブジェクトを返します。

そのため、このような簡素的なコードでは、エラーが起こりにくいかもしれませんが、
UsecasesやServicesなどのようにディレクトリを分岐させていった時に直面するので、findメソッドを使った時は、@tweet.present?を書いてハンドリングすることをお勧めします。

＊ハンドリング・・・エラーに対して対処する事

さらに「Did you mean? name_was」があります。
これは、記述してあるメソッドがスペルミスで、name_wasではないのか？とRailsが提案しています。
これでエラーが解消されることもあります

# undefined local variable or method

これもあるあるで、エラー文で示された行に`未定義の変数、もしくはメソッド`があると言うことですね！

`undefined`は「定義されていない」という意味です。

よくあるのは、"hello"のようなString型のオブジェクトを返したかったにも関わらず、helloと変数もしくはメソッドにしてしまっていた、、、などがあると思います。

# RoutingError (No route matches [GET] "/tweets")

単に、`URIにマッチするRoutingが存在していない！`ということです。

しかし、これに関しては、たまに起こると思います！

特にdeviseで Userサインアップ、サインインのルーティングが存在している時に、usersコントローラを作成し、usersコントローラのルーティングをdeviseのルーティングの前に記述すると、users/:idのルーティングの時にエラーになるので気をつけましょう。
```
$ rails routes
```
このコマンドでルーティングを確認

または`config/routes.rb`で足りないルーティングがあるか確認する


# ActiveRecord::NodatabaseError

データベースが存在しない時に出るエラーです。

```
$ bundle exec rails db:create # Rails ver5以降
$ bundle exec rake db:create  # Rails ver5以前
```

これでデータベースが作成されるのでエラーが解決します

# SyntaxError
単純に文法のミスです！
タイプミスをしていないかの確認、修正でいいと思います。

# ActionView::MissingTemplete
該当するViewが存在していないよ〜ってことですね！

コントローラに対するViewが存在するかを確認してください！
renderなどを用いると発生しやすいと思います。

# ArgumentError
Argumentは引数のことですので、
メソッドの引数の数などが正しいことを確認しましょう！



