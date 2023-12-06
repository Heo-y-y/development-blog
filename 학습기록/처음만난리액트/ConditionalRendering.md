# Conditional Rendering

## Conditional Rendering의 정의와 Inline Conditions

### Conditional Rendering

먼저 컨디션이라고 하면 조건, 상태라는 뜻을 가지고 있다. 보통 우리가 오늘 컨디션이 좋다, 나쁘다라고 표현할 때 컨디션의 뜻은 상태라는 의미를 가지고 있다. 그래서 컨디션이 좋다 나쁘다라는 것은 상태가 좋다, 나쁘다라는 뜻이 된다.

하지만 컴퓨터 프로그래밍에서의 **컨디션**은 **조건**을 의미한다. 따라서 **Conditional Rendering**이라고 하면 **조건에 따른 렌더링**이라고 해석하고 일반적으로 **조건부 렌더링**이라고 줄여서 부른다. 결론적으로 조건부 렌더링은 **어떠한 조건에 따라서 렌더링이 달라지는 것**을 의미한다. 여기에서의 조건은 우리가 프로그래밍에서 사용하는 **조건문**이라고 생각하면 된다. 조건문의 결과는 true 아니면 false가 나오는데, 이 **결과에 따라서 렌더링을 다르게 처리**하는 것을 조건부 렌더링이라고 정의하는 것이다.

예를 들어, 조건문의 값이 true이면 버튼을 보여주고 false이면 버튼을 가리는 것도 하나의 조건부 렌더링이라고 할 수 있다.

**예제 코드**

```jsx
function UserGreeting(props) {
    return <h1>다시 오셨군요!</h1>;
}

function GuestGreeting(props) {
    return <h1>회원가입을 해주세요.</h1>;
}
```

`UserGreeting`은 이미 회원인 사용자에게 보여줄 메시지를 출력하는 컴포넌트이고, `GuestGreeting`는 아직 가입하지 않은 게스트 사용자에게 보여줄 메시지를 출력하는 컴포넌트이다.

이제 회원인지 아닌지에 따라 이 두개의 컴포넌트를 선택적으로 보여줘야 한다.

**예제 코드**

```jsx
function Greeting(props) {
    const isLoggeIn = props.isLoggedIn;

    // 조건부 렌더링
    if (isLoggedIn) {
        return <UserGreeting />;
    }
    return <GuestGreeting />;
}
```

위 코드에서는 조건부 렌더링을 사용하여 구현하고 있다. 여기서 `Greeting` 컴포넌트는 `isLoggedIn`이라는 변수의 값이 `ture`에 해당하면 `UserGreeting` 컴포넌트를 `return`하고, 그렇기 않으면 `GuestGreeting` 컴포넌트를 `return`한다.

`props`로 들어오는 `isLoggedIn` 의 값에 따라서 화면에 출력되는 결과가 달라지게 된다. 이처럼 **조건에 따라 렌더링의 결과가 달라지도록 하는 것**을 **조건부 렌더링**이라고 한다.

### JavaScript의 Truthy와 Falsy

보통의 프로그래밍 언어에서는 참과 거짓을 구분하기 위해 Boolean 형태의 자료형이 존재하고, 그 값은 true와 false 둘 중에 하나가 된다. 그리고 만약 Boolean과 자료형이 다른 값을 비교하게 되면 오류가 발생하게 된다.

하지만 **자바스크립트**에서는 **true는 아니지만 true로 여겨지는 값**이 존재하는데 이것을 **Truthy**라고 부른다. 마찬가지로 **false는 아니지만 false로 여겨지는 값**을 **Falsy**라고 부른다.

```jsx
// Truthy
true
{} (empty object)
[] (empty array)
42 (number, not zero)
"0", "false" (string, not empty)

// Falsy
false
0, -0 (zero, minus zero)
0n (Bigint zero)
'', "", `` (empty string)
null
undefined
NaN (not a number)
```

### Element Variables

조건부 렌더링을 사용하다 보면 **렌더링 해야 될 컴포넌트를 변수처럼 다루고 싶을 때**가 있다. 이때 사용할 수 있는 것이 바로 **Element Variables**이다. 한글로는 **엘리먼트 변수**라고 부른다. 엘리먼트 변수는 이름 그대로 **리액트의 엘리먼트를 변수처럼 다루는 방법**이다.

**예제 코드**

```jsx
function LoginButton(props) {
    return (
        <button onClick={props.onClick}>
            로그인
        </button>
    );
}

function LogoutButton(props) {
    return (
        <button onClick={props.onClick}>
            로그아웃
        </button>
    )
}
```

위 코드에는 로그인 버튼과 로그아웃 버튼 두 개의 컴포넌트가 있다.

**예제 코드**

```jsx
function LoginControl(props) {
    const [isLoggedIn, setIsLoggedIn] = useState(false);

    const handleLoginClick = () => {
        setIsLoggedIn(true);
    }

    const handleLogoutClick = () => {
        setIsLoggedIn(false);
    }

    // isLoggedIn의 값에 따라서 버튼이라는 변수의 컴포넌트를 대입
    let button;
    if (isLoggedIn) {
        button = <LogoutButton onClick={handleLogoutClick} />;
    } else {
        button = <LoginButton onClick={handleLoginClick} />;
    }

    return (
        <div>
            <Greeting isLoggedIn={isLoggedIn} />
						// 컴포넌트가 대입된 변수를 return에 넣어 렌더링
            {button}
        </div>
    )
}
```

위 코드의 로그인 컨트롤 컴포넌트는 사용자의 로그인 여부에 따라 앞에 나온 로그인 버튼과 로그아웃 버튼 두 개의 컴포넌트를 선택적으로 보여주게 된다.

위 코드처럼 **엘리먼트를 변수처럼 저장해서 사용하는 방법**을 **Element Variables**이라고 한다. 그리고 이렇게 별도로 변수를 선언하여 조건부 렌더링을 할 수도 있지만, inline 조건문을 사용하면 조금 더 코드를 간결하게 작성할 수 있다.

### Inline Conditions

먼저 Inline이라는 단어는 In + Line이라는 의미를 가지고 있다. 말 그대로 코드를 별도로 분리된 곳에 작성하지 않고, **해당 코드가 필요한 곳 안에 직접 집어 넣는다**는 뜻이다. 그래서 **Inline Conditions**라고 하면 **조건문을 코드 안에 집어 넣는 것**이라는 의미이다.

- **Inline If**

Inline If는 `if`**문을 필요한 곳에 직접 집어 넣어 사용하는 방법**이다. 다만 실제로 `if`문을 넣는 것은 아니고, `if`문과 동일한 효과를 내기 위해 `&&` **연산자를 사용**한다. 

Inline If는 이 `&&` **연산자를 JSX 코드 안에서 중괄호를 사용하여 직접 집어 넣는 방법**이다.

```jsx
function Mailbox(props) {
    const unreadMessages = props.unreadMessages;

    return (
        <div>
            <h1>안녕하세요!</h1>
            {unreadMessages.length > 0 &&
                <h2>
                    현재 {unreadMessages.length}개의 읽지 않은 메시지가 있습니다.
                </h2>
            }
        </div>
    );
}
```

위 코드를 보면 `unreadMessages.length`가 0보다 큰지 평가하는 조건문의 결과 값에 따라서 뒤에 나오는 `h2` 태그로 둘러싸인 부분이 렌더링이 되거나 안되거나하게 된다. 만약 `unreadMessages.length`가 0보다 크다면 뒤에 나오는 `h2` 태그 부분이 렌더링 될 것이고, 0보다 작으면 아무것도 렌더링 되지 않을 것이다.

`&&` 연산자를 사용하는 이러한 패턴은 단순하지만 리액트에서 굉장히 많이 사용한다.

참고로 유의할 점이 `&&` 연산자를 사용할 때 조건문에 false expression을 사용하면 뒤에 나오는 expression은 평가되지 않지만 false expression의 결과 값이 그대로 리턴되기 때문에 주의해야 한다.

```jsx
function Counter(props) {
    const count = 0;

    return (
        <div>
            {count && <h1>현재 카운트: {count}</h1>}
        </div>
    );
}
```

예를 들어 위 코드의 결과는 화면에 아무것도 안 나오는 것이 아니라 `count` 값인 0이 들어가서 `div` 태그를 열고, 다음에 숫자 0, 그 다음에 `div` 태그가 닫아집니다.

- **Inline If-Else**

Inline If-Else는 `if-else`문을 **필요한 곳에 직접 넣어서 사용하는 방법**이다. If-Else 문의 경우 `?`**연산자를 사용**한다. 즉, **조건문의 값에 따라서 다른 엘리먼트를 보여줄 때 사용**한다.

```jsx
function UserStatus(props) {
    return (
        <div>
            이 사용자는 현재 <b>{props.isLoggedIn ? '로그인' : '로그인하지 않은'}</b> 상태입니다.
        </div>
    )
}
```

위 코드에서 보면 true인 경우 ‘로그인’을 출력하고, false인 경우 ‘로그인하지 않은’을 출력한다.

```jsx
function LoginControl(props) {
    const [isLoggedIn, setIsLoggedIn] = useState(false);

    const handleLoginClick = () => {
        setIsLoggedIn(true);
    }

    const handleLogoutClick = () => {
        setIsLoggedIn(false);
    }

    return (
        <div>
            <Greeting isLoggedIn={isLoggedIn} />
            {isLoggedIn
                ? <LogoutButton onClick={handleLogoutClick} />
                : <LoginButton onClick={handleLoginClick} />
            }
        </div>
    )
}
```

위 코드처럼 문자열이 아닌 엘리먼트를 넣어서 사용할 수도 있다.

여기에서는 true인 경우 로그아웃 버튼을 출력하고, false인 경우 로그인 버튼을 출력한다.

이처럼 **Inline-If-Else**는 **조건에 따라 각기 다른 엘리먼트를 렌더링하고 싶을 때 사용**한다.

### Component 렌더링 막기

컴포넌트를 렌더링하고 싶지 않을 때는 어떻게 할까?

`null`을 리턴하면 렌더링이 되지 않는다.

```jsx
function WarningBanner(props) {
    if (!props.warning) {
        return null;
    }

    return (
        <div>경고!</div>
    );
}
```

여기서 `WarningBanner`라는 컴포넌트는 `props.warning`의 값이 false인 경우에 `null`을 리턴한다. 다시 말하면 `props.warning`의 값이 true인 경우에만 경고 메시지를 출력하고 `null`인 경우에는 아무것도 호출하지 않는 것이다.

```jsx
function MainPage(props) {
    const [showWarning, setShowWarning] = useState(false);

    const handleToggleClick = () => {
        setShowWarning(prevShowWarning => !prevShowWarning);
    }
    
    return (
        <div>
            <WarningBanner warning={showWarning} />
            <button onClick={handleToggleClick}>
                {showWarning ? '감추기' : '보이기'}
            </button>
        </div>
    )
}
```

`MainPage` 컴포넌트는 `showWarning` 이라는 `state`의 값을 `WarningBanner` 컴포넌트의 `props`로 전달하여 `showWarning`의 값에 따라 경고문을 표시하거나 가린다. 이처럼 리액트에서는 특정 컴포넌트를 렌더링하고 싶지 않을 경우 `null`을 리턴하면 된다

참고로 **클래스 컴포넌트**의 렌더 함수에서 `null`을 리턴하는 것은 **컴포넌트의 생명주기 함수에 전혀 영향을 미치지 않는다**. 예를 들면, `ComponentDidUpdate` 함수는 여전히 호출된다.

**참고 자료**

- [https://www.inflearn.com/course/lecture?courseSlug=처음-만난-리액트&unitId=113228&category=questionDetail&tab=curriculum](https://www.inflearn.com/course/lecture?courseSlug=%EC%B2%98%EC%9D%8C-%EB%A7%8C%EB%82%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8&unitId=113228&category=questionDetail&tab=curriculum)
