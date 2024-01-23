# Redux Provider

## Provider란?

<Provider> 구성 요소는 Redux Store 저장소에 액세스해야 하는 모든 중첩 구성 요소에서 Redux Store 저장소를 사용할 수 있도록 한다.

React Redux 앱의 모든 React 구성 요소는 저장소에 연결할 수 있으므로 대부분의 응용 프로그램은 전체 앱의 구성 요소 트리가 내부에 있는 최상위 수준에서 <Provider>를 렌더링한다.

그런 다음 Hooks 및 연결 API는 React의 컨텍스트 메거니즘을 통해 제공된 저장소 인스턴스에 액세스할 수 있다.

### Provider 설치

```jsx
npm install react-redux --save
```

### Provider를 렌더링

```tsx
import { Provider } from 'react-redux';

const render = () => root.render(
  <React.StrictMode>
    <Provider store={store}>
      <App 
        value={store.getState()}
        onIncrement={() => store.dispatch({type: "INCREMENT"})}
        onDecrement={() => store.dispatch({type: "DECREMENT"})}
      />
    </Provider>
  </React.StrictMode>
);
render();
```

### Todo UI 생성

```tsx
<form onSubmit={addTodo}>
      <input type='text' value={todoValue} onChange={handleChange} />
      <input type='submit' />
</form>
```

```jsx
const [todoValue, setTodoValue] = useState("");
```

```tsx
const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setTodoValue(e.target.value);
  }
  const addTodo = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    setTodoValue("");
  }
```

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
