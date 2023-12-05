## **Hooks의 개념과 useState, useEffect**

![스크린샷1 2023-11-28 오후 5.04.25.png](https://github.com/Heo-y-y/development-blog/assets/112863029/b00dc428-2f17-46bb-a985-7612d6dd5593)

리액트 컴포넌트에는 두 가지 종류가 있다고 했다. 하나는 클래스 컴포넌트이고, 하나는 함수 컴포넌트이다. 또한 컴포넌트에는 state라는 중요한 개념이 등장한다. 이 state를 사용하여 렌더링에 필요한 데이터를 관리하는 것이다.

![스크린샷 2023-12-05 오후 8.46.59.png](https://github.com/Heo-y-y/development-blog/assets/112863029/7a65f719-a192-4241-bc08-83fddfca4984)

클래스 컴포넌트에서는 생성자에서 이 state를 정의하고 `setState()` 함수를 통해 state를 업데이트한다. 이처럼 클래스 컴포넌트는 state와 관련된 기능 뿐만 아니라 컴포넌트의 생명주기 함수들까지 모두 명확하게 정의되어 있기 때문에 잘 가져다 쓰면 된다.

하지만 기존 함수 컴포넌트는 클래스 컴포넌트와는 다르게 코드도 굉장히 간결하고, 별도로 state를 정의해서 사용하거나 컴포넌트의 생명주기에 맞춰 어떤 코드가 실행되도록 할 수 없다. 따라서 함수 컴포넌트에 이러한 기능을 지원하기 위하여 나온 거싱 바로 **Hook**이다.

![스크린샷 2023-12-05 오후 8.51.34.png](https://github.com/Heo-y-y/development-blog/assets/112863029/130a1224-de25-487e-965e-dc89d0f30fae)

**Hook**을 사용하면 **함수 컴포넌트도 클래스 컴포넌트의 기능을 모두 동일하게 구현**할 수 있다.

Hook이라는 영어 단어는 갈고리라는 뜻을 갖고 있는데, 보통 프로그래밍에서는 원래 존재하는 어떤 기능에 마치 갈고리를 거는 것처럼 끼어 들어가 같이 수행되는 것을 의미한다. 비슷하게 자주 사용되는 용어로는 **webhook**이라는 것이 있다.

**리액트의 Hook**도 마찬가지로 **리액트의 state와 생명주기 기능에 갈고리를 걸어 원하는 시점에 정해진 함수를 실행되도록 만든 것**이다.

그리고 이때 실행되는 함수를 **Hook**이라고 부른다. 이러한 **Hook의 이름은 모두 use로 시작**한다. Hook이 수행하는 기능에 따라서 이름을 짓게 되었는데, 각 기능을 사용하겠다는 의미로 use를 앞에 붙여쓴다. 개발자가 직접 커스텀 Hook을 만들어서 사용할 수도 있는데, 커스텀 Hook은 개발자 마음대로 이름을 지을 수 있지만 Hook의 규칙에 따라 이름 앞에 use를 붙여서 Hook이라는 것을 나타내 주어야 한다.

### useState()

`useState`는 이름에서 알 수 있듯이 **state를 사용하기 위한 Hook**이다.

함수 컴포넌트에서는 기본적으로 state라는 것을 제공하지 않기 때문에 클래스 컴포넌트처럼 state를 사용하고 싶으면 useState Hook을 사용해야 한다.

**예제 코드**

```jsx
import React, { useState } from "react";

function Counter(props) {
    var count = 0;

    return (
        <div>
            <p>총 {count}번 클릭했습니다.</p>
            <button onClick={() => count++}>
                클릭
            </button>
        </div>
    );
}
```

위 코드에는 `Counter` 컴포넌트라는 함수 컴포넌트가 쓰이는데, 이 컴포넌트는 버튼을 클릭하면 `count`를 하나씩 증가시키고, 현재 `count`를 보여준다.

그런데 만약 위 코드처럼 `count`를 함수의 변수로 선언하여 사용하게 되면 `button` 클릭 시 `count` 값을 증가시킬 수는 있지만 재렌더링이 일어나지 않아 새로운 `count` 값이 화면에 표시되지 않게 된다.

따라서 이런 경우에는 state를 사용해서 값이 바뀔 때마다 재렌더링이 되도록 해야 하는데, 함수 컴포넌트에는 해당 기능이 따로 없기 때문에 `useState`를 사용하여 state를 선언하고 업데이트해야 한다.

**useState() 사용법**

```jsx
const [변수명, set함수명] = useState(초기값);
```

`useState`를 호출할 때에는 파라미터로 선언할 state의 초기값이 들어간다.

클래스 컴포넌트의 생성자에서 state를 선언할 때 초기값을 넣어주는 것과 동일한 것이라고 생각하면 된다.

이렇게 초기값을 넣어 `useState`를 노출하면 `return` 값으로 배열이 나온다. 해당 배열에는 두 가지 항목이 들어 있는데, 첫번째 항목은 state로 선언된 변수이고, 두 번째 항목은 해당 state의 set 함수이다.

**useState를 적용한 예제**

```jsx
import React, { useState } from "react";

function Counter(props) {
    const [count, setCount] = useState(0);

    return (
        <div>
            <p>총 {count}번 클릭했습니다.</p>
            <button onClick={() => setCount(count + 1)}>
                클릭
            </button>
        </div>
    );
}
```

위 코드는 `useState`를 사용하여 `count` 값을 state로 관리하도록 만든 것이다. 버튼이 눌리면 `setCount` 함수를 호출해서 `count`를 1 증가 시키고, 그리고 `count`의 값이 변경되면 컴포넌트가 재렌더링 되면서 화면에 새로운 `count` 값이 표시된다. 이 과정은 클래스 컴포넌트에서 setState 함수를 호출해서 state가 업데이트 되고 이후 컴포넌트가 재렌더링 괴는 과정과 동일하다고 보면 된다.

다만 **클래스 컴포넌트**에서는 `setState` **함수 하나를 사용해서 모든 state 값을 업데이트**할 수 있었지만, `useState`를 사용하는 방법에서는 **변수 각각에 대해 set 함수가 따로 존재**한다.

### useEffect()

`useEffect`는 **side effect를 수행하기 위한 Hook**이다.

side effect는 그럼 뭘까?

**side effect**는 사전적으로 **부작용**이라는 뜻을 갖고 있다. 원래 단어의 의미 자체가 부정적인 느낌을 가지고 있는데, 컴퓨터 프로그래밍에서도 **부정적인 의미로 사용**되곤 한다. 개발자가 **의도치 않은 코드가 실행되면서 버그가 나타나면 side effect가 발생했다고 말한다**.

하지만 리액트에서의 side effect는 부정적인 의미는 아니다. **************************************************************************리액트에서 말하는 side effect는 효과 혹은 영향을 뜻하는 effect의 의미**************************************************************************에 가깝다.

예를 들면, 서버에서 데이터를 받아오거나 수동으로 DOM을 변경하는 등의 작업을 의미한다. 이런 작업을 **effect라고 부르는 이유**는 **이 작업들이 다른 컴포넌트에 영향을 미칠 수 있으며, 렌더링 중에는 작업이 완료될 수 없기 때문**이다. 즉, **렌더링이 끝난 이후에 실행되어야 하는 작업들**이다. 그래서 이러한 작업들이 사이드로 실행된다는 의미에서 side effect라고 불리며 `useEffect`는 **리액트의 함수 컴포넌트에서 side effect를 실행할 수 있게 해주는 Hook**이다.

**side effect**는 **클래스 컴포넌트에서 제공**하는 **생명 주기 함수**인 `componentDidMount`, `componentDidUpdate` 그리고 `componentWillUnmount`와 **동일한 기능을 하나로 통합해서 제공**한다.

그래서 useEffect Hook만으로 이러한 생명 주기 함수와 동일한 기능을 수행할 수 있다.

**useEffect() 사용법**

```jsx
useEffect(이펙트 함수, 의존성 배열);
```

여기서 **의존성 배열**은 말 그대로 이 **이펙트가 의존하고 있는 배열**인데, **배열 안에 있는 변수 중에 하나라도 값이 변경되었을 때** `effect` **함수가 실행**된다.

기본적으로 `effect` 함수는 **처음 컴포넌트가 렌더링 된 이후**와 **업데이트로 인한 재렌더링 이후에 실행**된다.

만약 `effect` 함수가 `mount`와 `unmount` 시에 **단 한 번씩만 실행**되게 하고 싶으면 아래처럼 **의존성 배열에** `bin` **배열을 넣으면 된다**.

```jsx
useEffect(이펙트 함수,[]);
```

이렇게 하면 해당 이펙트가 `props`나 state에 있는 어떤 값에도 의존하지 않는 것이 되므로 여러 번 실행되지 않는다.

그리고 **의존성 배열은 생략**할 수도 있는데, **생략하게 되면 컴포넌트가 업데이트 될 때마다 호출**된다.

```jsx
useEffect(이펙트 함수);
```

**useEffect()를 사용한 예제 코드**

```jsx
import React, { useState, useEffect } from "react";

function Counter(props) {
    const [count, setCount] = useState(0);

    // componentDidMount, componentDidUpdate와 비슷하게 작동
    useEffect(() => {
        // 브라우저 API를 사용해서 document의 title을 업데이트
        document.title = `You clicked ${count} times`;
    });

    return (
        <div>
            <p>총 {count}번 클릭했습니다.</p>
            <button onClick={() => setCount(count + 1)}>
                클릭
            </button>
        </div>
    );
}
```

`useEffect`를 사용해서 클래스 컴포넌트에서 제공하는 `componentDidMount`, `componentDidUpdate`와 같은 **생명주기 함수의 기능을 동일하게 수행**하도록 만들었다.

`useEffect` 안에 있는 `effect` 함수에서는 브라우저에서 제공하는 API를 사용해서 document의 title을 업데이트한다.

document의 title은 브라우저에서 페이지를 열었을 때 표시되는 문자열이다. 크롬 브라우저의 경우 탭에 나오는 제목이라 생각하면 된다.

위 코드처럼 **의존성 배열 없이** `useEffect`**를 사용**하면 **리액트는 DOM이 변경된 이후에** 해당 `effect` **함수를 실행하라는 의미로 받아들인**다. 그래서 기본적으로 **컴포넌트가 처음 렌더링 될 때를 포함해서 매번 렌더링 될 때마다 이펙트가 실행**된다고 보면 된다.

해당 코드의 경우 `effect` 함수가 처음 컴포넌트가 마운트 되었을 때 실행되고, 이후 컴포넌트가 업데이트 될 때마다 실행된다. 결과적으로 `componentDidMount` , `componentDidUpdate` 와 **동일한 역할**을 하게 되는 것이다.

또한 **이펙트는 함수 컴포넌트 안에서 선언**되기 때문에 **해당 컴포넌트의 props와 state에 접근할 수 있다**.

해당 코드에서는 `count`라는 state에 접근하여 해당 값이 포함된 문자열을 생성해서 사용하는 것을 볼 수 있다.

그렇다면 `componentWillUnmount` 와 동일한 기능은 `UseEffect`로 어떻게 구현할 수 있을까?

**예제 코드**

```jsx
import React, { useState, useEffect } from "react";

function UserStatus(props) {
    const [isOnline, setIsOnline] = useState(null);

    function handleStatusChange(status) {
        setIsOnline(status.isOnline);
    }

    useEffect(() => {
        ServerAPI.subscribeUserStatus(props.user.id, handleStatusChange);
        return () => {
            ServerAPI.unsubscribeUserStatus(props.user.id, handleStatusChange);
        }
    });

    if (isOnline == null) {
        return '대기 중...';
    }
    return isOnline ? '온라인' : '오프라인';
}
```

위 코드는 `useEffect`에서 먼저 `ServerAPI`를 사용하여 사용자의 상태를 구독하고 있다.

이후 함수를 하나 리턴하는데 해당 함수 안에는 구독을 해지하는 API를 호출하도록 되어있다.(**컴포넌트가 unmount 될 때 호출**)

`useEffect`에서 **리턴하는 함수**는 **컴포넌트가 mount 해제, 즉 unmount 될 때 호출**된다.

결과적으로 `useEffect`의 리턴 함수의 역할은 `componentWillUnmount` 함수가 하는 역할과 동일하다.

또한 useEffect Hook은 **하나의 컴포넌트에 여러 개를 사용**할 수 있다.

**두 개의 useEffect Hook 사용 예제**

```jsx
function UserStatusWithCounter(props) {

    const [count, setCount] = useState(0);
    useEffect(() => {
        document.title = `총 ${count}번 클릭했습니다.`;
    });

    const [isOnline, setIsOnline] = useState(null);
    useEffect(() => {
        ServerAPI.subscribeUserStatus(props.user.id, handleStatusChange);
        return () => {
            ServerAPI.unsubscribeUserStatus(props.user.id, handleStatusChange);
        }
    });

    function handleStatusChange(status) {
        setIsOnline(status.isOnline);
    }
```

위 코드를 보면 useState Hook과 useEffect Hook을 각각 두 개씩 사용하는 것을 볼 수 있다.

**useEffectHook의 사용법을 자세하게 정리**하면,

```jsx
useEffect(() => {
    // 컴포넌트가 마운트 된 이후,
    // 의존성 배열에 있는 변수들 중 하나라도 값이 변경되었을 때 실행됨
    // 의존성 배열에 빈 배열([])을 넣으면 마운트와 언마운트시에 단 한 번씩만 실행됨
    // 의존성 배열 생략 시 컴포넌트 업데이트 시마다 실행된
    ...

    return () => {
        // 컴포넌트가 마운트 해제되기 전에 실행됨
        ...
    }
}, [의존성 변수1, 의존성 변수2, ...]);
```

## **useMemo, useCallback, useRef**

### useMemo()

**useMemo Hook**은 **memoized value를 리턴하는 Hook**이다.

그럼 memoized value란 무엇일까?

useMemo와 useCallback Hook에서는 memoization이라는 개념이 나온다. 컴퓨터 분야에서 **memoization**은 **최적화를 위해 사용하는 개념**이다. 

**비용이 높은, 즉 연산량이 많이 드는 함수의 호출 결과를 저장해 두었다가 같은 입력 값으로 함수를 호출하면 새로 함수를 호출하지 않고, 이전에 저장해 놨던 호출 결과를 바로 반환**하는 것이다. 이렇게 하면 결과적으로 함수 호출 결과를 받기까지 걸리는 시간이 짧아질 뿐더러 불필요한 중복 연산도 하지 않기 때문에 컴퓨터의 자원을 적게 쓰게 될것이다. **********************************************************************************************memoization이 된 결과 값**********************************************************************************************을 영어로는 **********************************************************************************************Memoized Value**********************************************************************************************라고 부른다.

memoization의 Memo는 우리가 흔히 메모하다라고 표현하는 것과 비슷한 맥락이라고 생각하면 된다. 즉, 메모를 해두었다가 나중에 다시 사용하는 것이다.

**useMemo() 사용법**

```jsx
const memoizedValue = useMemo(
    () => {
        // 연산량이 높은 작업을 수행하여 결과를 반환
        return computeExpensiveValue(의존성 변수1, 의존성 변수2);
    },
    [의존성 변수1, 의존성 변수2]
);
```

**useMemo Hook**은 **파라미터**로 **memoized value를 생성하는** `create` **함수**와 **의존성 배열**을 받는다.

memoization의 개념처럼 **의존성 배열에 들어있는 변수가 변했을 경우**에만 **새로** `create` **함수를 호출하여 결과값을 반환**하며, **그렇지 않은 경우에는 기존 함수의 결과값을 반환**한다.

useMemo Hook을 사용하면 컴포넌트가 다시 렌더링 될 때마다 연산량이 높은 작업을 반복하는 것을 피할 수 있다. 결과적으로 빠른 렌더링 속도를 얻을 수 있는 것이다.

**useMemo Hook**을 사용할 때 기억해야 할 점은 **useMemo로 전달된 함수는 렌더링이 일어나는 동안 실행**된다는 점이다. 그렇기 때문에 일반적으로 렌더링이 일어나는 동안 실행돼서는 안될 작업을 useMemo의 함수에 넣으면 안된다. 예를 들면, useEffect Hook에서 실행돼야 할 side effect 같은 것이 있다. 서버에서 데이터를 받아오거나 수동으로 DOM을 변경하는 작업 등은 렌더링이 일어나는 동안 실행돼서는 안되기 때문에 useMemo Hook 함수에 넣으면 안되고, useEffect Hook을 사용해야 한다.

```jsx
const memoizedValue = useMemo(
    () => computeExpensiveValue(a, b)
);
```

그리고 또한 위 코드와 같이 **의존성 배열을 넣지 않을 경우 렌더링이 일어날 때마다 매번** `create` **함수가 실행**된다. 따라서 useMemo Hook에 의존성 배열을 넣지 않고 사용하는 것은 아무런 의미가 없다.

```jsx
const memoizedValue = useMemo(
    () => {
        return computeExpensiveValue(a, b);
    },
    []
);
```

그리고 이렇게 의존성 배열에 빈 배열을 넣게 되면 컴포넌트가 마운트 될 때만 `create` 함수가 호출된다. 결국 마운트 이후에는 값이 변경되지 않는 것이다. 따라서 **마운트 시점에만 한번 값을 계산할 필요가 있을 경우**에는 위 코드처럼 사용하면 된다.

하지만 대부분의 경우에는 useMemo Hook에 의존성 배열의 변수들을 넣고 해당 변수들의 값이 바뀜에 따라 새로 값을 계산해야 할 경우에 사용한다.

### useCallback()

**useCallback Hook**은 **useMemo Hook과 유사하지만 값이 아닌 함수를 반환**한다. 쉽게 말해 컴포넌트가 렌더링 될 때마다 매번 함수를 새로 정의하는 것이 아니라 **의존성 배열의 값이 바뀐 경우에만 함수를 새로 정의해서 리턴**해주는 것이다.

**useCallback() 사용법**

```jsx
const memoizedValue = useCallback(
    () => {
        doSomething(의존성 변수1, 의존성 변수2);
    },
    [의존성 변수1, 의존성 변수2]
);
```

**useCallback Hook**은 useMemo Hook과 마찬가지로 **함수**와 **의존성 배열**을 **파라미터로 받는다**. 파라미터로 받는 이 함수를 **callback**이라고 부른다.

그리고 **의존성 배열에 있는 변수 중 하나라도 변경**되면 **memoization된** `callback` **함수를 반환**한다. 의존성 배열에 따라 memoization 값을 반환한다는 점에서는 useMemo Hook과 완전히 동일하다. 따라서 아래 보이는 두 줄의 코드는 동일한 역할을 한다고 볼 수 있다.

```jsx
useCallback(함수, 의존성 배열);
useMemo(() => 함수, 의존성 배열);
```

만약 useCallback Hook을 사용하지 않고, 컴포넌트 내에 함수를 정의한다면 매번 렌더링이 일어날 때마다 함수가 새로 정의된다. 따라서 useCallback Hook을 사용하여 특정 변수의 값이 변한 경우에만 함수를 다시 정의하도록 해서 불필요한 반복 작업을 없애주는 것이다.

**useCallback Hook을 사용하지 않은 코드**

```jsx
import React, { useState } from "react";

function ParentComponent(props) {
    const [count, setCount] = useState(0);

    // 재렌더링 될 때마다 매번 함수가 새로 정의됨
    const handleClick = (event) => {
        // 클릭 이벤트 처리
    };

    return (
        <div>
            <button
                onClick={() => {

                }}
            >
                {count}
            </button>

            <ChildComponent handleClick={handleClick} />
        </div>
    );
}
```

위 코드처럼 useCallbackHook을 사용하지 않고, 컴포넌트 내에서 정의한 함수를 자식 컴포넌트의 props로 넘겨 사용하는 경우에 부모 컴포넌트가 다시 렌더링 될 때마다 매번 자식 컴포넌트도 다시 렌더링된다.

**useCallbackHook을 사용한 코드**

```jsx
import React, { useState } from "react";

function ParentComponent(props) {
    const [count, setCount] = useState(0);

    // 컴포넌트가 마운트 될 때만 함수가 정의됨
    const handleClick = useCallback((event) => {
        // 클릭 이벤트 처리
    }, []);

    return (
        <div>
            <button
                onClick={() => {

                }}
            >
                {count}
            </button>

            <ChildComponent handleClick={handleClick} />
        </div>
    );
        }
```

**useCallbackHook**을 사용할 경우 **특정 변수의 값이 변한 경우에만 함수를 다시 정의**하게 되므로 **함수가 다시 정의되지 않는 경우에는 자식 컴포넌트도 재렌더링이 일어나지 않는다**.

위 경우에는 의존성 배열에 빈 배열이 들어갔기 때문에 컴포넌트가 처음 마운트 되는 시점에만 함수가 정의되고 이후에는 다시 정의되지 않으며, 결국 자식 컴포넌트도 불필요하게 재렌더링이 일어나지 않게 된다.

### useRef()

**useRef Hook**은 **reference를 사용하기 위한 Hook**이다.

그러면 reference란 무엇일까?

**리액트에서 reference**란 **특정 컴포넌트에 접근할 수 있는 객체**를 의미한다. 그리고 **useRef Hook**은 바로 이 **reference 객체를 반환**한다.

![스크린샷 2023-12-05 오후 11.09.55.png](https://github.com/Heo-y-y/development-blog/assets/112863029/da39c8b4-0c4e-466a-8890-05378c50e6ba)

reference 객체에는 **current**라는 속성이 있는데, 이것은 **현재 reference하고 있는 element를 의미**한다고 보면 된다.

**useRef() 사용법**

```jsx
const refContainer = useRef(초깃값);
```

파라미터로 초기값을 넣으면 해당 초기값으로 초기화된 reference 객체를 반환한다. 이렇게 **반환된 reference 객체는 컴포넌트의 LifeTime 전체에 걸쳐서 유지**된다. 즉, **컴포넌트가 마운트 해제 전까지는 계속 유지**된다는 이야기다. 쉽게 말해서 useRef Hook은 변경 가능한 current라는 속성을 가진 하나의 상자라고 생각하면 된다.

**예제 코드**

```jsx
function TextInputWithFocusButton(props) {
    const inputElem = useRef(null);

    const onButtonClick = () => {
        // 'current'는 마운트된 input element를 가리킴
        inputElem.current.focus();
    };
    
    return (
        <>
            <input ref={inputElem} type="text" />
            <button onClick={onButtonClick}>
                Focus the input
            </button>
        </>
    );
}
```

위 코드는 userRef Hook을 사용하여 버튼 클릭 시 `input`에 포커스를 하도록하는 코드이다.

초기값으로 `null`을 넣었고, 결과로 반환된 `inputElem`이라는 reference 객체를 `input` 태그에 넣어서 사용한다. 그리고 `button` 클릭 시 호출되는 함수에서 `inputElem.current`를 통해 실제 element에 접근하여 포커스 함수를 호출하고 있다.

```jsx
<div ref={myRef} />
```

**리액트**에서는 위 코드와 같이 작성하면 노드가 변경될 때마다 `myRef`의 **current 속성에 현재 해당되는 DOM 노드를 저장**한다.

`ref` 속성과 기능은 비슷하지만 **useRef Hook**은 **클래스의 instance 필드를 사용하는 것과 유사하게 다양한 변수를 저장할 수 있다는 장점**이 있다. 이런 것이 가능한 이유는 **useRef Hook이 일반적인 자바스크립트 객체를 리턴하기 때문**이다.

그럼 직접 current 속성이 포함된 자바스크립트 객체를 만들어 써도 된는거 아닌가라는 생각이 들 수 있다. 물론 그렇게 해도 목적 달성은 가능할 수 있지만 userRef Hook을 사용하는 것과 직접 current 속성이 포함된 모양의 객체를 만들어 사용하는 것의 차이점은 **useRef Hook**은 **매번 렌더링 될 때마다 항상 같은 reference 객체를 반환**한다는 것이다.

그리고 **useRef Hook**은 **내부의 데이터가 변경되었을 때 별도로 알리지 않는다**. 그래서 **current 속성을 변경한다고 해서 재렌더링이 일어나진 않는다**.

따라서 `ref`에 **DOM 노드가 연결되거나 분리되었을 경우**에 **어떤 코드를 실행하고 싶다면** **Callback ref를 사용**해야 한다. 

### Callback ref

DOM 노드의 변화를 알기 위한 가장 기초적인 방법으로 Callback ref를 사용하는 방법이 있다. **리액트**는 `ref`가 **다른 노드에 연결될 때마다 callback을 호출**하게 된다.

**예제 코드**

```jsx
function MeasureExample(props) {
    const [height, setHeight] = useState(0);

    const measuredRef = useCallback(node => {
        if (node !== null) {
            setHeight(node.getBoundingClientRect().height);
        }
    }, []);

    return (
        <>
            <h1 ref={measuredRef}>안녕, 리액트</h1>
            <h2>위 헤더의 높이는 {Math.round(height)}px 입니다.</h2>
        </>
    );
}
```

위 코드는 reference를 위해서 useRef Hookd을 사용하지 않고, useCallback Hook을 사용하는 Callback ref 방식이다. useRef Hook을 사용하게 되면 reference 객체가 current 속성이 변경되었는지를 따로 알려주지 않기 때문이다.

하지만 **Callback ref** 방식을 사용하게 되면 **자식 컴포넌트가 변경되었을 때 알림을 받을 수 있고, 이를 통해 다른 정보들을 업데이트**할 수 있다.

위 예제 코드에서는 `h1` 태그의 높이 값을 매번 업데이트하고 있다. 그리고 useCallback Hook의 의존성 배열로 비어있는 배열, 즉 mtrl을 넣었는데 이렇게 하면 `h1` 태그가 마운트, 언마운트 될 때만 `callback` 함수가 호출되며 재렌더링이 일어날 때에는 호출되지 않는다.

## **Hook의 규칙과 Custom Hook 만들기**

### Hook의 규칙

Hook은 단순한 자바스크립트 함수이지만 두 가지 지켜야 할 규칙이있다.

1. **Hook은 무조건 최상위 레벨에서만 호출해야 한다.**
    
    여기에서 말하는 **최상위 레벨**은 **리액트 함수 컴포넌트의 최상위 레벨**을 의미한다. 따라서 반복문이나 조건문 또는 중첩된 함수들 안에서 Hook을 호출하면 안 된다는 뜻이다. 이 규칙에 따라서 **Hook은 컴포넌트가 렌더링 될 때마다 매번 같은 순서로 호출되어야 한다**. 이렇게 해야 리액트가 다수의 useState Hook과 useEffect Hook에 호출해서 컴포넌트의 state를 올바르게 관리할 수 있게 된다.
    
2. **리액트 함수 컴포넌트에서만 Hook을 호출해야 한다.**
    
    일반적인 자바스크립트 함수에서 Hook을 호출하면 안된다. **Hook**은 **리액트 함수 컴포넌트에서 호출**하거나 **직접 만든 커스텀 Hook에서만 호출**할 수 있다. 이 규칙에 따라 리액트 컴포넌트에 있는 state와 관련된 모든 로직은 소스 코드를 통해 명확하게 확인이 가능해야 한다.
    

### eslint-plugin-react-hooks

Hook의 규칙과 관련해서 개발에 도움이 되는 **eslint-plugin-react-hooks**라는 패키지가 있다. 이 플러그인은 **Hook의 규칙을 따르도록 강제해주는 프러그인**이다. **eslint**는 **자바스크립트 코드에서 발견되는 문제 패턴을 식별하기 위한 정적 코드 분석 도구**이다. 그리고 이 플러그인을 사용하면 **리액트 컴포넌트가 Hook의 규칙을 따르는지 아닌지 분석**할 수 있다. 이 플러그인은 의존성 배열이 잘못되어 있는 경우에 자동으로 경고 표시를 해주며 고칠 방법을 제안해 주기도 한다.

### Custom Hook 만들기

리액트에서 기본적으로 제공되는 Hook들 이외에 추가적으로 필요한 기능이 있다면 직접 Hook을 만들어서 사용할 수 있다. 이것을 **Custom Hook**이라고 부르는데, Custom Hook을 만드는 이유는 **여러 컴포넌트에서 반복적으로 사용되는 로직을 Hook으로 만들어 재사용하기 위함**이다.

- **Custm Hook을 만들어야 하는 상황**

```jsx
import React, { useState, useEffect } from "react";

function UserStatus(props) {
    const [inOnline, setIsOnline] = useState(null);

    useEffect(() => {
        function handleStatusChange(status) {
            setIsOnline(status.isOline);
        }

        SeverAPI.subscribeUserStatus(props.user.id, handleStatusChange);
        return () => {
            SeverAPI.unsubscribeUserStatus(props.user.id, handleStatusChange);
        };
    });

    if (isOline === null) {
        return '대기중...';
    }
    return isOline ? '온라인' : '오프라인';
}
```

위 코드에 나와 있는 `userStatus`라는 컴포넌트는 `isOnline`이라는 state에 따라서 사용자의 상태가 온라인인지 아닌지를 텍스트로 보여주는 컴포넌트이다.

그리고 동일한 웹 사이트에서 연락처 목록을 제공하는데 이때 온라인인 사용자의 이름은 초록색으로 표시해주고 싶다고 가정하자. 이 컴포넌트의 이름을 `UserListItem`이라고 하고 여기에 비슷한 로직을 넣어야 한다.

```jsx
import React, { useState, useEffect } from "react";

function UserListItem(props) {
    const [inOnline, setIsOnline] = useState(null);

    useEffect(() => {
        function handleStatusChange(status) {
            setIsOnline(status.isOline);
        }

        SeverAPI.subscribeUserStatus(props.user.id, handleStatusChange);
        return () => {
            SeverAPI.unsubscribeUserStatus(props.user.id, handleStatusChange);
        };
    });

    return (
        <li style={{ color: isOline ? 'green' : 'black' }}>
            {props.user.name}
        </li>
    );
}
```

위 코드와 같이 작성하면 되는데, 앞에서 작성한 `UserStatus`와 `useState`, `useEffect` Hook을 사용하는 부분이 동일한 것을 확인할 수 있다. 즉, 여러 곳에서 중복되는 코드인 것이다.

기존의 리액트에서는 보통 이렇게 state와 관련된 로직이 중복되는 경우에 render props 또는 HOC라고 불리는 higher order component를 사용했다.

하지만 여기에서는 **중복되는 코드를 추출**하여 **Custom Hook**으로 만드는 새로운 방법을 적용해보자.

- **Custom Hook 추출하기**

두 개의 자바스크립트 함수에서 하나의 로직을 공유하도록 하고 싶을 때에는 새로운 함수를 하나 만드는 방법을 사용한다. **리액트 컴포넌트**와 **Hook**은 **모두 함수이기 때문에 동일한 방법을 사용할 수 있는 것**이다.

**Custom Hook**은 무언가 특별한 것이 아니라 **이름이 use로 시작하고 내부에서 다른 Hook을 호출하는 하나의 자바스크립트 함수**이다.

**useUserSatus라는 Custom Hook으로 추출한 코드**

```jsx
import React, { useState, useEffect } from "react";

function useUserStatus(userId) {
    const [inOnline, setIsOnline] = useState(null);

    useEffect(() => {
        function handleStatusChange(status) {
            setIsOnline(status.isOline);
        }

        SeverAPI.subscribeUserStatus(props.user.id, handleStatusChange);
        return () => {
            SeverAPI.unsubscribeUserStatus(props.user.id, handleStatusChange);
        };
    });

    return isOnline;
```

위 코드를 보면 특별한 것이 없고, 그냥 두 개의 컴포넌트에서 중복되는 로직을 추출해서 가져온 것이다. 다만 다른 컴포넌트 내부에서와 마찬가지로 **다른 Hook으로 추출하는 것은 무조건 Custom Hook의 최상위 레벨**에서만 해야 한다.

리액트 컴포넌트와 달리 **Custom Hook은 특별한 규칙이 없다**. 예를 들면, 파라미터로 무엇을 받을지, 어떤 것을 리턴해야 할지를 개발자가 직접 정할 수 있다. 다시 말하면 Custom Hook은 그냥 단순한 함수와도 같다. 하지만 이름은 **use로 시작하도록 해서 이것이 단순한 함수가 아닌 리액트 Hook이라는 것을 나타내 주는 것**이다.

또한 Hook이기 때문에 위에서 말한 **Hook의 규칙이 적용**된다.

useUserStatus Hook의 목적은 사용자의 온라인, 오프라인 상태를 구독하는 것이다. 그렇기 때문에 위 코드처럼 useUserStatus Hook의 파라미터로 `userId`를 받도록 진행하였고, 해당 사용자가 온라인인지 오프라인인지 상태를 리턴하게 했다.

- **Custom Hook 사용하기**

처음 Custom Hook을 만들기로 했을 때의 목표는 `UserStatus`와 `UserListItem` 컴포넌트로부터 중복된 로직을 제거하기 위함이였다. 그리고 두 개의 컴포넌트는 모두 사용자가 온라인 상태인지 오프라인 상태인지를 알기 원했다. 이제 중복되는 로직을 useUserStatus Hook으로 추출했기 때문에 이 Hook을 사용하여 아래와 같이 코드를 변경할 수 있다.

```jsx
function UserStatus(props) {
    const isOnline = useUserStatus(props.user.id);

    if (isOline === null) {
        return '대기중...';
    }
    return isOline ? '온라인' : '오프라인';
}

function UserListItem(props) {
    const isOnline = useUserStatus(props.user.id);

    return (
        <li style={{ color: isOline ? 'green' : 'black' }}>
            {props.user.name}
        </li>
    );
}
```

이 코드는 **Custom Hook을 적용하기 전과 동일하게 작동**한다. 동작에 변경이 없고, 중복되는 로직만을 추출하여 Custom Hook으로 만든 것이기 때문이다.

**Custom Hook**은 **리액트 기능이 아닌 Hook의 디자인에서 자연스럽게 따르는 규칙**이다.

정리하면,

1. Custom Hook의 이름은 꼭 use로 시작해야한다.
2. 여러 개의 컴포넌트에서 하나의 Custom Hook을 사용할 때 컴포넌트 내부에 있는 모든 state와 effects는 전부 분리되어 있다.
3. 각 Custom Hook 호출에 대해 분리된 state를 얻게 된다.
4. 각 Custom Hook의 호출 또한 완전히 독립적이다.

**참고 자료**

- [https://www.inflearn.com/course/lecture?courseSlug=처음-만난-리액트&unitId=113225&category=questionDetail](https://www.inflearn.com/course/lecture?courseSlug=%EC%B2%98%EC%9D%8C-%EB%A7%8C%EB%82%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8&unitId=113225&category=questionDetail)
