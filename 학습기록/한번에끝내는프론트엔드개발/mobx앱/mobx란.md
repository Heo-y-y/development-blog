# MobX란?

## MobX란

React에서 Redux 이후로 많이 사용되는 상태 관리 라이브러리이다.

**특징**

- 간단하고 확장 가능한 상태 관리
- 쉽고 확장성 있게 만들어 주는 검증된 라이브러리
- 렌더링 최적화를 쉽게 할 수 있다.
- 구조가 자유롭다.

원래는 @데코레이터를 사용했지만 mobx 6부터는 데코레이터 사용을 지양하는 중이다.

## MonX 문서에 대하여

MobX6 이전 버전에서는 더 나은 클래스 작성을 위해 데코레이터가 권장되었었다.

```jsx
class TodoList {
	@observable todos = []
	@computed get unfinishedTodoCount() {
		return this.todos.filter((todo) => !todo.finished).legth
	}
}
```

### mobx 작동 원리

![스크린샷 2024-01-24 오후 9.08.02.png](https://github.com/Heo-y-y/development-blog/assets/112863029/81b309dc-44ca-4382-ae79-904670f4a6e5)

이런식으로 Mobx 는 작동하는데, 예제 소스 코드를 보면서 Actions가 뭔지 Observable State가 뭔지 하나씩 알아보자.

![스크린샷2 2024-01-24 오후 9 09 38](https://github.com/Heo-y-y/development-blog/assets/112863029/9e5c21f5-2f53-47b0-bd27-e2481f59c5f9)

모든 이벤트(`onClick`, `setIntervak`)는 observable state(`myTimer.secondsPassed`)를 변경시키는 action(`myTimer.increase`, `myTimer.reset`)을 호출한다. observable state의 변경 사항은 모든 연산과 변경사항에 따라 달라지는 부수효과(`TimerView`)에 전파된다.

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
