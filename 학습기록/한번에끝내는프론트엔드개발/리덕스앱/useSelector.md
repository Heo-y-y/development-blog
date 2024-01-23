# useSelector & useDispatch

## Provider로 둘러 쌓인 컴포넌트에서 store 접근

리액트에서 Hooks가 있듯이 리덕스에도 Hooks가 있는데, 그게 바로 useSelector와 useDispatch이다.

이 2개를 이용해서 provider로 둘러싸인 컴포넌트에서 store에 접근이 가능하다.

## useSelector

useSelector Hooks를 이용해서 스토어의 값을 가져올 수 있다.

```jsx
const counter = useSelector((state) => state.counter)
```

![스크린샷 2024-01-23 오후 11 35 53](https://github.com/Heo-y-y/development-blog/assets/112863029/782254b1-2e28-4f4a-9130-f7dbd80454e1)

### 해결법

1. Root Reducer에 RootState 타입을 생성하기
    
    ```tsx
    const rootReducer =combineReducers({
      counter,
      todos
    })
    
    export default rootReducer;
    
    export type RootState = ReturnType<typeof rootReducer>;
    ```
    
2. 생성한 RootState를 State 객체에 제공하기
    
    ```tsx
    const todos: string[] = useSelector((state: RootState) => state.todos);
    const counter = useSelector((state: RootState) => state.counter);
    ```
    
    ```tsx
    return (
        <div className="App">
          Clicked: {counter} times
        <button onClick={onIncrement}>
          +
        </button>
        <button onClick={onDecrement}>
          -
        </button>
    
      <ul>
        {todos.map((todo, index) => <li key={index}>{todo}</li>)}
      </ul>
    
        <form onSubmit={addTodo}>
          <input type='text' value={todoValue} onChange={handleChange} />
          <input type='submit' />
        </form>
        </div>
      );
    }
    ```
    
    ## useDispatch
    
    store에 있는 dispatch 함수에 접근하는 hooks이다.
    
    ```tsx
    const addTodo = (e: React.FormEvent<HTMLFormElement>) => {
        e.preventDefault();
        // store.dispatch({ type:"ADD_TODO", text: todoValue })
        dispatch({ type:"ADD_TODO", text: todoValue })
        setTodoValue("");
      }
    ```
    
    ---
    
    **참고 자료**
    
    - <https://fastcampus.co.kr/dev_online_frontend>
