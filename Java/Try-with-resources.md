# Try-with-resources

자바에서 외부 자원에 접근하는 경우 주의해야 할 점은 외부 자원을 사용한 뒤 제대로 자원을 닫아줘야 한다. 

Java7 버전 이전에는 다 사용하고 난 자원을 반납하기 위해서 try-catch-finally 구문을 사용했었는데, Java7 버전 이후에는 try-with-resources 구문을 사용하여 코드의 가독성을 더 증가 시킬 수 있다.

그러면 먼저 두 구문을 비교하기 위해 이전 버전에서 사용한 try-catch-finally를 사용하는 방법부터 살펴보자.

## T**ry-catch-finally**

사용 후에 반납해주어야 하는 자원들은 Closable 인터페이스를 구현하고 있고, **사용 후에** `close` **메서드를 호출**해야 한다. Java7 이전에는 close를 호출하기 위해 try-catch-finally 를 사용해서 `null` 검사와 함게 직접 호출해야 했는데, 대표적으로 파일의 내용을 읽는 경우를 아래와 같이 구현할 수 있다.

**예제 코드**

```java
public class TryCatchFinally {
    public static void main(String[] args) throws IOException {
        FileInputStream fi = null;
        try {
            fi = new FileInputStream("exFile.tet");
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } finally {
            if (fi != null) fi.close();
        }
    }
}
```

### 단점

- 자원 반납에 의해 코드가 복잡해진다.
- 작업이 번거롭다.
- 실수로 자원을 반납하지 못하는 겨우가 발생한다.
- 에러로 자원을 반납하지 못하는 경우가 발생한다.
- 에러 스택 트레이스가 누락되어 디버깅이 어렵다.

이러한 문제를 해결하기 위해 try-with-resources 가 Java7부터 추가됐다.

## Try-with-resources

위 문제를 해결하고자 Java7부터 **자원을 자동으로 반납**해주는 try-with-resources 문법이 추가됐다.

자바는 **AutoCloseable 인터페이스를 구현**하고 있는 자원에 대해 try-with-resources 를 적용 가능하도록 하였고, 이를 사용함으로써 코드가 유연해지고, 누락되는 에러없이 모든 에러를 잡을 수 있게 됐다.

**예제 코드**

```java
public class TryWithResources {
    public static void main(String[] args) {
        try(FileOutputStream out = new FileOutputStream("exFile.txt")) {
            // ... 이후 입출력 로직 처리 ...
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### ****Closeable과 AutoCloseable의 관계****

여기서 알아야할 것은 기존의 Cloesable에 부모 인터페이스로 AutoCloesable을 추가했다는 점인데, 정리하면 Cloesable에 부모 인터페이스인 AutoCloesable을 새로 추가한 것이다.

![스크린샷 2023-12-07 오후 7.12.49.png](https://github.com/Heo-y-y/development-blog/assets/112863029/674a8029-c9cd-41a4-84f2-6182d8e88250)

보통은 먼저 만든 인터페이스를 자식 클래스나 인터페이스에 사용하는 것이 일반적이다. 그러나 자바 개발자들은 먼저 만들어진 Closeable 인터페이스에 부모 인터페이스인 AutoCloseable을 추가함으로써 하위 호환성을 100% 달성함과 동시에 변경 작업에 대한 수고를 덜 수 있었다고 한다.

만약 Closeable을 부모로 만들면, 기존에 만들어 둔 클래스들이 모두 Closeable이 아닌 AutoCloseable를 구현하도록 수정이 필요했을 것이다.

하지만 이러한 구조 덕분에 기존에 구현된 자원 클래스를 모두 try-with-resources가 사용 가능해졌다.

```java
public interface Closeable extends AutoCloseable {

    public void close() throws IOException;

}

public interface AutoCloseable {

    void close() throws Exception;

}
```

## Try-with-resources를 사용해야 하는 이유

### **에러 스택 트레이스가 누락되는 경우**

try-catch-finally를 사용하면 에러가 발생해도 에러 스택 트레이스가 누락되는 경우가 발생할 수 있다. 이는 디버깅 및 원인 파악을 매우 어렵게 만든다. 

예시로 아래 코드가 있다고 가정하자.

```java
public class Resource implements AutoCloseable {

    @Override
    public void close() throws RuntimeException{
        System.out.println("close");
        throw new IllegalStateException();
    }

    public void hello() {
        System.out.println("hello");
        throw new IllegalStateException();
    }
}
```

위 예시 코드를 try-catch-finally 과 try-with-resources 로 반환하여 비교해보자.

- **try-catch-finally**

```java
public void TryCatchFinally() {
    Resource resource = null;

    try {
        resource = new Resource();
        resource.hello();
    } finally {
        if (resource != null) {
            resource.close();
        }
    }
}
```

해당 코드는 자원을 사용하고, `finally` 구문에서 `null`이 아닌 경우를 검사하여 반납한다.

문제는 위에서 발생한 에러 트레이스가 `hello` 호출 시에서 찍힌 부분은 누락되고, `close` 호출 시에 찍힌 부분만 남는다는 것이다.

```java
hello
close

java.lang.IllegalStateException
	at com.example.testing.composition.MangKyuResource.close(MangKyuResource.java:8)
	at com.example.testing.composition.SteakTest.temp2(SteakTest.java:49)
```

위 로그를 보면 알 수 있지만, `hello`에서 발생한 에러 트레이스가 정상적으로 찍히지 않았다. 만약 이러한 문제가 실제 서비스 운영 중에 발생한다면 원일을 파악하는데 상당히 많은 시간을 허비할 것이다.

하지만 try-with-resources를 사용하면 이 문제를 해결할 수 있다.

- **try-with-resources**

```java
public void TryWithResources() {
    try (Resource resource = new Resource()) {
        resource.hello();
    }
}
```

```java
hello
close

java.lang.IllegalStateException
	at com.example.testing.composition.MangKyuResource.hello(MangKyuResource.java:13)
	at com.example.testing.composition.SteakTest.temp4(SteakTest.java:67)
    ... 생략
    
	Suppressed: java.lang.IllegalStateException
		at com.example.testing.composition.MangKyuResource.close(MangKyuResource.java:8)
		at com.example.testing.composition.SteakTest.temp4(SteakTest.java:68)
```

에러가 발생하는 `hello`와 `close` 모두 에러 트레이스가 남는 것을 확인할 수 있다. 이러한 이유 때문에 try-with-resources를 사용하는 것이 좋다.

### 에러로 자원을 반납하지 못하는 경우

try-with-resources를 사용하는 이유에서 편리함과 실수 방지도 있지만, **누락없이 모든 자원을 반납**하기 위함도 있다. try-catch-finally는 자원 반납이 누락되는 경우가 발생할 수 있지만 **try-with-resources**는 이 문제를 해결해준다.

```java
public class Resource implements AutoCloseable {

    @Override
    public void close() throws RuntimeException{
        System.out.println("close");
        throw new IllegalStateException();
    }

    public void hello() {
        System.out.println("hello");
    }
}
```

앞서 예시 코드와 다르게 현재는 `hello` 메서드에서는 예외가 발생하지 않는다.

- **try-catch-finally**

```java
public void TryCatchFinally() {
    Resource resource1 = null;
    Resource resource2 = null;

    try {
        resource1 = new Resource();
        resource2 = new Resource();
        resource1.hello();
        resource2.hello();
    } finally {
        if (resource1 != null) {
            resource1.close();
        }

        if (resource2 != null) {
            resource2.close();
        }
    }
}
```

`resource1`을 `close`하는 경우 `IllegalStateException` 예외가 발생할텐데, 그래도 `resource2`가 반납되기를 기대할 것이다. 하지만 코드를 실행하면 아래 결과가 나온다.

```java
hello
hello
close
```

위 결과를 보면 `resource2` 가 정상적으로 반납되지 않았는데, 이러한 이유는 `resource1` 을 `close`하는 `finally` 구문 안에 `IllegalStateException` 예외를 catch하는 코드가 없기 때문이다.

이 문제를 해결하려면 `resource1` 과 `resource2` 를 반납하는 부분을 각각 try-catch-finally로 묶어야 하는데, 이는 코드를 더 복잡하게 만든다.

하지만 try-with-resources를 사용하면 별도의 처리 없이 모두 자원을 반납할 수 있다.

- **try-with-resources**

```java
public void TryWithResources() {
    try (Resource resource1 = new Resource(); Resource resource2 = new Resource()) {
        resource1.hello();
        resource2.hello();
    }
}
```

위 코드를 살펴보면 확실히 try-catch-finally를 사용한 방식보다 코드가 간결해졌다.

```java
hello
hello
close
close
```

`resource2`도 누락없이 정상적으로 자원이 반납되는 것을 확인할 수 있다. 이유를 설명하자면, **자바 파일이 Class 파일로 컴파일 될 때 try-with-resources에서 누락없이 모든 경우를 try-catch-finally로 반환**해주기 때문이다. 즉, 위에서 직접 구현해주아야 했던 부분을 컴파일러가 처리해주는 것이다.

try-with-resources를 사용하는 이유를 정리하자면,

- 코드를 간결하게 할 수 있다.
- 번거러운 자원 반납 작업을 하지 않아도 된다.
- 실수로 자원을 반납하지 못하는 경우를 방지할 수 있다.
- 에러로 자원을 반납하지 못하는 경우를 방지할 수 있다.
- 모든 에러에 대한 스택 트레이스를 남길 수 있다.

**참고 자료**

- <https://hianna.tistory.com/546>
- <https://dev-coco.tistory.com/20>
- <https://mangkyu.tistory.com/217>
