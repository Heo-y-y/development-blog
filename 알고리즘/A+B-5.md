## A + B - 5(수학, 구현, 사칙연산)

### 문제

![스크린샷 2023-11-06 오후 10 31 21](https://github.com/Heo-y-y/development-blog/assets/112863029/2d64b56e-9375-4575-b187-5ec4a1a1a2df)

### 풀이

`BufferReader`를 사용하여 입력 값을 한줄로 받고, 각 `a` 와 `b`를 `str`의 0번째, 2번째 값을 -48을 하여 `int`로 저장시키고, 값이 0 0일 경우에 `while`문을 종료 시킨다.

그리고 `StringBuilder`로 결과 값을 모아서 한번에 출력시킨다.

```java
public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringBuilder sb = new StringBuilder();

        while (true) {
            String str = br.readLine();
            int a = str.charAt(0) - 48;
            int b = str.charAt(2) - 48;

            if (a == 0 && b == 0) {
                break;
            }
            sb.append(a + b).append('\n');
        }
        System.out.println(sb);
    }
}
```

### 결과

![스크린샷 2023-11-06 오후 10 35 37](https://github.com/Heo-y-y/development-blog/assets/112863029/867b6364-cb03-487c-8796-997b48b70f7d)
