# js 画像プレビュー

## 記述項目
①Gemにjsが使えるようにする

`gem 'jquery-rails`

②application.jsに記述

`//= require jquery`

③assetsファイルにjsファイルを作る（例：image_preview.js)


$(document).ready(function () {

  // 画像が選択された時に発火します

  $(document).on('change', '#stretch_image', function () {

    console.log("ok")

    // .file_filedからデータを取得して変数fileに代入します

    const file = this.files[0];

    // FileReaderオブジェクトを作成します

    const reader = new FileReader();

    // DataURIScheme文字列を取得します

    reader.readAsDataURL(file);

    // 読み込みが完了したら処理が実行されます

    reader.onload = function () {

      // 読み込んだファイルの内容を取得して変数imageに代入します

      const image = this.result;

      // もし既に画像がプレビューされていれば画像データのみを入れ替えます

      $('.prev-content .prev-image').attr({ src: image });

    }

  });

});


④application.html.erbにjsの記述

<%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track': 'reload' %>

<%= javascript_include_tag 'application', 'data-turbolinks-track': 'reload' %>

⑤assets.rbにjsの記述

`Rails.application.config.assets.precompile += %=( admins/image_preview.js)`

⑥プレビューしたいページにコードを追加(一番下)

`<%= javascript_include_tag 'admins/image_preview.js' %>`

⑦indexに画像フォームを作成

`<%= form_for(stretch, url:"/admins/stretchs/#{stretch.id}") do |f| %>

    <div class='col-xs-6'>

      <%= f.label :image, class: 'image_label' do %>

        <div class='prev-content'>

          <%= attachment_image_tag stretch, :image, :fill, 400, 400, fallback: "no_image.png", size:'400x400', class: "prev-image" %>

        </div>

      <% end %>

      <%= f.attachment_field :image %>
      
    </div>
`