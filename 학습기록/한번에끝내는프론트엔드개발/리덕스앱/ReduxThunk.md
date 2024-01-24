# Redux Thunk

## 리덕스 Thunk란?

리덕스를 사용하는 앱에서 비동기 작업을 할 때 많이 사용하는 방법이 redux-thunk 이다.

이것도 앞서 만들어본 logger 미들웨어처럼 리덕스 미들웨어이며, 리덕스를 개발한 Dan Abramov가 만들었다.

## Thunk 용어는?

“thunk”라는 단어는 일부 지연된 작업을 수행하는 코드 조각” 을 의미하는 프로그래밍 용어이다. 지금 일부 논리(logic)를 실행하는 대신 나중에 작업을 수행하는 데 사용할 수 있는 함수 본문이나 코드를 작성할 수 있다.

```jsx
// calculation of 1 + 2 is immediate
// x === 3
let x = 1 + 2

// caculation of 1 + 2 is deplayed
// foo can be called later to perform the calculation
// foo is a thunk!
let foo = () => 1 + 2
```

## 비동기 작업을 해야 할 때는?

여러 경우가 있지만 서버에 요청을 보내서 데이터를 가져올 때 주로 비동기 요청을 보낸다.

비동기로 https://josnplaceholder.typicode.com 에 요청을 보내면 Dummy 데이터를 받아올 수 있다. 이 더미 데이터로 포스트를 만들어보자.

### Axios 모듈 설치

Api request를 위한 모듈이다.

```jsx
npm install axios --save
```

### posts 리듀서 생성

**posts**

```tsx
enum ActionType {
  FETCH_POSTS = "FETCH_POSTS",
  DELETE_POSTS = "DELETE_POSTS"
}

interface Post {
  userId: number;
  id: number;
  title: string;
}

interface Action {
  type: ActionType;
  payload: Post[]
}

const posts = (state = [], action: Action) => {
  switch (action.type) {
    case 'FETCH_POSTS':
      return [...state, ...action.payload]
    default:
      return state;
  }
}

export default posts;
```

**index**

```jsx
import { combineReducers } from "redux";
import counter from "./counter";
import todos from "./todos";
import posts from "./posts";

const rootReducer =combineReducers({
  counter,
  todos,
  posts
})

export default rootReducer;

export type RootState = ReturnType<typeof rootReducer>;
```

### posts 데이터를 위한 요청 보내기

```tsx
useEffect(() => {
   dispatch(fetchPosts()) 
  }, [dispatch])

  const fetchPosts = (): any => {
    return async function fetchPostsThunk(dispatch:any, getState: any) {
      const response = await axios.get("https://josnplaceholder.typicode.com/posts");
      dispatch({type: "FETCH_POSTS", payload: response.data})
    }
  }
```

**에러 발생**

![스크린샷 2024-01-24 오후 12.08.23.png](https://github.com/Heo-y-y/development-blog/assets/112863029/714eaa29-6086-4a7b-a181-af613bd29c29)

### 에러가 나는 이유는?

원래 Action은 객체여야 하는데, 현재는 함수를 dispatch하고 있다. 그러기 때문에 나는 에러이다.

그러면 함수를 dispatch 할 수 있게 해주는 Redux-Thunk 미들웨어를 설치해서 사용해보자.

```jsx
npm install redux-thunk --save
```

**redux thunk 적용**

```jsx
const middleware = applyMiddleware(thunk, loggerMiddleware);
```

### posts 데이터 화면에 표출

```tsx
const todos: string[] = useSelector((state: RootState) => state.todos);
const counter = useSelector((state: RootState) => state.counter);
const posts: Post[] = useSelector((state: RootState) => state.posts);
```

```tsx
interface Post {
  userId: number;
  id: number;
  title: string;
}
```

```tsx
<ul>
  {posts.map((post, index) => <li key={index}>{post.title}</li>)}
</ul>
```

![스크린샷2 2024-01-24 오후 12 23 04](https://github.com/Heo-y-y/development-blog/assets/112863029/51fd3479-fff6-461c-9a8a-4c522210a59b)

### actions 들은 actions 폴더로 따로 분리

![스크린샷3 2024-01-24 오후 12 27 05](https://github.com/Heo-y-y/development-blog/assets/112863029/1d48b1ce-d228-43f7-a872-4435c6319d0c)

**posts**

```tsx
import axios from 'axios';

export const fetchPosts = (): any => {
  return async function fetchPostsThunk(dispatch:any, getState: any) {
    const response = await axios.get("https://josnplaceholder.typicode.com/posts");
    dispatch({type: "FETCH_POSTS", payload: response.data})
  }
}
```

### modern ES2015 형태로 변경

```tsx
export const fetchPosts = (): any => async (dispatch: any, getState: any) => {
  const response = await axios.get("https://josnplaceholder.typicode.com/posts");
  dispatch({type: "FETCH_POSTS", payload: response.data})
}
```

### 결론

이렇게 리덕스 던크를 사용함으로써 액션 생성자가 그저 하나의 액션 객체를 생성할 뿐 아니라 그 내부 안에서 여러 가지 작업도 할 수 있게 만들어준다.

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
