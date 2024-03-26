# 오류 코드와 메시지 처리 - rejectValue, reject

앞서 진행했던 방식은 아직 불편한 감이 좀 있다.

그래서 `BindingResult`가 제공하는 `rejectValue` 와 `reject` 를 활용해서 좀 더 자동화시켜 보는 방법을 알아보자.

`rejectValue` 와 `reject` 를 사용하면 `FieldError`, `ObjectError`를 직접 생성하지 않고, 깔끔하게 검증 오류를 다룰 수 있다.

컨트롤러에서 `BindingResult`는 검증해야 할 객체은 `target` 바로 다음에 온다. 따라서 `BindingResult`는 이미 본인이 검증해야 할 객체인 `target`을 알고 있다.

컨트롤러에서 로그를 찍어보면 아래와 같이 뜨는 것을 확인할 수 있다.

```java
log.info("objectName={}", bindingResult.getObjectName());
log.info("target={}", bindingResult.getTarget());

// 결과
objectName=item //@ModelAttribute name
target=Item(id=null, itemName=상품, price=100, quantity=1234)
```

**Controller**

```java
@Controller
@RequestMapping("/validation/v2/items")
@RequiredArgsConstructor
public class ValidationItemControllerV2 {

    private final ItemRepository itemRepository;

    @PostMapping("/add")
    public String addItemV4(@ModelAttribute Item item, BindingResult bindingResult, RedirectAttributes redirectAttributes, Model model) {

        // 검증 로직
        if (!StringUtils.hasText(item.getItemName())) {
            bindingResult.rejectValue("itemName", "required");
        }
        if (item.getPrice() == null || item.getPrice() < 1000 || item.getPrice() > 1000000) {
            bindingResult.rejectValue("price", "range", new Object[]{1000, 1000000}, null);
        }
        if (item.getQuantity() == null || item.getQuantity() >= 9999) {
            bindingResult.rejectValue("quantity", "max", new Object[]{9999}, null);
        }

        // 특정 필드가 아닌 복합 룰 검증
        if (item.getPrice() != null && item.getQuantity() != null) {
            int resultPrice = item.getPrice() * item.getQuantity();
            if (resultPrice < 10000) {
                bindingResult.reject("totalPriceMin", new Object[]{10000, resultPrice}, null);
            }
        }

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

**rejectValue**

```java
void rejectValue(@Nullable String field, String errorCode,
         @Nullable Object[] errorArgs, @Nullable String defaultMessage);
```

**reject**

```java
void reject(String errorCode, @Nullable Object[] errorArgs, @Nullable String
 defaultMessage);
```

- `field`: 오류 필드명
- `errorCode`: 오류 코드
- `errorArgs`: 오류 메시지에서 치환하기 위한 값
- `defaultMessage`: 오류 메시지를 찾을 수 없을 때 사용하는 기본 메시지

신기한 점은 `errors.properties`에 있는 코드 경로를 다 적지 않았는데, 정상 동작을 한다.

앞에서 `BindingResult` 는 어떤 객체를 대상으로 검증하는지 `target`을 이미 알고 있다고 했다. 따라서 `target`에 대한 정보는 없어도 된다.

---

**참고 자료**

- [https://www.inflearn.com/course/스프링-mvc-2](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2)
