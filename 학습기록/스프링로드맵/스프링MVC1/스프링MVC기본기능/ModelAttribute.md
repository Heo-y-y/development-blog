# HTTP **요청 파라미터** - @ModelAttribute

실제 개발을 하면 요청 파라미터를 받아서 필요한 객체를 만들고 그 객체에 값을 넣어주어야 한다.

```java
@RequestParam String username;
@RequestParam int age;

HelloData data = new HelloData();
data.setUsername(username);
data.setAge(age);
```

스프링은 이 과정을 완전히 자동화해주는 `@ModelAttribute` 기능을 제공한다.

먼저 요청 파라미터를 바인딩 받을 객체를 생성하자.

**HelloData**

```java
package hello.springmvc.basic;
import lombok.Data;

@Data
public class HelloData {
    private String username;
    private int age;
}
```

- `@Data`
    - `@Getter`, `@Setter`, `@ToString`, `@EqualsAndHashCode`, `@RequiredArgsConstructor`를 자동으로 적용해준다.

**@ModelAttribute 적용 - modelAttributeV1**

```java
@ResponseBody
@RequestMapping("/model-attribute-v1")
public String modelAttributeV1(@ModelAttribute HelloData helloData) {
    log.info("username={}, age={}", helloData.getUsername(), helloData.getAge());
    return "ok";
}
```

`HelloData` 객체가 생성되고, 요청 파라미터의 값도 모두 들어간다.

스프링 MVC는 `@ModelAttribute`가 있으면 다음과 같이 실행한다.

- `HelloData` 객체를 생성
- 요청 파라미터의 이름으로 `HelloData` 객체의 프로퍼티를 찾고, 해당 프로퍼티의 `setter`를 호출해서 파라미터 값을 바인딩
- ex) 파라미터 이름이 `username`이면, `setUsername()` 메서드를 찾아서 호출하면서 값을 입력한다.

**프로퍼티**

객체에 `getUsername()`, `setUsername()`메서드가 있으면, 이 객체는 `username` 이라는 프로퍼티를 가지고 있다.

`username` 프로퍼티의 값을 변경하면 `setUsername()`이 호출되고, `getUsername()`이 호출된다.

```java
class HelloData {
     getUsername();
     setUsername();
}
```

**바인딩 오류**

`age=abc` 처럼 숫자가 들어가야 할 곳에 문자를 넣으면 `BindException`이 발생한다.

**@ModelAttribute 생략 - modelAttributeV2**

```java
@ResponseBody
@RequestMapping("/model-attribute-v2")
public String modelAttributeV2(HelloData helloData) {
    log.info("username={}, age={}", helloData.getUsername(), helloData.getAge());
     return "ok";
}
```

`@ModelAttribute`는 생략할 수 있다.

스프링은 해당 생략시 아래와 같은 규칙을 적용한다.

- `String`, `int`, `Integer` 같은 단순 타입 = `@RequestParam`
- 나머지 = `@ModelAttribute`(agument resolver로 지정해둔 타입 외)

---

**참고 자료**

- [https://www.inflearn.com/course/스프링-mvc-1/dashboard](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard)
