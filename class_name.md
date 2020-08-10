# CSSのクラス名リスト

名前をつけることは難しいですが、とても重要なことです。

CSSには設計思想が必要ですが、実践するにあたり、名前と機能の意味がとおり、名前のつけ方にブレがないようにするべきです。

このドキュメントでは、CSSでよく使われる単語を分類し、意味や機能を短くまとめています。
ただし、見た目やUIの機能をクラス名にするのではなく、デザインの意図やそのコンポーネントの役割をクラス名にすることを推奨します。


## 形容詞

名詞の性質や状態を修飾する品詞。「〜の」「〜な」。

* `main` - 主要な。
* `primary`- 主要な。
* `secondary` - 補助的な・第二の。
* `tertiary` - 第三の。
* `quaternary` - 第四の。
* `common` - 共通の。
* `global` - 全体的な。
* `local` - 局所的な。
* `general` - 一般的な。
* `brand` - ブランドの。
* `site` - サイトの。

## 分類

サイトのページやカテゴリとして使える名詞。

* `abput` - ~について。
* `work` - 仕事・任務。
* `product` - 製品。
* `service` - サービス。
* `news` -お知らせ・近況。
* `event` - 行事・出来事。
* `history` - 歴史
* `archive` - 保存・保管・記録
* `concept` - 構想・概念・考え。
* `recommend` - おすすめ・推奨。
* `table of content` - 目次。
toc= Table of contentsの略語。
* `index` - 牽引・指標。
* `contact` -問い合わせ・連絡。
* `inquiry` - 問い合わせ・質問・調査。
* `access` -交通手段。
* `shop` - 店舗。
* `related` - 関連のある。
* `privacy` - 個人情報の利用・保護の方針。
* `input` - フォームの入力画面。
* `confirm` - フォームの確認画面。
* `finish ` - フォームの完了画面。
* `search-result` - 検索結果画面。
* `cart` - 購入するアイテムを一時的に保存する画面。
* `chekout` - 保存していたアイテムを購入する画面。

## Block

BEMのBlockで使われるような名詞。

*`section` - 区分・区画。
*`content` - 文章の内容。
*`article` - 記事。
*`post` -投稿。
*`top` -頂上・上部
*`home` -トップページ
*`sidebar` - 補足記事。
*`wrapper` - 内包する
wrap = wrapperの略語

container = wrapperの類語の類語。容器・入れ物。wrapperはレイアウト的に、containerは意味的に使うことが多い。
* `group` -集まり
* `area` - 特定の場所・範囲
* `emphasis` - 強調・重視
* `chatch` - 関心をつかむ
* `note`
* `description` -概要
desc = descriptionの略語
* `introdection` - 前置き・導入
intro - introdectionの略語
* `announce` - お知らせ
* `information` - 情報
info -infomationの略語
* `action` - Call To Action. 重要度の高い
* `more` -最も多くの
* `feature` -主要なもの
* `detail` -詳細・細部
* `summary` - 概要・要約

## Element

BEMのElementで使われるような名詞や形容詞など。

* `inner` - 内側の
* `outer` - 外側の
* `body` - 主要部分
* `head` - 上部
* `foot` - 下部
* `heading` - 見出し
* `titile` -題名
* `list` - 一覧・表
* `menu` - 一覧・表
* `item` - 項目
* `unit` - 一つの単位・１セット
* `colum` - 縦列
col = colmnの略語
* `text` - 本文
*` caption` - 画像
* `thumbnail` - 縮小した画像
thumb = thumbnailの略語
* ` avatar` - 人の顔を示す画像
* `feature` - 特徴を補足する画像
* `tel` -電話番号
* `address` - 住所
* `date` - 日付
* `time` - 日時
* `category - 分類・部類
cat ＝ categoryの略語
* `tag` - 識別子
* `next` - 次の
* `previous` - 前の
* `prev = previousの略語
* `mask` -覆い隠す
* `overlay` - 被せる・上書きする

## Modifier

BEMのModifierで使われるような名詞や形容詞など。

* `success` - 成功。
* `alert` - 注意・警戒。
* `attention` - 配慮・気配り。
* `error` - 間違い・失敗。
* `caution` - 用心・警告。
* `warning` - 警告・予告。
* `danger` - 危険・驚異。
* `tiny` - とても小さい。
xs - tinyの類語でExtra small（smallよりさらに小さい）こと。
* `small` - 小さい。
* `medium` - 中間。
* `large` - 大きい。
* `huge` - とても大きい。
xl - hugeの類語でExtra large（特大・超大型）のこと。
* `reverse` - 反転する。
* `push` - 前方に押す。
* `pull` - 自分の方に引く。
* `offset` - 相殺する・埋めあわせる。
* `left`- 左側の。
* `center` - 真ん中。
* `right` - 右側の。
* `top` - 上部。
* `middle` - 真ん中。
* `bottom `- 下部。
* `round` - 角丸。
* `circle` - 円形。


## State

状態を示す動詞や形容詞など。

* `show `- 見せる。
* `hide` - 隠す。
* `open` - 開く。
* `close` - 閉じる。
* `current` - 現在の。
* `active` - 活動中・有効な。
* `disabled` - 無効になっている。


## UI

利用者に情報を提供するためのインターフェイス。

* `link` - アンカーテキスト。
* `label` - 分類する・項目名。
* `tag` - 符号・識別子。
* `badge` - 残数を示す数値。
* `copyright` - 著作権表示。
* `tooltip` - マウスオーバー時に補足情報を表示するインターフェイス。
* `button` - オン・オフの選択に使うインターフェイス。
btn = buttonの略語。

## Image

* `image` - 画像。
img = imageの略語。
* `icon` - 対象の内容や機能などを小さな絵柄で表したもの。
* `loading` - 読み込み中であることを示すインターフェイス。
* `logo` - 社名や製品名などを図案化・装飾化した文字・文字列。
* `map` - 地図。
* `chart` - 理解しやすいような方法で情報を示すリストやグラフのこと。
* `graph` - chartの類語で視覚表現のための手段のこと。
*`hero` - キービジュアルになる大型の画像。
* `banner` - （主に宣伝用の）画像をともなうリンク。
*`carousel` - 画像などのコンテンツを上下左右にスライドさせて切り替えるインターフェイス。
*`slider` - carouselの類語。
*`ticker` - carouselの類語で自動でアイテムを左右に流しながら表示する。ユーザーは基本的にコントロールできない。検索する
*`lightbox` - carouselの類語で、モーダルのjQueryプラグインのこと。主な用途はサムネイル画像をクリックしてモーダルを開き画像を大きく見せること。


## Navigration

* `navigation` - 情報へ誘導するリンク。
nav = navigationの略語。
* `global-navigation` - ほとんどの画面で表示されている主要なナビゲーション。
* `local-navigation` - あるカテゴリ内専用のナビゲーション。グローバルナビゲーションの中や下に配置する場合や、サイドバーに配置する場合がある。
* `mega-menu` - 深い階層のカテゴリやページへのナビゲーション。ドロップダウン（クリックやマウスオーバー）で階層を表示していく。カテゴリやページ数の多いサイトのグローバルナビゲーションに使われることが多い。
* `breadcrumb` - パンくずリスト。トップページから現在ページまでの階層構造を示したリンク。
* `topicpath` - breadcrumbの類語。
* `springboard` - 同じサイズのリンクを2×2や3×3のように配置した同じ重要度を持つ並列なナビゲーション。
* `list-menu` - 縦に並んだリスト型のリンクで、階層構造をもったナビゲーション。
* `dashboard` - グラフやステータスなどを一つの画面にまとめたインターフェイス。
* `pagination` - 昇順にしたページ番号付きのナビゲーション。
* `linear-navigation` - その画面の前後に移動するためのナビゲーション。paginationとの違いはページ指定ができないこと。
* `back-to-top` - ページトップに戻るためのページ内リンク。
* `tab` - 書類などのインデックスタブを模した、画面内のコンテンツ表示を切り替えるインターフェイス。
* `tabbar` - おもに画面下部に固定されたタブで、画面全体を切り替えるインターフェイス。
* `segmented-control` - 機能的にはtabと同じで、見た目がタブではなくボタンである点が違う。
* `off-canvas` - 表示領域外から画面の大半を覆い隠したり（オーバーレイ）、ずらすようにスライドしながら表示するナビゲーション。
* `side-drawer` - off-canvasの類語。drawerは「引き出し」の意味。
* `toggle-menu` - 同一の操作で二つの状態を交互に切り換えるインターフェイスで、操作対象になるボタンを基準にナビゲーションを表示することが多い。
* `sitemap` - サイト内のすべてのページへのリンクをリスト化したもの。
* `sns` - ソーシャルネットワーキングサービス。

## Form

* `form` - 送信フォーム。
* `login` - ユーザー認証をするためのフォーム。
* `signin` - loginの類語。
* `logout` - signout - ユーザー認証を解除すること。
* `registration` - ユーザー登録をするためのフォーム。
signup = registrationの類語。
* `step-navigation` - 複数画面にわたるフォームの順序や、現在地を示したナビゲーション。
* `search-box` - キーワード検索するためのフォームエリア。
* `text-field` - input[type="text"]のような利用者が編集する改行なしのテキストフィールド。
* `textarea` - textareaのような利用者が編集する複数行のテキストフィールド。
* `checkbox` - input[type="checkbox"]のような1つ以上の項目を選択するインターフェイス。
* `radio` - input[type="radio"]のような1つの項目を選択するインターフェイス。検索する
* `select` - selectのような1つの項目を選択するインターフェイス。ラジオボタンと違い、（dropdownのように）項目が最初は隠れているのが特徴。
* `submit` - 送信ボタン。
* `reset` - リセット（取り消し）ボタン。
* `modify` - 修正ボタン。

## Other

* `cards` - トランプのような積み重なったカードのメタファーをもつインターフェイス。
* `dropdown` - 複数の項目を表示して、1つの項目を選択するインターフェイス。
* `pulldown` - dropdownの類語。
* `accordion` - 折りたたまれたコンテンツを選択することで表示させるインターフェイス。
* `scroll-tab` - 表示領域よりも横に長いナビゲーションで、左右にスクロールすることで非表示部分を見ることができるインターフェイス。
* `dialog` - （主に）注意や警告を知らせるために使用されるメッセージで、ユーザーに操作を要求するのが特徴。
* `toast` - dialogの類語で、dialogとの違いは時間が経つと自動的に消えること。
* `popup-window` - dialogの類語で、リンク先を別の小さなウィンドウで表示するインターフェイス。
* `toolbar` - ユーザーが利用できるツールやアクションをまとめたインターフェイス。
* `comment` - 記事に対する反応。
* `table` - テーブル・図表。
* `timeline` - チャットや年表のように時系列に並べたリスト。






















