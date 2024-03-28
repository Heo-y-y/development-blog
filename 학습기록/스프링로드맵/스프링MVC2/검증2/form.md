# Form **전송 객체 분리**

`Item`은 그냥 기본 `class`로 두고, 필요한 등록 폼과, 수정 폼 용 `class`를 만든다.

**Item**

```java
 @Data
 public class Item {
     private Long id;
     private String itemName;
     private Integer price;
     private Integer quantity;
}
```

**ItemSaveForm - Item 저장용 폼**

```java
 @Data
 public class ItemSaveForm {
     @NotBlank
     private String itemName;
     @NotNull
     @Range(min = 1000, max = 1000000)
     private Integer price;
     @NotNull
     @Max(value = 9999)
     private Integer quantity;
}
```

**ItemUpdateForm - Item 수정용 폼**

```java
@Data
 public class ItemUpdateForm {
     @NotNull
     private Long id;
     @NotBlank
     private String itemName;
     @NotNull
     @Range(min = 1000, max = 1000000)
     private Integer price;
}
```

**Controller**

```java
@Slf4j
@Controller
@RequestMapping("/validation/v4/items")
@RequiredArgsConstructor
public class ValidationItemControllerV4 {

    private final ItemRepository itemRepository;

    @PostMapping("/add")
    public String addItem(@Validated @ModelAttribute("item") ItemSaveForm form, BindingResult bindingResult, RedirectAttributes redirectAttributes) {

        // 특정 필드가 아닌 복합 룰 검증
        if (form.getPrice() != null && form.getQuantity() != null) {
            int resultPrice = form.getPrice() * form.getQuantity();
            if (resultPrice < 10000) {
                bindingResult.reject("totalPriceMin", new Object[]{10000, resultPrice}, null);
            }
        }

        // 검증에 실패하면 다시 입력 폼으로
        if (bindingResult.hasErrors()) {
            log.info("errors={} ", bindingResult);
            return "validation/v4/addForm";
        }

        // 성공 로직
        Item item = new Item();
        item.setItemName(form.getItemName());
        item.setPrice(form.getPrice());
        item.setQuantity(form.getQuantity());

        Item savedItem = itemRepository.save(item);
        redirectAttributes.addAttribute("itemId", savedItem.getId());
        redirectAttributes.addAttribute("status", true);
        return "redirect:/validation/v4/items/{itemId}";
    }

    @PostMapping("/{itemId}/edit")
    public String edit(@PathVariable Long itemId, @Validated @ModelAttribute("item") ItemUpdateForm form, BindingResult bindingResult) {

        // 특정 필드가 아닌 복합 룰 검증
        if (form.getPrice() != null && form.getQuantity() != null) {
            int resultPrice = form.getPrice() * form.getQuantity();
            if (resultPrice < 10000) {
                bindingResult.reject("totalPriceMin", new Object[]{10000, resultPrice}, null);
            }
        }

        if (bindingResult.hasErrors()) {
            log.info("errors={} ", bindingResult);
            return "validation/v4/editForm";
        }

        Item itemParam = new Item();
        itemParam.setItemName(form.getItemName());
        itemParam.setPrice(form.getPrice());
        itemParam.setQuantity(form.getQuantity());

        itemRepository.update(itemId, itemParam);
        return "redirect:/validation/v4/items/{itemId}";
    }
}
```

이제는 `Item`을 직접 받는 대신에 `ItemSaveForm`과 `ItemUpdateForm`을 전달 받는다. 그리고 `@Validated` 검증도 수행하고, `BindingResult`로 검증 결과도 받는다.

**주의할 점**

`@ModelAttribute`의 이름은 따로 설정하지 않으면, 규칙에 의해 해당 필드의 이름으로 바뀌므로 HTML에 적용한 타임리프의 `th:object` 이름과 같아야 한다.

---

**참고 자료**

- [https://www.inflearn.com/course/스프링-mvc-2](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2)
