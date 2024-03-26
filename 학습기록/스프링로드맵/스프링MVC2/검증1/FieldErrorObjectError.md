# **FieldError, ObjectError**

입련한 오류 메시지가 화면에 남도록 코드로 적용해보자.

**Controller**

```java
@Controller
@RequestMapping("/validation/v2/items")
@RequiredArgsConstructor
public class ValidationItemControllerV2 {

    private final ItemRepository itemRepository;

    @GetMapping("/add")
    public String addForm(Model model) {
        model.addAttribute("item", new Item());
        return "validation/v2/addForm";
    }

    @PostMapping("/add")
    public String addItemV2(@ModelAttribute Item item, BindingResult bindingResult, RedirectAttributes redirectAttributes, Model model) {

        // 검증 로직
        if (!StringUtils.hasText(item.getItemName())) {
            bindingResult.addError(new FieldError("item", "itemName", item.getItemName(), false, null, null, "상품 이름은 필수입니다."));
        }
        if (item.getPrice() == null || item.getPrice() < 1000 || item.getPrice() > 1000000) {
            bindingResult.addError(new FieldError("item", "price", item.getPrice(), false, null, null, "가격은 1,000 ~ 1,000,000 까지 허용합니다."));
        }
        if (item.getQuantity() == null || item.getQuantity() >= 9999) {
            bindingResult.addError(new FieldError("item", "quantity", item.getQuantity(), false, null, null, "수량은 최대 9,9999 까지 허용합니다."));
        }

        // 특정 필드가 아닌 복합 룰 검증
        if (item.getPrice() != null && item.getQuantity() != null) {
            int resultPrice = item.getPrice() * item.getQuantity();
            if (resultPrice < 10000) {
                bindingResult.addError(new ObjectError("item", null, null, "가격 * 수량의 합은 10,000원 이상이어야 합니다. 현재 값 = " + resultPrice));
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

여기서 수정된 FieldError 생성자를 살펴보자.

FieldError는 두 가지 생성자를 제공한다.

```java
// 1
public FieldError(String objectName, String field, String defaultMessage);

// 2
public FieldError(String objectName, String field, @Nullable Object
rejectedValue, boolean bindingFailure, @Nullable String[] codes, @Nullable
Object[] arguments, @Nullable String defaultMessage)
```

파라미터 목록

- `objectName`: 오류가 발생한 객체 이름
- `field`: 오류 필드
- `rejectedValue`: 사용자가 입력한 값(거절된 값)
- `bindingFailure`: 타입 오류 같은 바인딩 실패인지, 검증 실패인지 구분 값
- `codes`: 메시지 코드
- `arguments`: 메시지에서 사용하는 인자
- `defaultMessage`: 기본 오류 메시지

사용자의 입력 데이터가 `Controller`의 `@ModelAttribute`에 바인딩되는 시점에 오류가 발생하면 `Model` 객체에 사용자 입력 값을 유지하기가 어렵다.

예를 들어서 가격에 숫자가 아닌 문자가 입력되면, 가격은 `Integer` 타입이기 때문에 문자를 보관할 수 없다. 그래서 오류가 발생할 경우 사용자 입력 값을 보관하는 별도의 그릇이 필요하다. 만약 보관할 그릇이 있으면 보관한 입력 값을 검증 오류 발생시 화면에 다시 출력시키면 될 것이다.

`FieldError`는 오류 발생시 위 문제를 해결해주는 입력 값을 저장하는 기능을 제공한다.

위 생성자 파라미터 목록에서 `rejectedValue` 가 바로 오류 발생시 입력 값을 저장하는 필드이다. `bindingFailure` 는 타입 오류 같은 바인딩이 실패했는지 여부를 적어주면 된다. 위 상황에서는 바인딩이 실패한 것이 아니기 때문에 `false`를 적용했다.

**타임리프의 사용자 입력 값 유지**

타임리프의 `th:field`는 매우 스마트하게 동작하는데, 정상 상황에는 `Model` 객체의 값을 사용하지만, 오류가 발생하면 `FieldError`에서 보관한 값을 사용하여 값을 출력해준다.

**스프링의 바인딩 오류 처리**

타입 오류로 인하여 바인딩에 실패하면, 스프링은 `FieldError`를 생성하면서 사용자가 입력한 값을 넣어둔다. 그리고 해당 오류를 `BindingResult`에 담아서 컨트롤러를 호출한다. 따라서 타입 오류 같은 바인딩 실패시에도 사용자의 오류 메시지를 정상 출력할 수 있다.

---

**참고 자료**

- [https://www.inflearn.com/course/스프링-mvc-2](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2)
