# Java 17

부트캠프에서 Java 11을 써서 보통 작업을 11 버전으로 작업을 하다가, 최근에 스프링 부트가 이제 3.0 버전으로 업그레이드 된다고 하여 3.0 버전부터는 Java 17이상을 지원한다고 한다.

그리고 진행하고 있는 인스타 클론 프로젝트에도 적용을 고려하여 알아보는 이유기도 하다.

그래서 Java 17에 대해서 살펴보려 한다.

우선 기존 Java 11에서 크게 달라진 점은 없고, 기존의 기능들이 개선되었다.

## 텍스트 블록

**기존 코드**

```java
private static void oldStyle() {
    String text = "{\n" +
                  "  \"name\": \"Heo\",\n" +
                  "  \"age\": 30,\n" +
                  "  \"address\": \"Doe Street, 23, Java Town\"\n" +
                  "}";
    System.out.println(text);
}
```

기존의 JSON 문자열을 직접 생성할 때 이스케이프 처리로 가독성이 떨어지던 문제를 텍스트 블록을 통해 개선할 수 있다.

텍스트 블록은 3개의 큰 따옴표로 정의되며, 아래와 같이 작성할 수 있다.

**변경 코드**

```java
private static void jsonBlock() {
    String text = """
            {
              "name": "Heo",
              "age": 30,
              "address": "Doe Street, 23, Java Town"
            }
            """;
    System.out.println(text);
}
```

추가로 텍스트 블록의 끝에 있는 세개의 큰따옴표의 위치가 텍스트 블록의 시작 위치를 나타낸다.

```java
private static void jsonMovedBracketsBlock() {
    String text = """
              {
                "name": "John Doe",
                "age": 45,
                "address": "Doe Street, 23, Java Town"
              }
            """; // 이 라인 space 공백 2개 제거하여 왼쪽으로 이동
    System.out.println(text);
}
```

이렇게 마지막 큰 따옴표 세개를 왼쪽으로 이동하면 각 줄 앞에 2개의 공백을 출력한다.

```java
{ // 공백 2개
    "name": "John Doe",
    "age": 45,
    "address": "Doe Street, 23, Java Town"
  }
```

마지막으로 아래와 같이 큰 따옴표가 더 오른쪽으로 이동하게 되면, 텍스트 블록의 시작이 첫 번째 문자에 의해 결정된다.

```java
private static void jsonMovedEndQuoteBlock() {
    String text = """
              {
                "name": "John Doe",
                "age": 45,
                "address": "Doe Street, 23, Java Town"
              }
                   """;
    System.out.println(text);
}
```

```java
{ // 공백 없음
  "name": "John Doe",
  "age": 45,
  "address": "Doe Street, 23, Java Town"
}
```

## Switch 표현식

주어진 열거형 값에 따라 작업을 수행해야 하는 경우 스위치 표현식을 사용하여 스위치에서 값을 반환하고 할당 등에서 값을 사용할 수 있다.

**기존 코드**

```java
private static void oldStyleWithBreak(Animal animal) {
    switch (animal) {
        case LION, TIGER: 
        // multivalue labels, and are indeed not supported prior to Java 14
            System.out.println("Mammalia");
            break;
        case EAGLE, SEAGULL:
            System.out.println("Bird");
            break;
        default:
            System.out.println("Other animal");
    }
}
```

기존의 `break` 문은 누락, 가독성 문제가 있었는데 이를 아래와 같이 변경됐다.

**변경 코드**

```java
private static void withSwitchExpression(Animal animal) {
    switch (animal) {
        case LION, TIGER -> System.out.println("Mammalia");
        case EAGLE, SEAGULL -> System.out.println("Bird");
        default -> System.out.println("Other animal");
    }
}
```

반환값을 사용해야 할 경우 아래와 같이 사용할 수 있다.

```java
private static void withReturnValue(Animal animal) {
    String text = switch (animal) {
        case LION, TIGER -> "Mammalia";
        case EAGLE, SEAGULL -> "Bird";
        default -> "Other animal";
    };
    System.out.println(text);
}
```

하나 이상의 동작을 수행해야 할 때는 아래와 같이 사용할 수 있다.

```java
private static void withReturnValues(Animal animal) {
    String text = switch (animal) {
        case LION, TIGER -> {
            System.out.println("the given animal was: " + animal);
            yield "Mammalia"; // 예약어 사용
        }
        case EAGLE, SEAGULL -> "Bird";
        default -> "Other animal";
    };
    System.out.println(text);
}
```

### Switch문 문법

- `→` 화살표를 사용해 실행문을 나타낸다.
- 조건들을 `,` 를 이용해 나열할 수 있다.
- `break`을 사용하지 않는다.

## Records

Lombok의 여러 기능들을 Record를 사용하여 구현할 수 있다.

코드를 간결하게 만들기 위해 사용하던 Lombok은 종속성이 생기는 경우가 많은데 이를 개선했다. 그리고 DTO 클래스를 만들 때 여러 보일러 플레이트 코드를 추가해야 했는데 이도 개선할 수 있다.

**기존 코드**

```java
public class Dto {
    
    private final int data;

    public Dto(int data) {
        this.data = data;
    }

    public int getData() {
        return data;
    }
}
```

**변경 코드**

```java
public record Dto(
    int data
) {
}
```

Java 17에서는 Record를 활용하여 불필요한 코드를 제거할 수 있고, 클래스 자체로 명확한 의도를 파악할 수 있다.

### **Record 특징**

- 멤버 변수는 `private final`로 선언된다.
- 필드별 `getter`가 자동으로 생성된다.
- `equals, hashcode, toString`이 자동으로 생성된다.
- 기본생성자는 제공하지 않으므로 필요한 경우 직접 생성해야 한다.
- `final` 클래스이므로 다른 클래스를 상속하거나 상속시킬 수 없다.
- `private final fields` 이외의 인스턴스 필드를 선언할 수 없다.

## Sealed Class

Seald 클래스는 상속하거나, 구현 할 클래스를 지정해두고, 해당 클래스들만 상속, 구현이 가능하도록 제한하는 기능이다. 이에 따라 개발자는 Seald 클래스 코드만 봐도 어떤 클래스가 구현, 상속했는지 쉽게 파악할 수 있다.

또한 의도치 않은 클래스가 상속 받았을 경우, 컴파일 시점에 에럴르 체크할 수 있어 의도치 않은 실수를 방지할 수 있다.

```java
// Parent.java
public sealed class Parent permits Son, Daughter {
    ...
}

// Son.java
// permits 로 선언된 class 만 Parent class 를 상속할 수 있다.
public sealed class Son extends Parent {}
```

### Sealed Class 특징

- `super-class`에 `sealed` 키워드를 사용한다.
- `permits` 키워드 뒤에 해당 클래스를 상속받을 `sub-class`를 선언한다.
- `sealed`된 클래스를 활용하기 위해서는 같은 모듈 혹은 같은 패키지 안에 존재해야 한다.

## Stream.toList()

이제 `Stream`에서 `List`로 변환할 때 기존의 긴 `Collectors.toList()`를 호출하지 않아도 된다.

**기존 코드**

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

List<Integer> result = numbers.stream()
        .filter(number -> number > 1)
        .collect(Collectors.toList());
```

**변경 코드**

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

List<Integer> result = numbers.stream()
        .filter(number -> number > 1)
        .toList(); // 변경
System.out.println(result);
```

그러면 두 메서드는 완전 동일할까?

`JavaDocs`를 살펴보면, 두 메서드의 `return type`이 다르다.

- `Collectors.toList()`: ArrayList를 리턴하고 있으며, 불변 타입이 아니다.
    
    ![Untitled1](https://github.com/Heo-y-y/development-blog/assets/112863029/095b95e9-5f9e-4882-afcc-64270b6a41c0)
    
- `toList()`: 불변 List를 리턴한다.
    
    ![Untitled2](https://github.com/Heo-y-y/development-blog/assets/112863029/6187071e-9a39-4be9-a493-5057f06bf53f)
    

그렇다면 `toList()`는 `Collectors.toUnmodifiableList()` 과 동일할까?

`JavaDocs`에 따르면 `Collectors.toUnmodifiableList()`는 내부에서 `null` 체크를 하고 있지만, `toList()`는 `null` 체크를 하지 않아 `null` 값이 들어갈 수 있다.

- `Collectors.toUnmodifiableList()`
    
    ![Untitled3](https://github.com/Heo-y-y/development-blog/assets/112863029/72b06e07-6d82-4e96-843e-e1225b91025f)
    

## Instanceof 변수 생성

`instanceof` 체크 시에 변수를 생성할 수 있어서, 이제 새 변수를 생성하고 객체를 캐스팅하는데 추가 줄이 필요하지 않다.

**기존 코드**

```java
private static void oldStyle() {
    Object o = new GrapeClass(Color.BLUE, 2);
    if (o instanceof GrapeClass) {
        GrapeClass grape = (GrapeClass) o;
        System.out.println("This grape has " + grape.getNbrOfPits() + " pits.");
    }
}
```

**변경 코드**

```java
private static void patternMatching() {
     Object o = new GrapeClass(Color.BLUE, 2);
     if (o instanceof GrapeClass grape) { // 변수 생성
         System.out.println("This grape has " + grape.getNbrOfPits() + " pits.");
     }
}
```

## 컴팩트한 숫자 포맷 지원

**SHORT 포맷 스타일**

```java
NumberFormat fmt = NumberFormat.getCompactNumberInstance(Locale.ENGLISH, NumberFormat.Style.SHORT);
System.out.println(fmt.format(1000));
System.out.println(fmt.format(100000));
System.out.println(fmt.format(1000000));

[Output]
1K
100K
1M
```

**LONG 포맷 스타일**

```java
fmt = NumberFormat.getCompactNumberInstance(Locale.ENGLISH, NumberFormat.Style.LONG);
System.out.println(fmt.format(1000));
System.out.println(fmt.format(100000));
System.out.println(fmt.format(1000000));

[Output]
1 thousand
100 thousand
1 million
```

## 기간 지원 추가

새로운 “B” 패턴이 추가되었다.

```java
DateTimeFormatter dtf = DateTimeFormatter.ofPattern("B");
System.out.println(dtf.format(LocalTime.of(8, 0)));
System.out.println(dtf.format(LocalTime.of(13, 0)));
System.out.println(dtf.format(LocalTime.of(20, 0)));
System.out.println(dtf.format(LocalTime.of(23, 0)));
System.out.println(dtf.format(LocalTime.of(0, 0)));

[Output]
in the morning
in the afternoon
in the evening
at night
midnight
```

---

**참고 자료**

- <https://velog.io/@jinny-l/Java-17>
- <https://e-una.tistory.com/46>
