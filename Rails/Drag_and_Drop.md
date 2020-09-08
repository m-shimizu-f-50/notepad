# ドラッグ＆ドロップで並び替えて順番を保存できるリストを作る
`Rails` `act_as_list` `sortable.js`

## Introduction
例えばお気に入りリストアプリなどを作ろうとすると、リストのアイテムを順番通りに整列させて、ドラッグ＆ドロップで並び替えて、その順番を保存したい場面があると思います。
jQuery UIのsortableを利用している記事が多かったのですが、モバイルでも利用する前提だと他のjsも使わなくてはいけなかったりと面倒だったのですが、Sortable.jsというライブラリを利用すればモバイルにも対応してくれていたので「Sortable.js」＋「acts_as_list」を使ってドラッグ＆ドロップで並び替えできるリストを実装してみました。


## 0. Model
`User(belongs_to)`と`favorite(has_many)`で紐付ける。(今回はお気に入りのストレッチなので`stretch(has_many)`も紐づける)

Userは一つのお気に入りリスト(belongs_to)を持ち、favoriteは一人のユーザーに対して複数のお気に入りストレッチを持ちます

## 1. favoriteモデルに順番の概念を与える
モデルに順番の概念を与えるために`acts_as_list`を使っています

[GitHub - swanandp/acts_as_list](https://github.com/brendon/acts_as_list)
`Gemfile`
```gem:gem
gem 'acts_as_list'
```
```
$ bundle install
```
`acts_as_list`がインストールできたら、favoriteにposition:integerのカラムを追加してください。

`app/models/user.rb`
```models:app/models/user.rb
class User < ApplicationRecord
  has_many :favorites, -> { order(position: :asc) }
  has_many :stretchs, dependent: :destroy
end
```

`app/models/favorite.rb`
```models:app/models/favorite.rb
class Favorite < ApplicationRecord
  belongs_to :user
  belongs_to :stretch
  acts_as_list scope: :user
end
```

`app/models/stretch.rb`
```
# お気に入り投稿と関連付け
  has_many :favorites, dependent: :destroy
  has_many :favorite_users, through: :favorites, source: :user
  belongs_to :user, optional: true
```
これでfavoriteモデルに順番の概念を与えることができました。
favoriteがcreateされるごとにpositionカラムにシーケンシャルな数字が自動で追加してくれます。
また、用意されているメソッドを使うことで順番を変更することができます。
```
@favorite.insert_at(2)   #position=2に移動
@favorite.move_lower     #position++
@favorite.move_higher    #position--
@favorite.move_to_top    #position=1
@favorite.move_to_bottom #position=last
```

## 2.ドラッグ＆ドロップで並び替え可能なリストを作る
これには`Sortable.js`を使います。

[GitHub - SortableJS](https://github.com/SortableJS/Sortable)

GitHubからSortable.min.jsをダウンロードして`app/assets/javascripts/`に配置します。

Sortable.jsでは`getElementByIdでHTMLObject`を取得することでリストを並び替え可能にします。(Id='sorttable_list
')

``` controller:users_controller.rb
class UsersController < ApplicationController
  def show
    @user = current_user
    @reviews = Review.includes(:user, :stretch).where(user_id: @user.id).page(params[:page])  //ユーザーのレビュー一覧
    @favorite = Favorite.where('user_id = ?', @user)
    @favorites = @user.favorites.page(params[:page])　　//お気に入りリスト
  end
end
```

```html:_favorite.html.erb
<div class="likes-content"  id="sortable_list">
<% @favorites.each do |favorite| %>
    <div class='row' id='favorite-item'>
      <div class='col-md-6'>
        <%= link_to stretch_path(favorite.stretch),data: {"turbolinks" => false} do %>
          <% if favorite.stretch.stretch_images.present? %>
            <%= attachment_image_tag favorite.stretch.stretch_images.first, :image, :fill, 500, 500 , size: "300x300"%>
          <% end %>
        <% end %>
      </div>
      <div class='col-md-6' >
        <h2>ストレッチ名：<%= favorite.stretch.name %></h2>
        <h2>作用する筋肉：<%= favorite.stretch.action_muscles %></h2>
      </div>
    </div>
<% end %>
</div>

<%= hidden_field_tag :user_id, @user.id %>
<div class='child-center'>
  <%= paginate @favorites %>
</div>

<!-- ドラッグ＆ドロップ -->
<script src="drag.js"></script>
```

```javascript:drag.js
// ドラッグアンドドロップ
$(function() {
    var el, sortable;
    // getElementByIdで"sortable_list"htmlを取得
    el = document.getElementById("sortable_list");
    token = $('meta[name="csrf-token"]').attr('content');
    if (el !== null) {
      return sortable = Sortable.create(el, {
        delay: 100,
        animation: 150,
        onUpdate: function(evt) {
          // ドラッグ＆ドロップした際に発火
          return $.ajax({
            url: '/users/' + $("#user_id").val() + '/sort',
            type: 'patch',
            data: {
              from: evt.oldIndex,
              to: evt.newIndex
            }
          });
        }
      });
    }
  });
```

## 3. 並び替えをしたらDBが更新する
Sortable.jsでは並び替えが行われたときにonUpdateが呼び出されます。
これをトリガーにajaxでドラッグ＆ドロップされた要素の「ドラッグ前の位置」と「ドラッグ後（ドロップ）の位置」をバックエンドに連携して並び順をDB更新するようにします。

まずは「ドラッグ前後の位置」を受け取れるようにコントローラー側にsortアクションを追加します。
urlはparent/:id/sort、from（ドラッグ前の位置）とto（ドラッグ後の位置）を受け取る前提とします。

```
def sort
    @user = current_user
    favorite = @user.favorites.find_by(position: params[:from].to_i + 1)
    # ポジションカラムを指定してドラッグの前の位置を取得
    favorite.insert_at(params[:to].to_i + 1)
    # 新しいポジションを更新する
    head :ok
  end
```
favorite = @user.favorites.find_by(position: params[:from].to_i + 1)で対象のモデルを取得しています。
そして、favorite.insert_at(params[:to].to_i + 1）で対象のpositionを更新しています。
Sortable.jsで取得するリストの位置は0から始まるのに対して、insert_atは1から始まるところに注意して、insert_at向けの要素の順番には+1をしています。

Rails4ではrender nothing: trueなどが有効だったようですがRails5では使えなくなっているのでhead :okでフロント側に200OKを返却。

ルーティングも追加しておきます。
```
 patch 'users/:id/sort', to: 'users#sort'
 ```

## Conclusion
割とシンプルにドラッグ＆ドロップでの並び替えができたと思います。しかもモバイル対応！
ただし、acts_as_listの仕様上、並び替えの度にけっこうなDB更新がかかっていそう（from〜toの間の全てのレコードを更新しているっぽい）。リストが大きくなることが想定されている場合は順番をつける方式は検討した方が良いかも。ranked_modelの方がいいのかな？









































