# IoC와 DI

## IoC(Inversion of Control - 제어의 역전)


**IoC**란 **Inversion of Control**의 줄임말로, **제어의 역전**이라고 한다.
**스프링 애플리케이션**에서는 오브젝트(빈)의 생성과 의존 관계 설정, 사용, 제거 등의 작업을 애플리케이션 코드 대신 **스프링 컨테이너**가 담당한다.
이를 스프링 컨테이너가 코드 대신 **오브젝트에 대한 제어권을 가지고 있다**고 하여 **IoC**라고 한다.
따라서, 스프링 컨테이너를 **IoC 컨테이너**라고 한다.

### IoC 컨테이너

스프링에서는 IoC를 담당하는 컨테이너를 **빈 팩토리**, **DI 컨테이너**, **애플리케이션 컨텍스트**라고 한다.
오브젝트의 생성과 오브젝트 사이의 런타임 관계를 설정하는 **DI 관점**에서 보면, 컨테이너를 **빈 팩토리** 또는 **DI 컨테이너**라고 한다.

하지만 **스프링 컨테이너**는 단순한 DI 작업보다 더 많은 작업을 하는데, DI를 위한 빈 팩토리에 여러 가지 기능을 추가한 것을 **애플리케이션 컨텍스트**라고 한다.
즉, **애플리케이션 컨텍스트**는 그 자체로 **IoC와 DI를 포함한 그 이상의 기능**을 가진 것이다.

### 빈 팩토리와 애플리케이션 컨텍스트

빈 팩토리와 애플리케이션 컨텍스트의 관계를 아래 그림으로 보자.

![스크린샷 2023-07-20 오후 5 51 14](https://github.com/mo2-Study-Group/StudyGroup/assets/112863029/a2a9d67c-9752-4103-9ca4-c351d8a8ba34)


- **빈 팩토리**
    - 스프링 컨테이너의 **최상위** 인터페이스
    - **스프링 빈을 관리하고 조회**하는 역할을 담당
    - 대표적으로 `getBean()` 메서드를 제공

- **애플리케이션 컨텍스트**
    
    ```java
    public interface ApplicationContext extends EnvironmentCapable, ListableBeanFactory, 
                                                HierarchicalBeanFactory, MessageSource, ApplicationEventPublisher, 
                                                ResourcePatternResolver {
    ```
    
    **애플리케이션 컨텍스트**는 **빈 팩토리 기능을 모두 상속** 받아서 제공한다.
    위의 인터페이스에서 extends한 인터페이스들은 모두 빈 팩토리 인터페이스의 서브 인터페이스이고, **빈 팩토리에게 없는 추가 기능**을 가지고 있다.
    
    - **메시지 소스를 활용한 국제화 기능**
    한국에서 들어오면 한굴어로, 영어권에서 들어오면 영어로 출력
    - **환경 변수**
    로컬, 개발, 운영 등을 구분해서 처리
    - **애플리케이션 이벤트**
    이벤트를 발행하고 구독하는 모델을 편리하게 지원
    - **편리한 리소스 조회**
    파일, 클래스 패스, 외부 등에서 리소스를 편리하게 조회

### 설정 메타 정보

**IoC 컨테이너**의 가장 기초적인 역할은 **오브젝트를 생성하고 이를 관리**하는 것이다.
스프링 컨테이너가 관리하는 오브젝트는 **빈**이라고 부른다.
**설정 메타 정보**는 **빈을 어떻게 만들고 어떻게 동작하게 할 것인가에 관한 정보**이다.
스프링 컨테이너는 **자바**, **XML**, **Groovy** 등 다양한 형식의 설정 정보를 받아들일 수 있도록 **유연하게 설계**되어 있다.

![스크린샷 2023-07-20 오후 6 02 35](https://github.com/mo2-Study-Group/StudyGroup/assets/112863029/559694fa-47ed-403e-8991-3fe9b23c9381)


- **어노테이션 기반 자바 코드 설정**
    
    ```java
    @Configuration
    public class AppConfig {
    
            @Bean
            public MemberService memberService() {
                    return new MemberServiceImpl(memberRepository());
            }
    }
    ```
    
    `@Configuration` : 1개 이상의 빈을 제공하는 클래스의 경우 반드시 작성해야 한다.
    
    `@Bean` : 클래스를 빈으로 등록할 때 사용한다.
    
- **XML 기반의 스프링 빈 설정**
    
    ```xml
    <beans xmlns="http://www.springframework.org/schema/beans"xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"xsi:schemaLocation="http://www.springframework.org/schema/beans http://
    www.springframework.org/schema/beans/spring-beans.xsd">
    
         <bean id="memberService" class="hello.core.member.MemberServiceImpl">
                 <constructor-arg name="memberRepository" ref="memberRepository"/>
         </bean>
    </beans>
    ```
    
    XML기반의 설정 파일을 보면 자바 코드와 비슷한 느낌을 받을 것이다.
    XML 기반으로 설정하는 것은 최근에는 잘 사용하지 않는다.
    

### 스프링 빈 설정 메타 정보 - BeanDefinition

스프링이 이렇게 다양한 형식을 지원할 수 있는 이유는 **BeanDefinition**이라는 추상화가 있어서다.
XML을 읽어서 BeanDefinition을 만들고, 자바 코드를 읽어서 BeanDefinition을 만든다.
즉, **스프링 컨테이너**는 **자바 코드인지 XML 코드인지 알지 못한채 BeanDefinition만 이용**하는 것이다.
**BeanDefinition**을 **빈 설정 메타 정보**라고 하는데, `@Bean`과 `<bean>` 당 각각 하나씩 메타 정보가 생성된다.

![스크린샷 2023-07-20 오후 6 10 55](https://github.com/mo2-Study-Group/StudyGroup/assets/112863029/fc51a57d-205e-4859-9303-19edc0007096)


**AnnotationConfigApplicationContext**는 **AnnotatedBeanDefinitionReader**를 사용해서 AppConfig.class를 읽고 **BeanDefinition**을 생성한다.

마찬가지로, GenericXmlApplicationContext는 XmlBeanDefinitionReader를 사용해서 appConfig.xml 설정 정보를 읽고 BeanDefinition을 생성한다.

새로운 형식의 설정 정보가 추가되면, XxxBeanDefinitionReader를 만들어서 BeanDefinition을 생성하면 된다.

## DI(Dependency Injection - 의존 관계 주입)

### Dependency(의존 관계)

“**A가 B에 의존한다**” 는 굉장히 추상적인 표현이지만, 스프링에서는 “**의존 대상 B가 변하면, 그것이 A에 영향을 미친다**” 라고 한다.
즉, **B의 기능이 추가되거나 수정되면 그 영향이 A에게 미치는 것**이다.

아래 코드를 보며 이야기해보자

```java
class Barista {
    private CoffeeRecipe coffeeRecipe;

    public Barista() {
        coffeeRecipe = new CoffeeRecipe();        
    }
}
```

커피 레시피가 변하면, 변한 레시피에 맞춰서 Barista클래스를 수정해야 한다.
**레시피의 변화가 바리스타의 행위에 영향을 미치기 때문에 바리스타는 레시피에 의존**하고 있는 것이다.

### Dependency를 인터페이스로 추상화

위 코드를 보면, Barista는 CoffeeRecipe만 의존할 수 있는 구조로 되어 있다. 더 다양한 커피 레시피를 의존할 수 있게 구현하려면 **인터페이스를 추상화**해야 한다.

```java
class Barista {
    private CoffeeRecipe coffeeRecipe;

    public Barista() {
        coffeeRecipe = new AmericanoRecipe();
        //coffeeRecipe = new CafeLatteRecipe();
        //coffeeRecipe = new HazelnutRecipe();
    }
}

interface CoffeeRecipe {
    newCoffee();
} 

class AmericanoRecipe implements CoffeeRecipe {
    public Coffee newCoffee() {
        return new Americano();
    }
}
```

위 코드를 보면, 다양한 커피 레시피에 의존할 수 있는 Barista를 볼 수 있다. 이처럼 **의존 관계를 인터페이스로 추상화**하게 되면, **더 다양한 의존 관계**를 맺을 수 있고, **실제 구현 클래스와의 관계가 느슨해지며 결합도가 낮아진다**.

### DI(의존관계)

지금까지 설명한 내용은 Barista 내부적으로 의존 관계인 CoffeeRecipe가 어떤 값을 가질지 직접 정하고 있다.
이때 **DI**는 어떤 커피 레시피를 만들 지는 커피레시피 제조자가 정하는 상황이라 생각하면 된다.
즉, **Barista가 의존하고 있는 CoffeeRecipe를 외부(커피레시피제조자)에서 주입하는 것**이다.

```java
class Barista {
    private CoffeeRecipe coffeeRecipe;

    public Barista(CoffeeRecipe coffeeRecipe) {
        this.coffeeRecipe = coffeeRecipe;
    }
}

//의존관계를 외부에서 주입 -> DI
new Barista(new AmericanoRecipe());
new Barista(new CafeLatteRecipe());
new Barista(new HazelnutRecipe());
```

이처럼 **의존 관계를 외부에서 결정해서 주입 해주는** 것을 **DI**(의존 관계 주입)이라고 한다.

![스크린샷 2023-07-20 오후 6 53 47](https://github.com/mo2-Study-Group/StudyGroup/assets/112863029/47d276b6-6d7a-4b0f-81cc-e722be310dc2)

스프링에서는 외부의 대상이 **IoC 컨테이너**가 되어, 빈을 알아서 주입해준다.

### DI(의존 관계 주입) 구현 방법

**필드 주입**

```java
@Service
public class CoffeeService {

    @Autowired
    private CoffeeRecipe coffeeRecipe;
}
```

변수 선언부에 `@Autowired`를 사용한다.

- **장점**
    - 사용하기 편하다.
- **단점**
    - 단일 책임 원칙 위반 가능성이 커진다.
        - `@Autowired` 선언만 하면 되므로 의존성 주입이 쉬워져 하나의 클래스가 많은 책임을 갖게 될 가능성이 높아진다.
    - 의존성이 숨는다.
        - 생성자 주입에 비해 의존 관계를 한 눈에 파악하기 힘들다.
    - DI 컨테이너와의 결합도가 커지고, 테스트하기 어렵다.
    - 불변성을 보장할 수 없다.
    - 순환 참조가 발생할 수 있다.

**수정자 주입**

```java
@Service
public class CoffeeService {

		private CoffeeRecipe coffeeRecipe;

    @Autowired
    public void setCoffeeRecipe(CoffeeRecipe coffeeRecipe) {
			 this.coffeeRecipe =. coffeeRecipe;
}
```

이 방법은 **setter**를 사용한 주입이다.

- **장점**
    - **선택적인 의존성**을 사용할 수 있다.
- **단점**
    - 선택적인 의존성을 사용할 수 있다는 것은 CoffeeService에 모든 구현체를 주입하지 않아도 CoffeeRecipe 객체를 생성할 수 있고, 객체의 메소드를 호출할 수 있다는 것이다.
    즉, **주입받지 않은 구현체를 사용하는 메서드에서 NPE(NullPointerException)가 발생**한다.
    - 순환 참조 문제가 발생할 수 있다.

**생성자 주입**

```java
@Service
public class CoffeeService {

		private CoffeeRecipe coffeeRecipe;

    @Autowired
    public CoffeeRecipe(CoffeeRecipe coffeeRecipe) {
			 this.coffeeRecipe =. coffeeRecipe;
}
```

생성자에 `@Autowired` 어노테이션을 붙여 의존성을 주입받을 수 있고, **가장 권장되는 주입 방식**이다.

- 장점
    - 의존 관계를 모두 주입 해야만 객체 생성이 가능하므로 **NPE(NullPointerException)를 방지**할 수 있다.
    - **불변성을 보장**할 수 있다.
    - **순환 참조를 컴파일 단계에서 찾아낼 수 있다.**
    - 의존성을 주입하기 번거롭고, 생성자 인자가 많아지면 코드가 길어져 가독성이 떨어진다.
        - 이를 바탕으로 SRP 원칙을 생각하게 되고, 리팩터링을 적용하게 된다.

### 순환 참조란?

그러면 순환 참조는 무엇이고 어떤 상황에서 발생할까?
먼저 **순환 참조**란 **서로 다른 여러 빈들이 서로를 참조하고 있음을 의미**한다.

왜 발생할까?

특정 클래스에서 **IoC 컨테이너에 있는 빈을 주입받기 위해서는 세 가지 방법**을 사용할 수 있다.(**필드 주입, 생성자 주입, 수정자 주입**)
우선 생성자 주입 방식의 경우 이 두 가지 방식과 순환참조 문제가 조금 다르게 발생한다.

- **필드 주입과 수정자 주입**

```java
// 필드 주입
@Service
public class AService {
    @Autowired
    private BService bService;

    public void call() {
        bService.call();
    }
@Service
public class BService {
    @Autowired
    private AService aService;

    public void call() {
        aService.call();
    }

// 수정자 주입
@Service
public class AService {
    private BService bService;

    @Autowired
    public void setbService(BService bService) {
        this.bService = bService;
    }

    public void call() {
        bService.call();
@Service
public class BService {
    private AService aService;

    @Autowired
    public void setaService(AService aService) {
        this.aService = aService;
    }

    public void call() {
        aService.call();
    }
```

순환 참조를  컴파일 타임을 **Spring Boot 2.5 이하 버전**에서는 알 수 없다.
**2.5 이하 버전**은 런타임에 로직 호출 시에 알 수 있는데, 그 이유는 **컴파일 시점에는 각자 주입만 할 뿐 누가 자신을 사용하고 있는지는 알지 못하기 때문**이다.
이 두 방식은 **스프링 애플리케이션 로딩시 예외가 발생하지 않는다**.
즉, 애플리케이션 구동 시점에서는 필요한 의존성이 없을 경우에는 실제로 사용하는 시점에 주입하기 때문에 **null 상태로 유지**한다.

**2.6 이상 버전** 부터는 이를 개선하여 컴파일 타임에 알 수 있다.

```java
***************************
APPLICATION FAILED TO START
***************************

Description:

The dependencies of some of the beans in the application context form a cycle:

┌─────┐
|  AService (field private com.spring.spring.BService com.spring.spring.AService.bService)
↑     ↓
|  BService (field private com.spring.spring.AService com.spring.spring.BService.aService)
└─────┘
```

- **생성자 주입**

```java
@Service
public class AService {
    private BService bService;
    @Autowired
    public AService(BService bService) {
        this.bService = bService;
    }
@Service
public class BService {
    private AService aService;

    @Autowired
    public BService(AService aService) {
        this.aService = aService;
    }
```

생성자 주입으로 순환 참조 문제가 발생하도록 실행 시키면 이러한 로그를 볼 수 있다.

```java
***************************
APPLICATION FAILED TO START
***************************

Description:

The dependencies of some of the beans in the application context form a cycle:

┌─────┐
|  AService defined in file [/Users/heoyun-yeong/Desktop/spring/build/classes/java/main/com/spring/spring/AService.class]
↑     ↓
|  BService defined in file [/Users/heoyun-yeong/Desktop/spring/build/classes/java/main/com/spring/spring/BService.class]
└─────┘
```

정리하자면, 클래스가 서로 의존성 주입을 통해 순환참조하고 있을 때 발생하는 문제이다.
또한 **스프링 애플리케이션 로딩시점에서 예외가 발생**한다.

### @AutoWired란?

**@Autowired**는 **의존성 주입을 할 때 사용하는 어노테이션**으로 **객체의 타입에 해당하는 빈을 찾아 주입**하는 역할을 한다.
즉, 스프링 서버가 올라 갈 때 애플리케이션 컨텍스트가 @Bean, @Service, @Controller 등 어노테이션을 이용하여 등록한 스프링 빈을 생성하고, @Autowired 어노테이션이 붙은 위치에 의존 관계 주입을 수행하게 된다.
그럼 어떻게 빈을 찾을까?
우선 빈의 인스턴스가 만들어지는 **Bean Life Cycle**이 있다.
**Bean Life Cycle**이란 **해당 객체가 언제, 어떻게 생성되어 소멸되기 전까지 어떤 작업을 수행하고 언제 어떻게 소멸되는지 일련의 과정**이다.

정리하자면, 아래 순으로 진행된다.
1. 스프링 컨테이너 생성
2. 스프린 빈 생성
3. 의존성 주입
4. 초기화 콜백: 빈이 생성되고, 빈의 의존관계 주입이 완료된 후 호출
5. 사용
6. 소멸전 콜백: 빈이 소멸되기 직전에 호출
7. 스프링 종료

**@Autowired** 어노테이션을 들어가보자.

```java
/**
* Note that actual injection is performed through a BeanPostProcessor which in turn means
* that you cannot use @Autowired to inject references into BeanPostProcessor or 
* BeanFactoryPostProcessor types. Please consult the javadoc for the 
* AutowiredAnnotationBeanPostProcessor class (which, by default, checks for the presence 
* of this annotation).
* Since:
* 2.5
* See Also:
* AutowiredAnnotationBeanPostProcessor, Qualifier, Value
* Author:
* Juergen Hoeller, Mark Fisher, Sam Brannen
*/
@Target({ElementType.CONSTRUCTOR, ElementType.METHOD, ElementType.PARAMETER, ElementType.FIELD, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Autowired {
    boolean required() default true;

}
```

들어가 보면, 실제 타깃에 Autowired가 붙은 빈을 주입하는 것은 **BeanPostProcessor**이고 그것의 구현체가**AutowiredAnnotationBeanPostProcessor**인 것을 확인할 수 있다.

## 정리

### IoC 컨테이너란?

**스프링 애플리케이션**에서는 객체(빈)의 생성과 관계설정, 사용, 제거 등의 작업을 애플리케이션 코드 대신 **스프링 컨테이너가 담당**하는데, 이를 **IoC 컨테이너**라고 한다.

### IoC 컨테이너의 장점

스프링 애플리케이션의 객체(빈)을 IoC 컨테이너가 관리해줌으로써 **개발자의 부담이 줄고 비즈니스 로직에 더욱 집중**할 수 있게 해준다.

### DI란?

DI는 **객체(빈)들 간의 의존관계를 외부에서 결정하고 주입**하는 것이다.

### DI의 장점

- 의존성이 줄어든다.
    - 의존한다는 것은 그 의존대상의 변화에 취약하다는 것이다.
    - DI로 구현하면 주입받는 대상이 변해도 그 구현 자체를 수정할 일이 없거나 줄어든다.
- 재사용성이 높은 코드가 된다.
- 테스트하기 좋은 코드가 된다.
- 가독성이 높아진다.

### DI의 종류

DI는 생성자 주입, 수정자 주입, 필드 주입이 있다.
**생성자 주입**은 **생성자 호출시점에 딱 1번만 호출되는 것을 보장**하고 **불변, 필수 의존관계에 사용**한다.
**수정자 주입**은 선택, 변경 가능성이 있는 의존관계에 사용되며 **빈을 선택적으로 주입이 가능**하다.
**필드 주입**은 외부에서 변경이 불가능하여 테스트 하기가 힘고, DI 프레임워크 없이는 작동하기 힘들어 주로 애플리케이션과 관계없는 **테스트코드**나 `@Configuration` **같은 스프링 설정 목적으로 사용**한다.

### 순환 참조

**순환 참조**란 서로 **다른 여러 빈들이 서로를 참조하고 있음을 의미**한다.
**필드 주입**이나 **수정자 주입**은 객체 생성 후 비즈니스 로직 상에서 순환 참조가 일어나기 때문에 컴파일 단계에서 순환 참조를 잡아낼 수 없다.

반면에 **생성자 주입**을 사용하면 스프링 컨테이너가 **빈을 생성하는 시점에 순환참조를 확인**하기 때문에 컴파일 단계에서 순환 참조를 잡아낼 수 잇다.

### 생성자 주입을 사용해야하는 이유

- 의존 관계를 모두 주입하지 않은 경우에 객체를 생성할 수 없기 때문에 **NPE가 발생하지 않는다**.
- `final` 키워드를 사용하여 **불변성을 보장**할 수 있다.
- 생성자 주입은 **컴파일 단계에서 순환 참조를 잡아 낼 수 있다**.
- DI 컨테이너 없이 **직접 의존성을 주입**할 수 잇다.

### Spring Ioc/DI의 동작 과정

**IoC**는 프로그램의 제어 흐름을 직접 제어하는 것이 아니라 **외부에서 관리**하는 것으로 코드의 최종 호출은 개발자가 아닌 **프레임워크의 내부에서 결정**된 대로 진행된다.

**DI**는 스프링 프레임워크에서 지원하는 **IoC의 형태**로 객체(빈) 사이의 의존관계를 **빈 설정 정보를 바탕**으로 **DI 컨테이너가 자동으로 연결**한다.

### AutoWiring 동작 과정

스프링 서버가 올라갈 때 애플리케이션 컨텍스트가 `@Bean`이나 `@Service`, `@Controller` 등 어노테이션을 이용하여 등록한 스프링 빈을 생성하고, `@Autowired`어노테이션이 붙은 위치 또는 생성자, 수정자를 통해 주입한다.

### DI와 IoC의 차이

**DI**는 **의존관계를 어떻게 가질 것인가**에 대한 문제이고, **IoC**는 **누가 소프트웨어의 제어권을 갖고 있느냐**의 문제이다. **IoC 컨테이너가 빈을 생성할 때 빈들간의 의존관계를 DI를 통해 해결**한다.
**DI는 IoC 사용을 필수로 요구하지 않는다**는 점을 주의해야한다.

### 참고 자료

- <https://steady-coding.tistory.com/600>

- <https://ch4njun](https://ch4njun.tistory.com/269)>

- <https://beststar-1.tistory.com/40>
