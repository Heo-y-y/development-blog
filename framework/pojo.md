

## POJO란?


우선 **POJO**라는 단어는 Java나 Spring을 공부하면 한번 쯤 들어봤을 단어이다.

간단하게 **POJO의 정의**를 살펴보자.

> **Plain Old Java Object**, 간단히 **POJO**는 말 그대로 해석을 하면 오래된 방식의 간단한 **자바** 오브젝트라는 말로서 **Java EE** 등의 중량 **프레임워크**들을 사용하게 되면서 해당 프레임워크에 종속된 “무거운 “ 객체를 만들게 된 것에 반발해서 사용되게 된 용어이다. 2000년 9월에 **마틴 파울러**, 레베카 파슨, 조쉬 맥킨지 등이 사용하기 시작한 용어로서 마틴 파울러는 다음과 같이 기원을 밝히고 있다.

> ”우리는 사람들이 자기네 시스템에 보통의 객체를 사용하는 것을 왜 그렇게 반대하는지 궁금하였는데, 간단한 객체는 폼 나는 명칭이 없기 때문에 그랬던 것이라고 결론지었다. 그래서 적당한 이름을 하나 만들어 붙였더니, 아 글쎄, 다들 좋아하더라고.” —**마틴 파울러**—
> 

그러면 오래된 방식의 간단한 **자바 오브젝트**라는 것은 뭘까?
쉽게 설명하자면 특정 ‘기술’에 종속되어 동작하는 것이 아닌 **순수한 자바 객체**를 말하는 것이다.

아래는 **POJO** 코드이다.

```java
public class Person {
    private String name;
    private int age;

    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

**POJO**는 개인 정보를 private 필드로 캡슐화하고, 해당 데이터에 접근하고 수정할 수 있는 getter 및 setter 메서드를 제공하는 **간단한 자바 클래스**이다.

**POJO가 아닌** 코드도 살펴보자.

```java
import org.springframework.beans.factory.annotation.Autowired;

public class OrderService {
    @Autowired
    private OrderRepository orderRepository;

    public void createOrder(String item) {
        Order order = new Order(item);
        orderRepository.save(order);
    }
}
```

이 코드는 클래스가 특정 프레임워크나 라이브러리에 종속된 추가적인 의존성과 동작을 가지고 있다.
OrderService 클래스는 `@Autowired` 의존성 주입 어노테이션을 가지고 있으며, 이는 Spring 프레임워크에 종속되었다는 것을 나타낸다. 이로 인해 **실제 POJO와 비교하여 다른 컨텍스트에서 재사용하기가 어려울 수 있다**.

그러면 위에 코드를 **좀 더 POJO스럽게 수정**해보자.

```java
public class OrderService {
    private OrderRepository orderRepository;

    public OrderService(OrderRepository orderRepository) {
        this.orderRepository = orderRepository;
    }

    public void createOrder(String item) {
        Order order = new Order(item);
        orderRepository.save(order);
    }
}
```

OrderService 클래스는 더 이상 Spirng 어노테이션과 프레임워크에 종속되어 있지 않는다.
대신, **생성자**를 통해 OrderRepository 인스턴스를 전달 받도록 변경했다. 이 접근 방식을 통해 테스트 용이성이 향상되며, 테스트 중에 OrderRepository의 모의 구현을 제공하기가 쉬워진다.

정리하자면, **POJO**의 정의적 특성은 그 **간결함**과 **특정 프레임워크나 라이브러리에 종속되지 않는 것**이다.
**POJO**는 **외부 기술과 강하게 결합되지 않고, 쉽게 이해하고 테스트하며 여러 응용 프로그램에서 재사용될 수 있는 자체 포함형 자바 클래스**여야 한다.

### Spring에서 POJO의 목적은 무엇일까?

**Spring 프레임워크**는 **객체 지향**적인 개발을 지향하며, **POJO를 중심으로 애플리케이션을 구성**하는 것을 장려한다. **Spring**은 **POJO를 활용하여 애플리케이션의 핵심 비즈니스 로직을 담은 클래스를 작성**하고, **이들 클래스를 서로 연결하며 구성함으로써 애플리케이션의 구조를 유연하고 모듈화된 형태로 구성**할 수 있게 도와준다.

Spring에서 POJO는 아래와 같은 특징을 가진다.

1. **의존성 주입(Dependency Injection)**: Spring은 POJO에 필요한 의존성을 주입하는 방식을 통해 객체 간의 결합도를 낮추고 유연한 구조를 제공한다.
2. **AOP(Aspect-Oriented Programming)**: Spring은 POJO에 관점 지향 프로그래밍을 적용하여 보안, 로깅, 트랜잭션 관리 등과 같은 횡단 관심사를 분리하여 관리할 수 있게 해준다.
3. **라이프사이클 관리**: Spring은 POJO의 생성부터 소멸까지의 라이프사이클을 관리하며, 필요한 초기화 작업이나 정리 작업을 수행할 수 있는 확장 포인트를 제공한다.
4. **POJO 기반 설정**: Spring은 XML 또는 Java Config와 같은 설정 방식을 통해 POJO들을 구성하고 관리할 수 있다.
5. **테스트 용이성**: POJO는 특정 프레임워크에 종속되지 않으므로, 단위 테스트 및 통합 테스트를 보다 용이하게 진행할 수 있다.

즉, **Spring**은 엔터프라이즈 서비스들을 **POJO 기반**으로 만든 비즈니스 오브젝트에서 사용할 수 있게 해준다.
**IoC 컨테이너**를 제공해서, 인스턴스들의 사이클을 관리하고, 특정 인터페이스를 구현하거나 상속할 필요가 없고 라이브러리를 지원하기에 용이하며 **객체 또한 가벼운 것이 특징**이며, OOP를 더 OOP답게 쓸 수 있게 해주는 **AOP** 기술을 적용해서 POJO 개발을 더 쉽게 만들어준다.

### 참고 자료

- <https://ko.wikipedia.org/wiki/Plain_Old_Java_Object>
- <https://yoo11052.tistory.com/133>
- <https://siyoon210.tistory.com/120>
