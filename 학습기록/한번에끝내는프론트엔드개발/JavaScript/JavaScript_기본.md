# **JavaScript 기본**

## 데이터 타입 확인

`typeof`를 이용하면 데이터의 타입을 확인할 수 있다.

```jsx
console.log(typeof 'Hello World!')
console.log(typeof 123)
console.log(typeof true)
console.log(typeof undefined)
console.log(typeof null)
console.log(typeof {})
console.log(typeof [])
```

![스크린샷 2024-01-07 오후 4.37.55.png](https://github.com/Heo-y-y/development-blog/assets/112863029/aa220dee-d821-4af2-b9e0-cd5f5b56e635)

하지만 객체 데이터 배열 데이터는 object로 나오기 때문에 다른 방법을 사용해야 한다.

```jsx
function getType(data) {
  return Object.prototype.toString.call(data).slice(8, -1)
}

console.log(getType(null))
console.log(getType({}))
console.log(getType([]))
```

![스크린샷 2024-01-07 오후 4.41.15.png](https://github.com/Heo-y-y/development-blog/assets/112863029/c9dfe8dd-2c14-4316-b3d2-3ef3f9de444d)

## 산술, 할당 연산자

### 산술 연산자

산술 연산자는 더하기, 빼기, 곱하기, 나누기, 나머지로 총 5가지 연산자가 존재한다.

```jsx
console.log(1 + 2)
console.log(5 - 7)
console.log(3 * 5)
console.log(10 / 2)
console.log(10 % 3)
```

![스크린샷 2024-01-07 오후 4.54.04.png](https://github.com/Heo-y-y/development-blog/assets/112863029/e04b1c4a-6ec2-490a-8959-9e3c375b2906)

### 할당 연산자

할당 연산자는 `const`, `let`을 활용하는데 `const`는 데이터 값을 변경할 수 없고, `let`은 변경이 가능하다.

```jsx
// 데이터 값 변경 불가
const a = 2
console.log(a)

// 데이터 값 변경 가능
let b = 2
b += 2
console.log(b)
```

![스크린샷 2024-01-07 오후 4.56.48.png](https://github.com/Heo-y-y/development-blog/assets/112863029/3543d3cb-e8db-490c-b25b-95a035cc606f)

## 비교, 논리 연산자

### 비교 연산자

비교 연산자 중 `=`를 3번 입력하면 일치 연산자라고 한다. 그리고 불일치 연산자는 다른지 확인하는 연산자이다.

```jsx
// 일치 연산자
const a = 2
const b = 1
console.log(a === b)

function isEqual(x, y) {
  return x === y
}
console.log(isEqual(1, 1))
console.log(isEqual(2, '2'))

// 불일치 연산자
const c = 1
const d = 3

console.log(c !== d)
```

![스크린샷 2024-01-07 오후 5.06.22.png](https://github.com/Heo-y-y/development-blog/assets/112863029/0ad9eee7-b6a6-4113-a0da-f863a75594c3)

### 논리 연산자

`&`를 사용하면 모든 비교값이 `true`여야 한다. 즉, 하나라도 다르면 `false`가 나온다.

```jsx
const a = 1 === 1
const b = 'AB' === 'AB'
const c = true

console.log(a)
console.log(b)
console.log(c)

console.log('&&: ', a && b && c)
```

![스크린샷 2024-01-07 오후 5.11.29.png](https://github.com/Heo-y-y/development-blog/assets/112863029/be53ccf9-97b7-44b4-9fd9-9f6abce8193c)

`|`를 사용하면 비교값 중 하나만 `true`여도 `true`를 반환한다.

```jsx
const a = 1 === 1
const b = 'AB' === 'EC'
const c = true

console.log(a)
console.log(b)
console.log(c)

console.log('||: ', a || b || c)
```

![스크린샷 2024-01-07 오후 5.13.13.png](https://github.com/Heo-y-y/development-blog/assets/112863029/14fd05ee-4b19-4a73-b0b5-3cf6d8e49d02)

`!`를 사용하면 부정 연산자로 특정한 데이터의 반대값이 나타난다.

```jsx
const b = 'AB' === 'EC'

console.log(b)
console.log('!: ', !b)
```

![스크린샷 2024-01-07 오후 5.16.24.png](https://github.com/Heo-y-y/development-blog/assets/112863029/9bec4b43-fbc8-4660-94f7-1954b7778a0c)

## 삼항 연산자

삼항 연산자를 사용하면 아래와 같이  `?`와 `:`를 기준으로 해서 총 3개의 항으로 `?` 앞에 조건을 적는다.

```jsx
console.log(a ? '참' : '거짓')
```

![스크린샷 2024-01-07 오후 5.21.35.png](https://github.com/Heo-y-y/development-blog/assets/112863029/6ae33dcd-7589-44ff-8d03-9ede04408a5c)

## 조건문

`Math`의 `random` 함수를 사용하여 테스트를 진행했다.

### If-Else

```jsx
const a = random()

if (a === 0) {
  console.log('a is 0')
} else if (a === 2) {
  console.log('a is 2')
} else {
  console.log('rest...')
}
```

실행할 조건이 하나면 `if`문만, 두개면 `if`와 `else`만, 그 이상이면 `else if`까지 사용하면 된다.

### Switch

```jsx
const a = random()

switch (a) {
  case 0:
    console.log('a is 0')
    break
  case 2:
    console.log('a is 2')
    break
  case 4:
    console.log('a is 4')
    break
  default: 
    console.log('rest...')
}
```

`case`를 통해 해당 조건을 걸 수 있고, `break`으로 종료시킨다. 그리고 `default`를 통해 `if`문의 `else`와 같은 역할을 할 수 있다.

## 반복문 For

`for` 문은 시작 조건이 종료 조건이 될 동안 변화를 줄 수 있다.

```jsx
// for (시작조건; 종료조건; 변화조건) {}
const ulEl = document.querySelector('ul')

console.log(ulEl);

for (let i = 0; i < 3; i += 1) {
  const li = document.createElement('li')
  li.textContent = `list-${i + 1}`
  ulEl.appendChild(li)
}
```

![스크린샷 2024-01-07 오후 5.45.35.png](https://github.com/Heo-y-y/development-blog/assets/112863029/85a9afb1-a6b7-4c19-b3f6-2d4b8072b2ec)

## 변수 유효범위

`let`이나 `const` 키워드를 사용하게 되면 그 변수가 선언되어져 있는 특정한 블럭 내부가 하나의 유효범위가 된다.

하지만 `var` 키워드는 함수 레벨의 유효범위의 유효범위를 가진다. 즉, 특정한 블럭 내부가 아니여도 값이 적용된다. 그래서 개발자가 의도하지 않은 범위에서 변수가 사용될 수 있고, 메모리 누수로 이어질 수 있어서 잘 사용하지는 않는다.

## 형 변환

데이터의 타입의 변환이 일어나는 것이다.

### Truthy(참 같은 값)

```jsx
true, {}, [], 1, 2, ‘false’, -12, ‘3.14’ …
```

### Falsy(거짓 같은 값)

```jsx
false, ‘’, null, undefined, 0, -0, NaN
```

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
