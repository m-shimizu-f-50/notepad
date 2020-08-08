# [Rails]『gemを使わない管理者機能』と『ActionMailerのメール送信機能で問い合わせ機能』を実装

## 実装すること

①ユーザーは問い合わせフォームから問い合わせを送ることができる

②管理者はユーザーからのお問い合わせ一覧から問い合わせの確認ができる

③管理者が問い合わせ詳細から返信すると、ユーザーの登録しているメールアドレスに届く

## ActonMailarとは

ActionMailerを使うと、railsアプリケーションのメイラークラスやビューでメールを送信することができます。

(例)

・メールマガジンの一斉送信

・ウェブサイトに会員登録した時のthank youメール

・お問い合わせフォームの記入内容が管理者にメールでも送信される

参考：Railsガイド　https://railsguides.jp/action_mailer_basics.html

## モデルの作成

今回はUserモデル、Adminモデル、Contactモデルを作成します。
Userモデル、Adminモデルはdeviseを使用してモデルを作成していきます。
deviseのviewはusersフォルダとadminsフォルダそれぞれに作成されるようにします。

## アソシエーションの確認

### Userモデル

`app/models/user.rb`
```
class User < ApplicationRecord
   has_many :contact, dependent: :destroy
end
```

### Contactモデル

reply以外のtitleとcontactにバリデーションを掛けます。

`app/models/contact.rb`

```
class Contact < ApplicationRecord
  belongs_to :user
  validates :title, presence: true
  validates :content, presence: true
end
```

####マイグレーションファイル

```
class CreateContacts < ActiveRecord::Migration[5.2]
  def change
    create_table :contacts do |t|
      t.text :title
      t.text :content
      t.text :reply
      t.integer :user_id
      t.string :email

      t.timestamps
    end
  end
end

```

### コントローラーの作成
```
$ rails g controller users
$ rails g controller admins
$ rails g controller contacts new
```
###メイラーの作成

```
$ rails g mailer ContactMailer
```

### サーバーの設定

今回はGmailを使用するメール送信機能を記述します。

・`raise_delivery_errors`は、メールの送信に失敗した時にエラーを出すかどうかです。

・`default_url_options`で、アプリケーションのホスト情報をメイラー内で使いたい場合は:hostパラメータを明示的に指定します。

・`delivery_method`は、デフォルトでsmtpです。

・`smtp_settings`で、詳細設定をします。

1.port => SMTPサーバーのポート番号

2.address => SMTPサーバーのホスト名

3.domain => HELOドメインを指定する必要がある場合はここで行なう

4.user_name => メール送信に使用するgmailのアカウント

5.password => メール送信に使用するgmailのパスワード

6.authentication => 認証方法

7.enable_starttls_auto => メールの送信にTLS認証を使用するか

developmentは開発環境なので、本番環境でやる際はproductionにも同じ記述をする

`config/environments/development.rb`

```
Rails.application.configure do
  #お問い合わせ送信
  config.action_mailer.raise_delivery_errors = true
  config.action_mailer.delivery_method = :smtp
  config.action_mailer.smtp_settings = {
    port:                 587,
    address:              'smtp.gmail.com',
    domain:               'gmail.com',
    user_name:            '<YOUR EMAIL ADDRESS>',　//サイトで作ったgmailアドレス
    password:             '<YOUR EMAIL PASSWORD>', //上のgmailのパスワード
    authentication:       'plain',
    enable_starttls_auto: true
  }
end

```

### メイラーを編集

`application_mailer`には、全メイラー共通の設定を、

`sample_mailer`には、メイラー個別の設定をします。

#### application_mailer.rb

`defaultメソッド`で、共通の処理・設定を記述します。

`app/mailers/application_mailer.rb`

```
class ApplicationMailer < ActionMailer::Base
  default from: 'from@example.com'
  layout 'mailer'
end
```

#### contact_mailer.rb

管理者の返信が、ユーザーのemailに届くようにします。

個別の設定には`mailメソッド`を使用します。

・`send_when_replyedメソッド`を呼び出す時に渡されるユーザーの情報から、
emailアドレスだけを取り出してメールの送信先としてします。

・`mailメソッド`が呼び出されると、メール本文が記載されているビューが読み込まれます。
インスタンス変数でメイラービューに値を渡してあげたいので、インスタンス変数を用意しています。

`app/mailers/contact_mailer.rb`

```
class ContactMailer < ApplicationMailer
  default from: "smb.stretch50@gmail.com"

  # お問い合わせ送信
  def send_when_admin_reply(user, contact) #メソッドに対して引数を設定
    @user = user #ユーザー情報
    @answer = contact.reply #返信内容
    mail to: user.email, subject: '【SMB】 お問い合わせありがとうございます'
  end
end
```

### メール本文を作成する

１つはHTMLフォーマット、もう一つはテキストメールです。

顧客によってはHTMLフォーマットのメールを受け取ることができない / 受け取りたくない人もいるので、テキストメールも作成しておくのが最善です。

#### contact_mailer/send_when_admin_reply.html.erb

`app/views/contact_mailer/send_when_admin_reply.html`

```
<h2><%= @user.family_name %> 様</h2>
<p>この度は、お問い合わせありがとうございました。<br>
　以下でご質問の回答となっております。</p>

<p><%= @answer %></p>

<p>今後とも SMB をよろしくお願いいたします。</p>
```

#### contact_mailer/send_when_admin_reply.text.erb

```
<%= @user.family_name%> 様

この度は、お問い合わせありがとうございました。
以下でご質問の回答となっておりますでしょうか。

<%= @answer %>

今後とも弊社をよろしくお願いいたします。
```

### お問い合わせフォームの作成（ユーザー側）

####users/contacts_controller.rb

`app/controllers/users/contacts_controller.rb`

```
class ContactsController < ApplicationController
  before_action :authenticate_user!
  before_action only: %i(new show edit)

  def new
    @contact = Contact.new
  end

  def create
    @contact = Contact.new(contact_params)
    @contact.user_id = current_user.id
    if @contact.save
      redirect_to root_path, notice: '質問を送信しました'
    else
      render :new
    end
  end

  private

  # Never trust parameters from the scary internet, only allow the white list through.
  def contact_params
    params.require(:contact).permit(:title,:content, :reply, :user_id ,:email)
  end
end

```

#### users/contacts/new.html.erb

`app/views/users/contacts/new.html`

```
<div class='row'>
    <%= form_for @contact do |f| %>
  <!-- 質問 -->
    <h2>質問</h2>
      <div class="form-group row">
        <div class="col-md-9">
          <%= f.label :title, 'タイトル ', class:'col-md-3 col-form-label' %>
          <%= f.text_field :title, autofocus: true, autocomplete: "title",class: "form-control"%>

          <%= f.label :content, '質問内容 ', class:'col-md-3 col-form-label' %>
          <%= f.text_area :content, class: "form-control", rows: "8",
            placeholder:'ストレッチについて聞きたいことや他にもいろんな質問に答えていきますのでご自由に質問してください！' %>
        </div>
      </div>
      <div class='action'>
        <%= f.submit "送信する",class: "btn-sm btn-succes" %>
      </div>
    <% end %>
  </div>
</div>
```

### 返信処理の作成（管理者側）

・コントローラーのアクションによって、application_mailer.rb / contact_mailer.rbを起動します。

・流れは、

①管理者が問い合わせ一覧(index)から

②問い合わせ詳細(edit)へ飛び、

③返信を送信(update)し、

④その内容をメールでユーザーに送信します。

・ユーザーが問い合わせをした時点で、Contactsテーブルの中に、
レコードが作成されます。その中に「返信(reply)」というカラムが用意されています。
updateアクションを使って、すでにあるレコードの中に管理者の返信のデータだけを更新することで、ContactMailerのsend_when_admin_replyアクションが発火します。


>`deliver_now`について

>いくつかの参考書や解説サイトでは、deliver_nowメソッドではなくdeliverメソッドを使うように記載されています。
>じつは、deliver_nowを使ってメール送信すると、メール送信が完了するまでRailsが反応しなくなることがあります。そのため、Rails4.2から「Active Job」という非同期処理が導入されました。「Active Job」を使うと、メール送信を裏で行わせながら処理を行うことができますので、メール送信完了を待つ必要がなくなるのです。
>そのため、「すぐに送信する」ということが分かるように、deliverではなく、deliver_nowという名称になっているのです。

参考：https://web-camp.io/magazine/archives/19143

#### admins/contacts_controller.rb

`app/controllers/admins/contacts_controller.rb`

```
class Admins::ContactsController < ApplicationController
  before_action :authenticate_admin!
  def index
    @contacts = Contact.all
    @users = User.all
  end

  def show
    @contact = Contact.find(params[:id])
  end

  def edit
    @contact = Contact.find(params[:id])
  end

  def destroy
    @contact = Contact.find(params[:id])
    @contact.destroy
    redirect_to admins_contacts_path, notice: '質問を削除しました'
  end

  def update
    contact = Contact.find(params[:id]) #contact_mailer.rbの引数を指定
    user = User.find(contact.user_id)
    contact.update(contact_params)
    ContactMailer.send_when_admin_reply(user, contact).deliver_now #確認メールを送信
    redirect_to admins_contacts_path,　notice: '質問を返信しました'
  end

   private
    def contact_params
        params.require(:contact).permit(:title, :content, :reply, :user_id ,:email)
    end
end
```

#### 問い合わせ一覧(admins/contacts/index.html.erb)

```
<div class='container'>
  <div class='row'>
    <h2>問い合わせ一覧</h2>
    <table>
        <thead>
        <tr>
          <th>質問ID</th>
          <th>タイトル</th>
          <th>受信日</th>
          <th></th>
          <th></th>
        </tr>
    </thead>
    <tbody>
      <% @contacts.each do |contact| %>
        <tr>
          <td><%= contact.id %></td>
          <td><%= contact.title %></td>
          <td><%=contact.created_at.strftime("%Y-%m-%d %H:%M") %></td>
          <td><%= link_to "詳細", admins_contact_path(contact),data: {"turbolinks" => false}%></td>
          <td><%= link_to "削除", admins_contact_path(contact),　data: {"turbolinks" => false}, method: :delete %></td>
          </tr>
            <% end %>
      </tbody>
    </table>
  </div>
</div>
```

#### お問い合わせ詳細(admins/contacts/show.html.erb)

```
<div class='container'>
    <div class='row'>
        <h2>お問い合わせ内容</h2>
        <%= form_for @contact, url: admins_contact_path do |f| %>
            <h3>顧客名</h3>
            <%= @contact.user.family_name %>さん
            <h3>問い合わせ日</h3>
            <%= @contact.created_at.strftime("%Y年%m月%d日 ") %>
            <h3>問い合わせ内容</h3>
            <%= @contact.content %>
            <h3>返信</h3>
            <%= f.text_area :reply, class:"form-control", placeholder: "返信内容" %>
            <%= f.submit "送信" %>
        <% end %>
    </div>
</div>
```

#### お問い合わせ内容(admins/contacts/edit.html.erb)

```
<div class='container'>
  <div class='row'>
    <h2>お問い合わせ内容</h2>
    <%= form_for @contact, url: admins_contact_path do |f| %>
      <h3>顧客名</h3>
      <%= @contact.user.family_name %>さん
      <h3>問い合わせ日</h3>
      <%= @contact.created_at.strftime("%Y年%m月%d日 ") %>
      <h3>タイトル</h3>
      <%= @contact.title %>
      <h3>問い合わせ内容</h3>
      <%= @contact.content %>
      <h3>返信</h3>
      <%= f.text_area :reply, class:"form-control", placeholder: "返信内容" %>
      <%= f.submit "送信" %>
    <% end %>
  </div>
</div>

```

### Google側での設定

メールを送る際に必要な設定

本番環境でも使用するため２段階認証の方が好ましい(セキュリティーの問題)

ブックマークでサイトを確認(やり方)

・2段階認証をオンにする

https://support.google.com/accounts/answer/185839?co=GENIE.Platform%3DDesktop&hl=ja
・アプリケーション用のパスワードを発行する

https://support.google.com/accounts/answer/185833?hl=ja

・2段階認証プロセスで使用するパスワードとコード

https://support.google.com/accounts/answer/1070457?hl=ja





