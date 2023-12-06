# Handling Events

## Event의 정의 및 Event 다루기

### Event

컴퓨터 프로그래밍에서 **이벤트**는 **사건**이라는 의미를 가지고있다. 즉, **특정 사건**을 의미하는 것이다.

예를 들면, 사용자가 버튼을 클릭하는 사건도 하나의 이벤트라고 볼 수 있다. 여기서 버튼을 클릭한 사건을 버튼 클릭 이벤트라고 부른다.

웹 사이트에는 굉장히 다양한 이벤트들이 있는데, 이러한 이벤트들이 발생했을 때 원하는 대로 처리를 잘 해주어야 웹 사이트가 정상적으로 돌아간다. 이때 **이벤트를 처리하는 것**을 **이벤트 핸들링 한다** 라고 표현한다.

먼저 리액트에서 이벤트를 처리하는 방법을 알아보기 전에 DOM의 이벤트에 대해서 알아보자.

**DOM의 Event**

```jsx
<button onclick="activate()">
    Activate
</button>
```

위 코드는 DOM에서 클릭 이벤트를 처리하는 예제 코드이다. 버튼이 눌리면 `Activate`라는 함수를 호출하도록 되어있다. 이처럼 **DOM**에서는 **클릭 이벤트를 처리할 함수를** `onclick`**을 통해서 전달**한다.

그렇다면 리액트에서는 이벤트를 어떻게 처리할까?

```jsx
<button onClick={activate}>
    Activate
</button>
```

위 코드는 앞에서 보여준 코드와 동일한 기능을 하도록 만든 리액트 코드이다. 자세히 보면 DOM의 이벤트와는 조금 다른 부분이 있는데, 먼저 이벤트의 이름인 `onclick`이 `c`가 대문자로 되어 있는 `onClick`으로 camel case 즉, camel 표기법으로 쓰여 있다.

그리고 두 번째 다른 점은 **DOM**에서는 **이벤트를 처리할 함수를 문자열로 전달**하지만, **리액트**에서는 **함수 그대로 전달**한다.

이처럼 DOM에도 이벤트가 있고, 리액트에도 이벤트가 있지만 사용하는 방법이 조금 다르다. 정리하면, 둘 사이에는 **이벤트 이름의 표기법**과 **함수를 전달하는 방식**의 **차이**가 있다.

### Event Handler

위에서 본 예제 코드처럼 **어떤 이벤트가 발생했을 때 해당 이벤트를 처리하는 함수**가 있는데, 이것을 **이벤트 핸들러**라고 부른다. 즉, **어떤 사건이 발생하면 사건을 처리하는 역할**을 하는 것이다. 이벤트 핸들러는 **이벤트가 발생하는 것을 계속 듣고 있는다는 의미**로 **이벤트 리스너**라고 부르기도 한다.

그렇다면 이벤트 핸들러는 어떻게 추가할까?

**예제 코드**

```jsx
class Toggle extends React.Component {
    constructor(props) {
        super(props);

        this.state = { isToggleOn: true };

        // callback에서 'this'를 사용하기 위해서는 바인딩을 필수적으로 해줘야 한다.
        this.handleClick = this.handleClick.bind(this);
    }

    handleClick() {
        this.setState(prevState => ({
            isToggleOn: !prevState.isToggleOn
        }));
    }

    render() {
        return (
            <button onClick={this.handleClick}>
                {this.state.isToggleOn ? '켜짐' : '꺼짐'}
            </button>
        );
    }
}
```

이 코드에는 `Toggle` 클래스 컴포넌트가 나온다. 그리고 컴포넌트의 `state`에는 `isToogleOn`이라는 `Boolean` 변수가 하나 있다. 버튼을 클릭하면 이벤트 핸들러 함수인 `handleClick` 함수를 호출하도록 되어 있는데, 여기서 자세히 봐야할 것은 `handleClick` 함수를 정의하는 부분과 `bind`를 사용하는 부분이다.

먼저 `handleClick` 함수를 정의하는 부분은 일반적인 함수를 정의하는 것과 동일하게 괄호와 중괄호를 사용해서 클래스의 함수로 정의한다. 이렇게 정의한 함수를 생성자에서 `bind`를 이용하여 `this.handleClick`에 대입해 준다. JSX에서 `this`의 의미에 대해 유의해야 하는데, `bind`를 하는 이유는 자바스크립트에서는 기본적으로 클래스 함수들이 바운드되지 않기 때문이다. 그래서 `bind`를 하지 않으면 `this.handleClick`은 **글로벌 스코프에서 호출**되는데, 글로벌 스코프에서 `this.handleClick`은 undefinded임으로 사용할 수가 없다. 이것은 리액트에만 해당되는 내용이 아니라 **자바스크립트 함수의 작동 원리의 일부분**이다. 그래서 **일반적으로 함수의 이름 뒤에 괄호 없이 사용하려면 무조건 해당 함수를** `bind` **해줘야 한다**.

**예제 코드**

```jsx
class MyButton extends React.Component {
    handleClick = () => {
        console.log('this is:', this);
    }

    render() {
        return (
            <button onClick={this.handleClick}>
                클릭
            </button>
        );
    }
}
```

`bind`를 사용하는 것이 번거롭다면 위 코드처럼 **Class fields syntax**를 사용할 수도 있다.

**예제 코드**

```jsx
class MyButton extends React.Component {
    handleClick() {
        console.log('this is:', this);
    }

    render() {
        // 이렇게 하면 `this`가 바운드된다.
        return (
            <button onClick={() => this.handleClick()}>
                클릭
            </button>
        );
    }
}
```

그리고 `bind`도 사용하지 않고, Class fields syntax 문법도 사용하지 않으려면 위 코드와 같이 이벤트 핸들러를 넣는 곳에 **Arrow function**을 사용하는 방법도 있다.

이 방식은 `MyButton` **컴포넌트가 렌더링 될 때마다 다른 콜백 함수가 생성되는 문제**가 있다. 대부분의 경우에는 상관없지만 이 콜백 함수가 하위 컴포넌트에 풀합으로 넘겨지게 되면 하위 컴포넌트에서 추가적인 렌더링이 발생하게 된다. 그래서 **성능 문제를 피하기 위해**서는 **bind나 Class fields syntax를 사용하는 것을 권장**한다.

그렇다면 함수 컴포넌트에서는 이벤트를 어떻게 처리할까?

**예제 코드**

```jsx
class Toggle(props) {
    const [isToggleOn, setIsToggleOn] = useState(true);

    // 방법 1. 함수 안에 함수로 정의
    function handleClick() {
        setIsToggleOn((isToggleOn) => !isToggleOn);
    }

    // 방법 2. arrow function을 사용하여 정의
    const handleClick = () => {
        setIsToggleOn((isToggleOn) => !isToggleOn);
    }

    return (
        <button onClick={handleClick}>
            {isToggleOn ? "켜짐" : "꺼짐"}
        </button>
    );
}
```

앞에서 나온 `Toggle` 컴포넌트를 함수 컴포넌트로 변환하면 위 코드와 같다.

**함수 컴포넌트 내부에서 이벤트 핸들러를 정의하는 방법**은 1처럼 **함수 안에 또 다른 함수로 정의하는 방법**과 방법 2처럼 **Arrow function을 사용하여 정의하는 방법**이 있다. 그리고 **함수 컴포넌트에서는 이벤트를 넣어줄 때** `this`를 사용하지 않고, 위 코드처럼 `onClick`에 **곧바로 정의한 이벤트 핸들러를 넘기면 된다**.

### Arguments 전달하기

이벤트 핸들러에 Arguments를 전달하는 방법을 알아보자.

먼저 영어 단어 **Arguments**는 **주장, 논쟁, 말다툼**이라는 뜻을 가지고 있다. 여기에서는 **주장**이라는 뜻과 더 가까운데, **함수에 주장할 내용**이라고 이해하면 된다. 다시 말하면 **함수에 전달할 데이터**를 **Arguments**라고 한다. 여기서의 함수는 이벤트 핸들러를 의미하기 때문에 이벤트 핸들러에 전달할 데이터라는 의미가 된다. 같은 의미로 **파라미터**라는 용어도 많이 사용한다. 우리 말로는 **매개변수**라고도 부른다. 이벤트 핸들러에 매개변수를 전달해야할 경우는 굉장이 많다. 예를 들어, 특정 사용자 프로필을 클릭했을 때 해당 사용자의 아이디를 매개변수로 전달해서 정해진 작업을 처리해야 하는 경우를 들 수 있다.

**예제 코드**

```jsx
<button onClick={(event) => this.deleteItem(id, event)}>삭제하기</button>
<button onClick={this.deleteItem.bind(this, id)}>삭제하기</button>
```

위 클래스 컴포넌트 예제 코드를 통해 어떤식으로 매개 변수를 이벤트 핸들러에 전달하는지 알아보자.

위 코드 두 줄은 모두 동일한 역할을 하지만, 하나는 Arrow function을 사용했고, 다른 하나는 bind를 사용했다. **이벤트라는 매게변수**는 **리액트의 이벤트 객체**를 의미한다. 두 방법 모두 첫번째 매개변수는 `id`이고, 두번째 매개변수로 이벤트가 전달된다. 

첫번째 Arrow function을 사용한 방법은 명시적으로 이벤트를 두번째 매개변수로 넣어주었고, 두번째 bind를 사용한 방법은 이벤트가 자동으로 `id` 이후에 두번째 매개변수로 전달된다. 이렇게 사용하는 방식은 클래스 컴포넌트에서 사용하는 방식이므로 지금은 거의 사용하지 않는다.

**함수 컴포넌트에서 이벤트 핸들러에 매개변수를 전달할 때** 아래와 같이 하면 된다.

```jsx
class MyButton(props) {
    const handleDelete = (id, event) => {
        console.log(id, event.target);
    };

    return (
        <button onClick={(event) => handleDelete(1, event)}>
            삭제하기
        </button>
    );
}
```

참고로 매개변수의 순서는 원하는 대로 변경해도 상관없다.

**참고 자료**

- [https://www.inflearn.com/course/lecture?courseSlug=처음-만난-리액트&unitId=113276&category=questionDetail](https://www.inflearn.com/course/lecture?courseSlug=%EC%B2%98%EC%9D%8C-%EB%A7%8C%EB%82%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8&unitId=113276&category=questionDetail)
