# **JavaScript 데이터**

## 구조 분해 할당

구조 분해 할당이라는 개념은 다른 말로 비구조화 할당이라고도 부른다. 구조 분해 할당이라는 것은 아래 코드처럼 `user`라는 객체 데이터에서 내용을 구조 분해해서 내가 원하는 속성들만 꺼내서 사용을 할 수 있는 개념이다.

```jsx
const user = {
  name: 'Heo',
  age: 30,
  email: 'heo123@gmail.com'
}
const { name, age, email, address } = user
// E.g user.address

console.log(`사용자의 이름은 ${name}입니다.`)
console.log(`${name}의 나이는 ${age}세입니다.`)
console.log(`${name}의 이메일 주소는 ${email}입니다.`)
console.log(address)
```

즉, 이러한 방식으로 객체 데이터를 구조 분해해서 각각의 변수로 만들어서 활용을 할 수 있는 것이다.

하지만 위 `address`라는 것은 `user`라는 객체 데이터 내부에 존재하지 않다. 그래서 정의되어 있지 않은 속성들은 꺼내온다고 해도 값이 당연하게 없다.

![스크린샷 2024-01-08 오후 5.23.55.png](https://github.com/Heo-y-y/development-blog/assets/112863029/b3c82fcc-0bf7-46ef-afdd-28ddabea8a76)

`user`라는 객체에서 구조 분해를 통해서 꺼내오는 방식은 결국 `user` 부분의 점표기법을 이용해서 속성의 이름을 명시하는 것과 동일하다.

객체 데이터에서 뒤쪽에 대괄호를 적어서 인덱싱 방법으로도 해당하는 속성의 값을 꺼내 사용할 수 있다.

정리하면, 객체 데이터와 배열 데이터는 구조 분해 할당이라는 문법을 통해서 그 데이터를 구조 분해하여 내용을 속성의 이름이라던가 순서대로 뽑아내서 활용할 수 있다.

## 전개 연산자

`…` 세개를 사용하는 것이 전개 연산자이다. 이 전개 연산자는 말 그대로 하나의 배열 데이터를 쉼표로 구분하는 각각의 아이템으로 전개해서 출력을 한다.

```jsx
const fruits = ['Apple', 'Banana', 'Cherry']
console.log(fruits)
console.log(...fruits) // console.log('Apple', 'Banana', 'Cherry')

function toObject(a, b, c) {
  return {
    a: a,
    b: b,
    c: c
  }
}
console.log(toObject(...fruits))
```

![스크린샷 2024-01-08 오후 5.39.41.png](https://github.com/Heo-y-y/development-blog/assets/112863029/6ab3740b-8595-47a1-a049-c76d91751952)

매개변수 안에도 전개 변수를 사용할 수 있는데, 아래처럼 사용하면 `c`부분이 배열 데이터로 들어간 것을 볼 수 있다.

```jsx
function toObject(a, b, ...c) {
  return {
    a: a,
    b: b,
    c: c
  }
}
console.log(toObject(...fruits))
```

![스크린샷 2024-01-08 오후 5.42.50.png](https://github.com/Heo-y-y/development-blog/assets/112863029/622fef32-2851-4248-9040-f6f3ea2a4929)

## 불변성

- **원시 데이터**: String, Number, Boolean, undefined, null
    
    ```jsx
    let a = 1
    let b = 4
    console.log(a, b, a === b)
    b = a
    console.log(a, b, a === b)
    a = 7
    console.log(a, b, a === b)
    let c = 1
    console.log(b, c, b === c)
    ```
    
    ![스크린샷 2024-01-08 오후 5.50.09.png](https://github.com/Heo-y-y/development-blog/assets/112863029/6bc56e43-156c-4303-b817-81ef37ad93c4)
    
- **참조형 데이터**: Object, Array, Function
    
    참조형 데이터는 원시형 데이터와 다르게 새로운 값을 만들 때마다 그것이 새로운 메모리 주소에 할당되는 구조를 가지고 있다. 그래서 참조형 데이터는 결국 불변성이 없다. 이것을 다른 말로 가변한다고도 얘기한다.
    
    ```jsx
    let a = { k: 1 }
    let b = { k: 1}
    console.log(a, b, a === b)
    a.k = 7
    b = a
    console.log(a, b, a === b)
    a.k = 2
    console.log(a, b, a === b)
    let c = b
    console.log(a, b, c, a === c)
    a.k = 9
    console.log(a, b, c, a === c)
    ```
    
    ![스크린샷 2024-01-08 오후 5.58.59.png](https://github.com/Heo-y-y/development-blog/assets/112863029/571e6cf0-b6fd-4724-8d87-b633a0f7bf22)
    
    즉, 같은 메모리를 바라보고 있는 여러 개의 변수들이 있을 때 한쪽에 변수에 있는 값을 수정을 하게 되면 다른 값에서 확인을 할 때 의도하지 않게 이미 값이 바뀌어져 있는 상태가 될 수 있다.
    
    결국 참조형 데이터는 할당 연산자를 사용할 때 복사가 돼서 새로운 객체 데이터가 만들어진다는 개념이 아니고, 메모리의 참조 주소만 옮겨 간다는 것이다.
    

## 얕은 복사와 깊은 복사

같은 메모리를 바라보고 있기 때문에 하나의 데이터만 수정해도 같이 바뀌게 된다.

```jsx
const user = {
  name: 'Heo',
  age: 30,
  emails: ['heo123@gmail.com']
}
const copyUser = user
console.log(copyUser === user)

user.age = 22
console.log('user', user)
console.log('copyUser', copyUser)
```

![스크린샷 2024-01-08 오후 6.06.38.png](https://github.com/Heo-y-y/development-blog/assets/112863029/f9914190-b6f5-48bc-b096-dcdccb5e8fb7)

위 코드를 아래 코드로 수정하면 복사를 하여 다른 메모리에 저장하기 때문에 데이터 값이 변하지 않는다. 이것이 바로 복사라는 개념이다.

- `Object.assign`**사용**

```jsx
const copyUser = Object.assign({}, user)
```

- **전개 연산자 사용**

```jsx
const copyUser = {...user}
```

![스크린샷 2024-01-08 오후 6.08.37.png](https://github.com/Heo-y-y/development-blog/assets/112863029/940af6b2-e4c0-4e82-9e80-6b043534f1d8)

현재 얕은 복사를 사용하면 아래처럼 이메일만 같게 적용된 것을 확인할 수 있다.

```jsx
user.emails.push('geo@hanmail.net')
console.log(user.emails === copyUser.emails)
console.log('user', user)
console.log('copyUser', copyUser)
```

![스크린샷 2024-01-08 오후 6.14.13.png](https://github.com/Heo-y-y/development-blog/assets/112863029/158f5a2d-d595-402e-89fb-878b74853293)

그러면 깊은 복사를 적용해보자.

깊은 복사 같은 경우에는 자바스크립트로 직접접으로 구현하기가 쉽지 않다.

우선 테스트를 위해 **lodash**를 사용하여 진행해보자.

```jsx
import _ from 'lodash'

const copyUser = _.cloneDeep(user)
```

![스크린샷 2024-01-08 오후 6.19.14.png](https://github.com/Heo-y-y/development-blog/assets/112863029/82811df7-e38b-4f18-8bae-e321b809f7cd)

이렇게 진행하면 `copyUser`에 이메일을 `push`한 영향이 없는 것을 확인할 수 있다.

어떤 특정한 객체 데이터 혹은 배열 데이터처럼 참조형 데이터를 복사할 때 얕은 복사로도 충분히 문제없이 복사가 된다고 판단이 되면, `Object.assign` 이나 전개 연산자를 통해서 해당하는 참조형 데이터를 복사하면 된다.

만약에 참조형 데이터가 그 내부에 또 다른 참조형 데이터를 위 예제처럼 들고 있으면 깊은 복사로 진행하는 것이 좋다. 이때는 **lodash**의 `cloneDeep`이라는 기능을 활용하자.

## Lodash 사용법

### uniqBy, unionBy

`uniqBy` 와 `unionBy`를 통해 중복을 제거할 수 있다.

`uniqBy` 는 하나의 배열 데이터에서 어떤 특정한 속성의 이름으로 고유화를 시켜주는 메서드이다.

`unionBy`는 합치기 전에 여러 개의 배열 데이터를 먼저 적어두고, 맨 마지막에 그 배열 데이터를 합칠 때 고유화 작업을 시킬 속성 이름을 명시해주면 고유화가 된 새로운 배열을 반환한다.

```jsx
.uniqBy(배열데이터, 중복을 구분할 고유한 속성의 이름)
.uniqBy(배열 데이터, 배열 데이터, 중복을 구분할 고유한 속성의 이름)
```

**테스트 코드**

```jsx
import _ from 'lodash'

const usersA = [
  { userId: '1', name: 'Heo' },
  { userId: '2', name: 'Neo' }
]
const usersB = [
  { userId: '1', name: 'Heo' },
  {userId: '3', name: 'Amy' }
]
const usersC = usersA.concat(usersB)
console.log('concat', usersC)
console.log('uniqBy', _.unionBy(usersC, 'userId'))

const usersD = _.unionBy(usersA, usersB, 'userId')
console.log('unionBy', usersD)
```

![스크린샷 2024-01-08 오후 9.18.32.png](https://github.com/Heo-y-y/development-blog/assets/112863029/95c96ba0-0cdd-4130-98fb-a539857e413a)

### find, findIndex, remove

배열 데이터 안에서 특정한 객체를 찾을 때 `find`를 사용하면 된다.

`findIndex`는 해당하는 객체의 `index` 번호를 반환한다.

`remove`는 해당하는 배열의 데이터를 찾아서 삭제한다.

```jsx
import _ from 'lodash'

const users = [
  {userId: '1', name: 'Heo'},
  {userId: '2', name: 'Kim'},
  {userId: '3', name: 'Amy'},
  {userId: '4', name: 'Beo'},
  {userId: '5', name: 'Neo'}
]

const foundUser = _.find(users, { name: 'Amy' })
const foundUserIndex = _.findIndex(users, { name: 'Amy' })
console.log(foundUser)
console.log(foundUserIndex)

_.remove(users, { name: 'Heo' })
console.log(users)
```

![스크린샷 2024-01-08 오후 9.30.30.png](https://github.com/Heo-y-y/development-blog/assets/112863029/63c4ff85-075a-4374-ba23-6b57b855e346)

## OMDb API

[OMDb API](https://www.omdbapi.com/)를 이용해서 영화 데이터를 요청하고 그것을 받아서 프로젝트에 출력해보자.

### Query String

![스크린샷 2024-01-08 오후 10.13.28.png](https://github.com/Heo-y-y/development-blog/assets/112863029/85c8d5d7-12b5-4275-8779-e3cc64a7f83d)

어떤 문자를 가지고서 뭔가를 검색한다는 의미라고 생각하면 된다. 즉, Query String은 특정한 주소로 접근을 할 때 기본적인 그 페이지에 대한 옵션을 명시하는 용도로 활용되는 어떠한 문자이다.

### 영화 데이터 적용

```jsx
import axios from 'axios'

function fetchMovies() {
  axios
    .get('https://www.omdbapi.com/?apikey=API-KEY&s=frozen')
    .then(res => {
      console.log(res)
      const h1El = document.querySelector('h1')
      const imgEl = document.querySelector('img')
      h1El.textContent = res.data.Search[0].Title
      imgEl.src = res.data.Search[0].Poster
    })
}
fetchMovies()
```

[axios](https://github.com/axios/axios)라는 패키지의 도움을 받아서 특정한 주소로 사용자를 인증하고, 검색하고 싶은 내용을 정리를 해서 데이터를 요청한다. 그러면 그 요청된 데이터를 서버에서 처리를 하여 기본적인 인증 과정을 거치고 응답을 해주게 되는데, 그 응답을 이 **axios**라는 패키지가 내부적으로 처리를 해서 결과적으로 `then`이라는 메서드의 콜백에서 처리할 수 있도록 지정한 `res`라는 변수에 그 내용을 담아주는 내부 로직이 **axios**에 이미 구현되어져 있는 것이다.

![스크린샷 2024-01-08 오후 10.38.40.png](https://github.com/Heo-y-y/development-blog/assets/112863029/6812a409-b9d1-4650-8872-68048cc8778d)

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
