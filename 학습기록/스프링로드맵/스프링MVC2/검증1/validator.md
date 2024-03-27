# Validator **분리**

이제는 `Controller`에 너무 많은 역할을 주고 있으니 복잡한 검증 로직을 별도로 분리해 관리하는 방법을 알아보자.

**ItemValidator**

```java
@Component
public class ItemValidator implements Validator {
    @Override
    public boolean supports(Class<?> clazz) {
        return Item.class.isAssignableFrom(clazz);
    }

    @Override
    public void validate(Object target, Errors errors) {
        Item item = (Item) target;

        // 검증 로직
        if (!StringUtils.hasText(item.getItemName())) {
            errors.rejectValue("itemName", "required");
        }
        if (item.getPrice() == null || item.getPrice() < 1000 || item.getPrice() > 1000000) {
            errors.rejectValue("price", "range", new Object[]{1000, 1000000}, null);
        }
        if (item.getQuantity() == null || item.getQuantity() >= 9999) {
            errors.rejectValue("quantity", "max", new Object[]{9999}, null);
        }

        // 특정 필드가 아닌 복합 룰 검증
        if (item.getPrice() != null && item.getQuantity() != null) {
            int resultPrice = item.getPrice() * item.getQuantity();
            if (resultPrice < 10000) {
                errors.reject("totalPriceMin", new Object[]{10000, resultPrice}, null);
            }
        }
    }
}
```

스프링은 검증을 체계적으로 제공하기 위해 아래와 같이 인터페이스를 제공한다.

```java
 public interface Validator {
     boolean supports(Class<?> clazz);
     void validate(Object target, Errors errors);
}
```

- `supports() {}`: 해당 검증기를 지원하는 여부 확인
- `validate(Object target, Errors, errors)`: 검증 대상 객체와 `BindingResult`

이렇게 따로 분리해 놓으면, `Controller`에서는 `ItemValidator`를 DI해서 부르기만 하면 된다.

**Controller**

```java
@Controller
@RequestMapping("/validation/v2/items")
@RequiredArgsConstructor
public class ValidationItemControllerV2 {

    private final ItemRepository itemRepository;
    private final ItemValidator itemValidator;

    @PostMapping("/add")
    public String addItemV5(@ModelAttribute Item item, BindingResult bindingResult, RedirectAttributes redirectAttributes, Model model) {

        itemValidator.validate(item, bindingResult);

        // 검증에 실패하면 다시 입력 폼으로
        if (bindingResult.hasErrors()) {
            return "validation/v2/addForm";
        }

        // 성공 로직
        Item savedItem = itemRepository.save(item);
        redirectAttributes.addAttribute("itemId", savedItem.getId());
        redirectAttributes.addAttribute("status", true);
        return "redirect:/validation/v2/items/{itemId}";
    }
}
```

### Validator 분리 - **@Validated 적용**

스프링이 `Validator` 인터페이스를 별도로 제공하는 이유는 체계적으로 검증 기능을 도입하기 위해서이다. 그런데 위에서는 검증기를 직접 불러서 사용한 방식이고, 이제 설명할 방식은 어노테이션을 사용하여 사용하는 방식이다.

**WebDataBinder 사용**

`WebDataBinder`는 스프링의 파라미터 바인딩의 역할을 해주며 검증 기능도 내부에 포함되어 있다.

```java
 @InitBinder
 public void init(WebDataBinder dataBinder) {
     dataBinder.addValidators(itemValidator);
 }
```

이렇게 `WebDataBinder`에 검증기를 추가하면 해등 컨트롤러에서는 검증기를 자동으로 적용할 수 있다.

`@InitBinder` → 해당 컨트롤러에서만 영향을 주고, 전체적으로 적용을 하려면 별도로 해야한다.

**@Validated 적용**

```java
@Controller
@RequestMapping("/validation/v2/items")
@RequiredArgsConstructor
public class ValidationItemControllerV2 {

    private final ItemRepository itemRepository;
    private final ItemValidator itemValidator;

    @InitBinder
    public void init(WebDataBinder dataBinder) {
        dataBinder.addValidators(itemValidator);
    }

    @PostMapping("/add")
    public String addItemV6(@Validated @ModelAttribute Item item, BindingResult bindingResult, RedirectAttributes redirectAttributes) {

        // 검증에 실패하면 다시 입력 폼으로
        if (bindingResult.hasErrors()) {
            return "validation/v2/addForm";
        }

        // 성공 로직
        Item savedItem = itemRepository.save(item);
        redirectAttributes.addAttribute("itemId", savedItem.getId());
        redirectAttributes.addAttribute("status", true);
        return "redirect:/validation/v2/items/{itemId}";
    }
}
```

검증 대상 앞에 `@Validated`만 적어주면 된다.

**동작 방식**

`@Validated`는 검증기를 실행하라는 어노케이션이다.

해당 어노테이션이 붙으면 `WebDataBinder`에 등록한 검증기를 찾아서 실행한다. 그런데 여러 검증기를 등록한다면, 그 중에 어떤 검증기가 실행되어야 할지 구분이 필요하다.

이때 `supports()`가 사용되는데, 해당 코드에서는 `Item.class`를 넘겨줬다. 그러면 결과가 `ItemValidator`의 `validate()`가 호출된다.

### 그로벌 설정 - 모든 컨트롤러에 적용

이경우에는 프로젝트 애플리케이션 클래스에 아래와 같이 적용해주면 된다.

```java
 @SpringBootApplication
 public class ItemServiceApplication implements WebMvcConfigurer {
     public static void main(String[] args) {
         SpringApplication.run(ItemServiceApplication.class, args);
}
     @Override
     public Validator getValidator() {
         return new ItemValidator();
     }
}
```

---

**참고 자료**

- [https://www.inflearn.com/course/스프링-mvc-2](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2)
