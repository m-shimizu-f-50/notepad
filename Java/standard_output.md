// 標準入力
import java.util.*;
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String data = sc.next();
        System.out.println("hello " + data);
    }
}

// 数値の標準入力
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int line = sc.nextInt();
        System.out.println(line);
    }
}

// 標準入力とループ処理
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String data = sc.next();
        System.out.println("hello " + data);
    }
}

// 標準入力とループ処理
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