// 標準入力
```
import java.util.*;
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String data = sc.next();
        System.out.println("hello " + data);
    }
}
```
// 数値の標準入力
```
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int line = sc.nextInt();
        System.out.println(line);
    }
}
```
// 標準入力とループ処理
```
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String data = sc.next();
        System.out.println("hello " + data);
    }
}
```
// 標準入力とループ処理
```
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int count = sc.nextInt();
        //System.out.println("データ個数 " + count);

        String data;
        for (int i = 0; i < count; i++) {
            data = sc.next();
            System.out.println(data + "は、スライムを攻撃した");
        }
    }
}
```

//標準入力と配列(標準入力で１行データを読み込む)
```
import java.util.*;
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String data = sc.nextLine();
        System.out.println(data);
    }
}
```

//文字列をカンマ区切りで分割して配列に格納する（splitメソッド）
```
String[] array = data.split(",");
```
```
import java.util.*;
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String data = sc.nextLine();
        System.out.println(data);

        String[] array = data.split(",");
        System.out.println(array[0]);
    }
}
```

//標準入力から複数行のデータを読み込む(hasNextLineメソッド)
```
import java.util.*;
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        while (sc.hasNextLine()) {
            String data = sc.nextLine();
            System.out.println(data);
        }
    }
}
```

//標準入力から読み込んだ複数行のデータを配列に格納する
```
import java.util.*;
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        ArrayList<String> array = new ArrayList<String>();

        while (sc.hasNextLine()) {
            String data = sc.nextLine();
            array.add(data);
        }

        for(String str : array) {
            System.out.println(str);
        }
    }
}
```