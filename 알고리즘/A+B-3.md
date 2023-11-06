# **A+B - 3(수학, 구현, 사칙연산)**

### 문제

![스크린샷 2023-11-06 오후 9 58 25](https://github.com/Heo-y-y/development-blog/assets/112863029/4264b918-899f-4fb3-8e7f-594f25dbb426)

### 풀이

먼저 `Scanner`를 적용한 코드이다.

`input`만큼 루프를 돌면서 `a`와 `b`를 입력 받고, `StringBuilder`를 이용하여 한번에 출력시킨다.

```java
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        StringBuilder sb = new StringBuilder();
        int input = sc.nextInt();

        for (int i = 0; i < input; i++) {
            int a = sc.nextInt();
            int b = sc.nextInt();
            sb.append(a + b).append('\n');
        }
        System.out.println(sb);
    }
}
```

아래는 `BufferedReader`를 사용한 코드다.

여기서는 `StringTokenizer`를 사용하여 공백으로 구분하여 숫자들을 토큰화하여 저장시켰다.

그리고 바로 `nextToken`으로 `+` 해주고 한번에 출력시킨다.

```java
public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        StringTokenizer st;
        StringBuilder sb = new StringBuilder();

        for (int i = 0; i < n; i++) {
            st = new StringTokenizer(br.readLine(), " ");
            sb.append(Integer.parseInt(st.nextToken()) + Integer.parseInt(st.nextToken()));
            sb.append('\n');
        }
        System.out.println(sb);
    }
}
```

### 결과

확실히 `Scanner`를 사용하는 방식보다 `BufferedReader` 를 사용하는 방식이 더 빠르다.
![스크린샷 2023-11-06 오후 10 02 25](https://github.com/Heo-y-y/development-blog/assets/112863029/a2f11953-c3fa-4021-aa5a-24a5fdaf9b22)
