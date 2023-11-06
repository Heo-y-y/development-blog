## x보다 작은 수

### 문제

![스크린샷 2023-11-06 오후 9 52 24](https://github.com/Heo-y-y/development-blog/assets/112863029/f4ab2fa1-d63d-4482-8a76-d13e165958ce)

### 풀이

먼저 `Scanner`를 적용한 코드이다.

`a`만큼 루프를 돌리고, `x`보다 작은 경우 `StringBuilder`에 공백으로 넣어준다.

```java
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        StringBuilder sb = new StringBuilder();
        int a = sc.nextInt();
        int x = sc.nextInt();

        for (int i = 0; i < a; i++) {
           int result = sc.nextInt();
           if (x > result) sb.append(result).append(' ');
        }
        System.out.println(sb);
    }
}
```

아래는 `BufferedReader`를 사용한 코드다.

여기서는 `StringTokenizer`를 사용하여 공백으로 구분하여 숫자들을 토큰화하여 저장시켰다.

그다음은 `Scanner`를 활용한 방법과 같다. 

```java
public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        int N = Integer.parseInt(st.nextToken());
        int X = Integer.parseInt(st.nextToken());

        StringBuilder sb = new StringBuilder();
        st = new StringTokenizer(br.readLine(), " ");

        for (int i = 0; i < N; i++) {
            int value = Integer.parseInt(st.nextToken());

            if (value < X) sb.append(value).append(' ');
        }
        System.out.println(sb);
    }
}
```

### 결과

확실히 `Scanner`를 사용하는 방식보다 `BufferedReader` 를 사용하는 방식이 더 빠르다.

![스크린샷 2023-11-06 오후 9 52 14](https://github.com/Heo-y-y/development-blog/assets/112863029/03eae23a-8882-4b1f-b05e-12e690bedf03)
