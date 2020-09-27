# jQueryで可視範囲に入ったら要素を表示するアニメーション

`jQuery` `HTML` `CSS`

## jQueryを取り込む
### 方法
①jQueryリファレンス で最新のjQueryをダウンロードしてダウンロードしたファイルを取り入れるリポジトリファイルに入れる

②CDNで取り込む

jQueryリファレンスCDN{https://code.jquery.com/}

HTMLファイルのheadに`script`タグで取り込む

## 実装

`HTML`
```
<body>
  <div id="first">
    <p>↓スクロールしてください↓</p>
  </div>
  <div id="second">
    <img src="sample.jpg" class="fadein">    //スクロールしたい要素にfadeinクラスを入れる
  </div>
</body>
```

アニメーション（動き）はCSSで指定します。

まず「.fadein」が付いた要素の透明度をゼロにして隠し、位置を下に100px下げます。

スクロールして、要素に「.active」が付いたら、透明度を1にし、位置を元の場所に戻します。

`CSS`
```
/* スクロールアニメーション */
.fadeIn_up {
  opacity: 0;     //要素の透明度(0は要素を隠す)
  transform: translate(0, 50%);
  transition: 2s;
}
.fadeIn_up.is-show {
  transform: translate(0, 0);
  opacity: 1;　　　//要素の透明度(0は要素を隠す)
}
```

`js`
```
$(function(){
    $(window).scroll(function (){
        $('.inview').each(function(){
            var position = $(this).offset().top;
            var scroll = $(window).scrollTop();
            var windowHeight = $(window).height();
            if (scroll > position - windowHeight + 200){
              $(this).addClass('is-show');
            }
        });
    });
});
```
２行目

「windowがスクロールされたら以下の処理を実行する」という指定になります。

スクロールメソッドを使っています。
```
$(window).scroll(function() {
    //　ここに処理内容を記述
});
```
3行目

.each()で、「.inview」が付いた要素1つずつに順番に処理を行うことを指定します。
```
$('.inview').each(function(){

});
```
4行目

.offset().topで、ページの最上部から「.fadein」が付いた要素までの距離を取得して、変数positionに代入します。
```
var position = $(this).offset().top;
```
5行目

scrollTop()で、スクロールした量を取得して、変数scrollに代入します。
```
var scroll = $(window).scrollTop();
```
6行目
.height()で、ウィンドウの高さを取得して、変数windowHeightに代入します。
```
var windowHeight = $(window).height();
```
7〜9行目
スクロール量が、「ページ最上部から要素までの距離」-「ウィンドウの高さ」を超えた時、つまりスクロール量が要素の位置を過ぎた瞬間、「.fadein」が付いた要素にactiveクラスが付きます。

そしてCSSに記述したアニメーションが開始します。
```
if (scroll > position - windowHeight){
  $(this).addClass('is-show');
}
```
しかし、これだと要素が画面内に入ってすぐにアニメーションが開始されてしまうので、少し余裕を持たせるために、以下の様に記述することが多いです。
```
if (scroll > position - windowHeight + 200){
  $(this).addClass('is-show');
}
```
数値をいじることで、アニメーション開始位置を変更できます。





