# Lifting State Up

## Shared State

**리액트**로 개발하다 보면 **하나의 데이터를 여러 개의 컴포넌트에서 표현해야 하는 경우**가 종종 있는데, 이러한 경우 각 컴포넌트의 state에서 데이터를 각각 보관하는 것이 아니라 **가장 가까운 공통된 부모 컴포넌트의 state를 공유해서 사용하는 것이 더 효율적**이다.

**Shared State**는 말 그대로 **공유된 state**를 의미한다. **자식 컴포넌트들이 가장 가까운 공통된 부모 컴포넌트의 state를 공유해서 사용**하는 것이다.

**Shared State**는 **어떤 컴포넌트의 state에 있는 데이터를 여러 개의 하위 컴포넌트에서 공통적으로 사용하는 경우**를 말한다.

![스크린샷 2023-12-11 오후 11.16.54.png](https://github.com/Heo-y-y/development-blog/assets/112863029/f64e4032-9129-4fc9-8640-9f963f3cf590)

위 그림에는 총 3개의 컴포넌트가 있다.

가장 위에 있는 컴포넌트는 부모 컴포넌트이고, 아래 화살표로 연결된 2개의 컴포넌트는 자식 컴포넌트이다.

부모 컴포넌트는 값을 가지고 있고, 왼쪽 아래 컴포넌트 A는 2를 곱해서 표시하는 컴포넌트이고, 컴포넌트 B는 3을 곱하는 컴포넌트이다.

이러한 경우 자식 컴포넌트들이 각각 값을 가지고 있을 필요가 없다. 그냥 부모 컴포넌트의 state에 있는 값에 각각 2와 3을 곱해서 표시하기만 하면 된다.

좀더 구체적인 예로 살펴보자.

![스크린샷 2023-12-11 오후 11.22.39.png](https://github.com/Heo-y-y/development-blog/assets/112863029/c614ebfa-7898-468e-9919-0bd1f2d3f06d)

마찬가지로 3개의 컴포넌트가 있고, 부모 컴포넌트는 degree라는 이름의 섭씨 온도값을 가지고 있으며, 왼쪽 아래에 있는 컴포넌트 C는 온도를 섭씨로 표현하는 컴포넌트이고, 오른쪽 아래에 있는 컴포넌트 F는 온도를 화씨로 표현하는 컴포넌트이다.

이 경우에도 자식 컴포넌트들이 각각 온도값을 가지고 있을 필요 없이 그냥 부모 컴포넌트의 state에 있는 섭씨 온도값을 변환해서 표시해주면 된다.

정리하면, **하위 컴포넌트가 공통된 부모 state를 공유하여 사용하는 것**을 **Shared State**라고 한다.

## 하위 컴포넌트에서 State 공유하기

간단한 예제로 살펴보자.

먼저 섭씨 온도값을 `props`로 받아서 물이 끓는지 안 끓는지를 문자열로 출력해주는 컴포넌트를 만들어보자.

```jsx
function BoilingVerdict(props) {
    if (props.celsius >= 100) {
        return <p>물이 끓습니다.</p>;
    }
    return <p>물이 끓지 않습니다.</p>;
}
```

위 코드는 섭씨 온도 값을 `props`로 받아서 100℃ 이상이면 물이 끓는다는 문자열을 출력하고, 그 외에는 물이 끓지 않는다는 문자열을 출력한다.

그럼 이 컴포넌트를 실제로 사용하는 부모 컴포넌트를 만들어보자.

```jsx
function Calculator(props) {
    const [temperature, setTemperature] = useState('');

    const handleChange = (event) => {
        setTemperature(event.target.value);
    }

    return (
        <fieldset>
            <legend>섭씨 온도를 입력하세요:</legend>
            <input 
                value={temperature} 
                onChange={handleChange} />
            <BoilingVerdict
                celsius={parseFloat(temperature)} />
        </fieldset>
    )
}
```

`Calculator` 컴포넌트는 state로 온도값을 하나 갖고 있다. 또한 사용자로부터 입력을 받기 위해서 `input` 태그를 사용하여 Controlled Component 형태로 구현되어 있다. 사용자가 온도값을 변경할 때마다 `handleChange` 함수가 호출되고, `setTemperature` 함수를 통해 온도값을 갖고 있는 `temperature`라는 이름의 state를 업데이트한다.

그리고 state에 있는 온도값은 위에서 만든 `BoilingVerdict` 컴포넌트에 `celsius`라는 `props`로 전달된다.

### 입력 컴포넌트 추출하기

다음으로는 `Calculator` 컴포넌트 안에 온도를 입력하는 부분을 별도의 컴포넌트로 추출해보자. 이렇게 하는 이유는 섭씨 온도와 화씨 온도를 각각 따로 입력 받을 수 있도록 하여 재사용이 가능한 형태로 컴포넌트를 만들어 사용하는 것이 효율적이기 때문이다.

```jsx
const scaleNames = {
    c: '섭씨',
    f: '화씨',
};

function TemperatureInput(props) {
    const [temperature, setTemperature] = useState('');

    const handleChange = (event) => {
        setTemperature(event.target.value);
    }

    return (
        <fieldset>
            <legend>
                온도를 입력해 주세요(단위: {scaleNames[props.scale]}):
            </legend>
            <input value={temperature} onChange={handleChange} />
        </fieldset>
    )
}
```

위 코드는 온도를 입력 받기 위한 `TemperatureInput` 컴포넌트이다. `Calculator` 컴포넌트에서 온도를 입력받는 부분을 추출하여 별도의 컴포넌트로 만든 것이다. 추가적으로 `props`에 단위를 나타내는 `scale`을 추가하여 온도의 단위를 섭씨, 또는 화씨로 입력 가능하도록 만들었다.

이렇게 추출한 컴포넌트를 사용하도록 `Calculator` 컴포넌트를 변경하면 아래와 같이 바뀐다.

```jsx
function Calculator(props) {
    return (
        <div>
            <TemperatureInput scale="c" />
            <TemperatureInput scale="f" />
        </div>
    );
}
```

총 2개의 입력을 받을 수 있도록 되어 있으며, 하나는 섭씨 온도를 입력 받고, 다른 하나는 화씨 온도를 입력 받는다.

그런데 여기서 한가지 문제가 있다. 사용자가 입력하는 온도 값이 `temperature` `input`의 state에 저장되기 때문에 섭씨 온도와 화씨 온도 값을 따로 입력 받으면 2개의 값이 다를 수 있다. 이를 해결하기 위해서는 값을 동기화 시켜줘야 한다.

### 온도 변환 함수 작성하기

섭씨 온도와 화씨 온도 값을 동시화 시키기 위해서 먼저 각각에 대해 변환하는 함수를 작성해야 한다.

```jsx
function toCelsius(fahrenheit) {
    return (fahrenheit - 32) * 5 / 9;
}

function toFahrenheit(celsius) {
    return (celsius * 9 / 5) + 32;
}
```

위 함수는 섭씨 온도를 화씨 온도로 변환하는 함수와 화씨 온도를 섭시 온도로 변환하는 함수이다.

이 함수를 호출하는 함수를 만들어보자.

```jsx
function tryConvert(temperature, convert) {
    const input = parseFloat(temperature);
    if (Number.isNaN(input)) {
        return '';
    }
    const output = convert(input);
    const rounded = Math.round(output * 1000) / 1000;
    return rounded.toString();
}
```

`tryConvert` 함수는 온도 값과 변환하는 함수를 파라미터로 받아서 값을 변환시켜 리턴해주는 함수이다.

`tryConvert` 함수를 실제로 사용하는 방법은 아래와 같다.

```jsx
tryConvert('abc', toCelsius); // empty string을 리턴
tryConvert('10.22', toFahrenheit); // '50.396'을 리턴
```

온도 값과 변환하는 함수를 파라미터로 넣어주면 된다.

### Shared State 적용하기

이제 하위 컴포넌트의 state를 공통된 부모 컴포넌트로 끌어올려서 Shared State를 적용해보자.

여기서 state를 상위 컴포넌트로 올린다는 것을 **Lifting State Up**이라고 한다. 영단어 Lifting은 들어 올린다라는 뜻을 가지고 있는데, 말 그대로 state를 위로 끌어올린다는 의미이다. 즉, **하위 컴포넌트의 state를 공통 상위 컴포넌트로 올리는 것**이다.

먼저 `TemperatureInput` 컴포넌트에서 온도값을 가져오는 부분을 아래와 같이 수정해야 한다.

```jsx
return (
    ...
            // 변경 전: <input value={temperature} onChange={handleChange} />
            <input value={props.temperature} onChange={handleChange} />
    ...
    )
}
```

이렇게 하면 온도값을 컴포넌트 state에서 가져오는 것이 아닌 `props`를 통해 가져오게 된다. 또한 컴포넌트 state를 사용하지 않게 되기 때문에 입력 값이 변경되었을 때 상위 컴포넌트로 변경된 값을 전달해 주어야 한다.

이를 위해 `handleChange` 함수를 아래와 같이 변경한다.

```jsx
const handleChange = (event) => {
        // 변경 전: setTemperature(event.target.value);
        props.onTemperatureChange(event.target.value);
    }
```

이제 사용자가 온도값을 변경할 때마다 `props`에 있는 `onTemperatureChange` 함수를 통해 변경된 온도값이 상위 컴포넌트로 전달된다.

최종적으로 완성된 `TemperatureInput` 컴포넌트의 모습은 아래와 같다.

```jsx
function TemperatureInput(props) {

    const handleChange = (event) => {
        props.onTemperatureChange(event.target.value);
    }

    return (
        <fieldset>
            <legend>
                온도를 입력해 주세요(단위: {scaleNames[props.scale]}):
            </legend>
            <input value={props.temperature} onChange={handleChange} />
        </fieldset>
    )
}
```

state는 제거되었고, 오로지 상위 컴포넌트에서 전달 받은 값만을 사용하고 있다.

### Calculator 컴포넌트 변경하기

마지막으로 변경된 `TemperatureInput` 컴포넌트에 맞춰서 `Calculator` 컴포넌트를 변경해야 한다.

```jsx
function Calculator(props) {
    const [temperature, setTemperature] = useState('');
    const [scale, setScale] = useState('c');

    const handleCelsiusChange = (temperature) => {
        setTemperature(temperature);
        setScale('c');
    }

    const handleFahrenheitChange = (temperature) => {
        setTemperature(temperature);
        setScale('f');
    }

    const celsius = scale === 'f' ? tryConvert(temperature, toCelsius) : temperature;
    const fahrenheit = scale === 'c' ? tryConvert(temperature, toFahrenheit) : temperature;

    return (
        <div>
            <TemperatureInput
                scale="c"
                temperature={celsius}
                onTemperatureChange={handleCelsiusChange} />
            <TemperatureInput
                scale="f"
                temperature={fahrenheit}
                onTemperatureChange={handleFahrenheitChange} />
            <BoilingVerdict
                celsius={parseFloat(celsius)} />
        </div>
    );
}
```

state로 `temperature`와 `scale`을 선언하여 온도값과 단위를 각각 저장하도록 했다. 이 온도와 단위를 이용하여 변환함수를 통해 섭시 온도와 화씨 온도를 구해서 사용한다.

`TemperatureInput` 컴포넌트를 사용하는 부분에서는 각 단위로 변환된 온도값과 단위를 `props`로 넣어주고, 값이 변경되었을 때 업데이트하기 위한 함수를 `onTemperatureChange`에 넣어주었다. 따라서 섭씨 온도가 변경되면 단위가 `c`로 변경되고, 화씨 온도가 변경되면 단위가 `f`로 변경된다.

그림으로 나타내면 아래와 같다.

![스크린샷 2023-12-12 오전 12.16.43.png](https://github.com/Heo-y-y/development-blog/assets/112863029/395a26cf-6760-43b2-8cac-5be19d4f7510)

상위 컴포넌트인 `Calculator`에서 온도값과 단위를 각각의 state로 가지고 있으며, 두 개의 하위 컴포넌트는 각각 섭씨와 화씨로 변환된 온도값과 단위 그리고 온도를 업데이트하기 위한 함수를 `props`로 가지고 있다.

이처럼 각 컴포넌트가 state의 값을 가지고 있는 것이 아니라 공통된 상위 컴포넌트로 올려서 공유하는 방법을 사용하면 리액트에서 더욱 간결하고 효율적으로 개발할 수 있다.

코드를 실행하면, 아래와 같이 결과가 나오는 것을 확인할 수 있다.

![스크린샷 2023-12-12 오전 12.28.31.png](https://github.com/Heo-y-y/development-blog/assets/112863029/2e3be944-a265-43fd-91f3-f540abe0fab6)

![스크린샷 2023-12-12 오전 12.28.42.png](https://github.com/Heo-y-y/development-blog/assets/112863029/3b245f9e-166d-4abd-b8d3-3a11dd59585d)

**참고 자료**

- [https://www.inflearn.com/course/lecture?courseSlug=처음-만난-리액트&unitId=113237&category=questionDetail](https://www.inflearn.com/course/lecture?courseSlug=%EC%B2%98%EC%9D%8C-%EB%A7%8C%EB%82%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8&unitId=113237&category=questionDetail)
