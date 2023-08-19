## Spring Bean이란?

Spring **IoC 컨테이너가 관리하는 자바 객체**를 **빈**이라고 부른다.

자바 프로그래밍에서 Class를 생성하고 new 키워드를 사용해 객체를 생성하는게 아니라, `ApplicationContect.getBean()`으로 얻어질 수 있는 개체이다.
즉, **Spring 컨테이너에서 관리하는 객체는 ApplicationContext가 생성한 객체**이다.

Spring은 컨테이너에 스프링 빈을 등록할 때 싱글톤으로 등록한다.

## 스프링 빈을 등록하는 두 가지 방법

### 컴포넌트 스캔과 자동 의존관계 설정

이 방법은 `@ComponentScan` **어노테이션을 이용**해 스프링 컨테이너에 의해 `@Component` 및 `@Service, @Repository, @Controller` 등 부여된 Class 들을 **스캔해서 자동으로 생성되어 스프링 빈으로 등록**한다.

![1](https://github.com/mo2-Study-Group/StudyGroup/assets/70151275/316161c7-dd3e-470f-8621-77181cd91792)

위 그림과 같이 @Service도 내부적으로는 @Component 어노테이션을 사용한다.
그래서 빈으로 등록이 되는 것이다.

### 빈 설정 파일에 직접 빈 등록

빈 설정 파일은 XML과 자바 설정파일로 작성할 수 있는데, XML 방식은 최근에는 잘 사용하지 않는다.

자바 설정파일은 자바 클래스를 생성해 작성할 수 있으며, 일반적으로 [이름]Configuration와 같이 명명한다.

클래스에는 `@Configuration`을 붙이고, 그 안에 `@Bean`을 사용해 직접 빈을 정의한다.

```java
@Configuration
public class SampleConfiguration {
    @Bean
    public SampleController sampleController() {
        return new SampleController;
    }
}
```

스프링은 `@Configuration` 어노테이션이 **명시된 클래스를 우선으로 읽는다**.

`@Configuration`은 **Bean으로 등록하는 설정파일임을 알려주는 어노테이션**이다.
이 어노테이션이 붙은 클래스내에서 생성된 빈 객체는 **싱글톤을 보장** 해준다.
그냥 `@Bean` 어노테이션만 쓰면 빈을 등록할 수 있지만, `@Configuration`을 **같이 사용해야 싱글톤을 보장** 해준다.

그 이유는 `@Bean` 어노테이션에 있는데, 특징을 설명하자면,

- **메소드의 리턴 객체가 스프링 빈 객체임을 선언**한다.
- 빈의 이름은 메소드 이름, @Bean(name = “heo”)으로 이름을 변경할 수 있다.
- `@Scope`를 통해 **객체 생성을 조정**할 수 있다.
- **빈 이름은 항상 다른 이름을 부여**해야 한다. 같은 이름이면 무시 되거나 덮어버리거나 설정에 따라 오류가 발생한다.

### @Configuration은 어떻게 빈을 등록하고 싱글톤으로 관리할까?

![2](https://github.com/mo2-Study-Group/StudyGroup/assets/70151275/9c497d1a-6990-42f1-9c13-2ae7989eefe1)

`@Configuration`에 보면 `@Component`를 **사용하기 때문에** `@ComponentScan`**의 스캔 대상**이 되고, 빈 설정파일이 읽힐 때 그 안에 정의한 빈들이 **IoC 컨테이너에 등록**되는 것이다.

두 개의 속성이 존재하는데, 속성 `value()`는 `@Configuration`이 **붙은 클래스가 빈으로 등록될 때의 이름을 설정**할 수 있게 해준다.
`proxyBeanMethods()`는 `@Configuration`이 **빈을 싱글톤으로 관리하는 것과 연관**이 있는 속성이다.

**proxyBeanMethods** 에 대해서 조금 더 살펴보자.

빈에 대한 프록시 객체를 생성할 지 여부를 결정한다.

디폴트 값은 true, : 빈에 대한 프록시 객체가 생성된다.

- **porxyBeanMethods = true일 때 config 빈의 상태**

![tr](https://github.com/mo2-Study-Group/StudyGroup/assets/70151275/3bb5f51e-0c93-4f9e-a610-d0fb9c360210)

- **porxyBeanMethods = false일 때 config 빈의 상태**

![fa](https://github.com/mo2-Study-Group/StudyGroup/assets/70151275/fd4e3ecb-044e-4122-98c8-47b080219dee)

프록시 객체로 생성된 빈의 클래스 이름을 보면 **$$EnhancerBySpringCGLIB$$** 라는게 추가된걸 볼 수 있다.

참고로 **CGLIB**는 **바이트 코드를 가지고 프록시 객체를 만들어주는 라이브러리**다. **런타임 시에 자바 클래스를 상속**하고, **인터페이스를 구현해 동적 프록시 객체를 만든다**. 즉, **디폴트 상태의 config bean**은 우리가 직접 생성한 객체가 아니라 **CGLIB 라이브러리에서 생성해준 프록시 객체**인 것이다.

그러면 **프록시 객체를 생성하는 이유**는 뭘까?

```java
public class Resource{

}
```

위의 클래스를 스프링 빈으로 등록하고자 할 때 `@Component`를 **이용해 자동으로 빈 등록** 하면, **스프링이 알아서 객체를 제어**한다.

반면에 `@Bean`을 이용해 **직접 빈으로 등록**한다면,

```java
public class Resource{
		@Bean 
    public Resource Resource() {
        return new Resource(); 
    } 
    
    @Bean 
    public MyFirstBean myFirstBean() { 
        return new MyFirstBean(mangKyuResource()); 
    } 
    
    @Bean 
    public MySecondBean mySecondBean() { 
        return new MySecondBean(mangKyuResource()); 
    }
}
```

**실수로 빈을 생성하는 메소드를 여러 번 호출 했을 때, 여러개의 빈 생성**이 된다. 그래서 스프링은 이런 문제를 방지하고자 `@Configuration`이 있는 클래스를 객체로 생성할 때 **CGLIB 라이브러리를 사용해 프록시 패턴을 적용해 싱글톤을 보장**하는 것이다.

### BeanLiteMode

**BeanLiteMode**는 **CGLIB를 이용해 바이트 코드 조작을 하지 않는 방식**을 의미한다. 즉, **스프링의 싱글톤을 보장하지 않는 것**이다.

```java
@Component //@Comfiguration
public class AppConfig {

    @Bean
    public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
    }

    @Bean
    public OrderService orderService() {
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }

    @Bean
    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }
}
```

**설정하는 방법**은 `@Configuration`이 아닌 `@Conponent`**로 변경**하면 된다. 이렇게 LiteMode로 스프링 빈을 생성한다. 정확하게 표현하면 objectMapperLiteBean 메서드가 LiteMode로 작동한다.

추가적으로, **ApplicationContext를 사용해서 설정파일을 가지고 빈을 수동 등록**한다면, `@Componet`**가 없어도 BeanLiteMode가 동작**한다.

### LiteMode는 왜 필요할까?

- `@Configuration` **을 사용한 코드**

```java
@Configuration
public class BeanConfig1 {
    @Bean
    public ObjectMapper objectMapperBean() {
        return new ObjectMapper();
    }

    @Bean
    public ObjectMapper anyObjectMapperBean() {
        return objectMapperBean();
    }
}
```

밑에 `anyObjectMapperBean()` 메서드를 작동하면, 내부에 만들어 놓은 스프링 빈을 리턴하기에 ObjectMapper 객체를 생성하는 `objectMapperBean()` 메서드는 **1번만 실행**한다.

- **LiteMode 적용**

```java
@Component
public class BeanConfig1 {
    @Bean
    public ObjectMapper objectMapperBean() {
        return new ObjectMapper();
    }

    @Bean
    public ObjectMapper anyObjectMapperBean() {
        return objectMapperBean();
    }
}
```

이렇게 되면 `anyObjectMapperBean()`를 호출해서 `objectMapperLiteBean()`을 호출하면 진짜 메서드를 호출하여 **ObjectMapper 객체를 하나 더 만들게** 된다. 즉, **ObjectMapper 객체가 2개**가 된다.

**LiteMode를 쓰는 이유**는 **프록시 객체를 동적으로 생성**하게 되고, 이 객체는 메서드에 대한 요청을 가로채게 된다. 그래서 `@Component`로 **설정 클래스를 만들면 이 클래스는 순수 객체로 만들어져서 메서드를 호출하게 됐을 때 가로채지 않고 메서드가 처리**되는 것이다.

**추가로 설명하자면,**

- 컴포넌트 스캔 방법이 더 편리하지만, 직접 스프링 빈을 등록해서 관리하는 장점이 있다.
    - 외부 라이브러리 사용 시 @Bean으로 클래스를 등록해줘야 하는 경우 사용되기도 한다.
    - SpringConfig 파일에서 하눈에 스프링 빈 객체가 어떤 게 등록 되어 있는지 파악하기 쉽다는 장점이 있다.
    - 해당 방법을 사용함으로 OCP 원칙을 지킬 수 있다.
- 실무에서는 주로 정형화된 컨트롤러, 서비스, 레파지토리 같은 코드는 컴포넌트 스캔을 사용한다. 그리고 정형화 되지 않거나, 상황에 따라 구현 클래스를 변경해야 하면 설정을 통해 스프링 빈으로 등록한다.

## 빈 생명주기 콜백(BeanLifeCycle)

먼저 **콜백**이란 주로 **콜백 함수를 부를 때 사용되는 용어**이며, **콜백 함수를 등록하면** **특정 이벤트가 발생했을 때 해당 메서드가 호출되**는 것이다. 즉, **조건에 따라 실행 유무 개념**이라고 보면 된다.

데이터베이스 커넥션 풀(DB 연결)이나, 네트워크 소캣 연결처럼 애플리케이션 시작 시점에 필요한 연결을 미리 한 뒤, 애플리케이션 종료 시점에 연결을 모두 종료하는 작업을 진행하려면, 객체의 초기화 및 종료 작업이 필요하다.

- **커넥션 풀의 connect, disconnect**

![3](https://github.com/mo2-Study-Group/StudyGroup/assets/70151275/b4a92c4d-3172-4a25-b72d-e6b1bbbed3a4)

**데이터베이스 커넥션 풀**이란 **데이터베이스와 연결된 커넥션을 미리 만들어 놓고, 이를 pool로 관리**하는 것이다. 장점으로는 **Connection에 필요한 비용을 줄여 DB에 빠르게 접속**할 수 있다. 또한 **커넥션 수를 제한할 수 있어서 과도한 접속으로 인한 서버 자원 고갈을 방지**할 수 있으며, **DB 접속 모듈을 공통화 해 DB스프링 빈도 위와 같은 원리로 초기화 작업과 종료 작업을 나눠서 진행**한다.

간단하게 객체 생성 → 의존관계 주입이라는 라이프 사이클을 가진다. 즉, **스프링 빈은 의존관계 주입이 다 끝난 다음에야 필요한 데이터를 사용할 준비**가 된다.

### 의존성 주입 과정

![4](https://github.com/mo2-Study-Group/StudyGroup/assets/70151275/d9e8b7ec-d932-4c15-9348-e36a06dea5ed)

가장 먼저 Spring IoC 컨테이너가 만들어지고, 빈들이 등록되는 과정이다.

![5](https://github.com/mo2-Study-Group/StudyGroup/assets/70151275/3cd58661-0972-466f-8b8c-3fbaa9e96d25)

`@Configuration` 방법으로 **빈으로 등록할 수 있는 어노테이션들과 설정파일들을 읽어 IoC컨테이너에 빈들을 등록**시킨다.

![6](https://github.com/mo2-Study-Group/StudyGroup/assets/70151275/3dd11c0b-114c-49af-8985-06c3ce5ef898)

의존관계 주입을 하기 전에 준비 단계가 있는데, 이 단계에서 객체 생성이 일어난다. 주입 종류에 따라 다른 부분이 있다. **생성자 주입**은 **객체 생성과 의존관계 주입이 동시**에 일어나고, **Setter 주입, 필드 주입**은 **객체가 생성되고 그 다음 의존관계 주입으로 라이프 사이클**이 나뉜다.

**왜 생성자 주입은 동시에 일어날까?**

예를 들어 MemberController가 있으면,

```java
@Controller
public class MemberController {
    private final CocoService cocoService;
 
    public MemberController(CocoService cocoService) {
        this.cocoService = cocoService;
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
 
        // MemberControllercontroller = new MemberController(); // 컴파일 에러
 
        MemberController controller1 = new MemberController(new CocoService());
    }
}
```

자바에서는 new 연산자를 호출하면 생성자가 호출된다.

의존관계가 존재하지 않는다면, Controller 클래스는 객체 생성이 불가능 하기 때문에 생성자 주입에서는 객체 생성, 의존관계 주입이 하나의 단계에서 일어나는 것이다.

![7](https://github.com/mo2-Study-Group/StudyGroup/assets/70151275/fdc66053-27a3-410d-bf9c-a0784651f6c4)

**스프링 컨테이너**는 **설정 정보를 참고해 의존관계를 주입**한다.

### 스프링 빈 이벤트 라이프사이클

먼저 **스프링 빈의 라이프사이클**을 보면,

**스프링 IoC 컨테이너 생성 → 스프링 빈 생성 → 의존관계 주입 → 초기화 콜백 메서드 호출 → 사용 → 소멸 전 콜백 메서드 호출 → 스프링 종료** 이렇게 **스프링은 의존관계 주입이 완료되면 스프링 빈에게 콜백 메서드를 통해 초기화 시점을 알려주며, 스프링 컨테이너가 종료되기 직전에도 소멸 콜백 메서드를 통해 소멸 시점을 알려준다**.

여기서 **객체 생성과 초기화를 분리하는 이유**는 생성자는 파라미터를 받고, 메모리를 할당 해서 객체를 생성하는 역할이고, **초기화**는 이렇게 **생성된 값들을 활용해 외부 커넥션을 연결하는 등 무거운 동작을 수행**한다. 그렇기 때문에 **생성자 안에서 동작들을 같이 하는 것 보다 명확하게 나누는 것이 유지보수 측면에서 좋다**. 물론 단순한 초기화 작업(내부 값들을 살짝 변경하는 작업)은 한번에 처리하는 게 더 낫다.

## 콜백 방법 3가지

- 인터페이스(InitializingBean, DisposableBean)
- 설정 정보에 초기화 메서드, 종료 메서드 지정
- `@PostConstruct`, `@PreDestroy` 어노테이션 지원

### 인터페이스**(InitializingBean, DisposableBean)**

```java
public class ExampleBean implements InitializingBean, DisposableBean {
 
    @Override
    public void afterPropertiesSet() throws Exception {
        // 초기화 콜백 (의존관계 주입이 끝나면 호출)
				System.out.println("시작한다~");
    }
 
    @Override
    public void destroy() throws Exception {
        // 소멸 전 콜백 (메모리 반납, 연결 종료와 같은 과정)
				System.out.println("끝났다!~");
    }
}
```

**InitializingBean**은 `afterPropertiesSet()` 메서드로 **초기화를 지원**한다.(의**존관계 주입이 끝난 후에 초기화 진행**)

**DisposableBean**은 `destroy()` 메서드로 **소멸을 지원**한다.(**빈 종료 전에 마무리 작업**)

- **단점**
    - 이 인터페이스는 스프링 전용 인터페이스라서 해당 인터페이스에 의존한다.
    - 초기화, 소멸 메서드의 이름을 변경하지 못한다.
    - 내가 코드를 고칠 수 없는 외부 라이브러리에 적용할 수 없다.
    - 스프링 초창기에 나온 방법이라서 지금은 거의 사용하지 않는다.

### 빈 설정 정보에 초기화 메서드, 종료 메서드 지정

```java
public class ExampleBean {
 
    public void initialize() throws Exception {
        // 초기화 콜백 (의존관계 주입이 끝나면 호출)
				System.out.println("시작한다~");
    }
 
    public void close() throws Exception {
        // 소멸 전 콜백 (메모리 반납, 연결 종료와 같은 과정)
				System.out.println("끝~");
    }
}
 
@Configuration
class LifeCycleConfig {
 
    @Bean(initMethod = "initialize", destroyMethod = "close")
    public ExampleBean exampleBean() {
        // ~~~~~~
    }
}
```

- **해당 방식의 장단점**
    - 메서드명을 자유롭게 쓸 수 있다.
    - 스프링 인터페이스(코드)에 의존하지 않는다.
    - 설정 정보를 사용하기 때문에 코드를 고칠 수 없는 외부 라이브러리에도 초기화, 종료 메서드를 적용할 수 잇다.
    - 단점으로는 Bean을 지정할 때 initMethod와 destoryMethod를 직접 지정해야 하기에 번거롭다.
- **@Bean의 destoryMehod 속성의 특징**
    - 라이브러리는 대부분 close, shutdown 이라는 종료 메서드를 사용한다.
    - `@Bean`의 destoryMethod는 기본 값이(inferred)(추론)으로 등록되어 있다.
    - 이 추론 기능은 close, shutdown 이라는 이름의 메서드를 자동으로 호출한다.
    - 그래서 직접 스프링 빈으로 등록하면 종료 메서드는 따로 작성하지 않아도 된다.
    - 이 기능을 쓰기 싫다면 `destoryMethod = “”`으로 빈 공백을 지정하면 된다.

### @PostConstruct, @PreDestory 어노테이션

```java
import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;
 
public class ExampleBean {
 
    @PostConstruct
    public void initialize() throws Exception {
        // 초기화 콜백 (의존관계 주입이 끝나면 호출)
    }
 
    @PreDestroy
    public void close() throws Exception {
        // 소멸 전 콜백 (메모리 반납, 연결 종료와 같은 과정)
    }
}
```

위 코드처럼 어노테이션만 작성하면 초기화와 종료를 실행할 수 있다.

- **장단점**
    - 최신 스프링에서 가장 권하는 방법이다.
    - 스프링 종속적인 기술이 아니라 JSR-250이라는 자바 표준이기 때문에 스프링이 아닌 다른 컨테이너에서도 동작한다.
    - 컴포넌트 스캔과 잘 어울린다.
    - 단점은 외부 라이브러리에는 적용하지 못한다. 외부 라이브러리를 초기화, 종료 해야하면 `@Bean` 기능인 메서드를 지정해야 한다.

**정리하자면**, `@PostConstruct`, `@PreDestroy` 어노테이션을 쓰자! **외부 라이브러리를 초기화, 종료 해야하면** `@Bean`의 **initMethod**, **destroyMethod**를 사용하자!


## 빈 스코프

**빈 스코프**란 **빈이 존재할 수 있는 범위**를 뜻한다. **빈이 애플리케이션이 구동되는 동안 하나만 생성해 쓸건지, http 요청마다 생성해서 쓸 것인지 등등을 결정**하는 것이다.

### 빈 스코프 종류

- **Singleton**: 스프링 컨테이너(애플리케이션)의 시작과 끝까지 유지되는 가장 넓은 범위 스코프(스프링 기본 제공)
- **프로토타입**: 프로토타입 빈의 생성과 의존관계 주입까지만 관여하고, 더는 관리 안하는 짧은 범위의 스코프(매번 사용할 때 마다 만듬)(스프링 기본 제공)
- **웹 관련 스코프(Spring web 모듈에서 제공하는 scope 방식)**
    - request: http 요청이 들어오고 나갈 때 까지 유지되는 스코프
    - session: 웹 세션이 생성되고 종료될 때 까지 유지되는 스코프
    - application: 웹 서블릿 컨텍스트와 같은 범위로 유지되는 스코프
    - websocket: 웹 소켓 라이프사이클 동안 유지되는 스코프

### 스코프 지정

스코프를 지정하는 방법은 `@Scope(”종류”)`로 지정한다.

```java
@Scope("prototype")
@Component
public class Bean {}//자동 등록

@Scope("prototype")
@Bean
PrototypeBean HelloBean() {//수동 등록
    return new HelloBean();
}
```

### 싱글톤 빈

- 생성된 하나의 인스턴스는 **Spring Beans Cache**에 저장되고, 해당 빈에 대한 요청과 참조가 있으며 **캐시된 객체를 반환**한다. 하나만 생성되기 때문에 **동일 참조를 보장**한다.
- 기본 스코프는 **싱글톤**이다.
- **같은 요청이 와도 같은 객체 인스턴스의 스프링 빈을 반환**한다.
- 싱글톤 타입으로 적합한 객체
    - 상태가 없는 공유 객체
    - 읽기 전용 상태인 객체
    - 쓰기가 가능한 상태를 가지면서 사용 빈도가 매우 높은 객체 → 이때는 동기화 전략이 필요

![8](https://github.com/mo2-Study-Group/StudyGroup/assets/70151275/1c67e6f3-1932-415e-b1bd-3ff4857fa34b)

### 프로토타입

- 종료 콜백 메서드가 호출되지 않는다.(`@PreDestory` **호출X**)
- **DI가 발생**할 때 마다(스프링 컨테이너에 요청할 때 마다) **새로운 객체가 생성되어 주입**된다.
- **스프링 컨테이너**는 **프로토타입 빈의 생성과 의존관계 주입, 초기화까지만 관여**한다.
- 해당 빈을 조회한 **클라이언트가 관리**해야하고, 종료 콜백 메서드로 **클라이언트가 직접**해야 한다.

![9](https://github.com/mo2-Study-Group/StudyGroup/assets/70151275/4a7649f6-e730-4799-97c7-99ef2950174f)

![10](https://github.com/mo2-Study-Group/StudyGroup/assets/70151275/512354e3-e7c6-4cda-a25b-d62ee310f7eb)

### 싱글톤과 프로토타입 빈을 같이 사용할 때 생기는 문제

![11](https://github.com/mo2-Study-Group/StudyGroup/assets/70151275/81cc35b1-abbe-4f76-ad93-68eca782ba1d)

이렇게 clientBean 안에 prototypeBean을 포함하면 스프링 컨테이너 생성 시점에 함께 생성되고, DI도 발생한다.

clientBean은 의존관계 자동주입을 하면서 prototypeBean을 스프링 컨테이너에게 요청하고 생성해서 이 싱글톤 빈에게 반환한다. 이때 count 값이 0인데,

![12](https://github.com/mo2-Study-Group/StudyGroup/assets/70151275/4f329378-4648-4c51-be5e-6b35ac1c3b10)

클라이언트 A가 clientBean.logic()을 호출하면 clientBean은 prototypeBean의 addCount()를 호출해서 프로토타입 빈의 count를 증가 시킨다. count 값이 1이 된다.

![13](https://github.com/mo2-Study-Group/StudyGroup/assets/70151275/f677be2f-563b-48a7-b777-705cd7f0099e)

클라이언트 B도 logic()을 호출하면 프로토타입 빈이 컨테이너에 소멸되지 않고, 해당 프로토타입 인스턴스를 가지고 있게 된다. 싱글톤 빈 안에서는 다른 결과를 가져온다.

### 해결법

1. **provider**

javax.inject.Provider이라는 JSR-330 자바 표준을 사용하는 방법.

이 방법을 사용하려면 스프링 부트 3.0 미만 ‘javax.inject:javax.inject:1’ 라이브러리를 스프링 부트 3.0 이상은 jakarta.inject-api:2.0.1를 gradle에 추가하면 된다.

```java
dependencies {
   implementation 'javax.inject:javax.inject:1'
   ...
}
```

```java
//스프링 부트 3.0 미만
public interface Provider<T> {
 T get();
}

//스프링 부트 3.0
@Autowired
private Provider<PrototypeBean> provider;
public int logic() {
    PrototypeBean prototypeBean = provider.get();
    prototypeBean.addCount();
    int count = prototypeBean.getCount();
    return count;
}
```

`logic()`메서드를 호출할 때 마다 다른 PrototypeBean 인스턴스가 호출된다. 

**provider**는 자바 표준이라서 **스프링에 독립적**이라는 장점이 있다.

2. **proxy mode 사용**

```java
@Component
@Scope(value = "prototype", proxyMode = ScopedProxyMode.TARGET_CLASS)
public class ProtoType {
}

@Component
@AllArgsConstructor
public class ScopeWrapper {
    ...

    @Getter
    ProtoProxy protoProxy;
}
```

Prototype에 proxyMode 설정을 추가한다.

**프록시 적용 대상**이 **클래스**면 `TARGET_CLASS`, **인터페이스**면 `INTERFACE`를 선택한다.

### 웹 스코프

**웹 환경에서만 동작하는 스코프**이며, 프로토 타입과 달리 **특정 주기가 끝날 때까지 관리** 해준다. 그래서 **소멸 콜백 메서드가 호출**된다.

**종류는 4가지**가 있다.

- **Request**
    - HTTP 요청 하나가 들어오고 나갈 때까지 유지되는 스코프
    - 각각의 HTTP 요청마다 별도의 빈 인스턴스가 생성되고 관리된다.
- **Session**
    - HTTP Session과 동일한 생명 주기를 가지는 스코프
- **Application**
    - 서블릿 컨텍스트와 동일한 생명 주기를 가지는 스코프
- **WebSocket**
    - 웹 소켓과 동일한 생명 주기를 가지는 스코프

**Request만 살펴보자.**

이 나머지도 범위만 다르지 동작 방식은 비슷하다고 한다.

- 동작 방식

![14](https://github.com/mo2-Study-Group/StudyGroup/assets/70151275/6bbd433c-5c89-4762-a6ba-ce40d24d0107)

라이브러리 추가

```java
implementation : 'org.springframework.boot:spring-boot-starter-web'
```

예를 들어 우리가 MyLogger 라는 로그를 찍는 클래스를 Request Scope로 등록했고, A가 요청을 보냈다고 가정하고, 컨테이너에서 myLogger 객체를 요청 받았다면, 스프링 컨테이너는 A 전용으로 사용할 수 있는 빈을 생성하여 컨트롤러에 주입 해준다.

로직이 진행되면서 서비스에서 다시 myLogger 객체가 필요해서 요청을 하게 되면 방금 A 전용으로 생성했던 빈을 그대로 활용해서 주입 받을 수 있다. 이후 요청이 끝나면 Request 빈은 소멸된다.

만약 다른 클라이언트 B가 A와 동시에 요청을 보낸다면, 클라이언트 B도 역시 컨트롤러와 서비스에서 각각 myLogger 객체가 필요한데, 이 때는 클라이언트 A에게 주입해  주었던 빈이 아닌 새로 생성해서 주게 된다. 따라서 Request Scope를 활용하면 디버깅하기 쉬운 로그 환경을 만들 수 있다.

### 스프링 빈은 Thread-Safe 한가?

**Thread-safe**란?

싱글 쓰레드에서 한 개의 쓰레드가 객체를 쓰지만 멀티 쓰레드 환경에서는 쓰레드들이 객체를 공유해서 작업해야 하는 경우가 있다. 이렇게 공유 자원으로 쓰이는 영역을 쓰레드가 동시에 접근하면 안되는 영역을 **임계 영역**이라고 한다.

이 문제를 해결하기 위해 나온 개념이 **세마포어(Semaphore), 상호 배제(Mutex)** 등이 있다.

- **세마포어(Semaphore)**: 공유된 자원의 데이터를 여러 프로세스가 접근하는 것을 막는 것
- **뮤텍스(Mutex)**: 공유된 자원의 데이터를 여러 쓰레드가 접근하는 것을 막는것

```java
public class Singleton {

    private static Singleton instance = new Singleton();

    private Singleton() {
    }

    public static Singleton getInstance() {
        return instance;
    }
}
```

**싱글톤 패턴**은 **인스턴스가 한번 초기화 하면 애플리케이션이 종료될 때 까지 메모리에 있다**. 만약 **싱글톤이 상태를 갖게 되면 멀티 쓰레드 환경에서 동기화 문제가 발생**한다.

그런데 우리가 쓰는 싱글톤 빈은 사용할 때 static 변수, private 생성자, static 메서드를 정의하지 않고 싱글톤으로 쓴다.

그래서 싱글톤 빈은 상태를 가져도 Thread-safe(동기화)할 것이라고 착각을 하는 경우가 있다.

정리하자면, 스프링은 싱글톤 레지스트리(자세한건 밑에서)를 통해 private 생성자, static 변수 등의 코드 없이 비즈니스 로직에 집중하고 테스트 코드에 용이한 싱글톤 객체를 제공해 주는 것 뿐이지, **동기화 문제는 개발자가 처리**해야 한다. 그래서 **여러 쓰레드가 동시에 이 인스턴스를 접근할 경우(멀티 쓰레드 환경) 이슈가 발생**한다.

### ThreadLocal

이 객체로 멀티 쓰레드 환경 이슈를 해결할 수 있다.

- **ThreadLocal**은 **쓰레드만 접근할 수 있는 특벽한 저장소**이다.
- 여러 쓰레드가 접근하더라도 ThreadLocal은 쓰레드들을 식별해 각각의 쓰레드 저장소를 구분한다.
    - 즉, ThreadLoacl 변수를 선언하면 멀티 쓰레드 환경에서 각 쓰레드마다 독립적인 변수를 가지고 접근할 수 있다.
    따라서 같은 인스턴스의 ThreadLocal 필드에 여러 쓰레드가 접근하더라도 상관이 없다.
- 대표적인 메서드는 `get()` 메서드로 조회, `set()` 메서드로 저장, `remove()` 메서드로 저장소 초기화가 있다.

### ThreadLocal이 적용 되지 않고 동시성 문제가 발생하는 경우

- **ExampleService.class**

```java
@Slf4j
public class ExampleService {

    private Integer numberStorage;

    public Integer storeNumber(Integer number) {
        log.info("저장할 번호: {}, 기존에 저장된 번호: {}", number, numberStorage);
        numberStorage = number;
        sleep(1000); // 1초 대기
        log.info("저장된 번호 조회: {}", numberStorage);

        return numberStorage;
    }

    private void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

- **ExampleServiceTest.class**

```java
@Slf4j
public class ExampleServiceTest {

    private ExampleService exampleService = new ExampleService();

    @Test
    void field() {
        log.info("main start");

        Runnable storeOne = () -> {
            exampleService.storeNumber(1);
        };
        Runnable storeTwo = () -> {
            exampleService.storeNumber(2);
        };

        Thread threadA = new Thread(storeOne);
        threadA.setName("thread-1");
        Thread threadB = new Thread(storeTwo);
        threadB.setName("thread-2");

        threadA.start();
        sleep(100); // 동시성 문제 발생
        threadB.start();

        sleep(3000); // 메인 쓰레드 종료 대기

        log.info("main exit");
    }

    private void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

실행을 하면,

![tr1](https://github.com/mo2-Study-Group/StudyGroup/assets/70151275/350b2612-e99d-4a18-af19-45166c586be9)

이처럼 동시성 문제가 발생하는 것을 확인할 수 있다.

thread-1이 2초 동안 대기중에 thread-2가 numberStorage에 2를 저장하여 thread-1에서도 2가 조회되는 것을 확인할 수 있다.

### ****ThreadLocal이 적용되어 동시성 문제가 해결되는 예제****

- **ThreadLocalExampleService.class**

```java
@Slf4j
public class ThreadLocalExampleService {

    private ThreadLocal<Integer> numberStorage = new ThreadLocal<>();

    public Integer storeNumber(Integer number) {
        log.info("저장할 번호: {}, 기존에 저장된 번호: {}", number, numberStorage.get());
        numberStorage.set(number);
        sleep(1000); // 1초 대기
        log.info("저장된 번호 조회: {}", numberStorage.get());

        return numberStorage.get();
    }

    private void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

- **ThreadLocalExampleServiceTest.class**

```java
@Slf4j
public class ThreadLocalExampleServiceTest {

    private ThreadLocalExampleService exampleService = new ThreadLocalExampleService();

    @Test
    void field() {
        log.info("main start");

        Runnable storeOne = () -> {
            exampleService.storeNumber(1);
        };
        Runnable storeTwo = () -> {
            exampleService.storeNumber(2);
        };

        Thread threadA = new Thread(storeOne);
        threadA.setName("thread-1");
        Thread threadB = new Thread(storeTwo);
        threadB.setName("thread-2");

        threadA.start();
        sleep(100); // 동시성 문제 발생
        threadB.start();

        sleep(3000); // 메인 쓰레드 종료 대기

        log.info("main exit");
    }

    private void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

실행을 하면 동시성 이슈가 해결된 것을 볼 수 있다.

![tr2](https://github.com/mo2-Study-Group/StudyGroup/assets/70151275/027e5671-7e03-44f1-9989-2602cfd4de6e)

**ThreadLocal 주의 해야할 점**

- **ThreadLocal**을 도입하면 **동시성 이슈를 해결**할 수 있지만, **메모리 누수를 일으켜 큰 장애를 야기**할 수 있다.
- **톰캣 같은 WAS**의 경우 **쓰레드를 새로 생성하는데 비용이 크기** 때문에 **자체적으로 ThreadPool을 가지고 있으면서 쓰레드를 재사용**한다.
→ 그래서 ThreadLocal은 쓰레드가 반환될 때 remove() 메서드를 통해 반드시 초기화 해줘야 한다.
    - 구현한 로직의 마지막에 초기화를 진행하거나, WAS에 반환될 때 인터셉터 혹은 필터 단에서 초기화하는 방법으로 진행한다.

## 싱글톤 레지스트리

예시) `@applicationContext`

스프링 싱글톤 레지스트리에서 다루는 싱글톤은 일반적인 싱글톤 디자인 패턴과 다른 방법으로 구현된다. 일반 싱글톤 디자인 패턴에서의 많은 단점을 해소한 버전이다.

### 싱글톤 방식을 쓰는 이유

스프링 프레임워크가 동작하는 환경이 대부분 서버 환경인데, 서버는 수 많은 오브젝트를 이용해서 사용자의 요청을 처리해준다. 요청을 초당 수백번 씩 받아야하는 경우도 있고, 대규모 시스템은 더 심한 경우도 있다.

이럴 때 마다 매번 오브젝트를 새로 만들면서 사용하면 비용이 너무 많이 들기 때문에, **서블릿(서비스 오브젝트)**를 사용하는데, 이게 **대부분 멀티 쓰레드 환경에서 싱글톤으로 동작**한다. 그래서 **서버 환경에서는 싱글톤 사용이 권장**된다.

### 싱글톤 패턴의 단점

싱글톤 패턴을 구현하는 방법

```java
public class UserSingleton {
    private static UserSingleton INSTANCE;
    
    private UserSingleton () {
    }
    
    public static synchronized UserSingleton getInstance() {
        if(INSTANCE == null) INSTANCE = new UserSingleton ();
        return INSTANCE;
    }
}
```

위 코드는 싱글톤 구현의 예시이다.

- **싱글톤의 단점**
    - private 생성자로 인해 상속이 불가능하다.
        - 상속이 불가능하여 객체지향의 특징을 활용할 수 없다.
    - 싱글톤 패턴은 테스트하기가 힘들다.
        - 만들어지는 방식이 제한적이어서 mock 오브젝트 등으로 대체하기 힘들다. 필요한 오브젝트는 직접 오브텍트를 만들어 사용할 수 밖에 없다.
        - 이런 경우 테스트용 오브젝트로 대체하기가 힘들다.
    - 서버 환경에서는 싱글톤이 하나만 만들어지는 것을 보장하지 못한다.
        - 여러 개의 JVM에 분산되어 설치가 되는 경우에도 각각 독립적으로 오브젝트가 생기기 때문에 싱글톤으로서의 가치를 보장하지 못한다.
    - 싱글톤의 사용은 전역 상태를 만들 수 있기 때문에 바람직하지 못하다.
        - static 필드와 메서드를 사용하며 싱글톤을 static 메서드를 통해 어느 곳에서든 접근할 수 있다.
        → 객체지향 프로그래밍에서는 권장되지 않는 프로그래밍 모델이다.

이러한 문제점 때문에 **스프링**은 **싱글톤 레지스트리를 제공해서 직접 싱글톤 형태의 오브젝트를 만들고 관리**할 수 있게 되었다.

**싱글톤 레지스트리를 정리하자면,**

private 생성자를 사용해야 하는 방법이 아닌 **평범한 자바 클래스를 싱글톤으로 활용**할 수 있게 해준다.

**스프링이 지지하는 객체지향적인 설계 방식의 원칙, 디자인 패턴 등을 적용하는데 아무런 제약이 없게** 만들어준다.

- **주의 할점**
    - 멀티 쓰레드 환경에서 여러 쓰레드가 싱글톤에 동시 접근 할 수 있기 때문에 상태관리를 해야한다.
        - 대부분 stateless(무상태)하게 만들어져야 한다.
        - 내부 상태 값이 동시수정이 이루어지는 경우 매우 위험하기 때문이다.(읽기 전용은 제외)
    - 관리 방법
        - 요청 정보, DB 서버 리소스로부터 얻은 정보 등은 파라미터, 로컬 변수, 리턴 값 등을 이용한다.
        → 스택 영역에 독립적으로 저장이 되기 때문에 쓰레드마다 분리되어 있다.
        - 자신이 사용하는 다른 싱글톤 빈을 저장하려는 용도라면 인스턴스 변수를 사용해도 무관하다.

**참고 자료**
- [https://atoz-develop.tistory.com/entry/Spring-스프링-빈Bean의-개념과-생성-원리](https://atoz-develop.tistory.com/entry/Spring-%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B9%88Bean%EC%9D%98-%EA%B0%9C%EB%85%90%EA%B3%BC-%EC%83%9D%EC%84%B1-%EC%9B%90%EB%A6%AC)
- <https://okimaru.tistory.com/114>
- [https://mimah.tistory.com/entry/Spring-스프링-빈을-등록하는-두-가지-방법](https://mimah.tistory.com/entry/Spring-%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B9%88%EC%9D%84-%EB%93%B1%EB%A1%9D%ED%95%98%EB%8A%94-%EB%91%90-%EA%B0%80%EC%A7%80-%EB%B0%A9%EB%B2%95)
- <https://tecoble.techcourse.co.kr/post/2023-05-22-configuration/>
- <https://mangkyu.tistory.com/234>
- <https://multifrontgarden.tistory.com/253>
- <https://docs.spring.io/spring-framework/reference/core/beans/factory-scopes.html>
- <https://steady-coding.tistory.com/594>
- [https://velog.io/@probsno/Bean-스코프란](https://velog.io/@probsno/Bean-%EC%8A%A4%EC%BD%94%ED%94%84%EB%9E%80)
- <https://spongeb0b.tistory.com/513>
- <https://yeonbot.github.io/java/ThreadLocal/>
- [https://velog.io/@jakeseo_me/토비의-스프링-정리-프로젝트-1.6-싱글톤-레지스트리와-오브젝트-스코프](https://velog.io/@jakeseo_me/%ED%86%A0%EB%B9%84%EC%9D%98-%EC%8A%A4%ED%94%84%EB%A7%81-%EC%A0%95%EB%A6%AC-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-1.6-%EC%8B%B1%EA%B8%80%ED%86%A4-%EB%A0%88%EC%A7%80%EC%8A%A4%ED%8A%B8%EB%A6%AC%EC%99%80-%EC%98%A4%EB%B8%8C%EC%A0%9D%ED%8A%B8-%EC%8A%A4%EC%BD%94%ED%94%84)
- <https://yjksw.github.io/spring-singleton-registry/>
