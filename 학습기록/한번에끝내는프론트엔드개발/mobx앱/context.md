# React Context를 이용한 Observable 공유하기

React Context는 전체 하위 트리와 observable을 공유하는 훌륭한 메커니즘이다.

현재 상태에서 mobx의 observable 값을 여러 컴포넌트에 주려면 아래와 같이 해야한다.

![스크린샷1 2024-01-24 오후 11 04 53](https://github.com/Heo-y-y/development-blog/assets/112863029/406e182a-b096-4472-a09e-a98e21d58d24)

React Context를 이용하면 Provider로 감싼 전체 하위 트리의 컴포넌트에 observable을 공유할 수 있다.

![스크린샷2 2024-01-24 오후 11 08 32](https://github.com/Heo-y-y/development-blog/assets/112863029/64535bcd-2fe5-448d-9067-52cc6049d050)

## Context 적용하기

![스크린샷 2024-01-24 오후 11 11 38](https://github.com/Heo-y-y/development-blog/assets/112863029/03f195ab-2209-4530-ad82-f1e9652d5be7)

**counterContext.js**

```jsx
import { createContext, useContext } from "react";

// Context 생성
export const CounterContext = createContext();
// Provider 생성
export const CounterProvider = CounterContext.Provider;
// Store에 있는 value를 사용하기 위한 Hooks
export const useCounterStore = () => useContext(CounterContext);
```

**index.js**

```jsx
// 인터페이스 생성
const store = new counterStore();

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <CounterProvider value={store} >
      <App />
    </CounterProvider>
  </React.StrictMode>
);
```

**App.js**

```jsx
import './App.css';
import { observer } from 'mobx-react';
import { useCounterStore } from './context/counterContext';

function App() {

  const myCounter = useCounterStore();
  
  console.log('myCounter', myCounter);
  return (
    <div style={{ textAlign: 'center', padding: 16 }}>
      카운트: {myCounter.count}
      <br />
      <br />
    마이너스?:{myCounter.isNegative}
      <br />
      <br />
      <button onClick={() => myCounter.increase()}>+</button>
      <button onClick={() => myCounter.decrease()}>-</button>
    </div>
  );
}

export default observer(App);
```

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
