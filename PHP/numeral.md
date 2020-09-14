# PHPで半角数字で直して、数字であるかチェックするプログラム


```
<?php
  $age= ２０;  //全角数字

  $age= mb_convert_kana($age, 'n', 'UTF-8');　　//①
  if(is_numeric($age)){　　　　//②
    print($age . '歳');
  }else{
    print('＊年齢が数字ではありません');
  }
  ?>
```


①mb_convert_kana(様々なカナ文字を変換するfunction)で、もし$ageが全角数字であれば半角数字に変換されて$ageに再代入される

②is_numeric(functionの種類)はパラメーターが数字かどうかを判断してくれるfunction

