### 문제

![스크린샷 2023-08-28 오후 11 12 55](https://github.com/Heo-y-y/development-blog/assets/112863029/9ac9ee08-5b2a-40b2-bc5a-aa4c140b4f5a)

레벨 별로 정복하고자 시작했는데, 초반에는 간단한 문제여서 리뷰 글이 생각보다 짧을 것 같다.

문제는 간단하다.  말 그대로 입력 받은 A값과 B값을 + 해주면 된다.

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int a = sc.nextInt();
        int b = sc.nextInt();

        System.out.println(a + b);
        sc.close();
    }
}
```

### 결과

![스크린샷 2023-08-28 오후 11 22 35](https://github.com/Heo-y-y/development-blog/assets/112863029/2199a51a-f5e8-4f9e-8d09-f354b3d6818d)
