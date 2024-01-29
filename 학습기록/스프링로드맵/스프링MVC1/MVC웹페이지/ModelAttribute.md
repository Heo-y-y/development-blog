# **상품 등록 처리** - @ModelAttribute

이제 상품 등록 폼에서 전달된 데이터로 실제 상품 등록 처리를 해보자.

상품 등록 폼은 아래 방식으로 서버에 데이터를 전달한다.

- **POST - HTML Form**
    - `content-type: application/x-www-form-urlencoded`
    - 메시지 바디에 쿼리 파라미터 형식으로 전달 `itemName=itemA&price=10000&quantity=10`
    - ex) 회원가입, 상품주문, HTML Form 사용

요청 파라미터 형식을 처리해야 하므로 `@RequestParam` 을 사용했다.

### **상품 등록 처리** - @RequestParam

**addItemV1 - BasicItemController에 추가**

```java
@PostMapping("/add")
public String addItemV1(@RequestParam String itemName, @RequestParam int price, @RequestParam Integer quantity, Model model) {
    Item item = new Item();
    item.setItemName(itemName);
    item.setPrice(price);
    item.setQuantity(quantity);

    itemRepository.save(item);

    model.addAttribute("item", item);

    return "basic/item";
}
```

- `@RequestParam String itemName`: `itemName` 요청 파라미터 데이터를 해당 변수에 받는다.
- `Item` 객체를 생성하고 `itemRepository`를 통해서 저장한다.
- 저장된 `item`을 모데렝 담아서 뷰에 전달한다.

여기서는 상품 상세에서 사용한 `item.html`뷰 템플릿을 그대로 재활용한다.

### **상품 등록 처리** - @ModelAttribute

`@RequestParam` 으로 변수를 하나하나 받아서 `Item`을 생성하는 과정은 불편하다.

이번에는 `@ModelAttribute` 를 사용해서 한번에 처리해보자.

**addItemV2 - 상품 등록 처리 - ModelAttribute**

```java
@PostMapping("/add")
public String addItemV2(@ModelAttribute("item") Item item, Model model) {
    itemRepository.save(item);
    model.addAttribute("item", item); // 자동 추가, 생략 가능

    return "basic/item";
}
```

**@ModelAttribute - 요청 파라미터 처리**

`@ModelAttribute` 는 `Item` 객체를 생성하고, 요청 파라미터의 값을 프로퍼티 접근법 (`setXxx`)으로 입력해준다.

**@ModelAttribute - Model 추가**

`@ModelAttribute` 는 중요한 한가지 기능이 더 있는데, `Model`에 `@ModelAttribute` 로 지정한 객체를 자동으로 넣어준다.

**addItemV3 - 상품 등록 처리 - ModelAttribute 이름 생략**

```java
@PostMapping("/add")
public String addItemV3(@ModelAttribute Item item) {
    itemRepository.save(item);
    return "basic/item";
}
```

`@ModelAttribute` 의 이름을 생략할 수 있다.

참고로 `@ModelAttribute` 의 이름을 생략하면 모델에 저장될 때 클래스명을 사용하는데, 이때 **클래스의 첫글자만 소문자로 변경해서 등록**한다.

ex) `@ModelAttribute` 클래스명 → 모델에 자동 추가되는 이름

`Item` → `item` / `HelloWorld` → `helloWorld`

**addItemV4 - 상품 등록 처리 - ModelAttribute 전체 생략**

```java
@PostMapping("/add")
public String addItemV4(Item item) {
    itemRepository.save(item);
    return "basic/item";
}
```

`@ModelAttribute` 자체도 생략 가능하다.

대상 객체는 모델에 자동 등록이 된다.(나머지 사항은 기존과 동일)

---

**참고 자료**

- [https://www.inflearn.com/course/스프링-mvc-1/dashboard](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard)
