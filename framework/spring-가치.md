# Spring의 가치

## Spring이란?


스프링 프레임워크는 Java 애플리케이션 개발을 위한 포괄적인 인프라 자원을 제공한다.

**Spring**에는 다음과 같은 모듈이 포함되어있다.

- Spring JDBC
- Spring MVC
- Spring Security
- Spring AOP
- Spring ORM
- Spring Test

이러한 모듈은 애플리케이션의 개발 시간을 대폭 단축시켜준다.

### Spring Framework의 주요 기능

- **DI(Dependency Injection)**
    
    DI란 **외부에서 두 객체 간의 관계를 결정해주는 디자인 패턴**으로, **인터페이스를 사이에 두고 클래스 레벨에서는 의존관계가 고정되지 않도록 하고 런타임 시에 관계를 동적으로 주입**하여 유연성을 확보하고 결합도를 낮출 수 있게 해준다.
    한 마디로 **의존성을 주입해서 객체 간 결합을 느슨하게 하는 것**이다.
    
    - **DI의 장점**
        - 두 객체 간의 관계라는 관심사의 분리
        - 두 객체 간의 결합도를 낮춤
        - 객체의 유연성을 높임
        - 테스트 작성을 용이하게 함
- **IoC(Invesion of Control)**
    
    IoC란 **컨트롤의 제어권이 개발자가 아닌 프에임워크가 대신해주는 것**을 말한다.
    Servlet이나 Bean 같은 코드를 개발자가 직접 작성하지 않고, 프레임워크가 대신 수행한다.
    
    예를 들어서, 내 차를 타고 도착지점까지 몰고 가는 것은 내가 직접 제어해서 운전하는 것이다.
    그런데 만약 술을 마셔서 집에 가기 위해 대리기사를 불러 대리기사가 대신 운전하는 것은 나의 제어권을 대리기사에게 넘겨주는 즉 **제어의 역전**이 되는 것이다.
    
    - **IoC의 장점**
        - 코드의 양을 줄일 수 있음
        - 클래스 간의 결합을 느슨하게 만듬
        - 테스트와 유지관리를 쉽게 해줌
- **AOP(Aspect Oriented Programming)**
    
    AOP란 **관점 지향 프로그래밍**이라고 불리는데, 관점 지향은 어떤 로직을 기준으로 **핵심적인 관점, 부가적인 관점으로 나누어서 보고 그 관점을 기준으로 모듈화**를 하는 것이다.
   
    즉, AOP는 **핵심기능을 제외한 부수적인 기능을 프레임워크가 제공**하는 것이다.
    예를들면 Spring 프로젝트에서 security를 적용하거나, logging 등을 추가하고 싶을 때 기존 비즈니스 로직을 건들지 않고 AOP로 추가할 수 있다.
    
- **중복 코드 제거**
    
    예를 들면 JDBC 같은 템플릿을 사용할 때 중복되는 코드도 많고 복잡한데, 이를 모두 제거한다.
    
- **다른 프레임워크와의 통합**
    
    Junit, Mockito와 같은 유닛 테스트 프레임워크와 통합이 간단하다.
    즉, 개발하는 프로그램의 질이 향상된다.
    

## Spring Boot란?

**Spring Boot**는 **Spring Framework의 모듈 중 하나**이다. 최소한의 구성 또는 거의 0에 가까운 구성으로 **독립 실행형 애플리케이션을 구축**하는 데 도움이 된다. 간단한 **Spring 앱**이나 **RESTful 서비스**를 개발하고자 할 때 유용하게 사용할 수 있다.

### Spring Boot의 주요 기능

- **독자적인 구성**
    
    Spring Boot는 많은 **공통 애플리케이션 구성 요소애 대해 미리 구성된 설정을 제공**하여 개발자가 작성해야 하는 **코드의 양을 줄여준다**.
    
- **Embedded server**
    
    개발자가 별도의 웹 서버를 배포할 필요 없이 애플리케이션을 독립 실행형 **jar**로 실행할 수 있는 Embedded server(Tomcat, jetty 또는 Undertow)가 포함되어 있다.
    
- **자동 구성**
    
    클래스 경로 및 기타 요인을 기반으로 데이터베이스 연결 및 메시지 대기열과 같은 **많은 구성 요소를 자동으로 구성**할 수 있다.
    
- **Actuator**
    
    Spring Boot Actuator는 상태 확인, 메트릭 등을 포함하여 애플리케이션을 모니터링하고 관리하기 위한 엔드포인트를 제공한다.
    
- **Developer tools**
    
    Spring Boot에는 자동 다시 시작, 코드 변경 핫스왑 등과 같은 개발자용 도구가 포함되어 있다.
    

## Spring MVC란?

**Spring MVC**는 **웹 애플리케이션 구축에 사용**되는 **Web MVC의 프레임워크**이다. 또한 다양한 기능에 대한 많은 구성 파일이 포함되어 있다. **HTTP 지향 웹 애플리케이션 프레임워크**이다.

### Spring MVC의 주요 기능

- **MVC architecture(Model-View-Controller)**
    
    Spring MVC는 MVC 패턴을 따르며 애플리케이션을 **model(데이터 및 비즈니스 로직), view(프레젠테이션 계층) 및 controller(요청을 처리하는 모델)로** 처리한다.
    
- **DispatcherServlet**
    
    요청 처리를 **중앙에 집중화**하고 애플리케이션을 통한 요청 흐름을 관리하는 **DispatcherServlet**이라는 프런트 컨트롤러를 사용한다.
    
- **HandlerMapping**
    
    **HandlerMapping**을 사용하여 들어오는 요청을 URL 패턴 및 기타 기준에 따라 컨트롤러 메서드에 매핑한다.
    
- **ViewResolver**
    
    ViewResolver를 사용하여 주어진 요청에 대한 적절한 view 템플릿을 찾는다.
    
- **Data binding**
    
    Spring MVC는 들어오는 **요청 매개변수를 java 객체로 또는 그 반대로 자동으로 변환**할 수 있는 데이터 바인딩 기능을 제공한다.
    
- **유효성 검사**
    
    Spring MVC에는 양식 데이터 및 기타 입력을 처리하기 전에 **유효성을 검사할 수 있는 유효성 검사 프레임워크가 포함**되어 있다.
    

## ****Spring Boot와 Spring의 차이점****
### Dependency

Spring은 dependency를 설정할 때 설정 파일이 길고, 모든 dependency에 대해 버전 관리도 하나하나 해줘야 한다.

아래는 **Spring**에서 web에 대한 dependency를 추가하는 코드다.

```java
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>5.3.5</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.5</version>
</dependency>
```

Spring과 달리 **Spring Boot**는 아래 코드처럼 하나의 종속성만 넣어주면 된다.

```java
implementation 'org.springframework.boot:spring-boot-starter-web'
```

다른 모든 종속성은 빌드 시간 동안 최종 아카이브에 자동으로 추가된다.
일반적으로 Spring Test, JUnit, Hamcrest 및 Mockito 라이브러리 세트를 사용한다. 만약 **Spring**으로만 사용할 때는 이러한 **모든 라이브러리를 종속성으로 추가**해야한다. 

하지만 **Spring Boot** 같은 경우에는 이러한 라이브러리를 자동으로 포함하도록 테스트하기 위한 스타터 종속성만 필요하다. 또한 **버전 관리도 자동**으로 해준다.

**Spring Boot**는 다양한 Spring 모듈에 대한 많은 스타터 **종속성**을 제공한다.

- spring-boot-starter-data-jpa
- spring-boot-starter-security
- spring-boot-starter-test
- spring-boot-starter-web
- spring-boot-starter-thymeleaf

### MVC 구성

Spring과 Spring Boot를 모두 사용하여 JSP 웹 애플리케이션을 만드는 데 필요한 구성을 살펴보자.

**Spring**은 디스패처 서블릿, 매핑 및 기타 지원 구성을 정의해야 한다. web.xml 파일이나 Initializer 클래스를 사용하여 해당 작업을 수행할 수 있다.

```java
public class MyWebAppInitializer implements WebApplicationInitializer {
 
    @Override
    public void onStartup(ServletContext container) {
        AnnotationConfigWebApplicationContext context
          = new AnnotationConfigWebApplicationContext();
        context.setConfigLocation("com.baeldung");
 
        container.addListener(new ContextLoaderListener(context));
 
        ServletRegistration.Dynamic dispatcher = container
          .addServlet("dispatcher", new DispatcherServlet(context));
         
        dispatcher.setLoadOnStartup(1);
        dispatcher.addMapping("/");
    }
}
```

또한 @Configuration 클래스에 @EnableWebMvc 주석을 추가하고 컨트롤러에서 반환된 viewResolver를 위해 ViewResolver를 정의해야한다.

```java
@EnableWebMvc
@Configuration
public class ClientWebConfig implements WebMvcConfigurer { 
   @Bean
   public ViewResolver viewResolver() {
      InternalResourceViewResolver bean
        = new InternalResourceViewResolver();
      bean.setViewClass(JstlView.class);
      bean.setPrefix("/WEB-INF/view/");
      bean.setSuffix(".jsp");
      return bean;
   }
}
```

이에 반해 **Spring Boot**는 웹 스타터를 추가한 후 작업을 수행하기 위해 몇 가지 속성만 작성하면 된다.

```java
spring.mvc.view.prefix=/WEB-INF/jsp/
spring.mvc.view.suffix=.jsp
```

위의 모든 Spring 구성은 **자동 구성**이라는 프로세스를 통해 Boot 웹 스타터를 추가하여 자동으로 포함된다.
즉, **Spring Boot**가 애플리케이션에 존재하는 종속성, 속성 및 빈을 살펴보고 이를 기반으로 구성을 활성한다는 것이다.

### Configuration

**Spring**의 경우 configuration 설정을 할 때도 엄청 길고, 모든 에노테이션 및 빈 등록 등을 설정해야 한다.

**Spring Boot**는 **application.properties** 파일이나 **application.yml** 파일에 설정만하면 된다.

아래는 **Spring**에서 Thymeleaf 템플릿을 사용할때 작성해야하는 코드다.

```java
@Configuration
@EnableWebMvc
public class MvcWebConfig implements WebMvcConfigurer {

    @Autowired
    private ApplicationContext applicationContext;

    @Bean
    public SpringResourceTemplateResolver templateResolver() {
        SpringResourceTemplateResolver templateResolver = 
          new SpringResourceTemplateResolver();
        templateResolver.setApplicationContext(applicationContext);
        templateResolver.setPrefix("/WEB-INF/views/");
        templateResolver.setSuffix(".html");
        return templateResolver;
    }

    @Bean
    public SpringTemplateEngine templateEngine() {
        SpringTemplateEngine templateEngine = new SpringTemplateEngine();
        templateEngine.setTemplateResolver(templateResolver());
        templateEngine.setEnableSpringELCompiler(true);
        return templateEngine;
    }

    @Override
    public void configureViewResolvers(ViewResolverRegistry registry) {
        ThymeleafViewResolver resolver = new ThymeleafViewResolver();
        resolver.setTemplateEngine(templateEngine());
        registry.viewResolver(resolver);
    }
}
```

하지만 **Spring Boot**에서 Thymeleaf를 사용하려면 아래와 같은 코드만 작성하면 된다.

```java
implementation 'org.springframework.boot:spring-boot-starter-thymeleaf
```

### Spring Boot의 AutoConfiguration

Spring과 달리 **Spring Boot**에는 **AutoConfiguration**이라는 것이 있다.

**Spring Boot**로 실행하는 애플리케이션을 만들면 애플리케이션 클래스에 `@SpringBootApplication`이라는 어노테이션이 있을 것이다.

![스크린샷 2023-07-19 오후 10 07 37](https://github.com/Heo-y-y/coffee-kiosk/assets/112863029/99eda6cc-eb83-4de9-90f7-e2a45d0a375e)

`@SpringBootApplication` 이 어노테이션을 지우고 프로그램을 실행시키면, 일반적인 Java 프로그램과 동일하게 실행된다. 해당 어노테이션 덕분에 **많은 외부 라이브러리, 내장 톰캣 서버 등이 실행**될 수 있었던 것이다.

`@SpringBootApplication` 에 들어가보면 아래와 같은 코드를 확인할 수 있다.

![스크린샷 2023-07-19 오후 10 09 13](https://github.com/Heo-y-y/coffee-kiosk/assets/112863029/4a2066a2-f48a-433e-87a6-b1dba9539d7b)

- **`@ComponentScan`**
    - `@Component`, `@Controller`, `@Repository`, `@Service`라는 어노테이션이 붙어있는 객체들을 스캔해 자동으로 **Bean**에 등록해준다.
- **`@EnableAutoConfiguration`**
    - `@ComponentScan` 이후 사전에 정의한 라이브러리들을 **Bean**에 등록해준다.

### ****Spring Security Configuration****

**Spring**은 애플리케이션에서 보안을 설정하기 위해 표준 **spring-security-web** 및 **spring-security-config** 종속성을 모두 필요로 한다.

아래 코드처럼 **SecurityFilterChain** 빈을 생성하고 `@EnableWebSecurity`를 사용하는 클래스를 추가해야 한다.

```java
@Configuration
@EnableWebSecurity
public class CustomWebSecurityConfigurerAdapter {
 
    @Autowired
    public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {
        auth.inMemoryAuthentication()
          .withUser("user1")
            .password(passwordEncoder()
            .encode("user1Pass"))
          .authorities("ROLE_USER");
    }
 
    @Bean
     public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http.authorizeRequests()
          .anyRequest().authenticated()
          .and()
          .httpBasic();
        return http.build();
    }
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

**Spring Boot**도 작동하려면 이러한 종속성이 필요하지만 아래 종속성만 정의하면 모든 관련 종속성이 클래스 경로에 자동으로 추가된다.

```java
implementation 'org.springframework.boot:spring-boot-starter-security'
```

### ****Application Bootstrap****

**Spring**과 **Spring Boot**에서 애플리케이션 부트스트래핑의 기본적인 차이점은 **서블릿**에 있다.
**Spring**은 부트스트랩 진입점으로 **web.xml** 또는 **SpringServletContainerInitializer**를 사용한다.

반면에 **Spring Boot**는 **Servlet 3** 기능만 사용하여 애플리케이션을 부트스트랩한다.

- **Spring Bootstraps**
    - Spring은 부트스트래핑의 레거시 **web.xml** 방식과 최신 **Servlet 3+** 방식을 모두 지원한다.
    - **단계별 web.xml 접근방식**
        1. 서블릿 컨테이너는 web.xml을 읽는다.
        2. web.xml에 정의된 DispatcherServlet은 컨테이너에 의해 인스턴스화된다.
        3. DispatcherServlet은 WEB-INF/{servletName}-servlet.xml을 읽어 WebApplicationContext를 생성한다.
        4. 마지막으로 DispatcherServlet은 애플리케이션 컨텍스트에 정의된 Bean을 등록한다.
    - **Servletc 3+ 접근 방식**
        1. 컨테이너는 ServletContainerInitializer를 구현하는 클래스를 검색하고 실행한다.
        2. SpringServletContainerInitializer는 WebApplicationInitializer를 구현하는 모든 클래스를 찾는다.
        3. WebApplicationInitializer는 XML 또는 `@Configuration` 클래스로 컨텍스트를 생성한다.
        4. WebApplicationInitializer는 이전에 생성된 컨텍스트로 DispatcherServlet을 생성한다.
- **Spring Boot Bootstraps**
    
    우선 **Spring Boot** 애플리케이션의 진입점은 `@SpringBootApplication` 주석이 달린 클래스이다.
    
    ```java
    @SpringBootApplication
    public class Application {
        public static void main(String[] args) {
            SpringApplication.run(Application.class, args);
        }
    }
    ```
    
    기본적으로 Spring Boot는 임베디드 컨테이너를 사용하여 애플리케이션을 실행한다. 이 경우 Spring Boot는 **public static void** 기본 진입점을 사용하여 임베디드 웹 서버를 시작한다.
    
    또한 응용 프로그램 컨텍스트에서 포함된 서블릿 컨테이너로의 **Servlet**, **Filter** 및 **ServletContextInitializer** 빈 바인딩을 처리한다.
    
    Spring Boot의 또 다른 기능은 구성 요소에 대한 Main-class의 동일한 패키지 또는 하위 패키지의 모든 클래스를 자동으로 스캔하는 것이다.
    
    그리고 Spring Boot는 외부 컨테이너에 웹 아카이브로 배포하는 옵션을 제공한다. 이 경우 **SpringBootServletInitializer**를 확장해야 한다.
    
    ```java
    @SpringBootApplication
    public class Application extends SpringBootServletInitializer {
        // ...
    }
    ```
    
    여기서 외부 서블릿 컨테이너는 웹 아카이브의 META-INF 파일에 정의된 Main-class를 찾고, **SpringBootServletInitializer**는 **Servlet**, **Filter** 및 **ServletContextInitializer** 바인딩을 처리한다.
    
    ### 패키징 및 배포
    
    두 프레임워크는 모두 **Maven** 및 **Gradle**과 같은 공통 패키지 관리 기술을 지원한다.
    하지만 배포와 관련해서는 두 프레임워크가 많이 다르다.
    
    **Spring**으로 개발한 애플리케이션의 경우, **war** 파일을 Web Application Server에 담아서 배포한다.
    
    하지만 **Spring Boot** 경우, Tomcat이나 Jetty와 같은 내장 WAS를 가지고 있어서 **jar** 파일로 간편하게 배포를 할 수 있다.
    
### 결론
    
정리하자면 **Spring**은 기존에 EJB를 대신해 **Java 애플리케이션을 더 쉽게 만들 수 있게** 해주고, **Spring Boot**는 Spring보다 **개발자가 좀더 개발에만 집중할 수 있도록 도와주는 프레임워크**인 것이다.
    

## ****Spring Boot와 Spring MVC의 차이점****

| Spring Boot | Spring MVC |
| --- | --- |
| Spring Boot는 적절한 기본값으로 Spring 기반 애플리케이션을 패키징하기 위한 Spring 모듈이다. | Spring MVC는 Spring 프레임워크 아래의 model, view, controller 기반 웹 프레임워크이다. |
| Spring 기반 프레임워크를 구축하기 위한 기본 구성을 제공한다. | 웹 애플리케이션을 구축하기 위해 즉시 사용할 수 있는 기능을 제공한다. |
| 구성을 수동으로 빌드할 필요가 없다. | 구성을 자동으로 빌드해야 한다. |
| deployment descriptor에 대한 요구 사항이 필요 없다. | deployment descriptor가 필요하다 |
| 상용구 코드를 피하고 종속성을 단일 단위로 래핑한다. | 각 종속성을 별도로 지정해야 한다. |
| 개발 시간을 줄이고 생산성을 높이는 데 도움이 된다. | 개발 시간이 더 걸린다. |

## Spring vs Spring MVC vs Spring Boot

일단 **Spring, Spring MVC** 및 **Spring Boot**는 모두 **Java 기반 애플리케이션을 구축하는 데 널리 사용되는 프레임워크**이다. 몇 가지 공통 기능과 모듈을 공유하지만 서로 다른 용도로 사용되며 다른 강점가 약점을 가지고 있다.

- **Spring**은 엔터프라이즈 애플리케이션 구축을 위한 광범위한 기능을 제공하는 프레임워크이다. 고도로 모듈화되고, 사용자 지정이 가능하며 확장 가능하지만 **다른 두 프레임워크보다 더 많은 구성 및 설정이 필요**하다.
- **Spring MVC**는 MVC패턴을 사용하여 웹 애플리케이션 구축을 지원하는 웹 프레임워크이다. 사용자 지정 및 확장이 가능하지만, **Spring Boot보다 더 많은 구성 및 설정이 필요**하다.
- **Spring Boot**는 독립 실행형 Spring 애플리케이션을 빠르게 생성하기 위한 독단적인 구성 및 자동 구성 접근 방식을 제공하는 도구이다. **다른 두 프레임워크보다 사용이 더 간단하고 빠르지만, 복작한 프로젝트에 맞게 사용자를 정의할 수 없다**.

### 결론

이러한 프레임워크 간의 선택은 프로젝트의 특정 요구 사항과 제약 조건에 따라 달라진다. **복잡한 엔터파이즈 애플리케이션의 경우 Spring**이 최선의 선택일 수 있고, 덜 **복잡한 소규모 프로젝트의 경우 Spring Boot**가 더 적합할 수 있다. **Spring MVC는 더 많은 제어 및 사용자 정의가 필요한 웹 애플리케이션**에 적합하다.

### 참고자료
- <https://www.codingninjas.com/studio/library/spring-vs-spring-boot-vs-spring-mvc>
- <https://www.javatpoint.com/spring-vs-spring-boot-vs-spring-mvc>
