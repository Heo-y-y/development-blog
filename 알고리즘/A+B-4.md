## A + B - 4(수학, 구현, 사칙연산)

### 문제

![스크린샷 2023-11-06 오후 10 11 47](https://github.com/Heo-y-y/development-blog/assets/112863029/31ba155e-5e70-47bc-afed-4047153f7705)

### 풀이

`BufferReader`를 사용하여 입력 값을 한줄로 받고, `StringTokenizer`를 사용해 공백 기준으로 구분했다.

그리고 구분한 값들을 더하여 `sb`에 모아서 한번에 출력시킨다.

```java
public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringBuilder sb = new StringBuilder();
        String str;

        while ((str = br.readLine()) != null) {
            StringTokenizer st = new StringTokenizer(str, " ");
            sb.append(Integer.parseInt(st.nextToken()) + Integer.parseInt(st.nextToken()));
            sb.append('\n');
        }
        System.out.println(sb);
    }
}
```

### 결과
![스크린샷 2023-11-06 오후 10 23 49](https://github.com/Heo-y-y/development-blog/assets/112863029/44b6563a-76ea-495f-97da-2f5d0b7244b9)
