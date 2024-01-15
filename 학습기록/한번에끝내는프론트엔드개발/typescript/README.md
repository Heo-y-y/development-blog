# **TypeScript**

### Typescript가 나오게 된 배경

자바스크립트는 원래 클라이언트 측 언어로 도입되었다. 그런데 Node.js의 개발로 인해서 자바스크립트를 클라이언트 측 뿐만이 아닌 서버 측 기술로도 활용되게 만들었다.

그러나 자바스크립트 코드가 커질수록 소스 코드가 더 복잡해져서 코드를 유지 관리하고 재사용하기가 어려워졌다. 더욱이 Type 검사 및 컴파일 시 오류 검사의 기능을 수용하지 못하기 때문에 자바스크립트가 본격적인 서버 측 기술로 엔터프라이즈 수준에서 성공하지 못한다. 

이러한 간극을 메우기 위해 **TypeScript**가 제시되었다.

### Typescript란?

타입스크립트는 **자바스크립트에 타입을 부여한 언어**이다. 자바스크립트의 확장된 언어라고 볼 수 있다. 타입스크립트는 자바스크립트와 달리 브라우저에서 실행하려면 파일을 한번 변환해주어야 한다. 이 변환 과정을 **컴파일**이라고 한다.

- **Typescript**: 정적 타입
- **Javascript**: 동적 타입

### Type System

Type System은 컴파일러에게 사용하는 타입을 명시적으로 지정하는 시스템이다.

컴파일러가 자동으로 타입을 추런하는 시스템이라고 생각하면 된다.

**특징**

- 개발 환경에서 에러를 잡는 걸 도와준다.
- type annotations를 사용해서 코드를 분석할 수 있다.
- 오직 개발 환경에서만 활성화 된다.
- 타입스크립트와 성능 향상과는 관계가 없다.

### Typescript을 사용하는 이유

- **Typescript는 자바스크립트 코드를 단순화하여 더 쉽게 읽고 디버그할 수 있도록 한다.**
- Typescript는 오픈 소스이다.
- Typescript는 정적 검사와 같은 자바스크립트 IDE 및 사례를 위한 매우 생산적인 개발 도구를 제공한다.
- Typescript를 사용하면 코드를 더 쉽게 읽고 이해할 수 있다.
- Typescript를 사용하면 일반 자바스크립트보다 크게 개선할 수 있다.
- Typescript는 ES6(ECMAScript 6)의 모든 이점과 더 많은 생산성을 제공한다.
- **Typescript는 코드 유형 검사를 통해 자바스크립트를 작성할 때 개발자가 일반적으로 겪는 버그를 피하는데 도움이 될 수 있다.**

## Typescript Type

타입이란 그 value가 가지고 있는 프로퍼티나 함수를 추론할 수 있는 방법이다.

**Property**

`string.length()`는 문자열의 속성인 문자열의 길이를 제공한다. 문자열 자체에는 아무 것도 하지 않습니다.

**Method**

`string.toLowerCase()`는 문자열을 소문자로 변환한다. 즉, 문자열에 작업을 수행한 다음 반환한다.

### Types in Typescript

Typescript는 자바스크립트에서 기본으로 제공하는 기본 제공 유형을 상속한다.

Typescript 유형은 아래와 같이 분류된다.

**Primitive Types**

| Name | Description |
| --- | --- |
| string | 문자열 |
| number | 숫자 |
| boolean | true와 false |
| null | null |
| undefined | undefined |
| symbol | 고유한 상수 값 |

**Object Types**

| Name | Description |
| --- | --- |
| function | 함수 |
| array | 배열 |
| classes | 클래스 |
| object | 객체 |

## Typescript 추가 제공 타입

### Any

애플리케이션을 만들 때, **잘 알지 못하는 타입을 표현**해야 할 수가 있다. 이 값들은 사용자로부터 받은 데이터나 서드 파티 라이브러리 같은 동적인 컨텐츠에서 올 수도 있다. 이 경우 타입 검사를 하지 않고, 그 값들이 컴파일 시간에 검사를 통과하길 원한다. 이를 위해 any 타입을 사용할 수 있다.

하지만 any 타입은 최대한 쓰지 않는게 좋다. 그래서 nolmplicitAny라는 옵션을 주면 any를 썼을 때 오류가 나오게 할 수 있다.

```jsx
let something: any = "Hello World!";
something = 23;
something = true;
```

```jsx
let arr: any[] = ["Heo", 212, true];
arr.push("Kim");
console.log(arr); // Output: ['Heo', 212, true, 'Kim']
```

### Union

Typescript를 사용하면 변수 또는 함수 매개변수에 대해 **둘 이상의 데이터 유형**을 사용할 수 있다. 이것을 유니온 타입이라고 한다.

```jsx
let code: (string | number);
code = 123; // OK
code = "ABC"; // OK
code = false; // Compiler Error

let empId: string | number;
empId = 111; // OK
empId = "E111"; // OK
empId = true; // Compiler Error
```

### Enum

enum은 enumerated type(**열거형**)을 의미한다.

Enum은 **값들의 집합을 명명**하고, 이를 사용하도록 만든다. 여기서는 PrintMedia라 불리는 집합을 기억하기 어려운 숫자 대신 **친숙한 이름으로 사용하기 위해 enum을 활용**할 수 있다. 열거된 각 PrintMedia는 별도의 값이 설정되지 않은 경우 기본적으로 0부터 시작한다.

```jsx
enum PrintMedia {
	Newspaper, // 0
	Newsletter, // 1
	Magazine, // 2
	Book // 3
}
```

아래 코드에서 mediaType 변수에 할당된 값은 3이다. 설정된 PrintMedia 열거형 데이터의 Book의 값이 숫자 3이기 때문이다.

```jsx
let mediaType: number = PrintMedia.Book // 3
```

enum에 설정된 아이템에 값을 할당할 수도 있다. 값이 할당되지 않은 아이템은 이전 아이템의 값에 +1된 값이 설정된다.

```jsx
enum PrintMedia {
	Newspaper = 1,
	Newsletter = 50,
	Magazine = 55,
	Book // 55 + 1
}
```

아래 코드에서 mediaType 변수에 할당된 값은 56이다. 설정된 PrintMedia 열거형 데이터의 Book의 값이 숫자 56이기 때문이다.

```jsx
let mediaType: number = PrintMedia.Book // 56
```

enum 타입의 편리한 기능으로 숫자 값을 통해 enum 값의 멤버 이름을 도출할 수 있다.

```jsx
let type: string = PrintMedia[55] // 'Magazine'
```

또한 어떠한 언어 코드를 정의하는 코드를 작성할 때 언어의 집합을 만들 때도 enum을 사용할 수 있다.

```jsx
export enum LanguageCode {
	korean = 'ko',
	english = 'en',
	japanese = 'ja',
}
```

이렇게 enum을 이용해서 언어 집합을 만들어주면 어떠한 코드가 어떠한 나라의 언어 코드가 무엇인지 알지 못해도 쉽게 코드를 작성할 수 있고, 코드를 읽는 사람 입장에서도 가독성이 높아진다.

```jsx
let LanguageCode = {
	korean: 'ko',
	english: 'en',
	japanese: 'ja',
}
```

이렇게 보면 enum과 JS의 object를 사용하는 것과 별 차이가 없어 보인다. 사실 enum은 그 자체로 객체이기도 하다. 

그래서 `object.keys(LanguageCode)`를 하면 실제 키 값이 배열에 담겨 나온다. → `[’korean’, ‘english’]`

`object.values(LanguageCode)` 를 하면 value 값이 → `[’ko’, ‘en’]`

**enum과 객체의 차이점**

object는 코드내에서 새로운 속성을 자유롭게 추가할 수 있지만, enum은 선언할 때 이후에는 변경할 수 없다.

object의 속성값은 JS가 허용하는 모든 타입이 올 수 있지만, enum의 속성삾으로는 문자열 혹은 숫자만 허용된다.

### Void

자바와 같은 언어와 유사하게 **데이터가 없는 경우 void가 사용**된다. 예를 들어 함수가 값을 반환하지 않으면 반환 유형으로 void를 지정할 수 있다.

타입이 없는 상태이며, any와 반대의 의미를 가진다.

void는 소문자로 사용해야하며, 주로 함수의 리턴이 없을 때 사용하면 된다.

```jsx
function sayHi(): void {
	console.log('Hi')
}

let speech: void = sayHi();
console.log(speech); // Output: undefind
```

### Never

Typescript는 **절대 방생하지 않을 값**을 나타내는 새 Type never를 도입했다.

Never 유형은 **어떤 일이 절대 일어나지 않을 것이라고 확신할 때 사용**된다.

일반적으로 함수의 리턴 타입으로 사용되는데, 함수의 리턴 타입으로 never가 사용될 경우, 항상 오류를 리턴하거나 리턴 값을 절대로 내보내지 않음을 의미한다. 이는 무한 루프에 빠지는 것과 같다.

```jsx
function throwError(errorMsg: string) never {
	throw new Error(errorMsg);
}

function keepProcessing(): never {
	while (true) {
		console.log('I alaways does something and never')
	}
}
```

### Void와 Never의 차이

**Void** 유형은 값으로 undefind나 null 값을 가질 수 있으며, **Never**는 어떠한 값도 가질 수 없다.

## type annotation, type inference

**type annotation**이란 개발자가 타입을 타입스크립트에게 직접 말해주는 것이다.

```jsx
const rate: number = 5 // number 타입 지정
```

**type inference**란 타입스크립트가 알아서 타입을 추론하는 것이다.

```jsx
const rate = 5 // 변수 선언과 동시에 초기화할 경우 타입을 알아서 추론
```

### 타입을 추론하지 못해서 타입 annotation을 꼭 해줘야 하는 경우

**any 타입을 리턴하는 경우**

`coordinates`에 hover를 보면 `const coordinates: any`라고 뜨는 것을 볼 수 있다. `JSON.parse`는 josn을 파싱해준다. input으로 들어가는 json을 확인하면 어떤 타입이 리턴될지 개발자는 예상할 수 있지만, 타입스크립트는 여기까지 지원하지 않는다. 리턴 타입이 일정하지 않으므로 any를 라턴한다고 추론해버린다. 그러므로 이 경우에는 타입 어노테이션을 해줘야 한다.

```jsx
const json = '{"x": 4, "y": 7}'
const coordinates = JSON.parse(json)
console.log(coordinates)
```

**변수 선언을 먼저하고 나중에 초기화하는 경우**

변수 선언과 동시에 초기화하면 타입을 추론하지만, 선언을 먼저하고 나중에 값을 초기화할 때에는 추론하지 못한다.

```jsx
let greeting
greeting = 'hello' // let greeting: any
```

**변수에 대입될 값이 일정치 못하는 경우**

여러 타입이 지정되어야 할 때에는 `| (or statement)`로 여러 타입을 어노테이션 해준다.

```jsx
let num = [-7, -2, 10]
let numAboveZero: boolean | number = false

for (let i = 0; i < num.length; i++) {
	if (num[i] > 0) {
		numAboveZero = num[i]
	}
}
```

---
**참고 자료**
- <https://fastcampus.co.kr/dev_online_frontend>
