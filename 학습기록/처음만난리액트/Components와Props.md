# **Components and Props**

## Components와 Props의 정의

### Components

일단 앞서서 **리액트**는 **컴포넌트 기반의 구조**라는 중요한 특징이 있다고 했다.

리액트에서는 모든 페이지가 컴포넌트로 구성되어 있고, 하나의 컴포넌트는 또 다른 여러 개의 컴포넌트의 조합으로 구성될 수 있다.

그리고 이러한 컴포넌트들은 마치 레고 블록을 조립하듯이 끼워 맞춰서 새로운 컴포넌트를 만들 수 있는 것이다.

즉, 리액트가 컴포넌트 기반이라고 부르는 이유는 작은 컴포넌트들이 모여서 하나의 컴포넌트를 구성하고, 또 이러한 컴포넌트들이 모여서 전체 페이지를 구성하기 때문입니다.

이렇게 하나의 컴포넌트를 반복적으로 사용함으로써 전체 코드의 양을 줄일 수 있어서 개발 시간과 유지 보수 비용을 줄일 수 있다.

![스크린샷 2023-11-27 오후 9.22.21.png](https://github.com/Heo-y-y/development-blog/assets/112863029/d6b6c23b-e414-4ef2-97c5-af9c9f053d76)

리액트 컴포넌트는 개념적으로는 자바스크립트의 함수와 비슷하다.

함수가 입력을 받아서 출력을 내뱉는 것처럼 리액트 컴포넌트도 입력을 받아서 정해진 출력을 내뱉는다. 그래서 리액트 컴포넌트를 그냥 하나의 함수라고 생각하면 좀 더 쉽게 개념을 이해할 수 있다.

하지만 리액트 컴포넌트의 입력과 출력은 일반적인 자바스크립트 함수와는 조금 다릅니다.

![스크린샷 2023-11-28 오전 12.00.05.png](https://github.com/Heo-y-y/development-blog/assets/112863029/87d070cd-a001-4cf5-8ec6-1201f4f3b726)

리액트 컴포넌트에서의 입력은 Props 라는 것이고, 출력은 React element이다.

결국 리액트 컴포넌트가 해주는 역할은 어떠한 속성들을 입력으로 받아서 그에 맞는 React element를 생성하여 리턴해주는 것이다.

리액트 컴포넌트는 만들고자 하는 대로 Props 즉, 속성을 넣으면 해당 속성에 맞춰서 화면에 나타날 element를 만들어주는 것이다.

즉, 앞서서 설명한 붕어빵이 만들어지는 과정과 비슷하다. 객체 지향과 합쳐서 설명하면 클래스라는 붕어빵 틀에서 인스턴스라는 실제 붕어빵이 만들어져서 나온다고 생각하면 된다. 정리하면, 리액트 컴포넌트는 객체 지향까지는 아니지만 비슷한 개념을 가지고 있다고 이해하면 된다.

### Props

먼저 Props는 Prop 뒤에 복수형을 나타내는 알파벳 s를 붙여서 Prop이 여러 개인 것을 의미한다.

그럼 Prop은 무엇일까?

Prop은 property라는 영어 단어를 줄여서 쓴 것이다. property는 재산이라는 뜻도 있지만 속성, 특성이라는 뜻도 갖고 있는데 리액트에서는 속성이라는 뜻으로 사용된다.

그러면 무엇의 속성일까?

바로 **리액트 컴포넌트의 속성**이다.

![스크린샷 2023-11-28 오전 12.14.02.png](https://github.com/Heo-y-y/development-blog/assets/112863029/e26427b6-164b-4c34-a19c-f74f9e8852b2)

앞서서 붕어빵을 예시로 들면, 리액트 컴포넌트는 붕어빵 틀에 해당한다고 말했다. Props는 바로 붕어빵에 들어가는 재료를 의미한다고 보면 된다. 같은 붕어빵이라도 어떤 재료를 넣느냐에 따라 다른 맛이 난다.

이처럼 Props는 같은 리액트 컴포넌트에서 눈에 보이는 글자나 색깔 등이 속성을 바꾸고 싶을 때 컴포넌트의 속 재료라고 생각하면 된다.

**Props**는 **컴포넌트에 전달할 다양한 정보를 담고 있는 자바스크립트 객체**이다.

우리가 컴포넌트의 어떤 데이터를 전달하고, 전달된 데이터에 따라 다른 모습의 element를 화면에 렌더링하고 싶을 때 해당 데이터를 Props에 넣어서 전달하는 것이다.

## **Props의 특징 및 사용법**

### Props의 특징

props의 중요한 특징은 **read-only** 즉, 읽기 전용이라는 것이다.

읽을 수만 있다는 것은 값을 변경할 수 없다는 것과 같다.

**props의 값은 리액트 컴포넌트가 element를 생성하기 위해서 사용하는 값**이다. 그런데 이 값들이 element를 생성하는 도중에 갑자기 바뀌어 버리면 제대로 된 element가 생성될 수 없을 것이다. 마치 붕어빵이 다 구워진 이후에 속 재료를 바꿀 수 없는 것과 마찬가지이다. 다 구워진 이후에 속재료를 바꾸려면 붕어빵의 배를 갈라야 하는데, 그렇게 하면 제대로 된 상품으로 판매할 수가 없을 것이다.

그렇다면 다른 props의 값으로 element를 생성하려면 어떻게 해야 할까?

그럴 경우 **새로운 값을 컴포넌트에 전달하여 새로 element를 생성**하면 된다. 이 과정에서 element가 다시 렌더링 되는 것이다. 

자바 스크립트의 함수의 속성을 잠깐 살펴보자.

**JavaScript 함수의 속성**

```jsx
function sum(a, b) {
    return a + b;
}
```

여기에는 `sum`이라는 이름을 가진 함수가 있다. 이 함수에서는 `a`와 `b`라는 파라미터 값을 변경하지 않고 있다. 그리고 `a`와 `b`라는 파라미터 집합의 값이 같은 경우에는 항상 같은 값을 리턴할 것이다. 우리는 이러한 함수를 Pure하다 라고 한다. 말 그대로 함수가 순수하다는 뜻인데, 이 말은 입력 값을 변경하지 않으며, 같은 입력 값에 대해서는 항상 같은 출력 값을 리턴한다는 의미입니다.

그렇다면 함수가 순수하지 않은 경우도 살펴보자.

```jsx
function withdraw(account, amount) {
    account.total -= amount;
}
```

여기에는 `withdraw`라는 함수가 있다. 쉽게 설명하면 계좌에서 출금을 하는 함수인데, 은행 계좌 정보와 총액을 파라미터로 받아서 계좌의 현재 총 잔액을 나타내는 `total`에서 출금할 금액인 `amount`를 빼는 것이다. 해당 코드에서 함수는 입력으로 받은 파라미터 `account`의 값을 변경했다. 이런 경우 우리는 Impure하다 라고 한다. 순수하지 않다는 뜻이다.

해당 설명에서 Pure 함수와 Impure 함수는 리액트 컴포넌트의 정의와 관련이 있다.

아래 문장은 리액트 공식 문서에 나오는 컴포넌트의 특징을 설명한 문장이다.

> All React components must act like pure functions with respect to their props.
> 

해당 문장을 해석하면 “모든 리액트 컴포넌트는 그들의 Props에 관해서는 Pure 함수 같은 역할을 해야 한다.” 이다.

이해가기 좀더 쉽게 설명하면 “**모든 리액트 컴포넌트는 Props를 직접 바꿀 수 없고, 같은 Props에 대해서는 항상 같은 결과를 보여줄것!**” 이다.

앞에서 리액트 컴포넌트가 자바스크립트의 함수와 같은 개념이라고 설명했다. 그렇기 때문에 리액트 컴포넌트의 입력으로 들어오는 Props는 자바스크립트 함수의 파라미터와 같다. 함수가 Pure하다는 것은 함수의 입력 값인 파라미터를 바꿀 수 없다는 의미를 포함하고 있기 때문에 **리액트 컴포넌트에서는 Props를 바꿀 수 없다는 의미**가 된다. 그리고 Pure 함수는 같은 입력 값에 대해서는 항상 같은 결과를 보여줘야 하기 때문에 **리액트 컴포넌트 관점에서 같은 Props에 대해서 항상 같은 결과를 보여줘야 한다는 의미**가 된다. 여기에서의 결과는 리액트의 element가 된다.

정리하면, **리액트 컴포넌트의 Props는 바꿀 수 없고, 같은 Props가 들어오면 항상 같은 element를 리턴**해야 한다고 이해하면 된다.

### Props 사용법

위에서 **Props**가 **컴포넌트에 전달할 다양한 정보를 담고 있는** **자바스크립트 객체**라고 설명했다. 그렇다면 컴포넌트에 Props라는 객체를 전달하기 위해서는 어떻게 해야 하는지 살펴보자.

**JSX를 사용한 경우**

```jsx
function App(props) {
    return (
        <Profile
            name="Heo"
            introduction = "안녕하세요, Heo입니다."
            viewCount={1500}
        />
    );
}
```

JSX를 사용하는 경우에는 위 코드와 같이 키와 값으로 이루어진 키와 값 상의 형태로 컴포넌트에 `props`를 넣을 수 있다. 위에서는 `Profile` 컴포넌트에 `name`, `introduction`, `viewCount`라는 3가지 속성을 넣었다. 

여기서 중요한 부분은 각 속성에 값을 넣을 때 중괄호를 사용한 것과 사용하지 않은 것의 차이인데, 앞에서는 중괄호를 사용할 때 무조건 자바스크립트 코드가 들어간다고 했다. 그래서 마찬가지로 `props`에 값을 넣을 때에도 문자열 이외에 정수, 변수 그리고 다른 컴포넌트 등이 들어갈 경우에는 중괄호를 사용해서 감싸주어야 한다. 물론 문자열도 중괄호로 감싸도 상관은 없다.

이렇게 하면 이 속성 값들이 모두 `Profile` 컴포넌트의 `props`로 전달되며 `props`는 아래와 같은 형태의 자바스크립트 객체가 된다.

```jsx
{
    name: "Heo",
    introduction: "안녕하세요, Heo입니다.",
    viewCount: 1500
}
```

그리고 `props` 의 중괄호를 사용해서 아래와 같이 `props` 의 값으로 컴포넌트도 넣을 수 있다.

```jsx
function App(props) {
    return (
        <Layout
            width={2560}
            height={1440}
            header={
                <Header title="Heo의 블로그입니다." />
            }
            footer={
                <Footer />
            }
        />
    );
}
```

이렇게 하면 `Layout` 컴포넌트의 `props`로는 정수 값을 가진 `width`, `height`과 리액트 `element`로 `header`, `footer`가 들오게 된다.

이처럼 JSX를 사용하는 경우에는 간단하게 컴포넌트에 `props`를 넣을 수 있다.

**CreateElement 함수를 사용하는 경우**

```jsx
React.createElement(
    type,
    [props],
    [...children]
)
```

해당 코드에서 두번째 파라미터가 바로 `props`이다. 여기에 자바스크립트 객체를 넣으면 그게 곧 해당 컴포넌트의 `props`가 된다.

위에서 작성한 코드를 JSX를 사용하지 않고, 작성하면 아래와 같다.

```jsx
React.createElement(
    Profile,
    {
        name: "Heo",
        introduction: "안녕하세요, Heo입니다.",
        viewCount: 1500
    },
    null
);
```

`type`은 컴포넌트 이름인 `Profile`이 들어가고, `props`로 자바스크립트 객체가 들어간다. 그리고 마지막으로 하위 컴포넌트가 없기 때문에 `children`에는 `null`이 들어갔다.

앞에서 리액트로 개발할 때는 무조건 JSX를 사용하는 것이 좋다고 했다. 그렇기 때문에 이 코드는 참고만 하고, 실제로 props를 사용할 때는 JSX를 사용하자!

## Component 만들기 및 렌더링

### Component 만들기

![스크린샷 2023-11-28 오후 5.04.25.png](https://github.com/Heo-y-y/development-blog/assets/112863029/bc0861c3-f1cb-4494-8de2-6c654bc63d43)

리액트에서의 컴포넌트는 위 그림처럼 크게 **클래스 컴포넌트**와 **함수 컴포넌트**로 나뉜다. 리액트 초기 버전에서는 클래스 컴포넌트를 주로 사용했는데, 불편하다는 의견이 많이 나와 이후에는 함수 컴포넌트를 개선해서 주로 사용하게 됐다. 함수 컴포넌트를 개선하는 과정에서 개발된 것이 바로 **Hook**이라는 것인데 일단은 알아만 두자.

현재 리액트 개발에서는 거의 **Hook**을 사용한다고 생각하면 된다.

### Function Component

앞에서 props에 대해서 설명할 때 모든 리액트 컴포넌트는 pure 함수 같은 역할을 한다고 했다. 이 말은 결국 리액트의 컴포넌트를 일종의 함수라고 생각한다는 뜻이다.

**Function Component 예제**

```jsx
function Welcome(props) {
    return <h1>안녕, {props.name}</h1>;
}
```

위 함수의 경우 하나의 `props` 객체를 받아서 인삿말이 담긴 리액트 element를 리턴하기 때문에 리액트 컴포넌트라고 할 수 있다. 이렇게 생긴 것을 함수 컴포넌트라고 부른다.

보면 코드가 굉장히 간단하다는 걸 느낄 수 있다. 함수 컴포넌트는 이처럼 간단한 코드를 장점으로 가진다고 할 수 있다.

### Class Component

클래스 컴포넌트는 자바스크립트 ES6의 class라는 것을 사용해서 만들어진 형태의 컴포넌트이다. 클래스 컴포넌트의 경우에는 함수 컴포넌트에 비해서 몇 가지 추가적인 기능을 갖고 있다.

**Class Component 예제**

```jsx
class Welcome extends React.Component {
    render() {
        return <h1>안녕, {this.props.name}</h1>;
    }
}
```

해당 코드는 위에서 작성한 함수 컴포넌트와 동일한 역할을 하는 코드이다. 함수 컴포넌트와 가장 큰 차이점은 **리액트의 모든 클래스 컴포넌트는** `React.Component`**를 상속받아서 만든다**는 것이다. 

위 코드는 `React.Component`를 상속받아서 `Welcome`이라는 클래스를 만들었고, `React.Component`를 상속받았기 때문에 결과적으로 **리액트 컴포넌트가 된다**.

### Component의 이름

컴포넌트의 이름을 어떻게 지어야 할까?

- 컴포넌트의 이름은 **항상 대문자로 시작**해야 한다.
    - 리액트는 소문자로 시작하는 컴포넌트를 DOM 태그로 인식한다.

**HTML div 태그로 인식(Dom 태그)**

```jsx
const element = <div />;
```

**Welcome이라는 리액트 컴포넌트로 인식**

```jsx
const element = <Welcom name="웰컴" />;
```

### Component 렌더링

컴포넌트를 다 만들고, 실제로 렌더링을 하려면 어떻게 할까?

앞에서 설명한 것 처럼 컴포넌트는 붕어빵의 틀에 해당한다 그래서 실제로 화면에 렌더링되는 것은 아니다. 컴포넌트라는 붕어빵 틀을 통하여 찍어줘서 나온 element라는 붕어빵이 실제로 화면에 보이게 되는 것이다.

그렇다면 렌더링을 위해서는 가장 먼저 컴포넌트로부터 element를 만들어야 할 것이다.

**DOM 태그를 사용한 element**

```jsx
const element = <div />;
```

**사용자가 정의한 Component를 사용한 element**

```jsx
const element = <Welcom name="웰컴" />;
```

위 두개의 코드는 모두 리액트 element를 만들어내게 된다. 그러면 우리는 이 element를 렌더링하면 되는 것이다.

**렌더링하는 코드**

```jsx
function Welcome(props) {
    return <h1>안녕, {props.name}</h1>;
}

const element = <Welcome name="웰컴" />;
ReactDOM.render(
    element,
    document.getElementById('root')
);
```

위 코드는 먼저 `Welcome` 이라는 함수 컴포넌트를 선언하고 있다. 그리고 `Welcome name=”웰컴”` 이라는 값을 가진 엘리먼트를 파라미터로 해서 `React.DOM.render` 함수를 호출한다. 이렇게 하면 리액트는 `Welcome` 컴포넌트에 `name` 웰컴이라는 props를 넣어서 호출하고, 그 결과로 리액트 element가 생성된다. 이렇게 생성된 element는 최종적으로 ReactDOM을 통하여 실제 DOM에 효과적으로 업데이트 되고, 우리가 브라우저를 통해서 볼 수 있게 되는 것이다.

## Component 합성과 추출

### Component 합성

컴포넌트 합성은 여러 개의 컴포넌트를 합쳐서 하나의 컴포넌트를 만드는 것이다.

**리액트**에서는 **컴포넌트 안에도 다른 컴포넌트를 사용할 수 있기 때문에 복잡한 화면을 여러 개의 컴포넌트로 나눠서 구현**할 수 있다.

**예제 코드**

```jsx
function Welcome(props) {
    return <h1>안녕, {props.name}</h1>;
}

function App(props) {
    return(
        <div>
            <Welcome name="Mike" />
            <Welcome name="Heo" />
            <Welcome name="Kim" />
        </div>
    )
}

ReactDOM.render(
    <App />,
    document.getElementById('root')
);
```

`div` 태그를 보면 props의 값을 다르게 해서 `Welcome` 컴포넌트를 여러 번 사용하는 것을 볼 수 있다. 이렇게 하면 `App`이라는 컴포넌트는 `Welcome` 컴포넌트 3개를 포함하고 있는 컴포넌트가 되는 것이다. 이렇게 여러 개의 컴포넌트를 합쳐서 또 다른 컴포넌트를 만드는 것을 **컴포넌트 합성**이라고 한다.

그림으로 표현하면 아래와 같다.

![스크린샷 2023-11-28 오후 5.38.15.png](https://github.com/Heo-y-y/development-blog/assets/112863029/59bc5cb8-bd77-4fbe-89ab-e141a42ea8de)

`App` 컴포넌트 안에 3개의 `Welcom` 컴포넌트가 있고, 각각의 `Welcome` 컴포넌트는 각기 다른 props를 가지고 있다. 이렇게 `App` **컴포넌트를 루트로 해서 하위 컴포넌트들이 존재하는 형태가 리액트로만 구성된 앱의 기본적인 구조**이다.

만약 기존에 있던 웹 페이지에 리액트를 연동한다면, 루트 노드가 하나가 아닐 수도 있기 때문에 이런 구조가 되지 않을 수도 있다.

### Component 추출

컴포넌트 합성과 반대로 **복잡한 컴포넌트를 쪼개서 여러 개의 컴포넌트로 나눌 수도 있는데 이러한 과정을 컴포넌트 추출**이라고 한다. **큰 컴포넌트에서 일부를 추출해서 새로운 컴포넌트를 만든다는 뜻**이다.

컴포넌트 추출을 잘 활용하면 컴포넌트의 **재사용성이 증가**한다. 왜냐하면 컴포넌트가 작아질수록 해당 컴포넌트의 **기능과 목적이 명확**해지고 **props도 단순**해지기 때문에 다른 곳에서 사용할 수 있을 확률이 높아진다.

**개발속도**도 올라간다.

**Component를 추출하는 과정**

```jsx
function Component(props) {
    return (
        <div className="commment">
            <div className="user-info">
                <img className="avatar" 
                    src={props.author.avatarUrl}
                    alt={props.author.name}
                />
                <div className="user-info-name">
                    {props.author.name}
                </div>
            </div>

            <div className="comment-text">
                {props.text}
            </div>

            <div className="comment-date">
                {FormData(props.date)}
            </div>
        </div>
    );
}
```

먼저 `comment`라는 컴포넌트가 하나 있다. 해당 컴포넌트는 댓글을 표시하기 위한 컴포넌트로 내부에 작성자의 프로필 이미지와 이름 그리고 댓글 내용과 작성일을 포함하고 있다.

그리고 해당 컴포넌트의 props는 아래와 같은 형태일 것이다.

```jsx
props = {
    author: {
        name: "Heo",
        avatarUrl: "http://...",
    },
    text: "댓글입니다.",
    date: Date.now,
}
```

이제 해당 컴포넌트에서 하나씩 컴포넌트를 추출해보자.

1. **Avatar 추출하기**
    
    `comment` 컴포넌트에서는 `img` 태그를 사용하여 사용자의 프로필 이미지를 표시하고 있는데, 이 부분을 추출해서 `avatar`라는 별도의 컴포넌트를 만들어보자.
    
    ```jsx
    function Avator(props){
        return(
            <img className="avatar"
                src={props.user.avatarUrl}
            alt={props.user.name}
            />
        );
    ```
    
    props에 기존에 사용하던 `author` 대신 조금 더 보편적인 의미를 가지고 있는 `user`로 변경했다. **보편적인 단어를 사용**하는 것은 **재사용성 측면을 고려**하는 것이라고 보면 된다.
    
    즉, 다른 곳에서 이 컴포넌트를 사용할 때도 props에 들어갈 속성들이 의미상 큰 차이가 없이 사용할 수 있게 하기 위함이다.
    
    이제 추출된 `Avatar` 컴포넌트를 실제로 `comment` 컴포넌트에 반영해보자.
    
    ```jsx
    unction Component(props) {
        return (
            <div className="commment">
                <div className="user-info">
                    // 적용 부분
                    <Avator user={props.author} />
                    <div className="user-info-name">
                        {props.author.name}
                    </div>
                </div>
    
                <div className="comment-text">
                    {props.text}
                </div>
    
                <div className="comment-date">
                    {FormData(props.date)}
                </div>
            </div>
        );
    }
    ```
    
    `Avator`라는 이름의 컴포넌트로 교체되고 나니 좀 더 코드의 가독성이 높아져 보인다.
    
2. **UserInfo 추출하지**
    
    이제 사용자 정보를 담고 있는 부분을 추출해보자.
    
    ```jsx
    function UserInfo(props) {
        return (
            <div className="user-info">
                <Avator user={props.user} />
                <div className="user-info-name">
                    {props.user.name}
                </div>
            </div>
        );
    }
    ```
    
    위 코드는 사용자 정보를 담고 있는 부분을 `Userinfo`라는 컴포넌트로 추출한 것이다.
    
    위에서 추출한 `Avatar` 컴포넌트도 여기에 함께 추출된 것을 볼 수 있다.
    
    이제 추출된 `Userinfo` 컴포넌트를 `comment` 컴포넌트에 반영해보자.
    
    ```jsx
    function Component(props) {
        return (
            <div className="commment">
                <UserInfo user={props.author} />
                <div className="comment-text">
                    {props.text}
                </div>
    
                <div className="comment-date">
                    {FormData(props.date)}
                </div>
            </div>
        );
    }
    ```
    
    코드가 점점 단순해지는 것을 확인할 수 있다.
    

지금까지 추출한 컴포넌트들의 구조를 그림으로 나타내면 아래와 같다.

![스크린샷 2023-11-28 오후 6.07.41.png](https://github.com/Heo-y-y/development-blog/assets/112863029/bfd6d3c6-f576-4271-97f3-9882ae20a014)

Comment 컴포넌트가 UserInfo 컴포넌트를 포함하고 있고, UserInfo 컴포넌트가 Avatar 컴포넌트를 포함하고 있는 구조이다.

컴포넌트를 어느 정도 수준까지 추출하는 것이 좋은지에 대해 정해진 기준은 없다. 하지만 **기능 단위로 구분하는 것이 좋고** 나중에 곧바로 **재사용이 가능한 형태로 추출하는 것이 좋다**.

**참고 자료**

- [https://www.inflearn.com/course/lecture?courseSlug=처음-만난-리액트&unitId=113272](https://www.inflearn.com/course/lecture?courseSlug=%EC%B2%98%EC%9D%8C-%EB%A7%8C%EB%82%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8&unitId=113272)
