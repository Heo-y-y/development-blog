## ACM 호텔

### 문제

![스크린샷 2023-11-03 오후 9 55 31](https://github.com/codingTest-study-group/coding-study/assets/112863029/f9d027ab-5d40-4ce8-a791-fa172f69b786)

### 풀이

먼저 `Scanner`를 적용한 코드이다.

`N % H == 0`이면 마지막 층으로 간주하여, 층에 100을 곱하여 몇번 째 방인지 계산하여 더한다.

반대로 마지막 층이 아니면, 방 번호를 1부터 시작하는 것이 아니라 0부터 시작하여 + 1을 하여 반환시킨다.

```java
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        StringBuilder sb = new StringBuilder();
        int test = sc.nextInt();

        for (int i = 0; i < test; i++) {
            int H = sc.nextInt();
            sc.nextInt();
            int N = sc.nextInt();

            if (N % H == 0) {
                sb.append((H * 100) + (N / H)).append('\n');
            } else {
                sb.append(((N % H) * 100) + (N / H) + 1).append('\n');
            }
        }
        System.out.println(sb);
    }
}
```

아래는 `BufferedReader`를 사용한 코드다.

```java
public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringBuilder sb = new StringBuilder();
        int input = Integer.parseInt(br.readLine());

        for (int i = 0; i < input; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine(), " ");
            int H = Integer.parseInt(st.nextToken());
            st.nextToken();
            int N = Integer.parseInt(st.nextToken());

            if (N % H == 0) {
                sb.append((H * 100) + (N / H)).append('\n');
            } else {
                sb.append(((N % H) * 100) + ((N / H) + 1)).append('\n');
            }
        }
        System.out.println(sb);
    }
}
```

### 결과

확실히 `Scanner`를 사용하는 방식보다 `BufferedReader` 를 사용하는 방식이 더 빠르다.

![스크린샷 2023-11-03 오후 10 43 09](https://github.com/codingTest-study-group/coding-study/assets/112863029/c4e7189e-97df-495b-825c-48d3c55ae24c)
