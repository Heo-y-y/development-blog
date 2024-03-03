# 변수 - SpringEL

타임리프에서 변수를 사용할 때는 변수 표현식을 사용한다.

변수 표현식의 방법은 아래와 같다.

- **변수 표현식**: `${…}`

그리고 이 변수 표현식에는 SpringEL이라는 스프링이 제공하는 표현식을 사용할 수 있다.

Object, List, Map으로 간단하게 살펴보자.

**BasicController**

```java
@Controller
@RequestMapping("/basic")
public class BasicController {

    @GetMapping("/variable")
    public String variable(Model model) {
        User userA = new User("userA", 10);
        User userB = new User("userB", 20);

        List<User> list = new ArrayList<>();
        list.add(userA);
        list.add(userB);

        Map<String, User> map = new HashMap<>();
        map.put("userA", userA);
        map.put("userB", userB);

        model.addAttribute("user", userA);
        model.addAttribute("users", list);
        model.addAttribute("userMap", map);

        return "basic/variable";
    }

    @Data
    static class User {
        private String username;
        private int age;

        public User(String username, int age) {
            this.username = username;
            this.age = age;
        }
    }
}
```

**variable.html**

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<h1>SpringEL 표현식</h1> <ul>Object
  <li>${user.username} =
  <li>${user['username']} = <span th:text="${user['username']}"></span></li>
  <li>${user.getUsername()} = <span th:text="${user.getUsername()}"></span></li>
</ul>
<ul>List
  <li>${users[0].username}    = <span th:text="${users[0].username}"></span></li>
  <li>${users[0]['username']} = <span th:text="${users[0]['username']}"></span></li>
  <li>${users[0].getUsername()} = <span th:text="${users[0].getUsername()}"></span></li>
</ul>
<ul>Map
  <li>${userMap['userA'].username} =  <span th:text="${userMap['userA'].username}"></span></li>
  <li>${userMap['userA']['username']} = <span th:text="${userMap['userA']['username']}"></span></li>
  <li>${userMap['userA'].getUsername()} = <span th:text="${userMap['userA'].getUsername()}"></span></li>
</ul>

</body>
</html>

```

- **Object**
    - `user.username`: user의 username을 프로퍼티 접근 → user.getUsername()
    - `user['username']`: 위와 같다. → user.getUsername()
    - `user.getUsername()`: user의 getUsername() 직접 호출
- **List**
    - `users[0].username`: List에서 첫 번째 회원을 찾고, username 프로퍼티 접근 → list.get(0).getUsername()
    - `users[0]['username']`: 위와 같다.
    - `users[0].getUsername()`: List에서 첫 번째 회원을 찾고 메서드 직접 호출
- **Map**
    - `userMap['userA'].username`: Map에서 userA를 찾고, username 프로퍼티 접근 → map.get(”userA”).getUsername()
    - `userMap['userA']['username']`: 위와 같다.
    - `userMap['userA'].getUsername()`: Map에서 userA를 찾고 메서드 직접 호출

### 지역 변수 선언

th:with를 사용하면 지역 변수를 선언하여 사용할 수 있다. 지역 변수는 선언한 태그 안에서만 사용 가능하다.

```html
<h1>지역 변수 - (th:with)</h1>
<div th:with="first=${users[0]}">
  <p>처음 사람의 이름은 <span th:text="${first.username}"></span></p>
</div>
```

`first`에  `model.addAttribute("users", list)`에 넣은 첫번째 값을 넣어 사용하는 것이다.

---

**참고 자료**

- [https://www.inflearn.com/course/스프링-mvc-2](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2)
