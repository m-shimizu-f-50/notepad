# Java 二次元配列
```
// 2次元配列の基本操作

public class Main {
    public static void main(String[] args) {
        String[][] teams = {{"勇者", "戦士", "魔法使い"}, {"盗賊", "忍者", "商人"}, {"スライム", "ドラゴン", "魔王"}};


        teams[0][0] = "魔導士";       //二次元配列の更新


        System.out.println(teams[0][0]);
        System.out.println(teams[0][1]);
        System.out.println(teams[0][2]);
    }
}

```
```
出力結果
魔道士
戦士
魔法使い
```

```
//配列の長さ
public class Main {
    public static void main(String[] args) {
        String[][] teams = {{"勇者", "戦士", "魔法使い"}, {"盗賊", "忍者", "商人"}, {"スライム", "ドラゴン", "魔王"}};
        System.out.println(teams.length);
        System.out.println(teams[0].length);
    }
}
```

```
出力結果
３
３
```

##for文で二次元配列を出力する
```
// 2次元配列をループで処理する
public class Main {
    public static void main(String[] args) {
        String[][] teams = {{"勇者", "戦士", "魔法使い"}, {"盗賊", "忍者", "商人"}, {"スライム", "ドラゴン", "魔王"}};

        for (int i = 0; i < teams.length; i++) {
            for(int j = 0;  j < teams[i].length; j++) {
                System.out.print(teams[i][j] + " ");
            }
            System.out.println("");
            System.out.println("---");
        }
    }
}
```
```
出力結果
勇者 戦士 魔法使い
---
盗賊 忍者 商人
---
スライム ドラゴン 魔王
---
```

##拡張forで二次元配列を出力する

```
// 2次元配列をループで処理する
public class Main {
    public static void main(String[] args) {
        String[][] teams = {{"勇者", "戦士", "魔法使い"}, {"盗賊", "忍者", "商人"}, {"スライム", "ドラゴン", "魔王"}};


        for (String[] team : teams) {
            for (String player : team) {
                System.out.print(player + " ");
            }
            System.out.println("");
            System.out.println("---");
        }
    }
}
```
```
出力結果
勇者 戦士 魔法使い
---
盗賊 忍者 商人
---
スライム ドラゴン 魔王
---
```

#二次元配列をnewで作成

```
// 2次元配列をnewで作成する

public class Main {
    public static void main(String[] args) {
        int[] numberB = new int[10];

        for (int i=0;i<numberB.length; i++){
            numberB[i]=i;
            System.out.println(numberB[i]);
        }
    }
}

```
```
出力結果
0
1
2
3
4
5
6
7
8
9
```
