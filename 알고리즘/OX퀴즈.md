## OX퀴즈(구현, 문자열)

### 문제

![스크린샷 2023-10-20 오후 1 41 24](https://github.com/Heo-y-y/development-blog/assets/112863029/d7739860-7f10-464b-85f8-19bf9a4b466a)

### 풀이

출력 결과를 누적시키기 위해 `StringBuilder`를 사용했다.

입력 받은 수만큼 반복하는데, 반복하면서 O가 연속으로 나오면 `count` 개수를 누적한 뒤 총 합을 계산한다.

X가 나올 경우 `count`를 0으로 초기화하여 진행한다.

총합이 끝나면, 줄 바꿈을 위해  `\n`을 넣어주고 누적시킨 `sb`를 출력한다.

```java
public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringBuilder sb = new StringBuilder();
        int testCase = Integer.parseInt(br.readLine());

        for (int i = 0; i < testCase; i++) {
            int count = 0;
            int sum = 0;

            for (byte value : br.readLine().getBytes()) {

                if (value == 'O') {
                    count++;
                    sum += count;
                } else {
                    count = 0;
                }
            }
            sb.append(sum).append('\n');
        }
        System.out.println(sb);
    }
}
```

### 결과

![스크린샷 2023-10-20 오후 1 48 26](https://github.com/Heo-y-y/development-blog/assets/112863029/c04cbdee-fba3-4da7-b19c-98fe475acdb8)
