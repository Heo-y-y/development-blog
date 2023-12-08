# Forms

## **Form과 Controlled Component**

### Form

**form**은 우리말로 **양식**이라는 뜻을 가지고 있다.

우리가 웹 사이트를 탐색하다 보면 아래 그림과 같이 정보를 입력하는 양식을 종종 볼 수 있다.

![스크린샷 2023-12-08 오후 2.18.32.png](https://github.com/Heo-y-y/development-blog/assets/112863029/a8aecd93-0685-4e67-891d-fd8bc476a2fe)

보통 회원가입을 하거나 로그인을 할 때 이와 같이 텍스트를 입력하는 양식을 많이 볼 수 있는데, 텍스트 입력 뿐만 아니라 체크박스 셀렉트 등 **사용자가 무언가 선택을 해야 하는 것 모두**를 **form**이라고 생각하면 된다. 정리하면, **form**은 **사용자로부터 입력을 받기 위해 사용하는 것**이다.

리액트에서의 form과 HTML의 폼은 조금 차이가 있다. **리액트**는 **컴포넌트 내부에서 state를 통해 데이터를 관리**한다. 반면에 **HTML** 폼은 **엘리먼트 내부에 각각의 state가 존재**한다.

**HTML Form**

```jsx
<form>
    <label>
        이름:
        <input type="text" name="name" />
    </label>
    <button type="submit">제출</button>
</form>
```

위 코드는 사용자의 이름을 입력 받고, 제출하는 아주 간단한 코드이다. 이 코드는 리액트에서도 잘 동작하지만 자바스크립트 코드를 통해 사용자가 입력한 값에 접근하기에는 불편한 구조이다. 자바스크립트 코드에서 사용자가 입력한 값에 접근하고 제어할 수 있어야 웹 페이지를 개발할 때 더 편리하다.

그러면 사용자가 입력한 값에 접근하고, 제어할 수 있도록 하는 **Controlled Component**를 알아보자

### **Controlled Component**

**Controlled Component**는 **사용자가 입력한 값에 접근하고, 제어할 수 있도록** 해주는 컴포넌트이다. 이름 그대로 누군가의 **통제를 받는 컴포넌트**인데, 여기에서 **통제를 하는 그 누군가가 바로 리액트**이다. 정리하면, **Controlled Component**는 **그 값이 리액트의 통제를 받는 Input Form Element를 의미**한다.

아래 그림을 보자.

![스크린샷 2023-12-08 오후 2.31.00.png](https://github.com/Heo-y-y/development-blog/assets/112863029/fad3fdff-338f-49d1-adce-541ed190fb10)

**HTML Form**에서는 **각 엘리먼트가 자체적으로 state를 관리**한다. 따라서 input, textarea, select 태그가 각각 내부의 state를 갖고있다. 이러면 자바스크립트 코드를 통해 각각의 값에 접근하기가 쉽지 않을 것이다.

하지만 **Controlled Component**에서는 **모든 데이터를 state에서 관리**한다. 또한 **state 값을 변경할 때**에는 무조건 **setState** 함수를 사용한다.

참고로 위 그림은 클래스 컴포넌트를 기준으로 그린 것인데, **함수 컴포넌트**에서는 **useState Hook을 사용하여 state를 관리**한다. 이처럼 Controlled Component는 리액트에서 모든 값을 통제할 수 있는 구조를 가지고 있다.

**예제 코드**

```jsx
function NameForm(props) {
    const [value, setValue] = setState('');

    const handleChange = (event) => {
        setValue(event.target.value);
    }

    const handleSubmit = (event) => {
        alert('입력한 이름: ' + value);
        event.preventDefault();
    }

    return (
        <form onSubmit={handleSubmit}>
            <label>
                이름:
                <input type="text" value={value} onChange={handleChange} />
            </label>
            <button type="submit">제출</button>
        </form>
    )
}
```

위 코드는 앞에서 나온 사용자의 이름을 입렫받는 HTML Form을 리액트의 Controlled Component로 만든 것이다.

위 코드에서 `value={value}` 를 볼 수 있는데 리액트 컴포넌트의 state에서 값을 가져다 넣어주는 것이다. 그래서 항상 state에 들어있는 값이 `input`에 표시된다.

또한 입력 값이 변경되었을 때에는 `onChange={handleChange}`를 사용했는데, `handleChange` 함수에서는 `setValue` 함수를 사용하여 새롭게 변경된 값을 `value`라는 이름의 state에 저장한다.

참고로 `onChange` callback 함수의 첫번째 파라미터인 `event`는 이벤트 객체를 나타낸다. 그리고 `event.target`은 현재 발생한 이벤트의 타겟을 의미하며, `event.target.value`는 해당 타겟의 `value` 속성의 값을 의미한다. 즉, 여기에서의 타겟은 input element가 되며 `event.target.value`는 input element의 값이 된다.

이처럼 **Controlled Component**를 사용하면 **입력 값이 리액트 컴포넌트의 state를 통해 관리**된다. 즉, **여러 개의 입력 양식의 값을 원하는 대로 조정할 수 있다**는 뜻이다. 입력 양식의 초기 값을 내가 원하는 대로 넣어줄 수도 있으며, 다른 양식의 값이 변경되었을 때 또 다른 양식의 값도 자동으로 변경시킬 수 있다는 것이다.

**예시 코드**

```jsx
const handleChange = (event) => {
    setValue(event.target.value.toUpperCase());
}
```

예를 들어 사용자가 입력한 모든 알파벳을 대문자로 변경시켜서 관리하고 싶다면 위 코드와 같이 작성하면 된다.

`handleChange` 함수로 들어오는 모든 `event`의 `target` 값을 `toUpperCase` 함수를 사용하여 모두 대문자로 변경한 뒤에 그 값을 state에 저장하는 것이다. 이처럼 **제어 컴포넌트**를 통해 **사용자의 입력을 직접적으로 제어**할 수 있다.

## **다양한 Forms**

### Textarea 태그

**Textarea** **태그**는 **여러 줄에 걸쳐 긴 텍스트를 입력받기 위한 HTML 태그**이다.

**HTML Textarea 태그**

```jsx
<textarea>
    안녕하세요, 여기에 이렇게 텍스트가 들어가게 됩니다.
</textarea>
```

HTML에서는 위 코드와 같이 텍스트를 태그가 감싸는 형태로 사용한다. 즉, HTML Textarea children으로 텍스트가 들어가는 형태이다.

반면에 리액트에서는 textarea 태그에 value라는 attribute를 사용하여 텍스트를 표시한다. 이 방식은 앞에서 말한 Controlled Component 방식인데 값을 컴포넌트의 state를 사용해서 다룰 수 있다.

**예제 코드**

```jsx
function RequestForm(props) {
    const [vlaue, setValue] = useState('요청사항을 입력하세요.');

    const handleChange = (event) => {
        setValue(event.target.value);
    }

    const handleSubmit = (event) => {
        alert('입력한 요청사항: ' + value);
        event.preventDefault();
    }

    return (
        <form onSubmit={handleSubmit}>
            <label>
                요청사항:
                <textarea value={value} onChange={handleChange} />
            </label>
            <button type="submit">제출</button>
        </form>
    )
}
```

위 코드는 고객으로부터 요청사항을 입력 받기 위한 `RequestForm` 컴포넌트이다.

state로는 `value`가 있고, 이 값을 `textarea` 태그에 `value`라는 attribute에 넣어줌으로써 화면에 나타나게 된다. 여기에서는 `value`를 선언할 때 초기 값을 넣어줬기 때문에 처음 렌더링 될 때부터 `textarea`에 텍스트가 나타나게 된다.

### Select 태그

**Select 태그**는 **Drop-down 목록을 보여주기 위한 HTML 태그**이다. **Drop-down 목록**은 **여러가지 옵션 중에서 하나를 선택할 수 있는 기능을 제공**한다.

**HEML Select 태그**

```jsx
<select>
    <option value="apple">사과</option>
    <option value="banana">바나나</option>
    <option selected value="grape">포도</option>
    <option value="watermelon">수박</option>
</select>
```

**HTML**에서는 위 코드와 같이 `option` **태그를** `select` **태그가 감싸는 형태로 사용**한다.

`option` 태그에서 현재 선택된 옵션의 경우에는 `selected`라는 attribute를 가지고 있다. 코드를 보면 `grape`에 `selected` 속성이 들어가 있는 것을 볼 수 있는데 현재 포도가 선택되어 있는 상태라는 것을 알 수 있다.

**리액트**에서는 `option` 태그에 `selected` 속성을 사용하지 않고, 대신에 `select` **태그에** `value`**라는 attribute를 사용해서 값을 표시**한다.

**예제 코드**

```jsx
function FruitSelect(props) {
    const [vlaue, setValue] = useState('grape');

    const handleChange = (event) => {
        setValue(event.target.value);
    }

    const handleSubmit = (event) => {
        alert('선택한 과일: ' + value);
        event.preventDefault();
    }

    return (
        <form onSubmit={handleSubmit}>
            <label>
                과일을 선택하세요:
                <select value={vlaue} onChange={handleChange}>
                    <option value="apple">사과</option>
                    <option value="banana">바나나</option>
                    <option value="grape">포도</option>
                    <option value="watermelon">수박</option>
                </select>
            </label>
            <button type="submit">제출</button>
        </form>
    )
}
```

위 코드에는 `FruitSelect`이라는 컴포넌트가 있고, 이 컴포넌트의 state로 `grape`라는 초기 값을 가진 `value`가 하나 있다. 그리고 이 값을 `select` 태그에 `value`로 넣어주고 있다. 값이 변경된 경우에는 앞에서와 마찬가지로 `handleChange` 함수에서 `setValue` 함수를 사용하여 값을 업데이트한다. 이 방식을 사용하게 되면 사용자가 옵션을 선택했을 때 `value`라는 하나의 값만을 업데이트하면 되기 때문에 더 편리하다.

만약 목록에서 다중으로 선택이 되도록 하려면 아래 코드와 같이 작성하면 된다.

```jsx
<select multiple={true} value={['B', 'C']}>
```

방식을 정리하면 아래와 같다.

```jsx
// input 태그
<input type="text" value={value} onChange={handleChange} />

// textarea 태그
<textarea value={value} onChange={handleChange} />

// select 태그
<select value={vlaue} onChange={handleChange}>
    <option value="apple">사과</option>
    <option value="banana">바나나</option>
    <option value="grape">포도</option>
    <option value="watermelon">수박</option>
</select>
```

### File input 태그

**File input** 태그는 말 그대로 **디바이스의 저장 장치로부터 하나 또는 여러 개의 파일을 선택할 수 있게 해주는 HTML 태그**이다. 보통은 서버로 파일을 업로드하거나, 자바스크립트 File API를 사용해서 파일을 다룰 때 사용한다. 

**HTML File input 태그**

```jsx
<input type="file" />
```

참고로 **File input 태그**는 그 값이 **읽기 전용**이기 때문에 **리액트에서는 Uncontrolled Component**가 된다. 즉, 값이 **리액트의 통제를 받지 않는다는 것**이다. 

지금까지는 하나의 컴포넌트에서 하나의 입력만을 다뤘는데, 만약 하나의 컴포넌트에서 여러 개의 입력을 다루기 위해서는 어떻게 해야 할까?

이런 경우에는 **여러 개의 state를 선언하여 각각의 입력에 대해 사용**하면 된다.

**예제 코드**

```jsx
function Reservation(props) {
    const [haveBreakfast, setHaveBreakfast] = useState(true);
    const [numberOfGuest, setNumberOfGuest] = useState(2);

    const handleSubmit = (event) => {
        alert(`아침식사 여뷔: ${haveBreakfast}, 방문객 수: ${numberOfGuest}`);
        event.preventDefault();
    }

    return (
        <form onSubmit={handleSubmit}>
            <label>
                아침식사 여부:
                <input 
                    type="checkbox"
                    checked={haveBreakfast}
                    onChange={(event) => {
                        setHaveBreakfast(event.target.checked);
                    }} />
            </label>
            <br />
            <label>
                방문객 수:
                <input 
                    type="number"
                    value={numberOfGuest}
                    onChange={(event) => {
                        setNumberOfGuest(event.target.vlaue);
                    }} />
            </label>
            <button type="submit">제출</button>
        </form>
    );
}
```

위 코드는 `Reservation`이라는 호텔 예약을 위한 컴포넌트이다. 예약을 하기 위한 정보 아침식사 여부와 방문객 수 두 가지 정보를 입력 받도록 되어있다.

아침식사 선택 유무를 입력받기 위한 `input` 태그는 `type`이 `checkbox`로 되어 있고, 값이 변경되면 `setHaveBreakfast` 함수를 통해 값을 업데이트한다.

방문객 수의 `input` 태그는 `type`이 `number`로 되어 있고, 값이 변경되면 `setNumberOfGuest` 함수를 통해 값을 업데이트한다.

클래스 컴포넌트에서는 `setState` 함수 하나로 모든 `state`의 값을 업데이트했지만, 함수 컴포넌트에서는 각 `state`의 변수마다 `set` 함수가 따로 존재하기 때문에 이와 같은 형태로 각각의 `set` 함수를 사용해서 구현하면 된다.

제어 컴포넌트에 value props를 정해진 값으로 넣으면 코드를 수정하지 않는 한 입력 값을 바꿀 수 없다. 만약 value props는 넣되 자유롭게 입력할 수 있게 만들고 싶다면 값에 undefined 또는 null을 넣어주면 된다.

**예제 코드** 

```jsx
ReactDOM.render(<input value="hi" />, rootNode);

setTimeout(function() {
    ReactDOM.render(<input value={null} />, rootNode);
}, 1000);
```

처음에는 input 값이 hi로 정해져 있어서 값을 바꿀 수 없는 입력 불가 상태였다가, 타이머에 의해 1초 뒤에 value가 null인 input 태그가 렌더링 되면서 입력 가능한 상태로 바뀐다.

이러한 방법을 잘 활용하면 value props를 넣으면서 동시에 사용자가 자유롭게 입력할 수 있다.

**참고 자료**

- [https://www.inflearn.com/course/lecture?courseSlug=처음-만난-리액트&unitId=113234](https://www.inflearn.com/course/lecture?courseSlug=%EC%B2%98%EC%9D%8C-%EB%A7%8C%EB%82%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8&unitId=113234)
