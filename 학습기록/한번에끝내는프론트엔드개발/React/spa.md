# SPA(Single Page Application)

## SPA(Single Page Application)이란?

웹 사이트의 전체 페이지를 하나의 페이지에 담아 동적으로 화면을 바꿔가며 표현을 하는 것을 **SPA**라고 한다.

### SPA에서 화면 변경은 어떻게 일어날까?

전통적인 웹 사이트는 a page에서 b page로 페이지를 전환 할 때 `a.html`을 보여주다가 `b.html`을 보여 주면 됐지만, `index.html` 밖에 없는 SPA에서는 어떻게 페이지 전환(브라우징)을 할까?

**HTML 5의 History API**를 사용해서 가능하게 만든다.

자바스크립트 영역에서 History API를 사용해서 현재 페이지 내에서 화면 이동이 일어난 것처럼 작동하게 해준다.

React에선느 React-Router-Dom을 사용하여 History API를 사용할 수 있다.

### History API

| History.back() | 세션 기록의 바로 뒤 페이지로 이동하는 비동기 메서드. 브라우저의 뒤로 가기를 누르는 것과 같은 효과를 낸다. |
| --- | --- |
| History.forward() | 세션 기록의 바로 앞 페이지로 이동하는 비동기 메서드. 브라우저의 바로 앞으로 가기를 누르는 것과 같은 효과를 낸다. |
| History.go() | 특정한 세션 기록으로 이동하게 해 주는 비동기 메서드. 1을 넣어 호출하면 바로 앞 페이지로, -1을 넣어 호출하면 바로 뒤 페이지로 이동한다. |
| History.pushState() | 주어진 데이터를 세션 기록 스택에 넣는다. 직렬화 가능한 모든 Javascript 객체를 저장하는 것이 가능하다. |
| History.replaceState() | 최근 세션 기록 스택의 내용을 주어진 데이터로 교체한다. |

---
**참고 자료**
- <https://fastcampus.co.kr/dev_online_frontend>
