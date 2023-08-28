# Filter, Interceptor, Argument Resolver

개발 하다보면 공통적으로 처리해야 할 업무들이 많다.

공통 업무에 관련된 코드를 페이지마다 작성한다면 중복 코드가 많아지게 되고, 프로젝트 단위가 커질수록 서버에 부하를 줄 수도 있으며, 소스 관리도 되지 않는다.

Spring은 공통적으로 여러 작업을 처리함으로써 중복된 코드를 제거 할 수 있는 기능들을 지원하고 있다.

- Filter(필터)
- Interceptor(인터셉터)
- AOP(관점 지향 프로그래밍)

Spring에서 사용하는  3가지 기능들은 모두 어떤 행동을 하기 전에 먼저 실행하거나, 실행한 후에 추가적인 행동 할때 사용되는 기능들이다.

- **전체 흐름**

![스크린샷 2023-08-28 오후 7 34 45](https://github.com/mo2-Study-Group/StudyGroup/assets/112863029/de403e15-8dfe-450a-9685-5a159a3c36a7)

## 필터란?

### 정의

**요청과 응답을 거른뒤 정제하는 역할**한다.

![스크린샷 2023-08-28 오후 7 35 06](https://github.com/mo2-Study-Group/StudyGroup/assets/112863029/38360534-041f-47a8-8e92-baf7ce4fd401)

스프링이 지원하는 기능이 아니라, **J2EE 표준 스펙**에 있는 기능으로 가장 앞단에 있는 **프론트 컨트롤러인 Dispatcher Servlet에 요청이 전달되기 전/ 후에 url 패턴에 맞는 모든 요청에 대해 부가 작업을 처리할 수 있는 기능을 제공**한다.

즉, 스프링 컨테이너가 아닌 **톰캣과 같은 웹 컨테이너에 의해 관리**가 되고, 스프링 범위 밖에서 처리 되는것이다.

필터는 주로 `@WebFilter` 어노테이션으로 등록할 수 있고, 스프링 시큐리티에서는 필터를 조립해서 계층적으로 구현할 수도 있다.

### 필터 메소드 종류

필터를 사용할려면 **javax.servlet의 Filter 인터페이스**를 구현 해야하며, 이러한 메서드를 가진다.

```java
public interface Filter {
 
    public default void init(FilterConfig filterConfig) throws ServletException {}
 
    public void doFilter(ServletRequest request, ServletResponse response,
            FilterChain chain) throws IOException, ServletException;
 
    public default void destroy() {}
```

1. `init()`
    - **필터 객체를 초기화**하고 **서비스에 추가**하기 위한 메소드다.
    - 웹 컨테이너가 1회 `init()`를 호출하여 필터 객체를 초기화하면 이후 요청들은 `doFilter()`를 통해 처리된다.
2. `doFilter()`
    - **url-pattern에 맞는 모든 HTTP 요청이 디스패처 서블릿으로 전달되기 전에 웹 컨테이너에 의해 실행**되는 메소드다.
    - doFilter의 파라미터로  **FilterChain**이 있는데, FilterChain의 doFilter 통해 다음 대상으로 요청을 전달할 수 있게 된다.
    - `chain.doFilter()`로 전, 후에 우리가 필요한 처리 과정을 넣어줌으로써 원하는 처리를 진행할 수 있다.
    
    	처리를 구현해 동작을 진행한후 받은 **FilterChain 파라미터**를 사용해 다음 대상으로 요청을 넘겨준다.
    
3. `destroy()`   
  	- **필터 객체를 제거**하고 **사용하는 자원을 반환**하기 위한 메소드.
    - 웹 컨테이너가 이걸 호출해 필터 객체를 종료하면 이후엔 doFilter에 의해 처리 되지 않는다.


## 인터셉터(Interceptor)

### 정의

**인터셉터**는 스프링이 제공하는 기술로써 **Dispatcher Servlet이 컨트롤러를 호출하기 전과 후에 요청을 가로채 요청과 응답을 참조하거나 가공할 수 있는 기능을 제공**한다.

![스크린샷 2023-08-28 오후 7 35 29](https://github.com/mo2-Study-Group/StudyGroup/assets/112863029/e6e4392e-c18e-4d96-b5af-ef152df63cfe)


웹 컨테이너에서 동작하는 필터와 달리 **인터셉터는 스프링 컨텍스트 내부에서 동작**한다.

디스패처 서블릿이 내부적으로 핸들러 매핑을 통해 컨트롤러를 찾도록 요청하는데, 그 결과로 **실행 체인**을 **반환**한다.

**실행 체인**은 **1개 이상의 인터셉터가 등록**되어 있다면 **순차적으로 인터셉터들을 거쳐 컨트롤러가 실행되도록 하고**, **인터셉터가 없다면 바로 컨트롤러를 실행**한다.

### 인터셉터의 메소드 종류

```java
public interface HandlerInterceptor {
 
	default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception {
 
		return true;
	}
 
	default void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
			@Nullable ModelAndView modelAndView) throws Exception {
	}
 
	default void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler,
			@Nullable Exception ex) throws Exception {
	}
```

1. `preHandle()`
    - **Controller가 호출되기 전**에 **실행**된다.
    - 컨트롤러 이전에 처리해야 하는 전처리 작업이나 요청 정보를 가공하거나 추가하는 경우에 사용할 수 있다.
2. `postHandle()`
    - **Controller가 호출된 후**에 **실행**된다.(View 렌더링 전)
    - 컨트롤러 이후에 처리해야 하는 후처리 작업이 있을 때 사용할 수 있다. 이 메소드는 컨트롤러가 반환하는 **ModelAndView** 타입의 정보가 제공되는데, 최근에는 RestAPI 기반의 컨트롤러를 만들면서 자주 사용되지 않는다.(`@RestController`)
3. `afterCompletion()`
    - **모든 뷰에서 최종 결과를 생성하는 일을 포함해 모든 작업이 완료된 후**에 **실행**된다.(View 렌더링 후)
    - 요청 처리 중에 사용한 리소스를 반환할 때 사용할 수 있다.
    - 반환값이 FALSE라면 컨트롤러로 요청을 하지 않는다.
        
        각 메서드의 반환값이 true이면 핸들러의 다음 체인이 실행되지만 false이면 중단하고 남은 인터셉터와 컨트롤러가 실행되지않는다.
        

## 필터와 인터센트 차이

가장 큰 차이는,

- 호출 시점
- **필터**는 **Web Application**에 등록하고, **인터셉터**는 **Spring Context**에 등록한다.
- 용도

![d3](https://github.com/mo2-Study-Group/StudyGroup/assets/70151275/2cd412a1-9a5d-4e43-bba3-67353c30a35f)

### Request, Response 객체 조작 가능 여부

**필터는 Request와 Response를 조작할 수 있지만, 인터셉터는 조작할 수 없다**.

```java
public class MyFilter implements Filter {
 
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
    throws IOException, ServletException {
        // 다른 request와 response를 넣어줄 수 있음
        chain.doFilter(request, response);
    }
}
```

필터가 다음 필터를 호출하기 위해선 **필터 체인**(다음 필터 호출)를 해줘야한다.

이때 request,response 객체를 넘겨 주므로, 우리가 원하는 request, response 객체를 넣어줄 수 있다.

인터셉터는 처리 과정이 필터와 다르다.

```java
public class MyInterceptor implements HandlerInterceptor {
 
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) 
    throws Exception {
        // Request, Response를 교체할 수 없고 boolean 값만 반환 가능
        return true;
    }
}
```

**디스패처 서블릿**이 여러 인터셉터 목록을 가지고 있고, 순차적으로 실행 시킨다.

그리고 true를 반환하면 다음 인터셉터가 실행되거나 컨트롤러로 요청이 전달되며, fasle가 반환되면 요청이 중단된다.

그렇기 때문에 다른 request, response객체를 넘겨줄 수 없다.

### 관리되는 컨테이너

![d4](https://github.com/mo2-Study-Group/StudyGroup/assets/70151275/d48a21dc-92ec-42b6-8bd2-93e3f9e5daeb)

**필터**는 **스프링 이전의 서블릿 영역에서 관리**되고, **인터셉터**는 **스프링 영역에서 관리된다**.

이로 인해 **필터는 스프링이 처리해주는 내용들을 적용 받을 수 없다**. 그래서 스프링에 의한 예외처리가 되지 않는다.

**그런데 DelegatingFilterProxy가 등장하면서 스프링에서 관리가 가능해졌는데, 일단 안되는걸로 치고 조금 있다 자세히 알려드립니다.**

### 스프링 예외 처리 여부

일반적으로 스프링을 사용하면 ControllerAdvice와 ExceptionHandler를 이용한 예외처리 기능을 주로 사용한다. 

예를 들면 MemberNotFoundException을 던질때 404Status로 응답을 반환하기 위해 이런 코드를 구현해서 활용하는데, 예외가 서블릿까지 전달되지 않고 처리된다.

```java
@RestControllerAdvice
public class GlobalExceptionHandler extends ResponseEntityExceptionHandler {

    @ExceptionHandler(MemberNotFoundException.class)
    public ResponseEntity<Object> handleMyException(MemberNotFoundException e) {
        return ResponseEntity.notFound()
            .build();
    }
    
    
}
```

그런데 필터는 스프링 앞의 서블릿 영역에서 관리 되기 때문에 스프링의 지원을 받을 수 없다. 그래서 **만약에 필터에서 MemberNotFoundException이 던져 졌으면, 에러가 처리되지 않고 서블릿까지 전달된다.** 

그래서 서블릿은 예외가 핸들링 되기를 기대했지만, 예외가 나와서 Exception을 만나는 상황이 되버린다. 그래서 내부에 문제가 있다고 판단해서 500 status로 응답을 반환한다. 이를 해결 할려면 필터에서 다음과 같이 응답 객체에 예외처리가 필요하다.

```java
public class MyFilter implements Filter {

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        HttpServletResponse servletResponse = (HttpServletResponse) response;
        servletResponse.setStatus(HttpServletResponse.SC_NOT_FOUND);
        servletResponse.getWriter().print("Member Not Found");
    }
}
```

### 용도

**필터의 사용 사례**

- 보안 및 인증/인가 관련 작업
- 모든 요청에 대한 로깅 또는 검사
- 이미지/데이터 압축 및 문자열 인코딩
- Spring과 분리 돼야 하는 기능들

필터는 기본적으로 스프링과 무관하게 전역적으로 처리해야 하는 작업들을 처리할 수 있다.

그리고 인터셉터보다 앞단에서 동작하기 때문에 보안 검사(XSS 방어 등)을 하여 올바른 요청이 아닐 경우 차단할 수 있다.

그러면 스프링 컨테이너까지 요청이 전달되지 못하고 차단되기때문에 안정성을 더욱 높일 수 있다.

필터는 이미지나 데이터의 압축, 문자열 인코딩과 같이 웹 어플리케이션에 전반적으로 사용되는 기능을 구현하기에 적당하다.

**인터셉터의 사용사례**

- 세부적인 보안 및 인증/인가 공통 작업
- API 호출에 대한 로깅 또는 검사
- Controller로 넘겨주는 정보(데이터)의 가공

인터셉터에서는 클라이언트의 요청과 관련해 전역적으로 처리해야 하는 작업들을 처리할 수 있다.

세부적으로 적용해야 하는 인증이나 인가와 같이 예를 들어,

특정 그룹의 사용자는 어떤 기능을 사용하지 못하는 경우가 있는데, 이런 작업들은 컨트롤러로 넘어 가기 전에 검사해야 하기 때문에 인터셉터가 처리하기에 적합하다.

인터셉터는 필터와 다르게 **HttpServletRequest**나 **HttpServletResponse** 등과 같은 **객체를 제공 받기 때문에 객체 자체를 조작할 수 없다**.

대신에 해당 **객체가 내부적으로 갖는 값은 조작 할 수 있어서 컨트롤러로 넘겨주기 위한 정보를 가공하기에 용이**하다.

예를 들면, JWT 토큰 정보를 파싱해서 컨트롤러에게 사용자의 정보를 제공하도록 가공할 수 있는 것.

그외에 여러 목적으로 API 호출에 대한 정보들을 기록해야 하는 상황에 

HttpServletRequest나 HttpServletResponse를 제공해주는 인터셉터가 클라이언트의 IP나 요청 정보들을 기록하기에 용이하다.

ex) 관리자만 접근할 수 있는 관리자 페이지에 접근하기 전에 관리자 **인증을 하는 용도로 활용**

### 정리

**공통점**은 **비즈니스 로직과 분리되어 특정 요구사항(보안,인증,인코딩 등)을 만족시켜야 할때 적용**한다.

**차이점**은 **필터**는 **특정 요청과 컨트롤러에 관계없이 전역적으로 처리해야 하는 작업이나 웹 어플리케이션에 전반적으로 사용되는 기능을 구현할 때 적용**.

**인터셉터**는 **클라이언트의 요청과 관련된 작업에 대해 추가적인 요구사항을 만족해야 할 때 적용**한다.

### 필터가 스프링에서 관리를 할수 있는 이유

과거엔 실제로 필터가 스프링 컨테이너에 의해 관리되지 않았기 때문에 필터를 빈으로 등록할 수도 없었고. 다른 빈을 주입 받을 수도 없었다. 

그런데 **DelegatingFilterProxy**가 나오면서 달라졌다.

### ****DelegatingFilterProxy와 SpringBoot의 등장****

**DelegatingFilterProxy 나온 이전**

서블릿 컨테이너에 의해 생성되어 서블릿 컨테이너에 등록이 된다. 그래서 스프링의 빈으로 등록도 안됐고, 빈을 주입받을 수 없었다.

그런데 필터에서도 DI같은 스프링 기술이 필요한 상황이 나오고 해서 스프링 개발자가 DelegatingFilterProxy를 개발하면서 필터도 스프링 빈을 주입 받을 수 있게 마련했는데,

**DelegatingFilterProxy 나온 이후**

Spring 1.2부터 DelegatingFilterProxy가 나오면서 서블릿 필터 역시 스프링에서 관리가 가능해졌다.

DelegatingFilterProxy는 서블릿 컨테이너에서 관리되는 프록시용 필터로써 우리가 만든 필터를 가지고 있다. 우리가 만든 필터는 스프링 컨테이너의 빈으로 등록되는데, 요청이 오면 DelegatingFilterProxy가 요청을 받아 스프링 빈(우리가 만든 필터)에게 요청을 위임한다.

정리 하면, 
1. Filter 구현체가 스프링 빈으로 등록된다.
2. ServletContext가 Filter 구현체를 갖는 DelegatingFilterProxy를 생성한다.
3. ServletContext가 DelegatingFilterProxy를 서블릿컨테이너에 필터로 등록한다.
4. 요청이 오면 DelegatingFilterProxy가 필터 구현체에게 요청을 위임해 필터 처리를  한다.

예를 들어 우리가 만든 필터를 등록하는 코드를 보면,

```java
@Component
public class MyFilter implements Filter {

   @Override
   public void doFilter(final ServletRequest request, final ServletResponse response, final FilterChain chain) throws ServletException, IOException {
      chain.doFilter(request, response);
   }

   @Override
   public void init(final FilterConfig filterConfig) throws ServletException {
      Filter.super.init(filterConfig);
   }

   @Override
   public void destroy() {
      Filter.super.destroy();
   }

}
```

이렇게 필터를 등록 해줄려면 **SpringBoot가 아니라 Spring에서는** 이렇게 **서블릿 컨텍스트에 addFilter로 추가 해줘야했다**.

```java
public class MyWebApplicationInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

   public void onStartup(ServletContext servletContext) throws ServletException {
      super.onStartup(servletContext);
      servletContext.addFilter("myFilter", DelegatingFilterProxy.class);//이렇게
   }
}
```

이 코드는 “myFilter”라는 빈을 찾아 DelegatingFilterProxy에 넣어주고, 서블릿 컨테이너에 myFilter대신 DelegatingFilterProxy를 등록하여 DelegatingFilterProxy를 통해 myFilter에 요청을 위임하라는 것이다.

Proxy가 add되는 곳은 ServletContext이고, 우리가 만든 필터는 스프링 컨테이너에 빈으로 먼저 등록된 후에 DelegatingFilterProxy에 감싸져 서블릿 컨테이너로 등록되는것이다.

이렇게 해서 우리가 만든 필터도 스프링 빈으로 등록되고, 스프링 컨테이너에서 관리 되기때문에 

빈 등록 및 빈의 주입까지 가능한 것이다.

## SpringBoot의 등장 이후

**SpringBoot**이면 **DelegatingFilterProxy도 필요없다.**

왜냐하면 **SpringBoot가 내장 웹 서버를 지원하면서 톰캣과 같은 서블릿 컨테이너까지 SpringBoot가 제어 가능**하기 때문이다.

그래서 필터 빈을 DelegatingFilterProxy로 감싸서 등록 안해도 된다. SpringBoot가 서블릿 필터의 구현체 빈을 찾으면 DelegatingFilterProxy 없이 바로 필터 체인에 필터를 등록해주기 때문이다.

이렇게 필터에서 doFilter 호출 전에 런타임 예외를 발생시켜서 로그를 출력시키면 이런 이력이 남는다. 

먼저 Spring 프레임워크에 DelegatingFilterProxy를 이용해 필터를 등록한 경우

```java
java.lang.RuntimeException
    at com.mangkyu.MyFilter.doFilter(MyFilter.java:17)
    at org.springframework.web.filter.DelegatingFilterProxy.invokeDelegate(DelegatingFilterProxy.java:358)
    at org.springframework.web.filter.DelegatingFilterProxy.doFilter(DelegatingFilterProxy.java:271)
    at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193)
    at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166)
```

보면 DelegatingFilterProxy가 doFilter로 요청을 받고, 이를 실제 구현체인 MyFilter로 위임(invokeDelegate)하는 것을 볼 수 있다.

그다음 SpringBoot에서 로그 확인했을때 DelegatingFilterProxy 관련 내용이 없다.

```java
at com.mangkyu.MyFilter.doFilter(MyFilter.java:17) [main/:na]
    at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189) [tomcat-embed-core-9.0.56.jar:9.0.56]
    at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162) [tomcat-embed-core-9.0.56.jar:9.0.56]
    at org.springframework.web.filter.RequestContextFilter.doFilterInternal(RequestContextFilter.java:100) [spring-web-5.3.15.jar:5.3.15]
    at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:117) [spring-web-5.3.15.jar:5.3.15]
    at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189) [tomcat-embed-core-9.0.56.jar:9.0.56]
    at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162) [tomcat-embed-core-9.0.56.jar:9.0.56]
```

## ****Spring ArgumentResolver****

### ****ArgumentResolver****

어떠한 요청이 컨트롤러에 들어왔을 때, 요청에 들어온 값으로부터 원하는 객체를 만들어 내는 일을 간접적으로 해줄 수 있다.

예를 들어 사용자가 jwt 토큰과 함께 로그인 요청을 보냈다 했을때, 우리는 이 토큰이 유효한지 검증을 거친후 토큰에 저장된 id를 꺼내 로그인 유저 객체로 만들어 내는 과정이 필요하다. 

예시 코드

```java
@GetMapping("/me")
public ResponseEntity<MemberResponse> getMemberOfMine(HttpServletRequest request) {
  String token = AuthorizationExtractor.extract(request);
  if(!jwtTokenProvider.isValidToken(token)) {
    throw new InvalidTokenException();
  }

  String id = jwtTokenProvider.getPayLoad(token);
  LoginMember loginMember = new LoginMember(Long.parseLong(id));

  MemberResponse memberResponse = memberService.findMember(loginMember.getId());
  return ResponseEntity.ok().body(memberResponse);
}
```

이렇게 토큰에 저장된 id를 꺼내 LoginMember라는 객체로 만들어내는 과정이 필요하다.

이럴때 ****ArgumentResolver를 사용하지 않는다면, 토큰을 검증하고 로그인 유저 객체로 변환하는 과정을 모든 컨트롤러마다 구현해야 한다.****

이렇게 되면 사용자 검증이 필요한 컨트롤러에 중복 코드가 생기고, 책임이 무거워진다. 

그리고 MemberController가 MemberService뿐만 아니라, JwtTokenProvider에도 의존해야 하기도 한다. 이러한 문제를 ArgumentResolver로 해결할수 있다.

## ****ArgumentResolver**** 사용

일단 `HandlerMethodArgumentResolver` 을 구현함으로써 시작된다.

```java
boolean supportsParameter(MethodParameter parameter);

@Nullable
Object resolveArgument(MethodParameter parameter, @Nullable ModelAndViewContainer mavContainer, NativeWebRequest webRequest, @Nullable WebDataBinderFactory binderFactory) throws Exception;
```

HandlerMethodArgumentResolver 인터페이스는 이 두가지 메소드를 구현하도록 되어 있는데, 

설명하자면, 우리는 원하는 ArgumentResolver가 실행되길 원하는 Parameter의 앞에 특정 어노테이션을 생성해 붙인다.

`supportsParameter` 는 요청 받은 메소드의 인자에 원하는 어노테이션이 붙어 있는지 확인하는 메소드. 포함하고 있으면 true 반환.(어떤 파라미터를 처리할 건지)

`resolveArgument` 는 `supportsParameter` 에서 true 받았을때(특정 어노테이션이 붙어 있는 어느 메소드가 있는경우) parameter가 원하는 형태로 정보를 바인딩하여 반환하는 메소드(어떤 값을 만들 건지)

이렇게 ****ArgumentResolver를 사용 했을때 Controller 구현 코드는 다음과 같다.****

```java
@GetMapping("/me")
  public ResponseEntity<MemberResponse> findMemberOfMine(@AuthenticationPrincipal LoginMember loginMember) {
      MemberResponse memberResponse = memberService.findMember(loginMember.getId());
      return ResponseEntity.ok().body(memberResponse);
  }
```

이렇게 하면 검증의 책임을 controller가 직접적으로 안가진다.

어노테이션만 붙여주면 유효한 토큰을 사용하는 것이 검증된 사용자가 필요한 정보(id)를 가지고 필요한 객체로 나오므로 편리하고 간단한 구현을 할 수 있다.

ex) `GET /users?name=park&page=1&size=20&sort=id,DESC`
라고 요청 시 Handler의 파라미터에 Pageable 객체를 넣어두면 page 관련 메타정보를 바탕으로 Pageable 객체를 만들어준다.

```java
@RestController
class UserController {
    
    @GetMapping("/user/{userId}")
    fun getUser(
        @PathVariable userId: Long <- 여기
    ) {
        // ...
    }
}
```

우리가 자주 쓰던 `@PathVariable`을 붙이면 path에서 자동으로 파라미터에 맵핑을 해주는게 이걸 **ArgumentResolver**가 해준다.(`@RequestParam`, `@RequestBody` 도 마찬가지)

(`@PathVariable`는 PathVariableMethodArgumentResolver.java에서 처리)

![스크린샷 2023-08-28 오후 7 36 23](https://github.com/mo2-Study-Group/StudyGroup/assets/112863029/510bab6f-4746-4ba6-88bf-13a58e39c6e3)


이렇게 핸들러 어댑터가 호출한다. 

핸들러 어댑터가 핸들러를 호출할 때 핸들러의 파라미터를 읽어 그에 해당하는 ArgumentResolver를 찾아서 호출한다.

### ArgumentResolver 추가하는 방법

스프링에서 대부분 추가 해놓았지만, 가끔 쓸일이 있다고 한다.

header에 token을 받아 UserInfoResponse를 만들어 주는 ArgumentResolver를 추가하는 걸로 예를 들면,

```java
@RestController
class UserController {
    
    @GetMapping("/user")
    fun getUser(
        user: UserInfoResponse << token 헤더를 읽어 UserInforResponse 생성
    ): UserInfoResponse {
        return user
    }
}
```

이렇게 추가할수 있다.

### 인터셉터와 *****Argument Resolver의 차이*****

![d6](https://github.com/mo2-Study-Group/StudyGroup/assets/70151275/ea0b89d9-dd90-4173-a25d-01454a6a4a97)
- **ArgumentResolver**는 **인터셉터 이후에 동작**을 하며, 어떠한 요청이 컨트롤러에 들어왔을 때, **청에 들어온 값으로부터 원하는 객체를 반환**한다.
- 반면, **인터셉터**는 **실제 컨트롤러가 실행되기 전에 요청을 가로채며**, **특정 객체를 반환할 수 없다**. 오직 boolean 혹은 void 반환 타입만 존재한다.

### 정리

1.**필터**

**Web Context 영역에 위치**하며,**요청,응답 객체를 다른 객체로 변경해 전달 할수 있다**.

용도는 스프링과 분리 되어야하는 기능을 적용 가능하게 해주며,

요즘은 스프링에서 제공하는 **DelegatingFilterProxy**로 프록시 필터 행태로 빈 등록가능한데,

**스프링 부트**로 내장 톰캣을 이용해 서블릿 컨테이너까지 스프링 부트가 제어하게 해줘서 **DelegatingFilterProxy감싸서 등록할 필요없다**.

1. **인터셉터**

필터와 비슷한데, **스프링 mvc가 제공하는 

**컨트롤러 호출 전에 호출되는데 클라이언트의 요청과 관련되어 전역적으로 처리해야 하는 작업들을 처리**할 수 있고, **검증 시 서비스 로직을 호출할 때 주로 사용**된다.

**차이점**

- 필터는 서블릿 컨테이너에서 실행, 인터셉터는 스프링 컨테이너 에서 실행.
- 필터는 디스패처 서블릿의 전후를 다룰 수 있고, 인터셉터는 컨트롤러의 전후를 다룰 수 있다.
- 그래서 필터는 스프링과 무관하게 전역적으로 처리해야 하는 작업을 하는 것이 좋고(보안 관련 공통 작업, 모든 요청에 대한 로깅)
- 인터셉터는 클라이언트의 요청과 관련되어 전역적으로 처리해야 하는 작업을 하는 것이 좋다.(인증,인가,API 호출에 대한 로깅,Controller로 넘겨주는 데이터 가공)

1. **Argument Resolver**

**Argument Resolver**는 주어진 요청을 처리할 때, **메서드 파라미터를 인자 값들에 주입해주는 전략 인터페이스**이다

어떠한 요청이 컨트롤러에 들어왔을 때, 요청에 들어온 값으로부터 원하는 객체를 반환한다.

인터셉터는 객체를 반환 못하기 때문에 그래서 인터셉터가 실행 된후에 Argument Resolver가 실행된다.


**참고자료**

- <https://dev-coco.tistory.com/173>

- <https://jaehoney.tistory.com/174>

- <https://mangkyu.tistory.com/173>

- <https://mangkyu.tistory.com/221>

- <https://tecoble.techcourse.co.kr/post/2021-05-24-spring-interceptor/>

- <https://pomo0703.tistory.com/65>
