# **JavaScript**

## 표기법

### dash-case(kebab-case)

- `-`  기호를 사용해서 구분시키는 표기법이다.
- HTML, CSS에서 주로 사용한다.

### snake_case

- `_` 언더바 기호를 사용해서 구분시키는 표기법이다.
- HTML, CSS에서 주로 사용한다.

### camelCase

- 첫번째 단어는 소문자로 시작하고, 그 다음 단어가 시작할 때 대문자로 구분시키는 표기법이다.
- 낙타 표기법이라고도 한다.
- JS에서 주로 사용한다.

### ParcelCase

- 첫번째 단어도 대문자로 시작하고, 그 다음 단어도 대문자로 구분시키는 표기법이다.
- JS에서 주로 사용한다.

### Zero-based Numbering

- 숫자 0을 기반으로 하는 번호를 매기는 방법이다.
- 특수한 경우를 제외하고, 0부터 숫자를 시작한다.

## 주석

**주석 방법**

```jsx
// 한 줄 메모
/* 한줄 메모 */

/**
 * 여러 줄
 * 메모1
 * 메모2
 */
```

**단축키**

`cmd + /`

## 데이터 종류(자료형)

### String(문자 데이터)

**사용 방법**

```jsx
// “”, ‘’, ``를 사용한다.
let myName = "HEO";
let email = 'heo123@gmail.com';
let hello = `Hello ${myName}?!`
```

### Number(숫자 데이터)

**사용 방법**

```jsx
// 정수 및 부동소수점 숫자를 나타낸다.
let number = 123;
let opacity = 1.57;
```

### Boolean

**사용 방법**

```jsx
// true, false 두 가지 값밖에 없는 논리 데이터이다.
let checked = true;
let isShow = false;
```

### Undefined

**사용 방법**

```jsx
// 값이 할당되지 않은 상태를 나타낸다.
let undef;
```

### Null

**사용 방법**

```jsx
// 어떤 값이 의도적으로 비어있음을 의미한다.
let empty = null;
```

### Object(객체 데이터)

**사용 방법**

```jsx
// 여러 데이터를 Key:Value 형태로 저장한다. { }
let user = {
  // Key: Value,
  name: 'Heo',
  age: 29,
  isValid: true
};
```

### Array(배열 데이터)

**사용 방법**

```jsx
// 여러 데이터를 순차적으로 저장한다. []
let fruits = ['Apple', 'Banana', 'Cherry'];
```

## 변수

데이터를 저장하고 참조(사용)하는 **데이터의 이름**이다.

`let`은 재할당이 가능하고, `const`는 재할당이 불가능하다.

**사용 방법**

```jsx
// 재사용이 가능
// 변수 선언
let a = 2;
let b = 5;

console.log(a + b); // 7
console.log(a - b); // -3
console.log(a * b); // 10 
console.log(a / b); // 0.4

// 값(데이터)의 재할당 가능
let a = 12;
console.log(a); // 12
a = 999;
console.log(a); // 999

// 값(데이터)의 재할당 불가
const a = 12;
console.log(a); // 12
a = 999;
console.log(a); // TypeError: Assignment to constant variable.
```

## 예약어

**특별한 의미**를 가지고 있어, 변수나 함수 이름 등으로 **사용할 수 없는 단어**이다.

```jsx
let this = 'Hello!'; // SyntaxError
let if = 123; // SyntaxError
let break = true; // SyntaxError
```

## 함수

특정 동작(기능)을 수행하는 일부 코드의 **집합**(부분)이다.

**사용 방법**

```jsx
// 함수 선언
function helloFunc() {
  // 실행 코드
  console.log(1234);
}

// 함수 호출
helloFunc(); // 1234

--------------------------

// 리턴 사용과 변수에 할당
function returnFunc() {
  return 123;
}

let a = returnFunc();

console.log(a);

--------------------------

// 함수 선언
function sum(a, b) { // a와 b는 매개변수(Parameters)
  return a + b;
}

// 재사용
let a = sum(1, 2); // 1과 2는 인수(Arguments)
let b = sum(7, 12);
let c = sum(2, 4);

console.log(a, b, c); // 3, 19, 6

--------------------------

// 기명(이름이 있는) 함수
// 함수 선언
function hello() {
  console.log('Hello~~');
}

// 익명(이름이 없는) 함수
// 함수 표현
let world = function () {
  console.log('World~~');
}

// 함수 호출
hello(); // Hello~~
world(); // World~~

--------------------------

// 객체 데이터
const heo = {
  name: 'Heo',
  age: 29,
  // 메서드(Method)
  getName: function () {
    return this.name;
  }
};

const hisName = heo.getName();
console.log(hisName); // Heo
// 혹은
console.log(heo.getName()); // Heo
```

## 조건문

조건의 결과(truthy, falsy)에 따라 다른 코드를 실행하는 구문이다.

**사용 방법**

```jsx
let isShow = true;
let checked = false;

if (isShow) {
  console.log('Show!') //Show!
}

if (checked) {
  console.log('Checked!');
}

--------------------------

let isShow = ture;

if (isShow) {
  console.log('Show!');
} else {
  console.log('Hide?');
}
```

## DOM API

**DOM**은 Document Object Model의 약어이다. 그리고 Document는 HTML에 들어있는 여러가지 Object Model을 의미하는데, 또 Object Model이라는 것은 `div` 요소, `span` 요소, `input` 요소들을 얘기하는데, 이러한 요소들을 Document에 들어있는 Object Model이라고 하여 줄여서 DOM이라고 한다.

**API**는 Application Programming Interface라는 단어의 약어인데, 쉽게 말해 어떤 애플리케이션 웹사이트가 동작하기 위해서 입력하는 프로그래밍 명령이라고 생각하면 된다.

그래서 **DOM API**는 JS에서 HTML을 제어하는 여러 가지 명령들이라고 이해하면 된다.

**사용 방법**

```jsx
// HTML 요소(Element) 1개 검색/찾기
const boxEl = document.querySelector('.box');

// HTNL 요소에 적용할 수 있는 메서드
boxEl.addEventListener();

// 인수(Arguments)를 추가 가능
boxEl.addEventListener(1, 2);

// 1 - 이벤트(Event, 상황)
boxEl.addEventListener('click', 2);

// 2 - 핸들러(Handler, 실행할 함수)
boxEl.addEventListener('click', function() {
  console.log('Click~!');
});

--------------------------

// HTML 요소(Element) 검색/찾기
const boxEl = document.querySelector('.box');

// 요소의 클래스 정보 객체 활용
boxEl.classList.add('active');
let isContains = boxEl.classList.contains('active');
console.log(isContains); // true

boxEl.classList.remove('active');
isContains = boxEl.classList.contains('active');
console.log(isContains); // false

--------------------------

// HTML 요소(Element) 모두 검색/찾기
const boxEls = document.querySelectorAll('.box');
console.log(boxEls);

// 찾은 요소들 반복해서 함수 실행
// 익명 함수를 인수로 추가
boxEls.forEach(function () {});

// 첫 번째 매개변수(boxEl): 반복 중인 요소
// 두 번째 매개변수(index): 반복 중인 번호
boxEls.forEach(function (boxEl, index) {});

// 출력
boxEls.forEach(function (boxEl, index) {
  boxEl.classList.add(`order-${index + 1}`);
  console.log(index, boxEl);
});

--------------------------

const boxEl = document.querySelectorAll('.box');

// Getter, 값을 얻는 용도
console.log(boxEl.textContent); // Box!!

// Setter, 값을 지정하는 용도
boxEl.textContent = 'Heo?!';
console.log(boxEl.textContent); // HEO?!
```

## 메서드 체이닝

메서드를 붙여서 사용하는 방법이다.

**사용 방법**

```jsx
const a = 'Hello~';
// split: 문자를 인수 기준으로 쪼개서 배열로 반환
// reverse: 배열을 뒤집기
// join: 배열을 인수 기준으로 문자로 병합해 반환
const b = a.split('').reverse().join(''); // 메서서드 체이닝

console.log(a); // Hello~
console.log(b); // ~olleH
```

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
