## N 찍기

### 문제

![스크린샷 2023-08-31 오후 6 17 07](https://github.com/Heo-y-y/development-blog/assets/112863029/c83eb7ec-c1f5-42bd-8359-c2a42d5b149f)

### 풀이

`BufferedReader`를 사용해 입력받는 `br`을 만들고, 정수 `N`에 그 값을 할당한다.

그리고 `i`에 0부터 돌지 못하게, **1을 할당**하고, `while(1부터 N까지)` 돌린다.

**while문이 끝날 때까지** `i` 값을 출력하고, 한번 돌 때마다 `i` 값을 올려준다. 

```java
public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        br.close();
        int i = 1;

        while (i <= N) {
            System.out.println(i);
            i++;
        }
    }
}
```

### 결과

![스크린샷 2023-08-31 오후 6 17 19](https://github.com/Heo-y-y/development-blog/assets/112863029/be0bd379-fc98-44b2-8351-46005ce869a2)
