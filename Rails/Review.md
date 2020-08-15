# 【Rails】レビュー用の星★の評価を実装する（入力、保存、表示）

`Ruby` `JavaScript` `Rails` `jQuery`

## 実現したいこと

1.星★による評価の入力と保存

まずは、newやeditページで★による入力と保存を行います

2.星★による評価の表示

次に、newやeditページで入力・保存した★による評価の値をshowページやindexページで表示します。

## 前提

reviewsテーブル(口コミ投稿用のテーブル)のrateカラム（型:float）に評価値を保存する
すでに、テーブルとカラムは作成されていることを前提に進めます。

カラムは、半分の星による評価を行う場合、「0.5」や「1.5」という値を保存することになるため、float型にしておく必要があります。

もし、integerやstringの型として作成してしまっていた場合は、データベースによって変更方法は異なりますが、型の変更が必要になります。下記の記事でこの型の変更の沼にハマってしまった時の解決策を書いています。


## 参考

[jQuery Raty](http://itref.fc2web.com/javascript/jquery/raty.html)

[Railsで星の評価機能をつける](https://qiita.com/kitaokeita/items/1e40724c3384507cec13)

[jQuery Ratyの使い方](https://github.com/wbotelhos/raty)


## javascriptの読み込み、★画像の読み込み

上記参考リンクを確認しながら、javascriptの読み込みと星の画像の読み込みを行います。

やり方（２種類）

①jQuery Ratyを使用するには、https://github.com/wbotelhos/raty からjquery.raty.jsをダウンロードする。

  ダウンロードしたスクリプトをウェブサイトの任意の場所に配置する。

  app/javascriptsのフォルダの中にダウンロードしたjquery.raty.jsファイルを配置する

②jQuery Ratyを使用するHTMLの中でJavaScriptを読み込む。RatyはjQueryのプラグインなので、jQueryのスクリプトも必要である。

```
    <script src="/js/jquery.min.js"></script>
    <script src="/js/jquery.raty.js"></script>
```


## マイグレーションファイル

create_reviews.rb
```
  class CreateReviews < ActiveRecord::Migration[5.2]
    def change
      create_table :reviews do |t|
        t.string 'title'
        t.text 'content'
        t.string 'picture'
        t.float 'rate'
        t.integer 'user_id'
        t.integer 'stretch_id'

        t.timestamps
      end
    end
  end
```

## モデルの記述（紐付け）

stretch.rb
```
    # 口コミ投稿との関連付け
    has_many :reviews, dependent: :destroy
```

review.rb
```
  # ユーザーとの関連付け
    belongs_to :user

  # ストレッチとの関連付け
    belongs_to :stretch

  # バリデーション
  validates :title, presence: true, length: { maximum: 50 }
  validates :rate, presence: true
  validates :rate, numericality: {
    # only_integer: true,
    less_than_or_equal_to: 5,
    greater_than_or_equal_to: 1,
  }

      ＊numericality＝空を許可する
        numericalityは、デフォルトでは小数も許容してしまいます。ageカラムでは整数のみ許可したいので、 only_integerを。
      ＊例
        validates :param5, :numericality => { :less_than_or_equal_to => 5 }
        # 数字が5以下であるか
        validates :param3, :numericality => { :greater_than_or_equal_to => １ }
        # 数字が１以上であるか

  # validates :content, presence: true
  validates :content, length: { maximum: 300 }
  validates :user_id, uniqueness: { scope: [:stretch_id] }
```

## コントローラーの記述

reviews_controller.rb
```
class ReviewsController < ApplicationController
  before_action :authenticate_user!
  def new
    @review = Review.find(params[:id])
  end

  def create
    @categorys = Category.where(is_valid: true)
    @stretch = Stretch.find(params[:review][:stretch_id])
    @review = current_user.reviews.build(review_params)
    if @review.save
      redirect_to stretchs_path, notice: '口コミを登録しました'
    else
      flash[:alert] = '既に投稿されています'
      render 'stretchs/show'
    end
  end

  def destroy
    review = current_user.reviews.find_by(id: params[:id]).destroy
    redirect_to stretch_path(review.stretch.id), notice: '口コミを削除しました'
  end

  def edit
    @review = Review.find(params[:id])
  end

  def update
    if Review.update(review_params)
      redirect_to reviews_path
    else
      render :edit
    end
  end

  def index
    @reviews = Review.includes(:user, :stretch).order(created_at: :desc).page(params[:page])
  end

  private

  def review_params
    params.require(:review).permit(:title, :rate, :content, :picture, :stretch_id)
  end
end
```

stretchs_controller.rb
```
class StretchsController < ApplicationController
  before_action :authenticate_user!
  def index
    # is_validがマッチするレコードを全て取得
    @categorys = Category.where(is_valid: true)
    # 検索機能(検索ワードをparams[:q]で受け取り＠stretchsに入れる)
    @q = Stretch.all.ransack(params[:q])
    @stretchs = @q.result(distinct: true).page(params[:page])
    @title = 'ストレッチ'
    @reviews = Review.includes(:user, :stretch)
  end

  def show
    @review = Review.new
    @categorys = Category.where(is_valid: true)
    @stretch = Stretch.find(params[:id])
    @reviews = Review.where(stretch_id: @stretch.id).page(params[:page])
    @favorite = current_user.favorites.find_by(stretch_id: @stretch.id)
    @favorites = @stretch.favorite_users
  end
end

```

[Rails で includes して N+1 問題対策](https://qiita.com/hirotakasasaki/items/e0be0b3fd7b0eb350327)
## 口コミ投稿フォームを作る

```
<!-- 口コミ入力フォーム -->
<div class='review-form'>
  <%= form_for review do |f| %>

    <!-- ストレッチコードの送信 -->
    <%= f.hidden_field :stretch_id, { value: stretch.id} %>

    <!--　タイトル -->
    <div class="form-group row">
      <%= f.label :title, 'タイトル ', class:'col-md-3 col-form-label' %>
      <div class="col-md-9">
        <%= f.text_field :title, class: "form-control" %>
      </div>
    </div>

    <!-- 評価 -->
    <div class="form-group row" id="star">
      <%= f.label :rate,'評価 ', class:'col-md-3 col-form-label' %>
      <%= f.hidden_field :rate, id: :review_star %>
    </div>

    <!-- レビューjavascript -->
    <script>
    $('#star').raty({
      size     : 36,
      starOff:  '<%= asset_path('star-off.png') %>',
      starOn : '<%= asset_path('star-on.png') %>',
      starHalf: '<%= asset_path('star-half.png') %>',
      scoreName: 'review[rate]',
      half: true,
    });
    </script>

    <!-- 口コミ -->
    <div class="form-group row">
      <%= f.label :content, '口コミ内容 ', class:'col-md-3 col-form-label' %>
      <div class="col-md-9">
        <%= f.text_area :content, class: "form-control", rows: "8",
          placeholder:'口コミ内容がなくても、タイトルと評価のみで投稿できます。
  まずは投稿してみましょう！投稿してから編集もできますよ！' %>
      </div>
    </div>

    <!-- 確認ボタン -->
    <div class="form-group row justify-content-end">
      <div class="col-md-9">
        <%= f.submit '投稿する', class:"btn btn-succes" %>
      </div>
    </div>
  <% end %>
</div>
```

### ポイントは下記

    # ★の半分の入力ができるようにするため
      half: true,



## 星による評価の表示

口コミ投稿一覧にて、★を表示したい。
上記の「★の入力、保存」と異なる点は、1.入力はせず表示する 、 2.繰り返し処理をするということになります


```
<% if reviews.present? %>
  <% reviews.each do |review| %>
    <div class="row" id="reviews">
      <div class="col-md-8 offset-md-2">
        <div class=" review-content">
          <p><%=review.created_at.strftime("%Y-%m-%d %H:%M") %></p>
          <h4 class="col-mb-3">評価：<%= review.rate %>点 | <%= review.title %></h4>
          <!--　レビュー　-->
         <div id="star-rate-<%= review.id %>"></div>
          <script>
            // 繰り返し処理でもidをidを一意にできるようにreview_idを入れる
          $('#star-rate-<%= review.id %>').raty({
            size: 36,
            starOff:  '<%= asset_path('star-off.png') %>',
            starOn : '<%= asset_path('star-on.png') %>',
            starHalf: '<%= asset_path('star-half.png') %>',
            half: true,
            // 読み込みだけで入力できない
            readOnly: true,
            score: <%= review.rate %>,
          });
          </script>

          <p><%= review.content %></p>
        </div>
        <div class="edit-button">
        <% if current_user == review.user %>
          <%= link_to '口コミを編集', edit_review_path(review), method: :get, class:'btn btn-primary' %>
          <%= link_to '口コミを削除', review, method: :delete, class:'btn btn-danger' %>
        <% end %>
      </div>
      </div>
      <div class="col-md-4">
        <%= link_to review.stretch,data: {"turbolinks" => false} do %>
          <div class="reviews-stretch">
            <% if review.stretch.stretch_images.present? %>
              <%= attachment_image_tag review.stretch.stretch_images.first, :image, :fill, 500, 500, size: "300x300" %>
            <% end %>
            <p><%=  review.stretch.name %></p>
          </div>
        <% end %>
      </div>
    </div>
  <% end %>
<% end %>
```


### ポイントは下記

    # 繰り返し処理を実行してもidを一意に保てるようにreview.idを埋め込む
    <div id="star-rate-<%= review.id %>"></div>

    # 読み取り専用（入力できない）
    readOnly: true,

    # 星の入力値を読み込む
    score: <%= review.rate %>,



## ストレッチ一覧ページにストレッチごとのレビューの平均値を出す

renderでストレッチのレビューの平均値を呼び出す

```
  <!-- レビュー平均 -->
  <script>
    // 繰り返し処理でもidをidを一意にできるようにstretch_idを入れる
    $('#star-rate-<%= stretch.id %>').raty({
      size: 36,
      starOff:  '<%= asset_path('star-off.png') %>',
      starOn : '<%= asset_path('star-on.png') %>',
      starHalf: '<%= asset_path('star-half.png') %>',
      half: true,
      // 読み込みだけで入力できない
      readOnly: true,
      // 星の入力値を読み込み(平均値)
      score: <%= stretch.reviews.average(:rate).to_f.round(1) %>,
    });
  </script>
```

stretchs_index.html.erb
```
  <span>（口コミ <%=stretch.reviews.count %> 件）</span>
  <div id="star-rate-<%= stretch.id %>"></div>
  <%= render 'reviews/read_rate', stretch: stretch %>
```








