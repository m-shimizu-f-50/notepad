# ArrayListクラス

// ArrayListを使う
```
import java.util.*;

public class Main {
    public static void main(String[] args) {
        ArrayList<String> team = new ArrayList<String>();

        team.add("勇者");           //要素を追加
        team.add("魔法使い");       //要素を追加
        for (String member : team) {　　　　　　　//ArrayListの要素をループ
            System.out.println(member);
        }
    }
}
```


//ArrayListのサイズを出力
```
System.out.println(team.size());
```

//ArrayListの更新
```
team.set(1, "忍者");
```

//ArrayListの削除
```
team.remove(1);
```