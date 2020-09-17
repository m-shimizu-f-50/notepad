# pry-byebug を使ってRailsアプリをデバックする方法

エラーの修正に手こずり、エラーの原因の特定が最初は難しく感じるかもしれません。そんな時のために、Rubyでは、pry-byebugと呼ばれるデバッガと呼ばれる、バグを修正するためのツール（gem）が用意されています。

## pry-byebugのインストール方法
Railsのプロジェクトの`Gemfile`に以下のように記述します。

```
gem 'pry-byebug', group: :development
```
追記してファイル保存したら、`bundle install`してインストール完了です。

## pry-byebugの使い方
基本はコード中にブレークポイントを書いて、デバッグモードにすることです。

デバッグをしたい箇所をブレークポイントとして、`binding.pry`を記述するだけです。Viewファイルでは`% binding.pry %>`のように記述します。

たとえば、メッセージボードで、ユーザー情報をモデルに渡してDB保存するコントローラのcreateアクション（メソッド）で、パラメータを確認したいという場合。
```
例）
def create
  @user = User.new(user_params)

  binding.pry

  if @user.save
    flash[:success] = 'ユーザーを登録しました'
    redirect_to @user
  else
    flash[:danger] = 'ユーザーを登録しました'
    render :new
  end
end
```
4行目あたりにbinding.pryがあるのがわかりますか？では、Railsサーバを起動して、試しにユーザー登録をしてみます。

```
From: /home/ec2-user/environment/microposts/app/controllers/users_controller.rb @ line 19 UsersController#create:

    14: def create
    15:   @user = User.new(user_params)
    16:
    17:   binding.pry
    18:
 => 19:   if @user.save
    20:     flash[:success] = 'ユーザーを登録しました'
    21:     redirect_to @user
    22:   else
    23:     flash[:danger] = 'ユーザーを登録しました'
    24:     render :new
    25:   end
    26: end

[1] pry(#<UsersController>)> @user
=> #<User:0x007f76c6026360 id: nil, name: "namae", email: "test@gmail.com", password_digest: "$2a$10$OWIqmjdKx/obwOquxDZLBenUmsu3o2KNT9Vscl2mLqU4ZP/WjTlNG", created_at: nil, updated_at: nil>
```
登録をするとターミナルで上記の表示がされます。`binding.pry`のブレークポイントで処理が止まります。

試しに@userと入力したら、それまでに入力した値などが出力されています。

```
[2] pry(#<UsersController>)> user_params
=> <ActionController::Parameters {"name"=>"namae", "email"=>"test@gmail.com", "password"=>"pass", "password_confirmation"=>"pass"} permitted: true>

[3] pry(#<UsersController>)> new
=> #<User:0x007f76c5bacc80 id: nil, name: nil, email: nil, password_digest: nil, created_at: nil, updated_at: nil>

[4] pry(#<UsersController>)> User
=> User(id: integer, name: string, email: string, password_digest: string, created_at: datetime, updated_at: datetime)
```
他も試しに入力して見ましょう。

user_paramsメソッド：このメソッドは出力制限されてないものだけ出すメソッドで、見事フィルタリングされて出力されています
newメソッド：モデルに入力したテーブル構造がnilで出力されました
Userモデル：モデルに入力したテーブル構造が出力されました
こんな感じでデバックするのか、とわかるかと思います。


##pry-byebug」のコマンド確認
最後にデバッグ（pry-byebug）に使えるコマンドを4つ紹介します。

|内容  |コマンド|
|:------:|:------:|
|ステップインで先へ進める | step / s|
|ステップオーバーで先へ進める|  next / n|
|現在のスコープを抜ける |finish / f|
|デバッグを終了して処理を再開する|  continue / c|






























