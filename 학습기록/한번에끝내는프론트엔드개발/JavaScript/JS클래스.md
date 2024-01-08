# **JavaScript 클래스**

## 생성자 함수

**하나의 객체 데이터를 생성하는 함수**이다.

그리고 생성자 함수를 통해 할당된 객체를 **인스턴스**라고 한다.

```jsx
function User(first, last) {
  this.firstName = first
  this.lastName = last
}
User.prototype.getFullName = function () {
  return `${this.firstName} ${this.lastName}`
}

const heo = new User('Heo', 'YounYoung')
const amy = new User('Amy', 'Clarke')
const neo = new User('Neo', 'Smith')

console.log(heo.getFullName())
console.log(amy.getFullName())
console.log(neo.getFullName())
```

`prototype`를 사용해서 `new` 라는 키워드와 함께 생성자 인스턴스를 만들어내는 개념들을 자바스크립트의 클래스라고 부른다.

## this

`this`라는 것은 일반 함수 내부에서는 **호출 위치**에 따라서 `this`가 정의된다.

그리고 화살표 함수는 자신이 선언된 **함수 범위**에서 `this`라는 것이 정의가 된다.

```jsx
const heo = {
  name: 'Heo',
  normal: function () {
    console.log(this.name)
  },
  arrow: () => {
    console.log(this.name)
  }
}
heo.normal()
heo.arrow()
```

![스크린샷 2024-01-08 오후 1.15.43.png](https://github.com/Heo-y-y/development-blog/assets/112863029/8c062f69-be8d-40a2-8091-6e00a624d83f)

## ES6 Classes

생성자 함수를 자바스크립트에서 지원하는 클래스라는 키워드를 통해서 어떻게 새로운 문법으로 갱신할 수 있는지 알아보자.

먼저 간단하게 설명하면, 자바스크립트는 프로토타입 기반의 프로그래밍 언어인데, 좀 더 안정적이고 신뢰도가 높은 다른 객체 지향 프로그래밍 언어들의 영향을 받아서 클래스라는 개념을 흉내내서 새로운 문법을 ES6에서 제공하기 시작했다.

**기존 코드**

```jsx
function User(first, last) {
  this.firstName = first
  this.lastName = last
}
User.prototype.getFullName = function () {
  return `${this.firstName} ${this.lastName}`
}

const heo = new User('Heo', 'YounYoung')
const amy = new User('Amy', 'Clarke')
const neo = new User('Neo', 'Smith')

console.log(heo)
console.log(amy.getFullName())
console.log(neo.getFullName())
```

**클래스 키워드 적용 코드**

```jsx
class User {
  constructor(first, last) {
    this.firstName = first
    this.lastName = last
  }
  getFullName() {
    return `${this.firstName} ${this.lastName}`
  }
}

const heo = new User('Heo', 'YounYoung')
const amy = new User('Amy', 'Clarke')
const neo = new User('Neo', 'Smith')

console.log(heo)
console.log(amy.getFullName())
console.log(neo.getFullName())
```

## 상속(확장)

`extends`를 통해 상속을 받을 수 있고, `super`를 통해 부모의 생성자를 받아 올 수 있다.

```jsx
class Vehicle {
  constructor(name, wheel) {
    this.name = name
    this.wheel = wheel
  }
}
const myVehicle = new Vehicle('운송수단', 2)
console.log(myVehicle)

class Bicycle extends Vehicle {
  constructor(name, wheel) {
    super(name, wheel)
  }
}
const myBicycle = new Bicycle('삼천리', 2)
const daughtersBicycle = new Bicycle('세발', 3)
console.log(myBicycle)
console.log(daughtersBicycle)

class Car extends Vehicle {
  constructor(name, wheel, license) {
    super(name, wheel)
    this.license = license
  }
}
const myCar = new Car('벤츠', 4, true)
const daughtersCar = new Car('포르쉐', 4, false)
console.log(myCar)
console.log(daughtersCar)
```

![스크린샷 2024-01-08 오후 1.50.01.png](https://github.com/Heo-y-y/development-blog/assets/112863029/dd357653-b07d-4392-9e1f-ca76a6e005d2)

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
