# CORS 이슈

## SOP(Same- origin policy)

CORS를 알려면 SOP가 뭔지 알아야 한다.

**SOP**란 같은 **Origin에만 요청을 보낼 수 있게 제한하는 보안 정책**을 의미한다.

**Origin 구성**은

- URI Schema(http, https)
- Hostname(localhost, naver.com)
- Port(80, 8080)

이중에 하나라도 구성이 다르면 SOP 정책에 걸려서 요청을 보낼 수 없다.

예시로 `http://www.example.com/dir/page.html` 요청을 보낼 때

![Untitled](https://github.com/Heo-y-y/development-blog/assets/112863029/ca06b821-4173-48b0-b32f-c91168193eca)

CORS는 서로 다른 Origin끼리 요청을 주고 받을 수 있게 정해둔 표준이라고 할 수 있다.

### 동작 원리

![Untitled1](https://github.com/Heo-y-y/development-blog/assets/112863029/c7cbd5a1-4585-4c90-afd1-af23c513c1b7)

1. 처음에 Get 요청인지 Post 요청인지 확인한다.
2. Content-Type과 Custom HTTP Header를 파악한다.
3. OPTIONS 요청을 통해 서버가 적절한 Access-Control를 가졌는지 확인한다.
4. 만약 적절한 Access-Contol를 가졌으면 실제 XHR을 트리거한다.
5. 적절하지 못한 Access-Contol을 가졌으면 Error를 발생시킨다.

이 과정에서 적절한 대처를 못했을 때 발생하는 것이 **CORS** 에러인 것이다.

## 해결방법

### Controller에 @CrossOrigin 추가

```java
@CrossOrigin(origins = "http://localhost:8080") // 추가
@RestController
public class MemberController {

  @GetMapping("/hello")
  public String hello() {
    return "안녕하세요?";
  }
}
```

해당 어노테이션은 Spring에서 CORS를 적용할 수 있게 만든 어노테이션이다.

Origin 속성으로 도메인을 설정하면, 해당 도메인은 다른 Origin을 가지고 있지만, 요청을 주고 받을 수 있게 된다.

### WebConfig에 CORS 설정

특정 도메인으로 오는 요청에는 모두 CORS를 적용하고 싶을 때 WebConfig를 설정하면 된다.

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

  @Override
  public void addCorsMappings(CorsRegistry registry) {
    registry.addMapping("/**")
        .allowedOrigins("http://localhost:8080");
  }
}
```

**WebMvcConfigurer**를 상속 받아서 **addCorsMappings**를 **Override**하면 **CORS 관련 설정이 가능**하다.

이 코드는 `http://localhost:8080`에서 오는 요청은 url이던 CORS를 적용 해주도록 설정해주는 것이다.

이렇게 하면 Controller에 `@CrossOrigin`을 안써도 된다.

### Proxy 만들기

서버에서 CORS 위반 한다고 서버에서 응답을 막는게 아니라

1. 클라이언트 - 서버에서 호출
2. 서버에서 응답
3. 브라우저에서 CORS 정책을 위반하면 응답 파기

이러한 과정으로 흘러간다.

즉, 요청을 보내는 쪽에서 프록시 서버를 만들어 간접적으로 전달하면 응답을 받을 수 있다.

```java
@Controller
public class CorsController {

  @GetMapping("/api/proxy")
  @ResponseBody
  public String proxy() {
    String url = "http://localhost:1000/hello";

    RestTemplate restTemplate = new RestTemplate();
    return restTemplate.getForObject(url, String.class);
  }
}
```

클라이언트와 같은 포트에서 API를 만든 후에, **RestTemplate**의 `getForObject(요청 url, 반환 클래스)`를 이용해 서버에 요청을 보내고 응답을 리턴해준다.

**참고 자료**

- <https://shinsunyoung.tistory.com/86>
- <https://evan-moon.github.io/2020/05/21/about-cors/>
- <https://wonit.tistory.com/572>
