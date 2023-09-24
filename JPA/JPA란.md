# JPA

**JPA**(**Java Persistence API**)에 대해서 알아보자. 

**JPA**는 자바 진영에서 **ORM**(**Object-Relational Mapping**) **기술에 대한 API 표준 명세**를 뜻한다.

즉, **JPA**는 라이브러리가 아닌 **ORM을 사용하기 위한 인터페이스의 모음**인 것이다. 이 말은 실제적으로 구현된것이 아니라 **구현된 클래스와 매핑을 해주기 위해 사용되는 프레임워크**인 것이다. JPA를 구현한 대표적인 **오픈소스**로는 **Hibernate**가 있다.

먼저 **ORM**에 대해서 잠시 살펴보자.

## ORM(Object-Relational Mapping)

**ORM**은 **객체와 관계형 데이터베이스를 매핑**한다. 

쉽게 말해서 객체는 객체대로, 관계형 데이터베이스는 관계형 데이터베이스대로 설계하고, **ORM 프레임워크가 중간에서 매핑**해주는 것이다.

### **장점**

- SQL문이 아닌 Method를 통해 DB를 조작할 수 있기 때문에 개발자가 객체 모델을 이용하여 좀 더 비즈니스 로직에 집중할 수 있다.
- Query와 같이 필요한 선언문, 할당 등의 부수적인 코드가 줄어들어, 객체에 대한 코드를 별도로 작성하여 코드의 가독성이 올라간다.
- 개발자가 데이터 중심인 관계형 데이터베이스를 사용하더라도 객체지향적인 개발에 집중할 수 있다.
- 매핑하는 정보가 Class로 명시 되었기 때문에 ERD를 보는 의존도를 낮추고, 유지보수 및 리팩토링에 좋다.

### **단점**

- 프로젝트 규모가 크고 복잡하여 잘못 설계된 경우, 속도 저하 및 일관성이 깨질 수 있다.
- 복잡하고 무거운 Query는 속도를 위해 별도의 튜닝이 필요해 따로 SQL문을 써야할 수도 있다.

다음으로는 JPA의 오픈소스인 **Hibernate**에 대해서 알아보자.

## Hibernate

**Hibernate**라는 **오픈소스 ORM 프레임워크**가 등**장하면서 하이버네이트를 기반으로 새로운 자바 ORM 기술 표준**이 만들어졌는데, 그것이 바로 **JPA**이다.

**Hibernate**는 개발된 지 10년이 넘었고, 대중적으로 많이 이용되는 JPA 구현체 중 하나이다.

JPA의 핵심들인 EntityManagerFactory, EntityManager, EntityTransaction 등을 상속 받아서 구현한다.

JPA를 구현하는 다른 구현체들로는 EclipseLink나 DataNucleus 등이 있다.

만약 JPA를 구현한 구현체를 변경할 경우 개발자가 직접 JPA 구현체를 만들어 사용할 수 있다.

Hibernate는 내부적으로 JDBC를 이용해 관계형 데이터베이스와 커넥션을 맺고 상호작용한다.

**패러다임 불일치란?**

추상화, 상속, 다형성과 같은 특성을 가진 객체지향과 이러한 특성이 없는 데이터베이스는 서로 기능과 표현방법이 모두 다른데, 이러한 **객체와 관계형 데이터베이스의 불일치**를 **패러다임 불일치**라고 한다.

## JPA(Java Persistence API)

JPA는 자바 진영의 ORM 기술 표준이다.

JPA를 사용하려면 JPA를 구현한 ORM 프레임워크를 선택해야하는데, 대표적으로 하이버네이트가 있다.

![스크린샷 2023-09-24 오후 6.25.53.png](https://github.com/Heo-y-y/development-blog/assets/112863029/07c97b12-6ce8-4de5-a126-c2f72d1584c0)

### JPA 역할

- Entity 분석
- INSERT SQL 생성
- JDBC API 사용
- 패러다임 불일치 해결

### 장점

- 특정 구현 기술에 대한 의존도를 줄일 수 있다.
- 다른 구현 기술로 손쉽게 이동할 수 있다.

### JPA를 사용하는 이유

1. **생산성**

자바 컬렉션에 객체를 저장하듯이 JPA에게 저장할 객체만 전달하면 JPA가 대신 처리해준다.

```java
jpa.persist(member); // 저장
Member member = jpa.find(memberId); // 조회
```

반복적인 코드와 CRUD 용 SQL을 개발자가 직접 작성하지 않아도 된다.

CREATE TABLE과 같은 DDL문을 자동으로 생성해줄 수 있다.

1. **유지보수**

SQL을 직접 다루면 엔티티에 필드를 하나만 추가해도 그에 해당하는 SQL과 결과를 매핑하기 위한 JDBC API 코드를 모두 변경해야하는데, JPA를 사용하면 이러한 과정을 JPA가 대신 처리해준다.

JPA가 패러다임 불일치 문제를 해결해주기 때문에 객체지향 언어가 가진 장점들을 활용해 유연하고, 유지보수가 좋은 도메인 모델을 설계할 수 있다.

1. **성능**

JPA는 애플리케이션과 데이터베이스 사이에서 동작하므로 최적화 관점에서 시도해볼 수 있는 것들이 많은데,

```java
String memberId = "test"

Member member1 = jpa.find(memberId); // 조회
Member member2 = jpa.find(memberId); // 조회
```

위 코드는 회원을 두번 조회하는 코드이다.

JPA를 사용하지 않았을 땐 두 번의 SELECT 쿼리가 수행된다.

JPA를 사용하면 한번만 SELECT 쿼리가 수행되고, 그 다음번에는 이미 조회한 회원 객체를 재사용한다.

1. **데이터 접근 추상화와 벤더 독립성**

JPA는 애플리케이션과 데이터베이스 사이에 추상화된 데이터 접근 계층을 제공해서 애플리케이션이 특정 데이터베이스 기술에 종속되지 않도록 한다.

데이터 베이스 변경도 유연하게 처리가 가능하다. 즉, JPA에게 다른 데이터베이스를 사용한다고 알려주기만 하면 된다.

## Spring Data JPA

Spring Data JPA는 JPA를 쓰기 편하게 만들어 놓은 모듈이다.

Spring Data JPA는 JPA를 한 단계 더 추상화시킨 Repository 인터페이스를 제공한다.

이러한 Spring Data JPA는 Hibernate와 같은 JPA 구현체를 사용해서 JPA를 사용하게 된다.

Spring Data JPA를 사용하면 사용자는 더욱 간단하게 데이터 접근이 가능해진다.

<img width="620" alt="스크린샷 2023-09-24 오후 6 55 32" src="https://github.com/Heo-y-y/development-blog/assets/112863029/92be1c83-6641-4e74-b58c-94d4e053d00f">

여기서 알아야 할 것은 Spring에서 흔히 사용하는 것으로 알고있는 JPA는 JPA가 아니고, JPA를 이용하는 spring-data-jpa 프레임워크이다.

**참고 자료**

- <https://devfunny.tistory.com/813>
- <https://sowon-dev.github.io/2021/03/22/210323jpaVSjdbc/>
- <https://dbjh.tistory.com/77>
