
## Java 8에 추가된 것들
- Lambda expression(람다 표현식)
- Functional interface(함수형 인터페이스)
- Default method(디폴트 메서드)
- Stream(스트림)
- Optional(옵셔널)
- 새롭게 추가된 날짜 API
- Completable future(컴플리터블 퓨처)
- JVM의 변화

## Lambda expression(람다 표현식)


**람다식**은 기능을 **메서드 인수로 처리**하거나 **코드 블록을 메서드 매개 변수로 전달**할 수 있는 **익명 함수**이다.
Java에서 보다 기능적인 프로그래밍 스타일을 촉진하고 **코드를 보다 간결하고 표현력** 있게 만든다.

코드로 살펴보자.

```java
// 익명 클래스로 Runnalbe을 구현
Thread thread = new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println("Start to new thread!");
    }
});

thread.start();

// 람다 표현식으로 단순하게 표현
Thread thread = new Thread(() -> System.out.println("Start to new thread!"));
        
thread.start();
```

### 람다 표현식의 구성

람다 표현식은 람다 파라미터`()`와 화살표`->`, 람다 바디`함수`로 구성되어 있다.
위 코드에서는 `()` 가 람다 파라미터, `->` 가 화살표, `System.out.println("Start to new thread!")` 가 람다 바디에 해당한다.

### 람다 표현식의 동작하는 방법

어떤 값이 들어가면(람다 파라미터)결과물이 나온다.**(람다 바디)**

예시로 살펴보자.

- 하나의 파라미터를 갖고 리턴 타입이 없는 람다 표현식
    
    ```java
    (String str) -> System.out.println("parameter is " + str);
    ```
    
    람다 **파라미터의 타입은 생략이 가능**하고, **컴파일러가 문맥에 맞게 해석하여 알맞은 객체를 선택**한다.
    만약 **파라미터가 한 개인 경우**에는 **괄호도 생략이 가능**하다.
    
    아래 코드를 보자.
    
    ```java
    // 타입을 생략한 코드
    (str) -> System.out.println("parameter is " + str);
    
    // 람다 파라미터 괄호를 생략한 코드
    str -> System.out.println("parameter is " + str);
    ```
    
- 하나의 파라미터를 갖고 리턴 타입이 있는 람다 표현식
    
    ```java
    // 람다 바디를 하나의 라인으로 표현
    (i) -> i + 10;
    
    // 람다 바디를 여러 라인으로 표현
    (i) -> {
        int result = i + 10;
        return result;
    };
    ```
    
    람다 바디가 짧아서 **한 줄**로 끝난다면 **중괄호와 return 키워드를 생략**할 수 있다.
    **두 줄 이상**이면 명시적으로 **중괄호와 return 키워드를 선언**해줘야 한다.
    

- 파라미터가 없고 리턴 타입이 있는 람다 표현식
    
    ```java
    ()-> Integer.MAX_VALUE;
    ```
    
    **파라미터가 없는 경우**에는 **빈 괄호를 명시적으로 표기**해야한다.
    

### 장단점

- **장점**
    - 간결성: 익명 함수이기 때문에 메서드 이름이나 클래스 정의 없이 간결하게 코드 작성 가능
    - 가독성: 코드를 간결하고 명확하게 만듬
    - 함수형 프로그래밍 지원: 함수형 프로그래밍으로 상태 변경을 피하고 불변성을 강조
    - 함수형 인터페이스 사용: 코드를 메서드 인수로 전달하고 동작을 추상화할 수 있음
    - 병렬 처리: 병렬 처리를 활용하여 프로그램의 성능을 향상시킬 수 있음
- **단점**
    - 호출식이 까다로움: 처음에는 어렵지만 익숙해지면 코드작성과 유지보수에 큰 이점
    - 디버깅: 익명 클래스보다 디버깅이 어렵고 복잡한 경우에는 코드 추적이 더 어려워짐
    - 가독성 저하: 표현식이 너무 복잡하거나 길어지면 가독성이 저하
    - 상태 관리의 어려움: 가변 상태를 다루는 데 어려움이 있음
    - 가독성 저하: 과도하게 사용하거나 중첩하면 가독성이 떨어짐

## Functional interface(함수형 인터페이스)


단 하나의 추상 메서드를 갖는 인터페이스를 **함수형 인터페이스**라고 한다. 
람다 식 및 메서드 참조의 대상 유형 역학을 한다. 
`@FunctionalInterface` 주석은 종종 인터페이스가 가능하도록 의도된 것인지 확인하는 데 사용된다.

### 함수형 인터페이스 구현

아래 코드를 보자.

```java
@FunctionalInterface
public interface Motorcycle {
    String drive(int speed);
}
```

위에 `@FunctionalInterface` 주석이 없어도 Motorcycle은 함수형 인터페이스이다. 해당 인터페이스가 규칙을 잘 지키고 있는지 **검증하는 역할**을 할 뿐이다.

인터페이스를 사용하는 코드이다.

```java
Motorcycle motorcycle = new Motorcycle() {
    @Override
    public String drive(int speed) {
        return speed == 100 ? "" : "오토바이가 시속 " + speed + " km의 속도로 달린다.";
    }
};

System.out.println(motorcycle.speed(100));
```

여기서 람다 표현식을 사용하면 함수형 인터페이스를 더 간결하게 표현할 수 있다.

아래는 람다 표현식을 사용한 코드다.

```java
Motorcycle motorcycle = (i) -> i == 100 ? "" : "오토바이가 시속 " + i + " km의 속도로 달린다.";
System.out.println(motorcycle.speed(100));
```

확실히 코드가 간결해졌다. 
**Java 8**에서 **람다 표현식은 함수형 인터페이스를 위해 등장했다 해도 과언이 아니다.** 
파라미터 X가 여러 함수형 인터페이스를 거쳐 최종적인 값 Y의 형태로 변경할 수 있기 때문이다.

![스크린샷 2023-08-12 오전 12 15 03](https://github.com/mo2-Study-Group/StudyGroup/assets/112863029/3593374b-e9a8-4903-921e-5bb9c1ef788a)

### 기본으로 제공되는 함수형 인터페이스

- **Fucntions**
    - 특정 오브젝트를 받아서, 특정 오브젝트를 리턴하는 메소드 시그니처
- **Suppliers**
    - 매개변수를 받지 않고, 특정 타입의 결과를 리턴
- **Consumers**
    - 매개변수를 받고 리턴은 하지 않음
    - 전달 받은 매개변수를 이용해 처리하는 작업을 수행
- **Predicates**
    - 특정 타입의 매개변수를 전달 받아 `boolean`값 리턴
- **Operators**
    - 하나의 매개변수를 받고, 동일한 타입을 리턴하는 함수형 인터페이스
    - `UnaryOperator`, `BinaryOperator`를 이용해 매개변수 개수 구분

## Default method(디폴트 메서드)


Java 8부터는 인터페이스에 디폴트 메서드를 도입하여 이전 버전과의 호환성을 유지하면서 기존 인터페이스에 새 메서드를 추가할 수 있도록 한다. 

```java
public interface SampleInterface {
    // abstract method
    String returnHello(String msg);

    // default method
    default void hello(String msg) {
        System.out.println("hello " + msg);
    }
}
```

`default` 키워드를 사용하여 간단하게 구현할 수 있다.

### 코드 호환성을 유지하며 새로운 기능 적용

그럼 실무에서는 어떤식으로 사용할까?

예를 들어 상사가 **“잠깐동안 특정 메뉴만 할인 쿠폰을 뿌리는 이벤트를 위해 Order 인터페이스를 구현한 Pizza, Hamburger, Chicken 클래스에 할인 쿠폰을 발행하는 메서드를 추가해주세요. 1개월 이후에 해당 이벤트는 다시 막을 수 있어요.”** 라고 업무를 줬다고 치자.

Order의 구현체별로 `giveChickenDiscountCoupon()`를 구현하는 방법도 있지만 기존 코드를 변경하지 않고, **디폴트 메서드**로도 가능하다.

```java
public interface Order {
    // 배달요청을 받다
    void deliver();
    
    // 결제받다    
    void receivePayment();

    // 픽업하다
    void pickUp();
    
    // 할인 쿠폰 발급
    default void giveDiscountCoupon() {
        System.out.println("할인 쿠폰 발급");
    }
}
```

**디폴트 메서드**는 **기존의 인터페이스 구현체들의 변경 없이 공통적인 기능을 제공할 때 사용**된다.

### 불필요한 구현부 제거

인터페이스를 구현하다보면 사용하지 않는 메서드를 빈 상태로 구현하는 일이 종종 있다.
예시로 Chicken과 Hamburger만 픽업이 가능하고 Pizza는 배달만 가능한 상가일때는 픽업을 할 수가 없다.
그러나 불필요하지만 비어있는 메서드를 구현해야 한다.

```java
public class Pizza{
    public void deliver() {
        System.out.println("00피자의 배달 요청을 받았습니다.");
    }
    
    public void pickUp() {
        
    }
}
```

이런 경우에도 **디폴트 메서드를 이용**하면 불필요한 코드를 줄일 수 있다.

```java
public interface Order {
    // 배달요청을 받다
    void deliver();
    
    // 결제받다    
    void receivePayment();

    // 픽업하다
    default void pickUp() {
		
		}
    
    // 할인 쿠폰 발급
    default void giveDiscountCoupon() {
        System.out.println("할인 쿠폰 발급");
    }
}
```

`pickUp()` 을 빈 상태로 **디폴트 메서드로 선언**한다면 **필요한 구현체에서만 Override 해서 구현**할 수 있다.
하지만 **이 방법은 인터페이스가 갖는 추상 메서드 구현의 강제성이 사라지므로 동업자들에게 왜 이렇게 했는지 꼭 인지시켜줘야 한다.**

### 장단점

- **장점**
    - 코드 호환성을 유지하며 새로운 기능추가: 해당 인터페이스를 구현하는 기존 클래스의 구현을 중단하지 않고 인터페이스에 새 메서드 추가 가능
    - 불필요한 구현부 제거
- **단점**
    - 클래스 구현에 대한 제어력 저하: 클래스 구현은 의도치 않게 기본 메서드를 상속하고 사용할 수 있으며, 기본 메서드의 논리가 특정 구현에 적합하지 않은 경우 예기치 않은 동작이 발생할 수 있음
    - 코드 복잡성: 더 많은 기본 메서드가 추가될 경우 복잡해짐
    - 다중 상속: 클래스가 동일한 이름을 가진 기본 메서드로 여러 인터페이스를 구현하는 경우 기본 메서드가 모호성을 유발할 수 있음

## Stream(스트림)


**Stream**은 **기능적 스타일로 데이터 컬렉션을 처리**할 수 있도록 하는 Java 8에서 가장 사랑받는 기능이다.
간단하게 필터링, 매핑, 축소 등을 위한 방법을 제공하여 데이터 처리를 보다 표현적이고 효율적이게 만든다.

간단한 예시로 Stream을 알아보자.

- **요구사항:** **“영화들 중 James Cameron 영화의 company 정보가 필요하다.영화 이름을 기준으로 정렬해라.”**

### Stream 미사용

```java

movies.sort(Comparator.comparing(Movie::getName));

List<String> filmsMadeByCameron = new ArrayList<>();
for (Movie movie : movies) {
    if (movie.getDirector().equals("James Cameron")) {
        filmsMadeByCameron.add(movie.getCompany());
    }
}
```

여기서 정렬을 먼저 한 이유는 James Cameron의 영화를 필터링하고 정렬하려면 새로운 List 객체를 만들어 담는 과정이 필요하기 때문이다.

### Stream 사용

```java
List<String> filmsMadeByCameron = 
            movies.stream()
                .filter(movie -> movie.getDirector().equals("James Cameron"))
                .sorted(Comparator.comparing(Movie::getName))
                .map(Movie::getCompany)
                .collect(Collectors.toList());
```

위에 코드를 보면 한 눈에 알기 쉽게 이해가 갈 것이다. **Stream**은 개발자가 **불필요한 for문과 if문을 사용하지 않도록 도와준다.**

### Stream의 특징
- **파이프라이닝 지원**
    - **메서드 체이닝으로 연결**
    - 위 코드처럼 `filter(), sorted(), map(), collect()` 등이 계속 이어지는데, 이렇게 스트림 객체끼리 연속처럼 하나의 파이프라인이 되어 최종적인 결과 값을 반환
- **내부 반복 지원**
    - 코드 외부에서 for문을 사용하지 않고, **filter()처럼 내부에서 알아서 처리**
- **딱 1번 탐색**
    - 스트림 기능 사용시 이전으로 복귀 불가능
    - 연산 이전의 값은 저장되지 않고, 새로운 값으로 반환
    - **재사용시 변수에 저장**해야함
    
    ```java
    // 스트림을 변수에 저장
    Stream<Movie> filmsMadeByDirector = books.stream()
            .filter(movie -> movie.getDirector().equals("James Cameron"));
    
    // 저장된 스트림을 사용
    List<String> filmsMadeByCameron = filmsMadeByDirector
            .sorted(Comparator.comparing(Movie::getName))
            .map(Movie::getCompany)
            .collect(Collectors.toList());
            
    // 다른 로직에 재사용
    List<Movie> collect = filmsMadeByDirector
            .sorted(Comparator.comparing(Movie::getCompany).reversed())
            .skip(3L)
            .limit(100L)
            .collect(Collectors.toList());
    ```
    
- **게으르다**
    - **필요한 시점에 동작하는 방식**
    - 중간 연산이 복잡해도 **종료 연산이 없으면 실행되지 않음**
    
    ```java
    movies.stream()
        .filter(movie -> { 
            System.out.println(movie.getDirector()); // 작가 출력
            return movie.getDirector().equals("James Cameron"); 
        })
        .sorted(Comparator.comparing(Movie::getName));
    ```
    
    위 코드는 종료 연산이 없기 때문에 중간 연산을 실행하지 않는다.
    즉, 중간 연산이 아무리 복잡해도 종료 연산이 없으면 실행되지 않는다는 의미이다.
    
- **중간 연산과 종료 연산으로 구분**
    - `Stream<T>` 형태로 스트림이 반환되면 중간연산, 그렇지 않으면 종료 연산
    - `filter()`, `sorted()`는 모두 중간연산

## Optional(옵셔널)


**Optional** 클래스는 존재하거나 null 값을 의미하는 선택적 값을 나타내기 위해 도입되었다. NPE를 피하는 데 도움이 되며 더 나은 null 값 처리를 제공한다.

### 기존 null 처리

```java
public class MovieService {
    // 파라미터로 Movie 객체를 받는다.
    // Movie 객체가 가진 Director객체의 getName()으로 감독의 이름을 반환한다.
    public String getDirectorName(Movie movie) {
        if (movie == null) {
            throw new NullPointerException("This movie is null");
        }
        
        Director director = movie.getDirector();
        return director.getName();
    }
}
```

이 코드에서 문제점은 Movie 객체가 가진 getDirector()의 결과 값이 null 일 가능성이 있는 것이다.
그러므로 Director 객체에 대한 **null 값을 확인하는 방어 코드를 추가**해야 한다.

```java
// movie.getDirector()에서 null이 반환된다면 director.getName() 코드에서 NPE가 발생한다.
Director director = movie.getDirector();

if (director == null) {
    throw new NullPointerException("This director is null");
}
```

그 다음 문제는 이 메서드가 null을 반환할 수 있다는 것이다. MovieService의 `getDirectorName()`을 호출하는 객체는 이 결과 값이 null인지 아닌지 알 수 없어 NPE을 방어하는 코드를 추가해야 한다.

```java
String directorName = movieService.getDirectorName(movie);

// 리턴 타입이 null이 아니라는 확신이 없으므로 확인 로직을 추가해야한다.
if (directorName == null) {
    throw new NullPointerException("This director name is null");
}
```

마지막으로 `getDirectorName()` 메서드가 어떤 비즈니스 로직을 수행하고 있는지 한 눈에 파악하기 힘들다.
이런 간단한 로직에서도 NPE를 막기 위해 고생해야 한다.

### **Optional을 활용한 null 처리**

```java
public class MovieService {
    // Optional을 사용하여 null에서 안전한 코드 작성하기
    public Optional<String> getDirectorName(Movie movie) {
        return Optional.ofNullable(movie)
                .map(movieObject -> movieObject.getDirector())
                .map(directorObject -> directorObject.getName());
    }
}
```

위 코드를 보면 **if 문을 통해 null을 확인하는 부분이 사라지고, 메서드의 반환 타입이 String → Optional<String> 으로 변경**되었다.

1. **null을 방어하는 코드를 추가해야 함**
    
    **Optional**을 활용하여 **null이 발생할 여지를 제거**하고, 파라미터의 **Movie 객체가 null이어도 메서드 내부에서는 NPE가 발생하지 않고 비즈니스 로직을 수행**함.
    
2. **getDirectorName()을 호출하는 곳에서 결과값으로 null을 받음**
    
    근본적인 문제는 해결하지 못했지만, 리턴 타입을 Optional<String>으로 했기 때문에 메서드를 사용하는 개발자는 **‘빈 결과 값이 반환될 수 있다’** 라고 인지할 수 있음. 만약 null이 반환되지 않는 것이 확실한 경우에는 리턴 타입을 String으로 표기하여 호출하는 입장에서 별도의 NPE 방어 코드를 넣지 않아도 되게 할 수 있음.
    
3. **비즈니스 코드와 null 방어 코드가 뒤섞여 분석하는데 오랜 시간이 걸림**
    
    null의 방어 코드가 완전히 사라져 getDirectorName()이 어떤 로직을 수행하는지 한 눈에 파악 가능
    

### Optional의 메서드

- **객체를 Optional로 감싸는 메서드(static 메서드)**
    - `of(T):Optional<T>`
        
        파라미터로 받은 객체를 Optional로 감싸 반환
        만약 파라미터가 null이면 NPE 발생
        
    - `ofNullable(T):Optional<T>`
        
        기본적으로 of()와 동일하지만 파리미터가 null이면 빈 Optional을 반환
        
    - `empty():Optional<T>`
        
        빈 Optional을 반환
        Optional 객체의 중간 연산 중에 값이 null이 되면 내부적으로 이 메서드를 호출함
        
- **Optional의 중간 연산(non static 메서드 || 반환 값이 Optional<T>)**
    - `filter(Predicate<? super T> predicate):Optional<T>`
        
        Stream API의 filter()와 동일
        predicate의 조건에 맞는 값을 필터링
        
    - `map(Function<? super T, ? extends U> mapper):Optional<T>`
        
        Stream API의 map()과 동일
        Optional로 감싸진 객체를 다른 객체로 변경하도록 데이터 변경
        
    - `flatMap(Function<? super T, Optional<U>> mapper):Optional<T>`
        
        Optional안의 Optional이 있는 이중 구조일 때, 단일 구조로 변경하여 map()의 기능을 수행
        
- **Optional의 종료 연산(Optional에서 벗어나 값으로 반환)**
    - `get():T`
        
        Optional의 값을 반환
        만약 값이 빈 값이라면 NPE가 발생
        
    - `orElse(T):T`
        
        get()과 동일한 기능을 수행하지만, 값이 비어있다면 파라미터로 받은 값으로 반환
        
    - `orElseGet(Supplier<? extends T> other):T`
        
        get과 동일한 기능을 수행하지만, 값이 비어있다면 파라미터에서 제공하는 값을 반환
        
    - `orElseThrow(Supplier<? extends X> exceptionSupplier):T`
        
        get과 동일한 기능을 수행하지만, 값이 비어있다면 파리미터에서 생성한 exception을 발생
        
- **분류가 되지 않는 기타 메서드**
    - `isPresent():boolean`
        
        Optional의 값이 있다면 true, 비어있다면 false를 반환
        상태를 확인할 뿐 값에 어떤 영향도 미치지 않음
        
    - `ifPresent(Consumer<? super T> consumer):void`
        
        Optional의 값이 있다면 파라미터를 실행하고, 비어있다면 false를 반환
        

## 새롭게 추가된 날짜 API


Java 8은 java.time 패키지에 있는 새로운 날짜 및 시간 API를 도입했다.
크게 기계용 날짜 API와 사람용 날짜 API로 나눌 수 있다.

### 기계용 날짜 API :: Instant

Instant 클래스는 Unix epoch time을 기준으로 특정 시점까지의 시간을 초로 표현한다.
**Unix epoch time**은 흔히 ‘**타임스탬프**’로 표현되는 시간이다.

```java
// 현재 시간을 Unix epoch time으로 나타내는 코드 (모든 시간을 초로 환산하여 표시)
System.out.println(Instant.now().getEpochSecond());

// Unix epoch time의 기준시를 나타내는 코드(기준시기 때문에 초로 환산해도 0으로 출력된다)
System.out.println(Instant.ofEpochSecond(0).getEpochSecond());
```

모든 시간은 초를 기준으로 계산되기 때문에 **연, 월, 달 또는 시와 분의 개념이 없다.**

### **사람용 날짜 API :: LocalDate, LocalTime, LocalDateTime**

기존의 Date 클래스는 이름과 다르게 날짜, 요일, 시간까지 포괄하는 정보를 갖는다. 너무 많은 역할을 가지고 있어 문제가 있었다. 하지만 **Java 8**에서 **날짜, 시간 별로 클래스를 분리**시켰다. 기본적으로 다루는 정보가 다른 것 외에는 모두 **동일한 사용법**을 가지고 있다.

날짜와 시간을 String으로 표현하는 코드

```java
// 현재 날짜와 시간을 String 값으로 표현
System.out.println(LocalDateTime.now().format(DateTimeFormatter.ISO_LOCAL_DATE_TIME));

// 2023-07-27T12:00:00 을 String 값으로 표현
System.out.println(LocalDateTime.of(2023, 7, 27, 12, 0).format(DateTimeFormatter.ISO_LOCAL_DATE_TIME));
```

### **날짜 포맷을 위한 새로운 클래스 :: DateTimeFormatter**

**DateTimeFormatter**의 등장 배경은 날짜의 포맷은 대부분은 개발자가 하드 코딩하거나 상수로 생성해서 사용해 왔는데, 이 부분을 **날짜 포맷이 상수로 등록**되어 있고, **API Document를 참고하여 원하는 포맷으로 변환도 가능**하게 해주었다.

```java
System.out.println(
    LocalDateTime
        .now()
        .format(DateTimeFormatter
                .ofPattern("오늘은 y-mm-dd일 (E)이고, 현재시간은 a h:s분입니다.")
                .withLocale(Locale.KOREA))
);
```

## **CompletableFuture(컴플리터블 퓨처)**


Java의 **비동기 프로그래밍의 불편함을 해소**하기 위해 추가 되었다.
대표적으로 생성한 스레드가 동작 중인지, 정상적으로 종료되었는지, 예외가 발생해 비정상 종료되었는지 메인 스레드에서는 알기가 어렵다.

간단한 예시로 알아보자.

**"제휴를 맺은 영화사인 유니버설 스튜디오와 월트 디즈니 픽처스에서 영화제작 가격을 조회하여 가장 저렴한 가격과 영화사를 출력해주세요. 단, 두 영화사의 API가 느려 각각  평균 1 ~ 2초 정도의 응답 시간이 걸립니다. 최대한 빠르게 응답받아 처리할 수 있도록 개발해주세요."**

### **Thread를 사용한 조회**

```java

// 유니버설 스튜디오에서 영화제작 가격 조회
new Thread(() -> {
    int filmmakingPrice = UniversalStudios.getFilmmakingPrice();
    System.out.println("UniversalStudios : " + filmmakingPrice);
}).start();

// 월트 디즈니 픽처스에서 영화제작 가격 조회
new Thread(() -> {
    int filmmakingPrice = WaltDisneyPictures.getFilmmakingPrice();
    System.out.println("WaltDisneyPictures : " + filmmakingPrice);
}).start();
```

이 코드는 두 스레드가 모두 끝난 지 메인에서 확인하는 방법도 마땅하지 않고, `getFilmmakingPrice()`에서 예외가 발생하면….. 머리가 아프다. 

### **CompletableFuture를 사용한 조회**

Java 8에서 새롭게 등장한 Future 인터페이스의 구현체이다.
먼저 예외처리를 지원하는 메서드와 순서의 의존관계를 맺는 스레드 프로그래밍, 콜백을 지원하기도 하며 여러 스레드를 하나로 묶어 처리하기에도 좋다.

```java
CompletableFuture<Integer> lowPriceSearchFuture = CompletableFuture.supplyAsync(() -> {
    int filmmakingPrice = UniversalStudios.getFilmmakingPrice();
    System.out.println("UniversalStudios : " + filmmakingPrice);
    return filmmakingPrice;
})
.thenCombine(CompletableFuture.supplyAsync(() -> {
    int filmmakingPrice = WaltDisneyPictures.getFilmmakingPrice();
    System.out.println("WaltDisneyPictures : " + filmmakingPrice);
    return filmmakingPrice;
}), (universalStudiosPrice, waltDisneyPicturesPrice) -> filmmakingPriceAndCompany(universalStudiosPrice, waltDisneyPicturesPrice))
.exceptionally(throwable -> {
    System.out.println(throwable.getMessage());
    return -1;
});

lowPriceSearchFuture.get(3, TimeUnit.SECONDS);
```

`thenCombin()`을 통해 서로 다른 스레드를 엮어 비동기로 진행하고 두 스레드가 정상적으로 처리되면 `filmmakingPriceAndCompany()`를 호출한다.
그리고 `exceptionally()`을 통해 예외상황도 처리했다.

## **JVM의 변화**

PermGen이 사라지고 Metaspace 영역이 생겼다. 실무 영역에서는 `-XX:PermSize`, `-XX:MaxPermSize` 삭제 후 `XX:MetaspaceSize`, `-XX:MaxMetaspzceSize` 옵션이 추가됐다.

### 변경된 내용

- PermGen은 메타 정보를 관리하는 메모리 공간으로 Heap 메모리에 포함됨
    - 이로 해당 정보에 대한 크기가 제한됨.
    - 이에 대해 **Java 8**에서는 **Metaspace**가 생기고 **Native 영역**으로 이동됨
    - **OS**가 해당 **크기를 자동으로 조정**해줌
    - 이를 통해 **OOM 발생 여지가 줄어듬**
