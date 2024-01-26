# HTTP **요청 메시지** - JSON

**RequestBodyJsonController**

```java
@Slf4j
@Controller
public class RequestBodyJsonController {

    private ObjectMapper objectMapper = new ObjectMapper();

    @PostMapping("/request-body-json-v1")
    public void requestBodyJsonV1(HttpServletRequest request, HttpServletResponse response) throws IOException {
        ServletInputStream inputStream = request.getInputStream();
        String messageBody = StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8);

        log.info("messageBody={}", messageBody);
        HelloData helloData = objectMapper.readValue(messageBody, HelloData.class);
        log.info("username={}, age={}", helloData.getUsername(), helloData.getAge());

        response.getWriter().write("ok");
    }
}
```

`HttpServletRequest`를 사용해서 직접 HTTP 메시지 바디에서 데이터를 읽어와서 문자로 변환한다.

문자로 된 JSON 데이터를 jackson 라이브러리인  `objectMapper`를 사용해서 자바 객체로 변환한다.

**requestBodyJsonV2 - @RequestBody 문자 변환**

```java
@PostMapping("/request-body-json-v2")
public String requestBodyJsonV2(@RequestBody String messageBody) throws IOException {

   log.info("messageBody={}", messageBody);
   HelloData helloData = objectMapper.readValue(messageBody, HelloData.class);
   log.info("username={}, age={}", helloData.getUsername(), helloData.getAge());

   return "ok";
}
```

`@RequestBody`를 사용해서 HTTP 메시지에서 데이터를 꺼내고 `messageBody`에 저장한다.

문자로 된 JSON 데이터인 `messageBody`를 `objectMapper`를 통해 자바 겍체로 변환한다.

문자로 변환하고 다시 json으로 변환하는 과정이 불편한 것 같다.

`@ModelAttribute`처럼 한번에 객체로 변환할 수 없는지 보자.

**requestBodyJsonV3 - @RequestBody 객체 변환**

```java
@PostMapping("/request-body-json-v3")
public String requestBodyJsonV3(@RequestBody HelloData helloData) {
    log.info("username={}, age={}", helloData.getUsername(), helloData.getAge());
    return "ok";
}
```

**@RequestBody 객체 파라미터**

- `@RequestBody HelloData data`
- `@RequestBody` 에 직접 만든 객체를 지정할 수 있다.

`HttpEntity`, `@RequestBody`를 사용하면 HTTP 메시지 컨버터가 HTTP 메시지 바디의 내용을 우리가 원하는 문자나 객체 등으로 변환해준다.

HTTP 메시지 컨버터는 문자 뿐만 아니라 JSON도 객체로 변환해주는데, V2에서 했던 작업을 대신 처리해준다.

**@RequestBody는 생략 불가능**

스프링은 `@ModelAttribute`, `@RequestParam`과 같은 해당 어노테이션을 생략시 아래와 같은 규칙을 적용한다.

- `String`, `int`, `Integer` 같은 단순 타입 = `@RequestParam`
- 나머지 = `@ModelAttribute` (argument resolver 로 지정해둔 타입 외)

따라서 이 경우 `HelloData`에 `@RequestBody`를 생략하면 `@ModelAttribute`가 적용되어 버린다.

`HelloData data` → `@ModelAttribute HelloData data`

따라서 생략하면 HTTP 메시지 바디가 아니라 요청 파라미터를 처리하게 된다.

**requestBodyJsonV4 - HttpEntity**

```java
@ResponseBody
@PostMapping("/request-body-json-v4")
public String requestBodyJsonV4(HttpEntity<HelloData> httpEntity) {HelloData data = httpEntity.getBody();
		log.info("username={}, age={}", data.getUsername(), data.getAge());
    return "ok";
}
```

**requestBodyJsonV5**

```java
@ResponseBody
@PostMapping("/request-body-json-v5")
public HelloData requestBodyJsonV5(@RequestBody HelloData data) {
    log.info("username={}, age={}", data.getUsername(), data.getAge());
    return data;
 }
```

`@ResponseBody`

응답의 경우에도 `@ResponseBody`를 사용하면 해당 객체를 HTTP 메시지 바디에 직접 넣어줄 수 잇다.

물론 이 경우에도 HttpEntity를 사용해도 된다.

- `@RequestBody` 요청
    - JSON 요청 → HTTP 메시지 컨버터 → 객체
- `@ResponseBody` 응답
    - 객체 → HTTP 메시지 컨버터 → JSON 응답

---

**참고 자료**

- [https://www.inflearn.com/course/스프링-mvc-1/dashboard](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard)
