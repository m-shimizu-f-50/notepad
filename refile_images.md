# refireの基本と複数画像のアップロード

Railsで商品画像や写真の投稿を行う際、とっても便利なファイルアップロードのgem 「 refile 」。
そんなわけで基本的な内容と複数画像についてのご紹介です

## 1:ImageMagick インストール

Macの方　`$ brew install ImageMagick`

## 2:Gem インストール

`gem "refile", github: 'refile/refile', require: "refile/rails"`

`gem "refile-mini_magick", github: 'refile/refile-mini_magick'`

## 3: model 設定・記述

今回は複数画像アップロードしたいので、dbはStretchモデルとStretchImageモデルを１対多にします。
Stretchモデルのカラムには name(string), explanation(text), action_muscles(text)
StretchImageモデルには stretch_id(integer), image_id(string)
注）画像が保存されるimage_idカラムは必ず_idをつける。データ型はstringで！

`/app/models/stretch.rb`

```
class Stretch < ApplicationRecord

  has_many :stretch_images, dependent: :destroy

  accepts_attachments_for :stretch_images, attachment: :image

end
```


## 4: controller 記述

#### 管理者側

`/app/controllers/stretches_controller.rb`



```
class Admins::StretchsController < ApplicationController
  before_action :authenticate_admin!

  def index

      @stretchs = Stretch.page(params[:page]).search(params[:str])

  end

  def new

      @stretch = Stretch.new
      @stretch.stretch_images.build
  end

  def create

      @stretch = Stretch.new(stretch_params)
      if @stretch.save
        redirect_to admins_stretchs_path, notice: 'ストレッチを追加しました'
    else
        flash[:alert] = '入力されていない箇所があります'
        render 'new'
      end
  end

  def show

    @stretch = Stretch.find_by(id:params[:id])
  end

  def edit

    @stretch = Stretch.find_by(id:params[:id])
  end

  def update

    @stretch = Stretch.find(params[:id])
    if @stretch.update(stretch_params)
      redirect_to admins_stretchs_path(@stretch.id)
    else
      render 'edit'
    end
  end

  private

  def stretch_params
  params.require(:stretch).permit(

      :category_id,
      :favorite_id,
      :name,
      :explanation,
      :action_muscles,
      :recommended,
      stretch_images_images: []
    )
    end
end
```


#### 会員側

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

## 5: View 記述

#### 管理者側

`/app/views/admins/stretchs/_form.html.erb`
```
<div class='row'>
  <%= form_for(stretch, url:"/admins/stretchs/#{stretch.id}") do |f| %>

      <%= f.label :image, class: 'image_label' do %>
      <%= f.attachment_field :stretch_images_images, multiple: true ,fallback: "no_image.png", class: "prev-image"%>
      <% end %>
      <img id="preview0" style="width:300px; height:300px;">
      <img id="preview1" style="width:300px; height:300px;">
      <img id="preview2" style="width:300px; height:300px;">
      <img id="preview3" style="width:300px; height:300px;">
      <img id="preview4" style="width:300px; height:300px;">


      <div class='row'>
        <div class='col-xs-4'>
          <%= f.label :name, 'ストレッチ名' %>
        </div>
        <div class='col-xs-6'>
          <%= f.text_field :name %>
        </div>
      </div>
      <div class='row'>
        <div class='col-xs-4'>
          <%= f.label :action_muscles, '作用する筋肉' %>
        </div>
        <div class='col-xs-6'>
          <%= f.text_field :action_muscles %>
        </div>
      </div>
      <div class='row'>
        <div class='col-xs-4'>
          <%= f.label :explanation, 'ストレッチ説明文' %>
        </div>
        <div class='col-xs-5'>
          <%= f.text_field :explanation %>
        </div>
      </div>
      <div class='row'>
        <div class='col-xs-4'>
          <%= f.label :category, 'カテゴリー名' %>
        </div>
        <div class='col-xs-5'>
          <%= f.collection_select :category_id, Category.all, :id, :name %>
        </div>
      </div>
      <div class='row'>
        <%= f.check_box :recommended, {}, Time.zone.now, nil %>
        <%= f.label :recommended, 'オススメに表示する' %>
      </div>

    <%= f.submit submit_text %>
  <% end %>
</div>


<script>

  <input type="file" id="myImage" accept="image/*">
$('#stretch_stretch_images_images').on('change', function (e) {
  if(e.target.files.length > 5){
      alert('一度に投稿できるのは五枚までです。');
      // 選択したファイルをリセット
      document.getElementById("stretch_stretch_images_images").value = "";
      // 画像のプレビューが残っていた場合は、
      // リセットしないと選択できていると勘違いを誘発するため初期化。
      for( var i=0; i < 5; i++) {
        $(`#preview${i}`).attr('src', "");
      }
    }else{
      var reader = new Array(5);
      // 画像選択を二回した時、一回目より数が少なかったりすると画面上に残るので初期化
      for( var i=0; i < 5; i++) {
        $(`#preview${i}`).attr('src', "");
      }
      for(var i=0; i<e.target.files.length; i++) {
        reader[i] = new FileReader();
        reader[i].onload = finisher(i,e);
        reader[i].readAsDataURL(e.target.files[i]);
        // onloadは別関数で準備しないとfor文内では使用できないので、関数を準備。
        function finisher(i,e){
          return function(e){
          $(`#preview${i}`).attr('src', e.target.result);
          console.log(`#preview${i}`);
          }
        }
      }
  }
});
</script>
```
#### 会員側

` app/views/admins/stretchs/show.html.erb `
```
<% if @stretch.stretch_images.present? %>
  <% @stretch.stretch_images.each do |image| %>
   <%= attachment_image_tag image, :image, :fill, 200, 200 %>
  <% end %>
<% else %>
  <%= image_tag 'no_image.jpg', size: '200x200' %>
<% end %>
```
`app/views/favorites/_favorite.html.erb`

```
<% if favorite.stretch.stretch_images.present? %>

  <%= attachment_image_tag favorite.stretch.stretch_images.first, :image, :fill, 500, 500 , size: "300x300"%>
<% end %>
```

`app/views/reviews/_review_index.html.erb `
```
<% if review.stretch.stretch_images.present? %>

  <%= attachment_image_tag review.stretch.stretch_images.first, :image, :fill, 500, 500, size: "300x300" %>

<% end %>
```

`app/views/stretchs/_stretchs_index.html.erb`
```
<% if stretch.stretch_images.present? %>
if文でストレッチの画像があることを確認している（.firstを記述してもエラーが起きない）
  <div class='stretch-image'>
  <%= link_to stretch_path(stretch),data: {"turbolinks" => false} do %>
  <%= attachment_image_tag stretch.stretch_images.first, :image, :fill, 500, 500 %>
  複数画像の最初の一枚だけ表示したいので.firstを記述
<% end %>
```

`app/views/stretchs/show.html.erb`

```
<div class="sliderArea">
  <div class="slider_thumb ">
    <% @stretch.stretch_images.each do |image| %>
      <%= attachment_image_tag image, :image, :fill, 500, 500 , size: "500x500"%>
    <% end %>
  </div>
  <div class="thumb">
    <% @stretch.stretch_images.each do |image| %>
      <%= attachment_image_tag image, :image, :fill, 500, 500, size: "500x500"%>
  <% end %>
</div>

//複数画像機能
<script>
$(document).on('ready', function() {
  $('.slider_thumb').slick({
      arrows:false,
      asNavFor:'.thumb',
  });
  $('.thumb').slick({
      asNavFor:'.slider_thumb',
      focusOnSelect: true,
      slidesToShow:4,
      slidesToScroll:1
  });
});
</script>
```

## 6: css 記述

`app/assets/stylesheets/stretchs.scss `

```
/*複数画像*/
.sliderArea {
    max-width: 100%;
    margin: 0 auto;
    padding: 0 25px;
}
.sliderArea.w300 {
    max-width: 300px;
}
.slick-slide {
    margin: 0 5px;
}
.slick-slide img {
    width: 100%;
    height: auto;
}
.slick-prev, .slick-next {
    z-index: 1;
}
.slick-prev:before, .slick-next:before {
    color: #000 !important;
}
.thumb {
    margin: 20px 0 0;
}

.thumb .slick-slide:hover {
    opacity: .7;
}
```

