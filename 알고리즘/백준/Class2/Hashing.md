## Hashing(구현, 문자열, 해싱)

### 문제

![스크린샷 2023-12-29 오후 12 45 26](https://github.com/Heo-y-y/development-blog/assets/112863029/2024fd4e-e20f-41a6-8d2e-f98d023af7fd)

### 풀이

반복문을 통해 하나하나 31의 제곱을 곱하고 더하고 마지막에 1234567891로 나머지 연산을 해주면 되는데, 이때 `char`형에 -96을 하면 `a`는 문제의 보기처럼 1, `b`는 2의 값을 얻을 수 있다.(아스키 코드 활용)

하지만 이렇게 진행하면 테스트에 실패한다. 이유는 L의 값이 5까지만 들어오는 small 케이스는 overflow가 나지 않지만, `L`의 값이 50까지 들어오는 large 케이스는 overflow가 나기 때문이다.

그렇기 때문에 `long`형으로 두고, 31의 제곱을 할 때마다 1234567891로 나머지 연산을 해야 한다.

**long 사용**

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    static final int M = 1234567891;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int L = Integer.parseInt(br.readLine());
        String alphabet = br.readLine();
        long sum = 0;
        long pow = 1;
        char[] arr = alphabet.toCharArray();

        for (int i = 0; i < L; i++) {
            sum+= ((arr[i] - 96) * pow);
            pow=(pow * 31) % M;
        }
        long result = sum % M;
        System.out.println(result);
    }
}
```

위 코드처럼 `long`으로만 활용도 가능하지만, `BinInteger`를 사용하면 중간 나머지 연산이 필요가 없다. 이유는 무한한 범위이기 때문에 31의 1000만 제곱이 와도 가능하기 때문이다.`

**BinInteger 사용**

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.math.BigInteger;

public class Main {
    static final int M = 1234567891;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int L = Integer.parseInt(br.readLine());
        String alphabet = br.readLine();
        BigInteger result = new BigInteger("0");

        for (int i = 0; i < L; i++) {
            result = result.add(BigInteger.valueOf(alphabet.charAt(i) - 96).multiply(BigInteger.valueOf(31).pow(i)));
        }
        System.out.println(result.remainder(BigInteger.valueOf(M)));
    }
}
```

### 결과

위에가 `BinInteger`이고, 아래가 `long`을 사용한 결과이다.

![스크린샷1 2023-12-29 오후 1 33 34](https://github.com/Heo-y-y/development-blog/assets/112863029/5cf07838-61ff-4b91-9918-dd9ab4b0be20)
