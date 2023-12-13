# Context

## Context란?

기존의 일반적인 리액트 애플리케이션에서는 데이터가 컴포넌트의 props를 통해 부모에서 자식으로 단방향으로 전달이 되었다. 하지만 여러 컴포넌트에 걸쳐 굉장히 자주 사용되는 데이터의 경우 기존 방식을 사용하면 코드도 너무 복잡해지고, 사용하기에 불편함도 많다. 그래서 나오게 된 것이 바로 **Context**이다.

**Context**는 리액트 컴포넌트들 사이에서 데이터를 기존의 props를 통해 전달하는 방식 대신 **컴포넌트 트리를 통해 곧바로 컴포넌트로 전달**하는 새로운 방식을 제공한다. 이를 통해 어떤 컴포넌트라도 데이에 쉽게 접근할 수 있다.

### 기존 방식

![스크린샷 2023-12-13 오후 9.12.07.png](https://github.com/Heo-y-y/development-blog/assets/112863029/86edac45-31e1-48f9-908b-db51e89827ae)

위 그림은 props를 통해 상위 컴포넌트에서 하위 컴포넌트로 데이터를 전달하는 일반적인 방식이다. 이 방식의 단점은 여러 컴포넌트에 걸쳐서 자주 사용되는 데이터 예를 들면, 로그인 여부나 프로필 정보 같은 것을 전달하려면 **반복적인 코드가 많이 생기고 지저분해진다**.

위 그림에서 루트 노드에 있는 데이터를 C 컴포넌트로 전달하려면 최소 2번을 props로 전달해야 한다. 만약 데이터를 전달하려는 컴포넌트가 10단계 밑에 있다면 10번이나 props를 타고 하위 컴포넌트에 전달해야 하는 것이다.

그래서 이러한 불편한 점을 개선하기 위해 생겨난 것이 바로 **Context**이다.

### Context 사용

![스크린샷 2023-12-13 오후 9.19.39.png](https://github.com/Heo-y-y/development-blog/assets/112863029/77def8fe-666f-4a91-8ec9-6fd1970f775a)

위 그림은 방금 전과 동일한 기능을 구현하기 위해 Context를 사용한 것이다. **Context**를 사용하면 일일이 props로 전달할 필요 없이 위 그림처럼 **데이터를 필요로하는 컴포넌트에 바로 전달** 할 수 있다. 따라서 코드도 매우 깔끔해지고, 데이터를 한 곳에서 관리하기 때문에 디버깅을 하기에도 굉장히 좋다.

### Context는 언제 사용할까?

먼저 여러 개의 컴포넌트들이 접근해야 하는 데이터에는 어떤 것들이 있는지 살펴보자.

여러 컴포넌트에서 자주 필요로 하는 데이터로는 사용자의 로그인 여부, 로그인 정보, UI 테마, 현재 선택된 언어 등이 있다. 예를 들어, 웹 사이트 상단에 위치한 네비게이션 바에 사용자의 로그인 여부에 따라서 로그인 버튼과 로그아웃 버튼을 선택적으로 보여주고 싶은 경우 현재 로그인 상태 데이터에 접근할 필요가 있을 것이다. 마찬가지로 UI 테마, 현재 선택된 언어 같은 데이터도 곳곳에 있는 컴포넌트에서 접근이 자주 일어날 가능성이 높은 데이터이다. 이러한 데이터들을 기존 방식대로 컴포넌트의 props를 통해 넘겨주게 되면 자식 컴포넌트의 자식 컴포넌트까지 계속해서 내려갈 수밖에 없게 된다.

**예제 코드**

```jsx
function App(props) {
    return <Toolbar theme="dark" />;
}

function Toolbar(props) {
    // 이 Toolbar 컴포넌트는 ThemedButton에 theme를 넘겨주기 위해서 'theme' prop을 가져야만 한다.
    // 현재 테마를 알아야 하는 모든 버튼에 대해서 props로 전달하는 것은 굉장히 비효율적이다.
    return (
        <div>
            <ThemedButton theme={props.time} />
        </div>
    );
}

function ThemedButton(props) {
    return <Button theme={props.theme}></Button>
}
```

위 코드는 현재 선택된 테마를 기존 방식을 사용하여 컴포넌트의 `props`로 전달하는 코드이다. 가장 사위 컴포넌트인 `App` 컴포넌트에서는 `Toolbar` 컴포넌트를 사용하고 있다. 이때 `theme`이라는 이름의 prop으로 현재 테마인 `dark`를 넘긴다. `Toolbar` 컴포넌트에서는 `ThemedButton` 컴포넌트를 사용하는데, `ThemedButton` 컴포넌트에서 현재 테마를 필요로 한다. 그래서 prop으로 받은 `theme`를 하위 컴포넌트인 `ThemedButton` 컴포넌트에 전달한다. 최종적으로 `ThemedButton` 컴포넌트에서는 `props.theme`으로 데이터에 접근하여 버튼에 어두운 테마를 입힌다.

이처럼 `props`를 통해서 데이터를 전달하는 기존 방식은 실제 데이터를 필요로 하는 컴포넌트까지의 깊이가 깊어질수록 복잡해진다. 또한 반복적인 코드를 계속해서 작성해 주어야 하기 때문에 비효율적이고, 직관적이지도 않다.

Context를 사용하면 이러한 방식을 깔끔하게 개선할 수 있다.

**예제 코드**

```jsx
// 컨텍스트는 데이터를 매번 컴포넌트를 통해 전달할 필요 없이 컴포넌트 트리로 곧바로 전달하게 해준다.
// 여기에서는 현재 테마를 위한 컨텍스트를 생성하며, 기본값은 'light' 이다.
const ThemeContext = React.createContext('light');

// Provider를 사용하여 하위 컴포넌트들에게 현재 테마 데이터를 전달한다.
// 모든 하위 컴포넌트들은 컴포넌트 트리 하단에 얼마나 깊이 있는지에 관계없이 데이터를 읽을 수 있다.
// 여기에서는 현재 테마값으로 'dark'를 전달하고 있다.
function App(props) {
    return (
        <ThemeContext.Provider value="dark">
            <Toolbar />
        </ThemeContext.Provider>
    );
}

// 이제 중간에 위치한 컴포넌트는 테마 데이터를 하위 컴포넌트로 전달할 필요가 없다.
function Toolbar(props) {
    return (
        <div>
            <ThemedButton />
        </div>
    );
}

 // 리액트는 가장 가까운 상위 테마 Provider를 찾아서 해당되는 값을 사용한다.
    // 만약 해당되는 Provider가 없을 경우 기본값(여기에서는 'light')을 사용한다.
    // 여기에서는 상위 Provider가 있기 때문에 현재 테마의 값은 'dark'가 된다.
function ThemedButton(props) {
    return (
        <ThemeContext.Consumer>
            {value => <Button theme={value} />}
        </ThemeContext.Consumer>
    );
}
```

위 코드는 Context를 사용하여 앞에 코드와 동일한 기능을 구현한 것이다. 먼저 `React.createContext` 함수를 사용해서 `ThemeContext`라는 이름의 컨텍스트를 하나 생성했다. 그리고 Context를 사용할 컴포넌트의 상위 컴포넌트에서 `Provider`로 감싸주어야 하는데, 위 코드에서는 최상위 컴포넌트인 `App` 컴포넌트를 `ThemeContext.Provider`로 감싸주었다. 이렇게 하면 `Provider`의 모든 하위 컴포넌트가 얼마나 깊이 위치해 있는지 관계없이 Context의 데이터를 읽을 수 있다.

Context를 사용한 코드를 보면 전체적으로 간결하고 깔끔하며 직관적으로 바뀐 것을 확인할 수 있다. 이처럼 여러 컴포넌트에서 계속해서 접근이 일어날 수 있는 데이터들이 있는 경우에는 Context를 사용하는 것이 좋다.

### Context를 사용하기 전에 고려할 점

Context는 다른 레벨의 많은 컴포넌트가 특정 데이터를 필요로 하는 경우에 주로 사용한다. 하지만 **무조건 Context를 사용하는 것이 좋은 것은 아니다**. 왜냐하면 **컴포넌트와 Context가 연동되면 재사용성이 떨어지기 때문**이다. 그래서 **다른 레벨의 많은 컴포넌트가 데이터를 필요로하는 경우가 아니라면 기존에 사용하던 방식대로 props를 통해 데이터를 전달하는 컴포넌트 컴포지션 방법이 더 적합**하다.

**예제 코드**

```jsx
// Page컴포넌트는 PageLayout컴포넌트를 렌더링
<Page user={user} avatarSize={avatarSize} />

// PageLayout컴포넌트는 NavigationBar컴포넌트를 렌더링
<PageLayout user={user} avatarSize={avatarSize} />

// NavigationBar컴포넌트는 Link컴포넌트를 렌더링
<NavigationBar user={user} avatarSize={avatarSize} />

// Link컴포넌트는 Avatar컴포넌트를 렌더링
<Link href={user.permalink}>
    <Avatar user={user} avatarSize={avatarSize} />
</Link>
```

위 코드에서는 사용자 정보와 아바타 사이즈를 몇 단계에 걸쳐서 하위 컴포넌트인 `Link`와 `Avatar`로 전달하는 페이지 컴포넌트가 있다. 여기에서 가장 하위 레벨에 위치한 `Avatar` 컴포넌트가 `user`와 `avatarSize`를 필요로 하기 때문에 이를 위해 여러 단계에 걸쳐서 `props`를 통해 `user`와 `avatarSize`를 전달해주고 있다.

하지만 이 과정은 굉장히 불필요하게 느껴진다. 또한 `Avatar` 컴포넌트에 추가적인 데이터가 필요해지면 해당 데이터도 추가로 여러 단계에 걸쳐서 넘겨주어야 하기 때문에 굉장히 번거롭다. 여기에서 `Context`를 사용하지 않고 이러한 문제를 해결할 수 있는 한가지 방법은 `Avatar` **컴포넌트를 변수에 저장하여 직접 넘겨주는 것**이다. 즉, **Element Variable** 형태를 사용하는 것이다. 그렇게 하면 중간 단계에 있는 컴포넌트들은 `user`와 `avatarSize`에 대해 전혀 몰라도 된다.

**예제 코드**

```jsx
function Page(props) {
    const user = props.user;

    const userLink = (
        <Link href={user.permalink}>
            <Avatar user={user} avatarSize={avatarSize} />
        </Link>
    );

    // Page컴포넌트는 PageLayout컴포넌트를 렌더링
    // 이때 props로 userLink를 함께 전달
    return <PageLayout userLink={userLink} />;
}

// PageLayout컴포넌트는 NavigationBar컴포넌트를 렌더링
<PageLayout userLink={...} />

// NavigationBar컴포넌트는 props로 전달받은 userLink element를 리턴
<NavigationBar userLink={...} />
```

위 코드에서는 `user`와 `avatarSize`가 `props`로 들어간 `Avatar` 컴포넌트를 `userLink`라는 변수에 저장한 뒤에 해당 변수를 하위 컴포넌트로 넘기고 있다. 이렇게 하면 가장 상위 레벨에 있는 `Page` 컴포넌트만 `Avatar` 컴포넌트에서 필요로 하는 `user`와 `avatarSize`에 대해 알고 있으면 된다. 

이런 방식은 중간 레벨의 컴포넌트를 통해 전달해야 하는 `props`를 없애고 코드를 더욱 간결하게 만든다. 또한 최상위에 있는 컴포넌트에 좀 더 많은 권한을 부여해준다.

다만 모든 상황에서 이 방식이 좋은 것은 아니다. **데이터가 많아질수록 상위 컴포넌트에 몰리기 때문에 상위컴포넌트는 점점 더 복잡해지고 하위 컴포넌트는 너무 유연해지게 된다**.

앞에서 사용한 방법을 좀 더 응용해서 **하위 컴포넌트를 여러 개의 변수로 나눠서 전달**할 수도 있다.

**예제 코드**

```jsx
function Page(props) {
    const user = props.user;

    const topBar = (
        <NavigationBar>
            <Link href={user.permalink}>
                <Avatar user={user} avatarSize={avatarSize} />
            </Link>
        </NavigationBar>
    );
    const content = <Feed user={user} />;

    return (
        <PageLayout
            topBar={topBar}
            content={content} 
        />
    );
}
```

위 방식은 하위 컴포넌트의 의존성을 상위 컴포넌트와 분리할 필요가 있는 대부분의 경우에 적합한 방법이다. 또한 렌더링 전에 하위 컴포넌트가 상위 컴포넌트와 통신해야 하는 경우 render props를 사용하여 처리할 수도 있다.

하지만 어떤 경우에는 **하나의 데이터에 다양한 레벨에 있는 중첩된 컴포넌트들이 접근할 필요**가 있을 수 있다. 이러한 경우에는 위 방식을 사용할 수 없고, **Context**를 사용해야 한다.

**Context**는 **해당 데이터와 데이터의 변경 사항을 모두 하위 컴포넌트들에게 Broadcast를 해주기 때문**이다. Context를 사용하는 것이 적합한 데이터의 대표적인 예로 현재 지역 정보, UI 테마 그리고 캐싱된 데이터 등이 있다.

## Context API

리액트에서 제공하는 Context API를 통해 Context를 어떻게 사용하는지 알아보자.

Context를 사용하기 위해서는 가장 먼저 Context를 생성하는 것이다. Context를 생성하기 위해서는 `React.createContext` 함수를 사용한다.

### React.createContext

**Context 생성**

```jsx
const MyContext = React.createContext(기본값);
```

위 코드처럼 함수의 파라미터로 기본값을 넣어주면 된다. 그리고 Context 객체가 만들어진다. **리액트에서 렌더링이 일어날 때 Context 객체를 구독하는 하위 컴포넌트가 나오면 현재 컨텍스트의 값을 가장 가까이에 있는 상위 레벨의 Provider로부터 받아오게 된다**.

그런데 **만약 상위 레벨에 매칭되는 Provider가 없다면** 이 경우에만 **기본값이 사용**된다. 그렇기 때문에 기본값은 Provider 없이 컴포넌트를 테스트할 때 유용하다.

참고로 **기본값으로 undefined를 넣으면 기본값이 사용되지 않는다**.

`React.createContext` 함수를 사용해서 Context를 만들었다면 이제 **하위 컴포넌트들이 해당 컨텍스트의 데이터를 받을 수 있도록 설정**해 주어야 한다. 이를 위해서 사용하는 것이 바로 **Provider**이다.

### Context.Provider

**Provider**는 제공자라는 뜻을 가지고 있다. 여기에서는 **데이터를 제공해주는 컴포넌트**라는 의미로 이해하면 된다. **모든 Context 객체는 Provider라는 리액트 컴포넌트를 가지고 있다**. `Context.Provider` 컴포넌트로 하위 컴포넌트들을 감싸주면 모든 하위 컴포넌트들이 해당 Context 데이터에 접근할 수 있게 된다.

Provider는 아래 코드처럼 사용하면 된다.

```jsx
<MyContext.Provider value={/* some value */}>
```

**Provider 컴포넌트에는 value라는 prop이 있으며 이것은 Provider 컴포넌트 하위에 있는 컴포넌트들에게 전달**된다. 그리고 **하위 컴포넌트들이 이 값을 사용하게 되는데, 이 하위 컴포넌트들이 데이터를 소비한다는 뜻**에서 **컨슈밍 컴포넌트**라고 부른다. **컨슈밍 컴포넌트는 Context 값의 변화를 지켜보다가 만약 값이 변경되면 재렌더링** 된다.

참고로 하나의 **Provider 컴포넌트는 여러 개의 컨슈밍 컴포넌트와 연결될 수 있으며 여러 개의 Provider 컴포넌트는 중첩되어 사용될 수 있다**. **Provider 컴포넌트로 감싸진 모든 컨슈밍 컴포넌트는 Provider의 value prop이 바뀔 때마다 재렌더링** 된다. **값이 변경되었을 때 상위 컴포넌트가 업데이트 대상이 아니더라도 하위에 있는 컴포넌트가 Context를 사용한다면 하위 컴포넌트에서는 업데이트가 일어난다**. 이때 **값의 변화를 판단하는 기준**은 **자바스크립트 객체의 object.is**라는 함수와 같은 방식으로 판단한다.

### Provider value에서 주의해야 할 사항

Context는 재렌더링 여부를 결정할 때 레퍼런스 정보를 사용하기 때문에 Provider의 부모 컴포넌트가 재렌더링 되었을 경우 의도치 않게 컨슈머 컴포넌트가 재렌더링이 일어날 수 있는 문제가 있다.

**예제 코드**

```jsx
function App(props) {
    return (
        <MyContext.Provider value={{ something: 'something' }}>
            <Toolbar />
        </MyContext.Provider>
    );
}
```

예를 들어 위 코드는 `Provider` 컴포넌트가 재렌더링 될 때마다 모든 하위 컨슈머 컴포넌트를 재렌더링하게 된다. 왜냐하면 `value` **prop을 위한 새로운 객체가 매번 새롭게 생성되기 때문**이다. 

이를 방지하기 위해서는 `value`**를 직접 넣는 것이 아닌 컴포넌트의 state로 옮기고 해당 state의 값을 넣어 주어야 한다**.

**예제 코드** 

```jsx
function App(props) {
    const [value, setValue] = useState({ something: 'something' });
    return (
        <MyContext.Provider value={value}>
            <Toolbar />
        </MyContext.Provider>
    );
}
```

위 코드는 state를 사용하여 불필요한 재렌더링을 막는 코드이다. `value`**를 직접 넣는 것이 아니라 state를 선언**하고, **state의 값을** `Provider`**에 넣는다**.

### Class.contextType

`Class.contextType`은 **Provider 하위에 있는 클래스 컴포넌트에서 Context의 데이터에 접근하기 위해 사용**하는 것이다. 

클래스 컴포넌트는 현재 거의 사용하지 않기 때문에 이런 방법이 있다는 정도만 알고 가자.

**예제 코드**

```jsx
class MyClass extends React.Component {
    componentDidMount() {
        let value = this.context;
        /* MyContext의 값을 이용하여 원하는 작업을 수행 가능 */
    }
    componentDidUpdate() {
        let value = this.context;
        /* ... */
    }
    componentWillUnmount() {
        let value = this.context;
        /* ... */
    }
    render() {
        let value = this.context;
        /* MyContext의 값에 따라서 컴포넌트들을 렌더링 */
    }
}
MyClass.contextType = MyContext;
```

위 코드는 `MyClass.contextType = MyContext;` 으로 해주면 `MyClass`라는 클래스 컴포넌트는 `MyContext`의 데이터에 접근할 수 있게 된다. 클래스 컴포넌트에 있는 `contextType` 속성에는 `React.createContext` 함수를 통해 생성된 Context 객체가 대입될 수 있다. 이 속성을 사용하게 되면 `this.context`를 통해 상위에 있는 Provider 중에서 가장 가까운 것의 값을 가져올 수 있다. 또한 예제 코드에 나와 있는 것처럼 render 함수를 포함한 모든 생명주기 함수 어디에서든지 `this.context`를 사용할 수 있다.

참고로 이 API를 사용하면 단 하나의 Context만을 구독할 수 있다.

### Context.Consumer

**Consumer** 컴포넌트는 앞에서 설명한 것처럼 **Context의 데이터를 구독하는 컴포넌트**이다.

**클래스 컴포넌트**에서는 앞에 나온 **class.context 타입을 사용**하면 되고, **함수 컴포넌트**에서는 바로 이 **Context.Consumer를 사용하여 Context를 구독**할 수 있다.

**예제 코드**

```jsx
<MyContext.Consumer>
    {value => /* 컨텍스트의 갑셍 따라서 컴포넌트들을 렌더링 */}
</MyContext.Consumer>
```

위 코드는 **Context.Consumer를 사용하는 방법**이다. 컴포넌트 자식으로 함수가 올 수 있는데, 이것을 **function as a child**라고 부른다. **Context.Consumer로 감싸주면 자식으로 들어간 함수가 현재 Context의** `value`**를 받아서 리액트 노드로 리턴**하게 된다. **이때 함수로 전달되는** `value`**는** `Provider`**의** `value prop`**과 동일**하다.

**만약 상위 컴포넌트에** `Provider`**가 없다면** 이 `value` 파라미터는 **createContext를 호출할 때 넣는 기본 값과 동일한 역할**을 한다.

### function as a child

**function as a child**는 **컴포넌트의 자식으로 함수를 사용하는 방법**이다.

**예제 코드**

```jsx
// children이라는 prop을 직접 선언하는 방식
<Profile children={name => <p>이름: {name}</p>} />

// Profile컴포넌트로 감싸서 children으로 만드는 방식
<Profile>{name => <p>이름: {name}</p>}</Profile>
```

**리액트**에서는 기본적으로 **하위 컴포넌트들을** `children`**이라는 프로그램으로 전달**해 주는데 `children`**으로 컴포넌트 대신 함수를 사용**하여 위 코드와 같이 사용할 수 있다.

### Context.displayName

**Context 객체**는 **displayName이라는 문자열 속성**을 가진다. 또한 크롭의 리액트 개발자 도구에서는 Context의 Provider나 Consumer를 표시할 때 이 `displayName`을 함께 표시해 준다.

**예제 코드**

```jsx
const MyContext = React.createContext(/* some value */);
MyContext.displayName = 'MyDisplayName';

// 개발자 도구에 "MyDisplayName.Provider"로 표시됨
<MyContext.Provider>

// 개발자 도구에 "MyDisplayName.Consumer"로 표시됨
<MyContext.Consumer>
```

예를 들어 위 코드를 작성하면 `MyDisplayName`이 리액트 개발자 도구에 표시된다.

앞에서 클래스 컴포넌트에서 class.context 타입을 사용하면 한 번에 하나의 Context만 사용할 수 있다고 했다. 그렇다면 여러 개의 Context를 동시에 사용하려면 어떻게 해야 할까?

### 여러 개의 Context 사용하기

**여러 개의 Context를 사용**하려면 **Context.Provider를 중첩해서 사용**하면 된다.

**예제 코드**

```jsx
// 테마를 위한 컨텍스트
const ThemeContext = React.createContext('light');

// 로그인 한 사용자를 위한 컨텍스트
const UserContext = React.createContext({
    name: 'Guest',
});

class App extends React.Component {
    return() {
        const { signedInUser, theme } = this.props;

        // App component that provides initial context values
        return (
            <ThemeContext.Provider value={theme}>
                <UserContext.Provider value={signedInUser}>
                    <Layout />
                </UserContext.Provider>
            </ThemeContext.Provider>
        );
    }
}
```

```jsx
// 컨텍스트 컴포넌트는 두 개의 컨텍스트로부터 값을 가져와서 렌더링함
function Content() {
    return (
        <ThemeContext.Consumer>
            {theme => (
                <UserContext.Consumer>
                    {user => (
                        <ProfilePage user={user} theme={theme} />
                    )}
                </UserContext.Consumer>
            )}
        </ThemeContext.Consumer>
    );
}
```

위 코드에서는 `ThemeContext`와 `UserContext` 이렇게 총 2개의 Context가 나온다. 그리고 `App` 컴포넌트에서는 각 Context에 대해 2개의 `Provider`를 사용하여 자식 컴포넌트인 `Layout`을 감싸줬다.

그리고 실제 Context의 데이터를 사용하는 Content 컴포넌트에서는 2개의 `Consumer` **컴포넌트를 사용하여 데이터를 전달**할 수 있다. 이렇게 하면 **여러 개의 Context를 동시에 사용**할 수 있다.

하지만 2개 또는 그 이상의 Context 값이 자주 함께 사용될 경우 모든 값을 한 번에 제공해주는 별도의 render prop 컴포넌트를 직접 만드는 것도 고려하면 좋습니다.

지금까지는 클래스 컴포넌트에서 Context를 사용하는 방법과 함수 컴포넌트에서 Provider와 Consumer를 사용해서 Context를 사용하는 방법을 알아보았다.

앞에서 말한 것처럼 **클래스 컴포넌트는 이제 거의 사용하지 않기 때문**에 **함수 컴포넌트에서 Context를 사용하는 방법을 더 중요하게 생각**하자. 그런데 함수 컴포넌트에서 Context를 사용하기 위해 컴포넌트를 매번 Consumer 컴포넌트로 감싸주는 것보다 더 좋은 방법이 있다. 

### useContext()

바로 **Hook**이다. **useContext() Hook**은 **함수 컴포넌트에서 컨텍스트를 쉽게 사용할 수 있게 해준다**.

**useContext() Hook을 사용한 예시** 

```jsx
function MyComponent(props) {
    const value = useContext(MyContext);

    return (
        ...
    )
 }
```

`React.createContext` **함수 호출로 생성된 Context 객체를 인자로 받아서 현재 Context의 값을 리턴**한다. `useContext() Hook`**을 사용하면 Context의 값을 다른 방식과 동일하게 컴포넌트 트리상에서 가장 가까운 상위** `Provider`**로부터 받아오게 된다**.

만약 **Context 값이 변경되면 변경된 값과 함께** `useContext() Hook`**을 사용하는 컴포넌트가 재렌더링** 된다. 그렇기 때문에 만약 useContext() Hook을 사용하는 컴포넌트의 렌더링이 꽤 무거운 작업일 경우에는 별도로 최적화 작업을 해줄 필요가 있다.

또한 `useContext() Hook`을 사용할 때에는 **파라미터로 Context 객체**를 넣어줘야 한다.

아래 코드처럼 Consumer나 Provider를 넣으면 안 된다.

```jsx
// 올바른 사용법
useContext(MyContext);

// 잘못된 사용법
useContext(MyContext.Consumer);
useContext(MyContext.Provider);
```

**참고 자료**

- [https://www.inflearn.com/course/lecture?courseSlug=처음-만난-리액트&unitId=113243&tab=curriculum](https://www.inflearn.com/course/lecture?courseSlug=%EC%B2%98%EC%9D%8C-%EB%A7%8C%EB%82%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8&unitId=113243&tab=curriculum)
