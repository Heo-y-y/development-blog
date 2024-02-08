# SSE를 통한 통신 연결 및 알림 기능 구현

## 알림이 발생하는 과정

우리 서비스는 현재 다음과 같은 경우에 알림이 발생한다.

- 본인을 팔로우 했을 경우
- 사용자가 본인의 댓글 또는 게시물에 댓글을 남겼을 경우
- 사용자가 본인을 태그했을 경우
- 채팅 메시지가 올경우
- 본인의 게시물 또는 댓글에 좋아요가 눌리는 경우

## 구현

Spring Framework 4.2부터 SSE 통신을 지원하는 SseEmitter 클래스를 이용해 구현할 계획이다.

구현은 기존 `Alarm`에 같이 적용할 생각이다.

### 연결 생성

**Controller**

클라이언트에서 구독하는 요청을 보내면, 컨트롤러는 `SseEmitter`를 만들어주는 서비스 레이어를 통해 전달 받은 `SseEmitter`를 반환한다.

```java
@RestController
@RequiredArgsConstructor
public class NotificationController {

		private final AlarmService alarmService;

		@GetMapping("/subscribe/{username}")
		public ResponseEntity<SseEmitter> subscribe(@PathVariable("username") String username) {
		  return new ResponseEntity<>(alarmService.connectSubscribe(username), HttpStatus.OK);
		}
}
```

**Service**

새로운 연결을 생성할 때에는 이후, SSE 응답을 할 때 아무런 이벤트도 보내지 않으면 재연결 요청을 보낼때나, 아니면 연결 요청 자체에서 오류가 발생하기 때문에, 첫 응답을 보내주었다.

```java
@Slf4j
@Service
@RequiredArgsConstructor
public class AlarmService {

	  private final Map<String, SseEmitter> emitters = new ConcurrentHashMap<>();

		@Transactional
    public SseEmitter connectSubscribe(String username) {
        securityUtil.checkLoginMember();
        memberRepository.findByUsername(username)
                .orElseThrow(() -> new BusinessException(MEMBER_NOT_FOUND));
        SseEmitter emitter = new SseEmitter();
        emitters.put(username, emitter);
        emitter.onTimeout(() -> emitters.remove(username));
        emitter.onCompletion(() -> emitters.remove(username));
        sendNotification(username, username + "님의 통신 연결이 완료되었습니다.");
        return emitter;
    }
}
```

`connectSubscribe()` 메서드는 구독 요청할 때 사용할 메서드이고, 앞에서 만든 `subscribe()` 메서드에서 사용한다. 이때 위에서 말한 내용처럼 메시지를 같이 내보내 오류 발생을 방지하였다. 또한 로그인 한 유저만 가능하게 진행하고, 해당 `user`가 존재하는지 확인한다.

통신 연결을 포스트맨으로 테스트하면 아래와 같이 잘 동작한다.

![스크린샷1 2024-01-19 오후 3 34 34](https://github.com/Heo-y-y/development-blog/assets/112863029/914a9d5e-5d82-41c8-b28c-432d2a6e3aac)

### 알림 전송

**Service**

```java
@Slf4j
@Service
@RequiredArgsConstructor
public class AlarmService {

	  private final Map<String, SseEmitter> emitters = new ConcurrentHashMap<>();

    public void sendNotification(String username, String message) {
        SseEmitter emitter = emitters.get(username);
        if (emitter != null) {
            try {
                emitter.send(SseEmitter.event().name("notification").data(message));
            } catch (IOException e) {
                emitter.complete();
                emitters.remove(username);
                throw new BusinessException(NOTIFICATION_CONNECTION_ERROR);
            }
        }
    }
}
```

`sendNotification()` 메서드는 구독한 클라이언트한테 특정 알림이 올 경우 알림을 보내주는 메서드이다. 해당 메서드는 각 알림이 발생하는 상황에 따라 다르게 메시지처리를 해서 내보낸다.

메시지를 내보내는 형식은 아래와 같다.

```java
@Getter
@AllArgsConstructor
public enum AlarmType {

    FOLLOW("{agent.username}님이 회원님을 팔로우하기 시작했습니다."),
    LIKE_POST("{agent.username}님이 회원님의 사진을 좋아합니다."),
    MENTION_POST("{agent.username}님이 게시물에서 회원님을 언급했습니다: {post.content}"),
    COMMENT("{agent.username}님이 댓글을 남겼습니다: {comment.text}"),
    LIKE_COMMENT("{agent.username}님이 회원님의 댓글을 좋아합니다: {comment.text}"),
    MENTION_COMMENT("{agent.username}님이 댓글에서 회원님을 언급했습니다: {comment.text}");

    private String messageTemplate;

    public String createAlarmMessage(Alarm alarm) {
        String message = messageTemplate;
        message = message.replace("{agent.username}", alarm.getAgent().getUsername());

        if (message.contains("{post.content}")) {
            message = message.replace("{post.content}", alarm.getPost().getContent());
        }

        if (message.contains("{comment.text}")) {
            message = message.replace("{comment.text}", alarm.getComment().getText());
        }
        return message;
    }
}
```

각 상황에 따라 누가 보냈는지, 어떤 게시물 또는 어떤 댓글인지 replace해서 실제 인스타그램 알림 기능처럼 처리했다.

간단하게 팔로우 상황으로 예시를 들어보자.

```java
@Transactional
public void sendFollowAlarm(Member agent, Member target, Follow follow) {
   final Alarm alarm = Alarm.builder()
           .type(FOLLOW)
           .agent(agent)
           .target(target)
           .follow(follow)
           .build();

   alarmRepository.save(alarm);

   sendNotification(target.getUsername(), alarm.getType().createAlarmMessage(alarm));
}
```

`sendFollowAlarm()` 메서드가 불리면, 만들어 놓은 `sendNotification()`를 통해 위 코드의 `createAlarmMessage()`가 변환시켜서 전달된다.

마찬가지로 팔로우 상황을 포스트맨으로 테스트하면 아래와 같이 동작하는 것을 확인할 수 있다.

![스크린샷2 2024-01-19 오후 3 43 35](https://github.com/Heo-y-y/development-blog/assets/112863029/dd3e8fcc-336f-45aa-b3e0-eddf7db5d267)

### 알림 메시지 테스트 코드

```java
@Test
@DisplayName("[alarm] send follow alarm:success")
void test_send_follow_alarm_success() {
    //given
    Member agent = Member.builder()
            .username("agentUsername")
            .build();

    Member target = Member.builder()
            .username("targetUsername")
            .build();

    Follow follow = Follow.builder()
            .member(agent)
            .followMember(target)
            .build();

    Alarm alarm = Alarm.builder()
            .type(FOLLOW)
            .agent(agent)
            .target(target)
            .follow(follow)
            .build();
    //when
    alarmService.sendFollowAlarm(agent, target, follow);

    //then
    verify(alarmRepository).save(Mockito.any());

    String expectedMessage = FOLLOW.getMessageTemplate()
            .replace("{agent.username}", agent.getUsername());

    assertEquals(expectedMessage, alarm.getType().createAlarmMessage(alarm));
    }
```

메시지를 확인하면 잘 동작하는 것을 볼 수 있다.

![스크린샷 2024-01-19 오후 3.30.40.png](https://github.com/Heo-y-y/development-blog/assets/112863029/1823c702-547e-422b-af60-ef7e4aa58ccb)
