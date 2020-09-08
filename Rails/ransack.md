# Rubyon Rails で検索機能を作ろう（ransack）
`Ruby` `Rails`

## はじめに
今回は`ransack`というgemを使って検索機能を作っていきます。ransackは少ないコードで複雑な検索機能を簡単に作れるgemでとても便利です。また、ransackではソート機能も簡単に作れるのでそれも含めて作成していきます。


## 基本的な検索結果の実装
今回検索の対象とするのは、ストレッチ(stretchsテーブル)一覧と、そのカテゴリ(categoriesテーブル)一覧とします。
ストレッチ一覧画面(indexビュー)に検索窓を設けます。

### gemをインストール
`Gemfile`
```
gem 'ransack';
```

`bundle install`でインストール

### ビュー
ストレッチのindexビューに検索窓をつける

```
<div class= serch.id>
      <%= search_form_for @q, class:'form-inline' ,url: stretchs_path do |f| %>
        <%= f.search_field :action_muscles_or_name_cont, class: 'form-control input-lg', placeholder: "ストレッチ名や筋肉名を入力" ,data: {"turbolinks" => false}%>
        <%= f.submit "検索", class: "btn btn-success btn-lg" %>
      <% end %>
</div>
```

### コントローラー
```
class StretchsController < ApplicationController
  def index
    # 検索機能(検索ワードをparams[:q]で受け取り＠stretchsに入れる)
    @q = Stretch.all.ransack(params[:q])
    @stretchs = @q.result(distinct: true).page(params[:page])
  end
end
```

検索ワードをparams[:q]で受け取り、検索で該当したデータをそれぞれ@stretchsに入れ、
再びindexビューに返す形です。

以上で簡単な検索機能の実装は完了です。