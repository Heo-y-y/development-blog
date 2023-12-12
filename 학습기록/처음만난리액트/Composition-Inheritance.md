# Composition vs Inheritance

## **Composition**

Composition이라는 영어 단어는 구성이라는 뜻을 가지고 있다.

**리액트**에서 **Composition**이라고 하면 **여러 개의 컴포넌트를 합쳐서 새로운 컴포넌트를 만드는 것**을 의미한다. 그래서 여기서는 Composition이 구성이라는 뜻보다는 **합성**이라는 뜻에 더 가깝다.

Composition이라고 해서 무작정 그냥 컴포넌트들을 붙이는 것이 아니라 여러 개의 컴포넌트들을 어떻게 조합할 것인가에 대한 고민이 필요하다.

조합 방법에 따라 Composition의 사용 기법이 나뉘는데 대표적인 Composition 사용 기법에 대해서 알아보자.

### Containment

Containment라는 영어 단어는 방지, 견제라는 뜻을 가지고 있다. 하지만 여기에서는 Contain의 의미가 조금 더 강하다고 할 수 있다. 영어 단어 Contain은 안에 담다, 표현하다 라는 뜻을 가지고 있다. 그래서 **Containment**는 **하위 컴포넌트를 포함하는 형태의 합성 방법**이라고 이해하면 된다.

보통 Sidebar나 Dialog 같은 박스 형태의 컴포넌트는 자신의 하위 컴포넌트를 미리 알 수 없다. 

예를 들어, 동일한 Sidebar 컴포넌트를 사용하는 2개의 쇼핑몰이 있다고 가정해보자. 하나의 쇼핑몰에는 의류와 관련된 메뉴가 8개 들어 있고, 다른 쇼핑몰에는 식료품과 관련된 메뉴가 10개 존재한다. Sidebar 컴포넌트 입장에서는 자신의 하위 컴포넌트로 어떤 것들이 들어 올지 알수 없다. 왜냐하면 해당 컴포넌트를 사용하는 개발자가 어떤 것을 넣느냐에 따라 하위 컴포넌트가 달라지기 때문이다. 그렇기 때문에 이러한 경우에는 Containment 방법을 사용하여 Composition을 사용하게 된다.

**Containment**를 사용하는 방법은 **리액트 컴포넌트의 props에 기본적으로 들어있는 children 속성을 사용**하면 된다.

**children prop을 사용한 코드**

```jsx
function FancyBorder(props) {
    return (
        <div className={'FancyBorder FancyBorder-' + props.color}>
            {props.children}
        </div>
    );
}
```

`props.children`을 사용하면 해당 컴포넌트의 하위 컴포넌트가 모두 `children`으로 들어오게 된다. `children`이라는 `prop`은 개발자가 직접 넣어주는 것이 아니라 **리액트에서 기본적으로 제공**해주는 것이다.

앞서서 [CreateElement 함수 포스팅](렌더링엘리먼트.md) 시 아래와 같은 형태로 호출했었다.

```jsx
React.createElement(
    type,
    [props],
    [...children]
);
```

여기에서 3번째에 들어가는 파라미터가 바로 `children`이다. `children` 이 배열로 되어 있는 이유는 여러 개의 하위 컴포넌트를 가질 수 있기 때문이다.

결과적으로 `FancyBorder` 컴포넌트는 자신의 하위 컴포넌트를 모두 포함하여 예쁜 테두리로 감사주는 컴포넌트가 된다.

실제로 `FancyBorder`를 사용하는 예제를 보자.

```jsx
function WelcomeDialog(props) {
    return (
        <FancyBorder color="blue">
            <h1 className="Dialog-title">
                어서오세요
            </h1>
            <P className="Dialog-message">
                우리 사이트에 방문하신 것을 환영홥니다!
            </P>
        </FancyBorder>
    );
}
```

`WelcomeDialog` 컴포넌트는 `FancyBorder` 컴포넌트를 사용하고 있다. `FancyBorder` 컴포넌트로 감싸진 부분 안에는 `h1` 태그와 `p` 태그 두 개의 태그가 들어가 있다. 이 두 태그는 모두 `FancyBorder` 컴포넌트에 `children`이라는 이름의 `props`로 전달된다. 결과적으로 파란색의 테두리로 모두 감싸지는 결과가 나오는 것이다.

**리액트에서는 하위 컴포넌트를** `props.children`**으로 하나로 모아서 제공**해준다.

그렇다면 여러 개의 `children` 집합이 필요한 경우는 어떻게 할까?

이런 경우에는 **별도의** `props`**를 정의해서 각각 원하는 컴포넌트를 넣어**주면 된다.

**예제 코드**

```jsx
function SplitPane(props) {
    return (
        <div className="SplitPane">
            <div className="SplitPane-left">
                {props.left}
            </div>
            <div className="SplitPane-right">
                {props.right}
            </div>
        </div>
    );
}

function App(props) {
    return (
        <SplitPane 
            left={
                <Contacts />
            }
            right={
                <Chat />
            }
        />
    );
}
```

위 코드는 화면을 왼쪽과 오른쪽으로 분할하여 보여주는 `SplitPane`이라는 컴포넌트가 있다.

그리고 바로 아래쪽에 있는 `App` 컴포넌트에서는 `SplitPane` 컴포넌트를 사용하고 있는데, 여기에서 `left`, `right`라는 두 개의 `props`를 정의하여 그 안에 각각 다른 컴포넌트를 넣어주고 있다. `SplitPane`에서는 이 `left`, `right`를 `props`로 받게 되고, 각각 화면의 왼쪽과 오른쪽에 분리해서 렌더링하게 된다.

이처럼 **여러 개의** `children` **집합이 필요한 경우**에는 **별도의** `props`**를 정의해서 사용**하면 된다.

위 예시로 살펴본 것처럼 `props.children`**이나** **직접 정의한** `props`**를 이용하여 하위 컴포넌트를 포함하는 형태로 합성하는 방법**을 **Containment**라고 한다.

### Specialization

영어 단어 Specialization은 전문화, 특수화라는 뜻을 가지고 있다.

예를 들어 설명하면, WelcomeDialog는 Dialog의 특별한 케이스이다. Dialog라는 것은 굉장히 범용적인 의미를 가지고 있다. 모든 종류의 Dialog를 다 포함하는 개념이라고 볼 수 있다. 반면에 WelcomeDialog는 누군가를 반기기 위한 Dialog라고 볼 수 있다. 즉, 범용적인 의미가 아니라 좀 더 구체화된 것이다. 이처럼 **범용적인 개념을 구별이 되게 구체화하는 것**을 **Specialization**이라고 한다.

기존의 객체지향 언어에서는 상속을 이용하여 Specialization을 구현한다. 하지만 **리액트**에서는 **합성을 사용하여 Specialization을 구현**하게 된다.

**예제 코드**

```jsx
function Dialog(props) {
    return (
        <FancyBorder color="blue">
            <h1 className="Dialog-title">
                {props.title}
            </h1>
            <P className="Dialog-message">
                {props.message}
            </P>
        </FancyBorder>
    );
}

function WelcomeDialog(props) {
    return (
        <Dialog
            title="어서 오세요"
            message="우리 사이트에 방문하신 것을 환영합니다!"
        />
    );
}
```

위 코드에서는 먼저 `Dialog`라는 범용적인 의미를 가진 컴포넌트가 나온다. 그리고 이 `Dialog`라는 컴포넌트를 사용하는 `WelcomeDialog` 컴포넌트가 나온다.

`Dialog` 컴포넌트에는 `title`과 `message`라는 두 가지 `props`를 가지고 있는데, 각각 `Dialog`에 나오는 제목과 메시지를 의미한다. 그래서 제목과 메시지를 어떻게 사용하느냐에 따라서 경고 `Dialog`가 될 수 있고, 인사말 `Dialog`가 될 수도 있다.

`WelcomeDialog` 컴포넌트는 제목을 "어서 오세요"라고 짓고 사이트에 접속한 사용자에게 인사말을 하는 `Dialog`이다.

위 예시처럼 **Specialization**은 **범용적으로 쓸 수 있는 컴포넌트를 만들어 놓고, 이를 특수화 시켜서 컴포넌트를 사용**하는 **Composition 방법**이다.

그렇다면 Containment와 Specialization을 같이 사용하려면 어떻게 할까?

**예제 코드**

```jsx
unction Dialog(props) {
    return (
        <FancyBorder color="blue">
            <h1 className="Dialog-title">
                {props.title}
            </h1>
            <P className="Dialog-message">
                {props.message}
            </P>
            {props.children}
        </FancyBorder>
    );
}
```

위 `Dialog` 컴포넌트는 이전에 나왔던 코드와 거의 비슷한데, Containment를 위해 끝 부분에 `props.children`을 추가했다. 이를 통해 하위 컴포넌트가 `Dialog` 하단에 렌더링된다.

**예제 코드**

```jsx
function SignUpDialog(props) {
    const [nickname, setNickname] = useState('');

    const handleChange = (event) => {
        setNickname(event.target.value);
    }

    const handleSignUp = () => {
        alert(`어서 오세요, ${nickname}님!`);
    }

    return (
        <Dialog
            title="화성 탐사 프로그램"
            message="닉네임을 입력해 주세요.">
            <input 
                value={nickname}
                onChange={handleChange} />
            <button onClick={handleSignUp}>
                가입하기
            </button>
        </Dialog>
    );
}
```

실제로 `Dialog` 컴포넌트를 사용하는 `SignUpDialog` 컴포넌트를 살펴보면 Specialization을 위한 `props`인 `title`, `message`에 값을 넣어주고 있으며 사용자로부터 닉네임을 입력받고, 가입을 하도록 유도하기 위해 `input`과 `button` 태그가 들어가 있다. 이 두 개의 태그는 모두 `props.children`으로 전달되어 `Dialog` 에 표시된다. 이러한 형태로 Containment와 Specialization을 동시에 사용할 수 있다.

각 방법을 따로 사용하거나 또는 동시에 함께 사용하면 다양하고 복잡한 컴포넌트를 효율적으로 개발할 수 있다.

## Inheritance

영단어 Inheritance는 **상속**이라는 뜻을 가지고 있다. 일상 생활에서 상속이라는 단어는 부모님이 자식에게 재산을 물려줄 때 사용하는 단어이다. 하지만 컴퓨터 프로그래밍의 상속은 **객체지향** 프로그래밍에서 나온 개념이다. **부모 클래스를 상속 받아서 새로운 자식 클래스를 만든다는 개념으로 자식 클래스는 부모 클래스가 가진 변수나 함수 등의 속성을 모두 가지게 된다**.

리액트에서는 다른 컴포넌트로부터 상속 받아서 새로운 컴포넌트를 만드는 것을 고려해 볼 수 있다. 하지만 리액트를 개발한 메타에서 수천 개의 리액트 컴포넌트를 사용했지만, 상속을 사용하여 컴포넌트를 만드는 것을 추천할 만한 사용 사례를 찾지 못했다고 한다.

결국 **리액트**에서는 상속이라는 개념을 사용하는 것 보다는 위에서 설명한 **Composition을 사용해서 개발하는 것이 더 좋은 방법**이다. 결론은 **복잡한 컴포넌트를 쪼개 여러 개의 컴포넌트로 만들고**, **만든 컴포넌트들을 조합해서 새로운 컴포넌트들을 만들면 된다**.

**참고 자료**

- [https://www.inflearn.com/course/lecture?courseSlug=처음-만난-리액트&unitId=113239&category=questionDetail](https://www.inflearn.com/course/lecture?courseSlug=%EC%B2%98%EC%9D%8C-%EB%A7%8C%EB%82%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8&unitId=113239&category=questionDetail)
