# JSX

## **JSX의 정의와 역할**

### JSX란?

자바스크립트를 줄여서 보통 JS라고 많이 표기한다. 자바스크립트 관련 라이브러리도 이름 뒤에 JS가 붙어서 자바스크립트 라이브러리라는 것을 나타내기도 한다.

참고로 React도 사실 공식 명칭은 ReactJS이다.

그러면 JS의 X는 뭘까?

**JSX**는 영어로 A syntax extension to JavaScript라는 의미를 가지고 있다. 말 그대로 **자바스크립트의 확장 문법**이라는 뜻이다.

그러면 문법을 어떻게 확장한 걸까?

**JavaScript + XML/HTML** 이 둘을 합친 것이라고 생각하면 된다.

그래서 JSX의 X는 javascript and xml의 압글자를 따서 JSX라고 부르는 것이다.

**JSX 코드 예시**

```jsx
const element = <h1>Hello, world!</h1>
```

코드를 보면 기존 방식과 다르게 html의 h1 태그를 사용한 것을 볼 수 있다.

해당 코드가 하는 역할은 h1 태그로 둘러싸인 문자열을 element라는 변수에 할당하는 것이다.

### JSX의 역할

JSX는 내부적으로 XML, HTML 코드를 JavaScript로 변환하는 과정을 거치게 된다. 그렇기 때문에 우리가 JSX로 코드를 작성해도 최종적으로는 JavaScript 코드가 나오게 되는 것이다.

여기서 **JSX 코드를 JavaScript 코드로 변환하는 역할**을 하는 것이 바로 React의 `createElement`라는 함수이다.

**JSX를 사용한 코드**

```jsx
class Hello extends React.Component {
		render() {
				return <div>Hello {this.props.toWhat}</div>;
		}
}

ReactDOM.render(
		<Hello toWhat="World" />,
		document.getElementById('root')
);
```

해당 코드를 보면 hello라는 이름을 가진 React 컴포넌트가 나오고 컴포넌트 내부에서 JavaScript 코드와 HTML 코드가 결합된 JSX를 사용하고 있는 것을 확인할 수 있다.

그리고 이렇게 만들어진 컴포넌트를 React DOM의 렌더 함수를 사용해서 실제 화면에 렌더링한다.

**JSX를 사용하지 않은 코드**

```jsx
class Hello extends React.Component {
		render() {
				return React.createElement('div', null, `Hello ${this.props.toWhat}`);
		}
}

ReactDOM.render(
		React.createElement(Hello, { toWhat: 'World' }, null),
		document.getElementById('root')
);
```

해당 코드는 JSX를 사용하지 않고, 순수한 JavaScript 코드만을 사용해서 위에 JSX를 사용한 코드와 동일한 역할을 하게 만든 코드이다.

두 코드를 비교해 보면 Hello 컴포넌트 내부에서 JSX를 사용했던 부분이 `React.createElement`라는 함수로 대체된 것을 확인할 수 있다.

정리하면, JSX 문법을 사용하면 React에서는 내부적으로 모두 `createElement`라는 함수를 사용하도록 변환되는 것이다.

그리고 최종적으로는 `createElement` 함수를 노출한 결과로 JavaScript 객체가 나오게 된다.

즉, 아래와 같은 JavaScript 객체가 생성된다.

```jsx
const element = {
		type: 'h1',
		props: {
				className: 'greeting',
				children: 'Hello, world!'
		}
}
```

React는 이 **객체들을 읽어서 DOM을 만드는 데 사용하고, 항상 최신 상태로 유지**한다.

React에서는 이 객체를 **element**라고 부른다.

그러면 `createElement` 의 함수로는 어떤것들이 들어갈까?

아래 코드는 `createElement` 함수의 파라미터를 나타낸 것이다.

```jsx
React.createElement(
		type,
		[props],
		[...children]
)
```

첫번째 파라미터는 `element`의 **타입 유형**을 나타낸다. 이 유형으로는 `div`나 `span` 같은 `html` 태그가 올 수도 있고, 다른 React 컴포넌트가 들어갈 수도 있다.

두번째 파라미터로는 `props`가 들어가는데, **속성**들이 들어가는 것이다.

마지막 세번째 파라미터로는 `children`이 들어가는데, 여기에서 `children`이란 **현재** `element`**가 포함하고 있는 자식** `element`라고 보면 된다.

React는 이런식으로 JSX 코드를 모두 `createElement` 함수를 사용하는 코드로 변환한다.

하지만 React에서 JSX를 사용하는 것이 필수는 아니다. 왜냐하면 직접 모든 코드를 `createElement` 함수를 사용해서 개발하면 되기 때문이다.

다만 JSX를 사용하면 코드가 더욱 간결해지고, 생산성과 가독성이 올라간다.

## JSX의 장점 및 사용법

### JSX의 장점

- 간결한 코드
- 가독성 향상
    - 버그를 발견하기 쉬움
- Injection Attacks 방어
    - Injection Attacks은 입력창에 문자나 숫자 같은 일반적인 값이 아닌 소스 코드를 입력하여 해당 코드가 실행되도록 만드는 해킹 방법이다.
        
        예를 들어 ID를 입력하는 입력창에 자바스크립트 코드를 넣었는데, 그 코드가 그대로 실행되면 큰 문제가 발생할 수 있다.
        
        **예시 코드**
        
        ```jsx
        const title = response.potentiallyMaliciousInput;
        
        // 해당 코드는 안전합니다.
        const element = <h1>{title}</h1>;
        ```
        
        해당 코드에는 title이라는 변수에 잠재적으로 보안 위험 가능성이 있는 코드가 삽입되었다.
        
        그리고 그 아래에 나오는 JSX 코드에서는 괄호를 사용해서 타이틀 변수를 삽입, 인베딩하고 있다.
        
        기본적으로 React DOM은 렌더링하기 전에 인베딩된 값을 모두 문자열로 변환한다. 그렇기 때문에 명시적으로 선언되지 않은 값은 괄호 사이에 들어갈 수 없다.
        
        즉, 결과적으로 XSS라 불리는 크로스 사이트 스크립팅 어택을 방어할 수 있는 것이다.
        
        이처럼 JSX를 사용하면 잠재적인 보안 위협을 줄일 수 있다.
        

### JSX 사용법

우선 JSX는 JavaScript + XML/HTML을 지원하기 때문에 XML과 HTML을 섞어서 사용하면 된다.

**예시**

```jsx
const name = 'Heo';
const element = <h1>안녕, {name}</h1>;

ReactDOM.render(
		element,
		document.getElementById('root')
);
```

위 코드에서 `element`를 선언하는 부분의 코드처럼 HTML과 자바스크립트가 섞인 형태로 코드를 작성하면 된다.

xml, html 코드를 사용하다가 중간에 자바스크립트를 사용하고 싶으면 중괄호를 써서 묶어주면 된다.

**예시**

```jsx
function formatName(user) {
		return user.firstName + ' ' + user.lastName;
}

const user = {
		firstName: 'YounYoung',
		lastName: 'Heo'
};

const element = (
		<h1>
				Hello, {formatUser(user)}
		</h1>
);

ReactDOM.render(
		element,
		document.getElementById('root')
);
```

해당 코드에서는 html 코드 사이의 괄호를 사용해서 변수가 아닌 `formatUser`라는 자바스크립트 함수를 호출하는 것을 볼 수 있다.

이런 식으로 JSX를 사용할 때 xml, html 코드 사이의 중괄호를 사용해서 자바스크립트 변수나 함수를 사용할 수 있다.

**예시**

```jsx
function getGreeting(user) {
		if(user) {
				return <h1>Hello, {formatName(user)}!</h1>;
		}
		return <h1>Hello, Stranger.</h1>
}
```

위 코드는 JSX를 사용해서 사용자 이름에 따라 다른 인삿말을 표시하도록 만든 컴포넌트이다.

사용자가 존재하면 `formatName`이라는 함수를 써서 포매팅된 이름을 출력하고 그렇지 않으면, `Stranger`에게 하는 인삿말을 출력한다.

그러면 HTML 태그 중간이 아닌 태그의 속성에 값을 넣고 싶을 때에는 어떻게 할까?

그럴 때에는 아래 코드처럼 큰 따옴표 사이에 문자열을 넣거나 중괄호 사이에 자바스크립트 코드를 넣으면 된다.

**태그의 속성(attribute)에 값을 넣는 방법**

```jsx
// 큰따옴표 사이에 문자열을 넣거나
const element = <div tabIndex="0"></div>

// 중괄호 사이에 자바스크립트 코드를 넣거나
const element = <img src={user.avatarUrl}></img>
```

그래서 JSX에서는 중괄호를 사용하면 무조건 자바스크립트 코드가 들어간다고 생각하면 된다.

**자식(children)을 정의하는 방법**

```jsx
const element = (
		<div>
				<h1>안녕하세요</h1>
				<h2>열심히 리액트를 공부하자!</h2>
		</div>
);
```

위 코드처럼 평소 HTML을 사용하듯이 상위 태그가 하위 태그를 둘러싸도록 하면 자연스럽게 children이 정의된다.

해당 코드에서 div 태그의 children은 h1태그와 h2태그가 된다.

이처럼 가독성도 높고, 간결하고, 직관적으로 코드를 작성할 수 있게 해주는 것이 바로 JSX의 역할이다.

**참고 자료**

- [https://www.inflearn.com/course/처음-만난-리액트/dashboard](https://www.inflearn.com/course/%EC%B2%98%EC%9D%8C-%EB%A7%8C%EB%82%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
