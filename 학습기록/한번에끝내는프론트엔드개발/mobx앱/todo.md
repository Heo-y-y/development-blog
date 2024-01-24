# MobX를 이용한 Todo 앱 만들기

![스크린샷1 2024-01-24 오후 11 23 54](https://github.com/Heo-y-y/development-blog/assets/112863029/9751c9cf-89a8-4e5c-801c-99ab8d8e44e6)

### 리액트 앱 설치

```jsx
npx create-react-app ./ --template typescript
```

### Store 생성하기

![스크린샷2 2024-01-24 오후 11 36 14](https://github.com/Heo-y-y/development-blog/assets/112863029/e658cd87-e4d8-49be-8f66-4af84d364aa0)

### makeObservervable

```jsx
export default class TodoStore {
    todos: TodoItem[] = []

    constructor() {
      makeObservable(this, {
        todos: observable,
        addTodo: action,
        toggleTodo: action,
        status: computed
      })
    }
}
```

```tsx
interface TodoItem {
  id: number;
  title: string;
  completed: boolean;
}
```

### **addTodo**

```tsx
addTodo(title: string) {
      const item: TodoItem = {
        id: getId(),
        title,
        completed: false
      }
      this.todos.push(item);
    }
}
```

### **toggleTodo**

```tsx
toggleTodo(id: number) {
      const index = this.todos.findIndex((item) => item.id === id)
      if (index > -1) {
        this.todos[index].completed = !this.todos[index].completed
      }
    }
```

### status

```tsx
get status() {
      let completed = 0, remaining = 0;
      this.todos.forEach((todo) => {
        if(todo.completed) {
          completed++;
        } else {
          remaining++;
        }
      })
      return {completed, remaining}
    }
}
```

## Todo 앱 UI 생성하기

### UI

![스크린샷 2024-01-24 오후 11 46 53](https://github.com/Heo-y-y/development-blog/assets/112863029/0aadd8c8-917b-4b11-915d-845dca436dc9)

```tsx
function App() {
  const todoStore = new TodoStore();
  return (
    <div className="App">
      <TodoList todoStore={todoStore}/>
    </div>
  );
}
```

```tsx
const TodoList:React.FC<TodoListProps> = observer(({todoStore}) => {
  const [value, setValue] = useState<string>('');
  return (
    <div>
      <input 
        value={value}
        onChange={event => {
          setValue(event.target.value)
        }}
        type='text'
      />
      <button
        onClick={() => {
          if(value) {
            todoStore.addTodo(value)
          }
          setValue('');
        }}
      >
        Submit
      </button>
    </div>
  )
})
```

### props 타입 정의

```tsx
interface TodoListProps {
  todoStore: TodoStore
}
```

### TodoList 나열하기

```tsx
<ul>
  {todoStore.todos.map(todo => {
    return (
      <li
        key={todo.id}
      >
       {todo.title} [{todo.completed ? 'x': ' '}]
      </li>
     )
   })}
</ul>
```

### Status 보여주기

```tsx
<div>Completed: {todoStore.status.completed}</div>
<div>Remaining: {todoStore.status.remaining}</div>
```

### toggle 기능 추가

```tsx
<li
  key={todo.id}
  onClick={() => {
    todoStore.toggleTodo(todo.id)
  }}
>
  {todo.title} [{todo.completed ? 'x': ' '}]
</li>
```

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
