# **오류 코드와 메시지 처리 - MessageCodeResolver**

### **MessageCodeResolver**

- 검증 오류 코드로 메시지 코드들을 생성한다.
- `MessageCodeResolver` 인터페이스이고, `DefaultMessageCodesResolver`는 기본 구현체이다.
- 주로 `ObjectError`, `FieldError`와 함께 사용한다.

### **DefaultMessageCodesResolver의 기본 메시지 생성 규칙**

**객체 오류**

```java
객체 오류의 경우 다음 순서로 2가지 생성 
1.: code + "." + object name 
2.: code

예) 오류 코드: required, object name: item
1.: required.item
2.: required
```

**필드 오류**

```java
필드 오류의 경우 다음 순서로4가지 메시지 코드 생성 
1.: code + "." + object name + "." + field 
2.: code + "." + field
3.: code + "." + field type
4.: code

예) 오류 코드: typeMismatch, object name "user", field "age", field type: int 
1. "typeMismatch.user.age"
2. "typeMismatch.age"
3. "typeMismatch.int"
4. "typeMismatch"
```

**동작 방식**

- `rejectValue()`, `reject()`는 내부에서 `MessageCodeResolver`를 사용한다. 여기에서 메시지 코드들을 생성하는 것이다.
- `FieldError`, `ObjectError`의 생성자를 보면, 오류 코드를 하나가 아니라 여러 오류 코드를 가질 수 있다. `MessageCodeResolver`를 통해서 생성된 순서대로 오류 코드를 보관한다.

**FieldError** `rejectValue(”itemName”, “required”)`을 입력하는 경우 아래 오류 코드를 자동으로 생성하는 것이다.

- required.item.itemName
- required.itemName
- required.java.lang.String
- required

**ObjectError** `reject(”totalPriceMin”)`을 입력하는 경우 아래 오류 코드를 자동으로 생성한다.

- totalPriceMin.item
- totalPriceMin

**오류 메시지 출력**

타임리프 화면을 렌더링 할 때 `th:errors`가 실행되는데, 이때 오류가 있으면 생성된 오류 메시지 코드를 순서대로 돌아가면서 메시지를 찾는다. 없으면 디폴트 메시지를 출력한다.

---

**참고 자료**

- [https://www.inflearn.com/course/스프링-mvc-2](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2)
