### Repository
📎 **[GitHub](https://github.com/Heo-y-y/study_toy_MSA/tree/main/Loan-Service/src)**

기본적인 클래스들은 따로 설명하지 않고 코드를 보며 넘어가겠습니다.

### Entity

```java
package com.study.Loan.Service.entity;

import lombok.Getter;

import javax.persistence.*;

@Getter
@Entity
public class Loan {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;
    @Column(nullable = false)
    private long userId;
    @Column(nullable = false)
    private long bookId;
}
```

### LoanService가 받아오는 각 Feign 클래스

```java
@FeignClient(name = "BookFeign", url = "http://localhost:8082/api")
public interface BookFeign {
    @GetMapping("/book/verify/{bookId}")
    AvailabilityStatus getBookRentalStatus(@PathVariable("bookId") long bookId);
}
```

```java
@FeignClient(name = "UserFeign", url = "http://localhost:8085")
public interface UserFeign {
    @GetMapping("/user/rentalRight/{userEmail}")
    UserRentalResponse getUserRentalStatus(@PathVariable("userEmail") String userEmail);
}
```

```java
@FeignClient(name = "HistoryServiceFeign", url = "http://localhost:8083/api")
public interface HistoryFeign {
    @PostMapping("/histories/loan/save")
    void saveLoanRecord(@RequestParam long userId, @RequestParam long bookId, @RequestParam int quantity);
}
```

우선 `BookFeign`은  `BookService`**에서 보내주는 대여 가능 상태를 받아오는 기능**을 합니다.
이와 마찬가지로 `UserFeign`은 `UserService` **에서 대여 가능 상태와 userId를 받아오는 기능**을 합니다.
마지막으로 `HistoryFeign`은 `HistroyService`의 **저장 기능**을 받아 옵니다.

### Response 클래스

```java
public enum AvailabilityStatus {
    AVAILABLE,
    NOT_AVAILABLE
}
```

`book`과 `user`가 사용하는 클래스입나다.

대여 가능과 불가능 상태를 나타냅니다.

```java
@AllArgsConstructor
@Getter
public class UserRentalResponse {
    private AvailabilityStatus rentalRight;
    private long userId;
}
```

`book`에서는 단순히 상태만 받아와서 `AvailabilityStatus` 를 사용하지만, 
`user`에서는 아래 그림처럼 `userId`와 상태값 두 개를 보내주기 때문에 따로 응답 클래스를 만들었습니다.

<img width="291" alt="스크린샷 2023-07-31 오전 11 22 48" src="https://github.com/heo-mewluee-Study-Group/cs-study/assets/112863029/63b70385-0c01-4845-8e16-30dfb741fcd0">

```java
@Getter
@AllArgsConstructor
public class LoanRequest {
    private String email;
    private long bookId;
    private int quantity;
}
```

마지막으로 대여 요청시 해당 유저 `email`과 `bookId`, `quantity`를 받기위해 request용 클래스를 만들었습니다.
이 클래스는 `controller`에서 `@RequestBody`에 사용합니다.

### LoanService 클래스(실질적인 비즈니스 담당)

```java
@Service
@RequiredArgsConstructor
public class LoanService {
    private final BookFeign bookFeign;
    private final HistoryFeign historyFeign;
    private final UserFeign userFeign;

    public String processLoanRequest(String email, long bookId, int quantity) {
        AvailabilityStatus bookStatus = bookFeign.getBookRentalStatus(bookId);
        UserRentalResponse userResponse = userFeign.getUserRentalStatus(email);

        if (bookStatus == AvailabilityStatus.AVAILABLE && userResponse.getRentalRight() == AvailabilityStatus.AVAILABLE) {
            historyFeign.saveLoanRecord(userResponse.getUserId(), bookId, quantity);
            return "대여 완료";
        } else {
            return "대여 불가능";
        }
    }
}
```

실질적인 비즈니스 로직을 담당하는 클래스입니다.
`processLoanRequest()`는 위에 만든 `feign`들을 받아와서 `user`와 `book`**이 둘다 대여가 가능한 상황에만 저장소인** `HistoryService` **에 데이터를 저장**하는 비즈니스 로직입니다.

### LoanController 클래스

```java
@RestController
@RequestMapping("/api")
@RequiredArgsConstructor
public class LoanController {
    private final LoanService loanService;

    @PostMapping("/loan/request")
    public ResponseEntity<String> processLoanRequest(@RequestBody LoanRequest request) {
        String result = loanService.processLoanRequest(request.getEmail(), request.getBookId(), request.getQuantity());
        return ResponseEntity.ok(result);
    }
}
```

### LoanServiceTest

```java
@ExtendWith(MockitoExtension.class)
class LoanServiceTest {
    @Mock
    private BookFeign bookFeign;
    @Mock
    private HistoryFeign historyFeign;
    @Mock
    private UserFeign userFeign;
    @InjectMocks
    private LoanService loanService;

    @Test
    @DisplayName("대여가 가능한 경우")
    public void whenBookAndUserAvailable() {
        // given
        long bookId = 1L;
        int quantity = 1;
        String email = "test@gmail.com";
        UserRentalResponse userResponse = new UserRentalResponse(AvailabilityStatus.AVAILABLE, 1L);

        given(bookFeign.getBookRentalStatus(bookId)).willReturn(AvailabilityStatus.AVAILABLE);
        given(userFeign.getUserRentalStatus(email)).willReturn(userResponse);
        // when
        String result = loanService.processLoanRequest(email, bookId, quantity);
        // then
        assertEquals("대여 완료", result);
        verify(historyFeign, times(1)).saveLoanRecord(userResponse.getUserId(), bookId, quantity);
    }

    @Test
    @DisplayName("책과 유저 둘다 불가능한 경우")
    public void whenBookAndUserNotAvailable() {
        // given
        long bookId = 1L;
        int quantity = 1;
        String email = "test@gmail.com";
        UserRentalResponse userResponse = new UserRentalResponse(AvailabilityStatus.NOT_AVAILABLE, 1L);

        given(bookFeign.getBookRentalStatus(bookId)).willReturn(AvailabilityStatus.NOT_AVAILABLE);
        given(userFeign.getUserRentalStatus(email)).willReturn(userResponse);
        // when
        String result = loanService.processLoanRequest(email, bookId, quantity);
        // then
        assertEquals("대여 불가능", result);
        verify(historyFeign, never()).saveLoanRecord(anyLong(), anyLong(), anyInt());

    }

    @Test
    @DisplayName("책이 불가능한 경우")
    public void whenBookNotAvailable() {
        // given
        long bookId = 1L;
        int quantity = 1;
        String email = "test@gmail.com";

        given(bookFeign.getBookRentalStatus(bookId)).willReturn(AvailabilityStatus.NOT_AVAILABLE);
        // when
        String result = loanService.processLoanRequest(email, bookId, quantity);
        // then
        assertEquals("대여 불가능", result);
        verify(historyFeign, never()).saveLoanRecord(anyLong(), anyLong(), anyInt());
    }

    @Test
    @DisplayName("유저가 불가능한 경우")
    public void whenUserNotAvailable() {
        // given
        long bookId = 1L;
        int quantity = 1;
        String email = "test@gmail.com";
        UserRentalResponse userResponse = new UserRentalResponse(AvailabilityStatus.NOT_AVAILABLE, 1L);

        given(bookFeign.getBookRentalStatus(bookId)).willReturn(AvailabilityStatus.AVAILABLE);
        given(userFeign.getUserRentalStatus(email)).willReturn(userResponse);
        // when
        String result = loanService.processLoanRequest(email, bookId, quantity);
        // then
        assertEquals("대여 불가능", result);
        verify(historyFeign, never()).saveLoanRecord(anyLong(), anyLong(), anyInt());
    }
}
```

기존의 테스트와 동일하게 진행했지만 한 가지 다른 점은 저장을 하지 않았는지에 대한 테스트를 할때 새로운 것을 사용했습니다. 
`verify(historyFeign, never()).saveLoanRecord(anyLong(), anyLong(), anyInt());`
여기서 `never()` 메서드는 `verify` 메소드의 인수로 사용되는데, 지정된 메서드(`saveLoanRecord`)가 테스트 중에 `historyFeign` 모의 개체에서 호출되지 않을 것으로 예상한다는 메서드입니다.
