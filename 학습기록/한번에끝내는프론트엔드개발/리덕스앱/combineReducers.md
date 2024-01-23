# combineReducers

## ToDo 기능 추가

현재까지 만든 Counter 앱에 ToDo 앱을 추가해보자.

## root reducer와 sub reducer

현재까지 counter 리듀서만 있는데, 하나를 더 추가해주려면 Root 리듀서를 만들어서 그 아래 counter와 todos라는 서브(sub) 리듀서를 넣어주면 된다. Root 리듀서를 만들 때 사용하는 것이 combineReducers이다.

![스크린샷 2024-01-23 오후 10 50 39](https://github.com/Heo-y-y/development-blog/assets/112863029/08ad5bc9-c109-4e57-b04c-9d45e2dc4b9e)

**index.tsx**

```tsx
import { combineReducers } from "redux";
import counter from "./counter";
import todos from "./todos";

const rootReducer =combineReducers({
  counter,
  todos
})

export default rootReducer;
```

**todos.tsx**

```tsx
enum ActionType {
  ADD_TODO = "ADD_TODO",
  DELETE_TODO = "DELETE_TODO"
}

interface Action {
  type: ActionType;
  text: string
}

const todos = (state = [], action: Action) => {
  switch (action.type) {
    case 'ADD_TODO':
      return [...state, action.text]
    default:
      return state;
  }
}

export default todos;
```

**counter.tsx**

```tsx
interface Action {
  type: string;
}

const counter = (state = 0, action: Action) => {
  switch (action.type) {
    case 'INCREMENT':
        return state + 1;
    case 'DECREMENT':
        return state - 1;
    default:
        return state;
  }
}

export default counter;
```

### createStore에 루트 리듀서로 대체

```jsx
import rootReducer from './reducers';

const store = createStore(rootReducer);
```

```tsx
const store = createStore(rootReducer);

store.dispatch({
  type: "ADD_TODO",
  text: "USE REDUX"
})
console.log('sotre.getState', store.getState());
```

![스크린샷 2024-01-23 오후 11.01.31.png](https://github.com/Heo-y-y/development-blog/assets/112863029/e37c3b27-3eb3-4055-8889-56764b43be27)

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
