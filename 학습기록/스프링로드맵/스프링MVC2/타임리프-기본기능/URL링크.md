# URL 링크

타임리프에서 URL을 생성할 때는 `@{…}` 을 사용하면 된다.

코드로 간단하게 살펴보자.

**BasicController**

```java
@GetMapping("/link")
public String link(Model model) {
    model.addAttribute("param1", "data1");
    model.addAttribute("param2", "data2");
    return "basic/link";
}
```

**link.html**

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<h1>URL 링크</h1>
<ul>
  <li><a th:href="@{/hello}">basic url</a></li>
  <li><a th:href="@{/hello(param1=${param1}, param2=${param2})}">hello query param</a></li>
  <li><a th:href="@{/hello/{param1}/{param2}(param1=${param1}, param2=${param2})}">path variable</a></li>
  <li><a th:href="@{/hello/{param1}(param1=${param1}, param2=${param2})}">path variable + query parameter</a></li>
</ul>
</body>
</html>
```

![](https://github.com/Heo-y-y/development-blog/assets/112863029/dae96a72-83c3-4768-98a8-fa8ac902a58c)

좀 더 자세하게 살펴보면 아래와 같다.

- **단순한 URL**
    - `@{/hello}` → /hello
- **쿼리 파라미터**
    - `@{/hello(param1=${param1}, param2=${param2})}` → /hello?param1=data1&param2=data2
        - `()`에 있는 부분은 쿼리 파라미터로 처리된다.
- **경로 변수**
    - `@{/hello/{param1}/{param2}(param1=${param1}, param2=${param2})}` → `/hello/data1/data2`
        - URL 경로상에 변수가 있으면 `()` 부분은 변수로 처리된다.
- **경로 변수 + 쿼리 파라미터**
    - `{/hello/{param1}(param1=${param1}, param2=${param2})}` → /hello/data1?param2=data2
        - 경로 변수와 쿼리 파라미터를 함께 사용할 수 있다.

상대 경로, 절대 경로, 프로토콜 기준을 표현할 수 있다.

- `/hello`: 절대 경로
- `hello`: 상대 경로

---

**참고 자료**

- [https://www.inflearn.com/course/스프링-mvc-2](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2)
