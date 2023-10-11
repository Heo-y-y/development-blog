# Scanner, InputStream, BufferedReader

## System.in & InputStream

일단 Stream에 대해서 먼저 알아보자.

쉽게 생각해서 출발지와 도착지를 연결해주는 **빨대**라고 생각하면 된다.

### 입력장치 —stream—> 프로그램 —stream—> 출력장치

한 곳에서 다른 곳으로의 **데이터 흐름**을 **stream**이라고 한다.

**stream**은 **단방향**이기 때문에 입력과 출력이 동시에 발생할 수 없다. 그래서 용도에 따라 **입력스트림**, **출력스트림**으로 나뉜다.

즉, **자바**에서는 **입력스트림 → InputStream, 출력스트림 → OutputStream** 이다.

`System.in`은 `InputStream` 타입의 필드다.

![스크린샷 2023-06-27 오전 2.13.41.png](https://github.com/Heo-y-y/development-blog/assets/112863029/ff718a13-62de-48e2-a3f9-bf76f43a91c3)

![스크린샷 2023-06-27 오전 2.12.21.png](https://github.com/Heo-y-y/development-blog/assets/112863029/913ece4d-061b-45cc-801f-605454851686)

**특징**
- UTF-8로 입력 받는다.
- `read()` 메소드는 1byte 만 읽기 때문에 나머지 byte는 스트림에 잔존하게 된다.
- 읽어들인 byte 값은 메모리에 UTF-16에 대응되는 문자의 인코딩방식으로 2진수 값이 저장된다.
- 출력시 메모리에 저장되어있던 2진수에 대응되는 문자가 UTF-8로 변환되어 출력된다.

## Scanner(System.in) & InputStreamReader(System.in)

우선 위에 글을 보면 이해 할 수 있는 것은 `Scanner(System.in)`**은 입력스트림인 InputStream을 통해 표준 입력을 받으려고 한다**는 것이다.

`Scanner()`에 왜 InputStream이 들어가는지 Scanner  클래스를 들어가보면 확인할 수 있다.

![스크린샷 2023-06-27 오전 2.01.52.png](https://github.com/Heo-y-y/development-blog/assets/112863029/ef756d5e-681a-4cf5-8740-32049f4918be)

Scanner 클래스에는 **Scanner라는 생성자**가 **Overloading**되어있다.

![스크린샷 2023-06-27 오전 2.03.47.png](https://github.com/Heo-y-y/development-blog/assets/112863029/5b577ae5-0b0c-41ae-8274-95723af6aa18)

위 코드가 `Scanner(System.in)`의 경로이다.

여러 `Scanner()` 생성자들은 `private Scanner(Readable source, Pattern pattern)`으로 넘겨진다.

여기서 **InputStreamReader**가 보일 것이다.

InputStream은 문자를 온전하게 읽어들이지 못한다. 이를 보완하여 나온게 **InputStreamReader**이다.

정리하면, **InputStreamReader**는 InputStream의 바이트 단위로 읽어 들이는 형식을 **문자단위 데이터로 변환해주는 중개자 역할을 해준다.**

![스크린샷 2023-06-27 오전 2.16.46.png](https://github.com/Heo-y-y/development-blog/assets/112863029/f4f6aeed-1448-456c-b676-9f8bf49cc7b9)

54728는 자바 메모리상에서 처리된 UTF-16의 16진수 값이다.

InputStreamReader 자체가 character 데이터로 변환하면서 메모리에 UTF-16 포맷으로 올라가기 때문에 위와 같은 값이 나오는 것이다.

아래를 보면 char 배열로도 데이터를 받을 수 있다.

![스크린샷 2023-06-27 오전 2.22.38.png](https://github.com/Heo-y-y/development-blog/assets/112863029/08940ff4-d7d8-4dc8-a7e0-2bfc6281cb9e)

**InputStreamReader의 가장 큰 특징**

- **바이트 단위 데이터**를 **문자 단위 데이터로 변환**해준다.
- **char 배열로 데이터를 받을 수 있다**.

이제 실질적으로 입력 받는 방법을 알아보자.

우선 흔히 사용하는 입력을 통한 메서드는 다음과 같이 구성되어 있다.

- `next()`
- `nextInt()`
- `nextDouble()`
- `nextFloat()`

`nextInt()`로 살펴보자.

`Scanner.nextInt()` 를 쓰면 `nextInt()` 메서드에서 오버로딩된 아래의 `nextInt(int radix)`메서드로 보내진다.

```java
public int nextInt() {
        return nextInt(defaultRadix);
    }

    public int nextInt(int radix) {
        // Check cached result
        if ((typeCache != null) && (typeCache instanceof Integer)
            && this.radix == radix) {
            int val = ((Integer)typeCache).intValue();
            useTypeCache();
            return val;
        }
        setRadix(radix);
        clearCaches();
        // Search for next int
        try {
            String s = next(integerPattern());
            if (matcher.group(SIMPLE_GROUP_INDEX) == null)
                s = processIntegerToken(s);
            return Integer.parseInt(s, radix);
        } catch (NumberFormatException nfe) {
            position = matcher.start(); // don't skip bad token
            throw new InputMismatchException(nfe.getMessage());
        }
    }
```

중요한것은 try-catch 문의 `String s = next(integerPattern());` 이다.

이 메서드로 가면 다음과 같이 나온다.

![스크린샷 2023-06-27 오전 2.35.45.png](https://github.com/Heo-y-y/development-blog/assets/112863029/c233f9ae-91fb-41d3-8510-251415673b8c)

여기서 중요한 것은 `buildIntegerPatternString()`이다.

![스크린샷 2023-06-27 오전 2.37.38.png](https://github.com/Heo-y-y/development-blog/assets/112863029/47479cdd-acfa-4c0a-b2fd-482176362f6d)

여기서 **Scanner의 단점이자 장점**이 나온다.

우리가 입력받은 문자는 해당 메서드로 보내져서 위에 보이는 **정규식들을 검사하고 문자열을 반환**한다.

우선 **단점**부터 말하자면 **불필요할 정도로 많이 검사**하기 때문에 **속도가 느리다.**

**장점**이라고 얘기하면 많은 **정규식 검사로 인해 타입 변환의 안정성이 뛰어나다.**

**Scanner의 생성자와** `nextInt()` **메서드 흐름을 정리**하면,

- `InputStream(byteStream)`을 통해 입력 받고
- 문자로 받기 위해 **InputStreamReader**를 통해 **char 타입으로 데이터를 처리**하고
- 입력받은 문자는 입력 메서드(`next()`, `nextInt()` 등등)의 타입에 맞게 **정규식을 검사**하고
- 반환된 Pattern 타입을 **String으로 변환**하고
- String은 입력 **메서드의 타입에 맞게 변환**된다.

## **BufferedReader & InputStreamReader(System.in)**

이제 **BufferReader**에 대해서 살펴보자.

- BufferedReader 객체 생성, 선언

```java
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
```

위 코드를 풀어 쓰면

```java
InputStream inputStream = System.in;
InputStraemReader sr = new InputStreamReader(inputStream);
BufferedReader br = new BufferedReader(sr);
```

1. 일단 바이트 스트림인 **InputStream**을 통해 **바이트 단위로 데이터를 입력** 받는다.
2. 입력 데이터를 **char 형태로 처리**하기 위해 **문자스트림** **InputStreamReader**를 **사용**한다.

그러면 **BufferedReader**는 왜 쓰지?

일단 Scanner에서 **InputStreamReader**는 문자열이 아닌 **문자**를 처리한다.

만약 문자열을 입력하고 싶다면 매번 배열을 선언해야 한다.

그래서 쓰는 것이 **Buffer를 통해 입력받은 문자를 쌓아둔 뒤 한 번에** **문자열처럼** 내보내는 것이다.

정리하자면,

- Byte Type = InputStream
- Char Type = InputStreamReader
- Char Type의 직렬화  = BufferedReader

그러면 위의  **BufferedReader 객체 생성, 선언** 코드의 흐름은

**System.in = InputStream -> InputStreamReader -> BufferedReader** 순으로

**byte 타입으로 읽어들이는 in을 char 타입으로 처리한 뒤 String처럼 저장할 수 있게 하는 것**이다.

**BufferedReader 의 특징**

- **버퍼가 있는 스트림**이다.
- **별다른 정규식을 검사하지 않는다.**

이 특징들 덕분에 Scanner 보다 성능이 우수한 것이다.

### 입력장치 —Buffer[c][o][d][e][][]— 프로그램

버퍼는 따로 설정하지 않으면 **디폴트로 8192개의 문자를 저장**할 수 있다.

개행이 입력되거나 버퍼가 꽉 차게 되면 버퍼를 비우면서 프로그램으로 데이터를 내보낸다.

정리하면, 하나하나 문자를 보내는 것이 아니라 **모아둔 다음 한 번에** 보내기 때문에 속도가 더 빠르고, **별다른 정규식을 검사하지 않으니 더 빠른 것**이다.
