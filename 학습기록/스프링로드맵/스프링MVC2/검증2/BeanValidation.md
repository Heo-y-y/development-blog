# Bean Validation이란?

검증 기능을 매번 코드로 작성하는 것은 번거롭다. 특히 특정 필드에 대한 검증 로직은 빈 값인지 아닌지, 특정 크기를 넘는지 아닌지와 같이 매우 자주 사용되는 로직이다.

```java
 public class Item {
     private Long id;
     @NotBlank
     private String itemName;
     @NotNull
     @Range(min = 1000, max = 1000000)
     private Integer price;
     @NotNull
     @Max(9999)
     private Integer quantity;
}
```

이러한 검증 로직을 모든 프로젝트에 공통화하고, 표준화 하는 것이 Bean Validation이다.

Bean Validation을 잘 활용하면, 어노테이션을 하나로 검증 로직을 매우 간단하게 적용할 수 있다.

먼저 Bean Validation은 특정한 구현체가 아니라 Bean Validation 2.0(JSR-380)이라는 기술 표준이다. 간단하게 말해서, 검증 어노테이션과 여러 인터페이스의 모음인 것이다.

Bean Validation을 구현한 기술 중 일반적으로 사용하는 구현체는 하이버네이트의 Validator이다.

### Bean Validation 설정

**의존관계 추가**

**build.gradle**

```java
implementation 'org.springframework.boot:spring-boot-starter-validation'
```

이렇게 의존관계를 추가하면 라이브러리가 추가 된다.

**Jakarta Bean Validation**

- `jakarta.validation-api`: Bean Validation 인터페이스
- `hibernate-validator`: 구현체

위에서 설정한 Item의 유효성을 간단하게 테스트하면 아래와 같이 할 수 있다.

```java
public class BeanValidationTest {

    @Test
    void beanValidation() {
        ValidatorFactory factory = Validation.buildDefaultValidatorFactory();
        Validator validator = factory.getValidator();

        Item item = new Item();
        item.setItemName(" ");
        item.setPrice(0);
        item.setQuantity(10000);

        Set<ConstraintViolation<Item>> violations = validator.validate(item);
        for (ConstraintViolation<Item> violation : violations) {
            System.out.println("violation = " + violation);
            System.out.println("violation = " + violation.getMessage());
        }
    }
}
```

![스크린샷 2024-03-27 오후 9 52 47](https://github.com/Heo-y-y/development-blog/assets/112863029/fc491031-bab1-4fc0-acab-af8b72e48f09)

**검증기 생성**

위 코드와 같이 검증기를 생성한다. 스프링과 통합하면 직접 이러한 코드를 작성할 필요는 없기 때문에 사용하는 방법만 알아두자.

```java
 ValidatorFactory factory = Validation.buildDefaultValidatorFactory();
 Validator validator = factory.getValidator();
```

**검증 실행**

검증 대상인 item을 직접 검증기에 넣고 해당 결과를 받는다. Set에는 ConstraintViolation이라는 검증 오류가 담기는데, 결과가 비어있으면 검증 오류가 없는 것이다.

```java
Set<ConstraintViolation<Item>> violations = validator.validate(item);
```

---

**참고 자료**

- [https://www.inflearn.com/course/스프링-mvc-2](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2)
