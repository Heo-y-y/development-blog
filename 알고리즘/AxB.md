## A x B(수학, 구현, 사칙연산)

### 문제

![스크린샷 2023-11-06 오후 10 46 25](https://github.com/Heo-y-y/development-blog/assets/112863029/f030f7cd-8169-4e82-b643-a378a6be6e12)

### 풀이

```java
public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String str = br.readLine();
        StringTokenizer st = new StringTokenizer(str, " ");

        int a = Integer.parseInt(st.nextToken());
        int b = Integer.parseInt(st.nextToken());

        System.out.println(a * b);
    }
}
```

### 결과

![스크린샷 2023-11-06 오후 10 46 16](https://github.com/Heo-y-y/development-blog/assets/112863029/adba13b0-b5f1-49e6-8bd2-943e65932d0e)
