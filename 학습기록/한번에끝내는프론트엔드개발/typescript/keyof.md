# Keyof operator

keyof 연산자는 제공된 타입의 키를 추출하여 새로운 Union 유형으로 변환한다.

```tsx
interface IUser {
  name: string;
  age: number;
  address: string;
}

// 'name' | 'age' | 'address'
type UserKeys = keyof IUser;
```

```tsx
const user = {
  name: "Heo",
  age: 20,
  address: 'seoul'
}

type UserKeys = keyof typeof user

enum UserRole {
  admin,
  manager
}

type UserRoleKeys = keyof typeof UserRole
```

즉, 타입에서 키를 추출해야 할 때 사용한다고 생각하면 된다.

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
