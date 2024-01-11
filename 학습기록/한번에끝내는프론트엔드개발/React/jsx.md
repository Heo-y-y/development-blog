# JJSX(JavaScript syntax extension)

## JSX(JavaScript syntax extension)이란?

JSX는 자바스크립트의 확장 문법이다. 리액트에서는 이 JSX를 이용해서 화면에서 UI가 보이는 모습을 나타내준다.

JSX를 사용하면 UI를 나타낼 때 자바스크립트(logic)와 HTML구조(markup)를 같이 사용할 수 있기 때문에 기본 UI에 데이터가 변하는 것들이나 이벤트들이 처리되는 부분을 더욱 쉽게 구현할 수 있다.

### React에서 JSX 사용은 의무일까?

의무는 아니지만 자바스크립트 안에서 UI 작업을 하는게 더 편리하기 때문에 React를 사용할 때는 거의 JSX를 사용한다.

### JSX를 사용하지 않는 리액트 화면을 그리는 방식

`React.createElement` API를 사용해서 엘리먼트를 생성한 후(객체가 된다) 이 엘리먼트를 In-Memory에 저장한다. 그리고 `ReactDOM.render` 함수를 사용해서 실제 웹 브라우저에 그려준다.

### JSX는 createElement를 쉽게 사용 가능

모든 UI를 만들때 마다 createElement를 사용해서 컴포넌트를 만들 수는 없다. 그러기에 JSX를 사용한 후 그걸 바벨이 다시 createElement로 바꿔서 사용한다.

### JSX를 사용하면서 주의해야 할 문법(문법 규칙)

JSX를 사용하면서 지켜줘야 할 규칙들이 있다.

- JSX는 컴포넌트에 여러 엘리먼트 요소가 있다면 반드시 부모 요소 하나로 감싸줘야 한다.

## JSX Key 속성 이해하기

React에서 요소의 리스트를 나열할 때는 key를 넣어줘야 한다.

key는 React가 변경, 추가 또는 제거된 항목을 식별하는 데 도움이 된다. 요소에 안정적인 ID를 부여하려면 배열 내부의 요소에 key를 제공해야 한다.

### React는 가상 돔을 이용해서 바뀐 부분만 실제 돔에 적용한다.

가상 돔을 시용해서 바뀐 부분만 실제 돔에 적용해준다고 했는데, React에서는 List를 나열할 때 바뀐 부분만 찾을 때 어떻게 할까?

**key를 이용해서 어떠한 부분이 바뀌었는지 인식할 수 있다.**

key에는 유니크한 값을 넣어줘야 한다.

index도 0부터 시작해서 유니크한 값을 가지지만 만약 List가 추가되거나 제거되면 해당 List들의 key값도 바뀌게 된다.

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
