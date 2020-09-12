# headerをスクロールしても固定する

例えばWebページを作成したときに下にスクロールしてもheaderがついてくるようにしたい場合

`style.css`
```
header{
  position: fixed;  //headerの位置を固定
  top: 0;           //様々な不具合を防ぐために二つを記述
  z-index: 100;     //スクロールして重なった際にも一番上になるように設定
}
```