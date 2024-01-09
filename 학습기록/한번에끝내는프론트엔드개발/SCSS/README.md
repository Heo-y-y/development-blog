# SCSS

## 개요

CSS를 조금 더 쉽게 사용하기 위해서 여러가지 강력한 기능을 제공하는 도구이다. 그래서 이것을 **CSS 전처리 도구**라고 부른다.

![스크린샷1 2024-01-09 오후 2 45 09](https://github.com/Heo-y-y/development-blog/assets/112863029/f6626364-1172-4da9-ba7d-c5949f142820)

우리는 우리의 컴퓨터에서 기본적으로 프로젝트를 진행할 수 있다. 그랬을 때 우리는 프로젝트에서 sass 문법을 통해서 코딩을 진행하는데, 이 말은 이제 표준의 css를 프로젝트에서 사용하지 않는 것이다.

sass 문법이 표준의 css 문법보다 훨씬 더 쉽고, 강력한 기능들을 가지고 있기 때문인데, 이때 문제는 사용자의 브라우저에서 실제로 동작할 수 있는 것은 표준의 css 뿐이라는 것이다.

그래서 sass 문법을 css 문법으로 변환을 해줘야 되는데, 그런 변화되는 과정을 컴파일이라고 한다. 컴파일 할 수 있는 기본적인 구성 옵셥을 작성하면 컴퓨터가 알아서 sass의 문법을 컴파일하여 css로 변환해서 사용자의 브라우저에서 동작할 수 있게 만들어 준다.

## 주석

주석은 아래 두가지 형태로 진행할 수 있다.

```scss
//

/**/
```

이 둘의 차이점은 `//` 같은 경우에는 실제로 SCSS를 CSS로 컴파일 했을 때 변환된 결과로 나타나지 않는다.

그런데 표준의 CSS 주석인 `/**/` 를 사용하면 SCSS에서 CSS로 변환을 하더라도 잘 적용이 돼서 들어간다.

## 중첩 with SassMeister

상위 선택자를 반복해서 작성하지 않아도 SCSS의 중첩 기능을 사용하면 좀 더 가독성이 좋게 작성할 수 있다.

**CSS 코드**

```css
.container ul li {
  font-size: 40px;
}
.container ul li .name {
  color: royalblue;
}
.container ul li .age {
  color: orange;
}
```

**SCSS 코드**

```scss
.container {
  ul {
    li {
      font-size: 40px;
      .name {
        color: royalblue;
      }
      .age {
        color: orange;
      }
    }
  }
}
```

## 상위(부모) 선택자 참조

**CSS 코드**

```scss
.btn {
  position: absolute;
}
.btn.active {
  color: red;
}

.list li:last-child {
  margin-right: 0;
}
```

**SCSS 코드**

```scss
.btn {
    position: absolute;
    &.active {
        color: red;
    }
}

.list {
    li {
        &:last-child {
            margin-right: 0;
        }
    }
}
```

`&` 기호는 해당 기호가 있는 범위에 해당하는 선택자다.

즉, `&` 는 자신이 포함된 영역의 상위 선택자를 참조하는 개념이라고 생각하면 된다.

## 중첩된 속성

**CSS 코드**

```css
.box {
  font-weight: bold;
  font-size: 10px;
  font-family: sans-self;
  margin-top: 10px;
  margin-left: 20px;
  padding-top: 10px;
  padding-bottom: 40px;
  padding-left: 20px;
  padding-right: 30px;
}
```

**SCSS 코드**

```scss
.box {
    font: {
        weight: bold;
        size: 10px;
        family: sans-self;
    };
    margin: {
        top: 10px;
        left: 20px;
    };
    padding: {
        top: 10px;
        bottom: 40px;
        left: 20px;
        right: 30px;
    };
}
```

좀 더 해당 값에 어떻게 주었는지 세분화하여 보여지는 느낌이다. `속성: {};` 기준으로 작성한다.

## 변수

동일한 내용들을 한번에 변경 시킬 때 변수를 사용한다.

**CSS 코드**

```css
.container {
  position: fixed;
  top: 100px;
}
.container .item {
  width: 100px;
  height: 100px;
  transform: translateX(100px);
}
```

**SCSS 코드**

```scss
$size: 200px;

.container {
    position: fixed;
    top: $size;
    .item {
        width: $size;
        height: $size;
        transform: translateX($size);
    }
}
```

변수는 `$변수: 값;` 으로 적용시키면 된다. 즉, 이런 변수들은 스타일 부분에서 어떤 반복되는 수치라던가 혹은 해당하는 페이지에서 주요하게 사용하는 메인 색상 몇 가지를 변수로 만들어서 반복적으로 사용할 때 매우 유용하게 쓸 수 있다.

변수를 사용할 때는 주의사항이 있는데, **변수는 선언된 범위에서 유효범위**를 가진다.

## 산술 연산

**CSS 코드**

```css
div {
  width: 40px;
  height: 30px;
  font-size: 20px;
  margin: 30px/2;
  padding: 6px;
}
```

CSS의 결과를 보면, 나누기는 CSS로 제대로 변환시키지 못한다. 이유는 `/` 기호는 단축 속성을 사용할 때 쓰는 것이기 때문이다.

**SCSS 코드**

```scss
div {
    width: 20px + 20px;
    height: 40px - 10px;
    font-size: 10px * 2;
    margin: 30px / 2;
    padding: 20px % 7;
}
```

그러면 어떻게 해야할까? 간단하다.

```scss
// 가로로 감싼다.
margin: (30px / 2);

// 변수로 적용한다.
margin: $size / 2;

// 다른 연산자와 같이 사용한다.
margin: (10px + 12px) / 2;
```

그리고 SCSS에서는 기본적인 단위가 같아야 연산이 가능하다.

## 재활용(Mixins)

**CSS 코드**

```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}
.container .item {
  display: flex;
  justify-content: center;
  align-items: center;
}

.box {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

**SCSS 코드**

```scss
@mixin center {
    display: flex;
    justify-content: center;
    align-items: center;
}
.container {
    @include center;
    .item {
        @include center;
    }
}
.box {
    @include center;
}
```

`center`라는 이름으로 재활용될 수 있는 구조가 바로 SCSS에서 제공하는 기능이다. 그때 `@mixin` 이라는 키워드를 붙여서 해당하는 내용을 만들 수 있다.

`@mixin`에서 적용한 속성을 `@include` 키워드를 사용하여 적용할 수 있는 것이다.

하지만 여러 곳에서 재활용을 하고 있는데, 만약 한 곳에서만 값을 변경해야할 경우에는 어떻게 해야할까?

`@mixin`에서는 인수라는 것을 제공한다.

아래 코드처럼 인수를 만들어 변수를 할당해 넣어주면 되는 것이다.

즉, 하나의 함수처럼 사용할 수 있는 것이다.

```scss
@mixin center($size) {
    width: $size;
    display: flex;
    justify-content: center;
    align-items: center;
}
.container {
    @include center(200px);
    .item {
        @include center(100px);
    }
}
.box {
    @include center(100px);
}
```

그리고 기존 값을 적용해주고 싶으면 아래와 같이 적용하면 한 곳만 적용하면 된다. 그리고 함수와 마찬가지로 `,`를 사용하여 인수를 여러 개 넣을 수 있다.

```scss
@mixin center($size: 100px) {
    width: $size;
    display: flex;
    justify-content: center;
    align-items: center;
}
.container {
    @include center(200px);
    .item {
        @include center;
    }
}
.box {
    @include center;
}
```

여러 개 인수를 적용할 때는 키워드 인수 방식으로 적용하면 직접적으로 순서와 상관없이 해당하는 이름과 매칭이 되어 적용된다.

```scss
@mixin center($size: 100px, $color: tomato) {
    width: $size;
    display: flex;
    justify-content: center;
    align-items: center;
		background-color: $color;
}
.container {
    @include center(200px);
    .item {
        @include center($color: red);
    }
}
.box {
    @include center;
}
```

## 반복문

**CSS 코드**

```css
.box:nth-child(1) {
  width: 100px;
}

.box:nth-child(2) {
  width: 200px;
}

.box:nth-child(3) {
  width: 300px;
}

.box:nth-child(4) {
  width: 400px;
}

.box:nth-child(5) {
  width: 500px;
}
```

**SCSS 코드**

```scss
// for (let i = 0; i < 5; i += 1) {
//.     console.log(`loop-${i}`)   
// }

@for $i from 1 through 5 {
    .box:nth-child(#{$i}) {
        width: 100px * $i;
    }
}
```

자바스크립트 코드와 마찬가지로 `@for` 키워드를 사용해 `i`라는 변수를 만들어 사용한다. 그리고 `#{}`를 사용해 `i`를 넣을 수 있다.

자바스크립트의 반복문 같은 경우에는 제로 베이스부터 시작해서 5보다 작은 경우까지 해서 총 5번을 반복하게 되는데, SCSS 같은 경우에는 어디까지나 CSS 개념이기 때문에 제로 베이스가 아니고 숫자 1부터 시작하는 개념으로 돌아간다.

## 함수

SCSS에서는 자바스크립트의 함수와 동일한 실제로 `@function`이라는 키워드로 해당하는 함수를 제공한다. 그리고 `@return` 키워드로 해당 값을 내보낸다.

**CSS 코드**

```css
.box {
  width: 100px;
  height: 50px;
  display: flex;
  justify-content: center;
  align-items: center;
}
```

**SCSS 코드**

```scss
@mixin center {
    display: flex;
    justify-content: center;
    align-items: center;
}

@function ratio($size, $ratio) {
    @return $size * $ratio
}

.box {
    $width: 100px;
    width: $width;
    height: ratio($width, 1/2);
    @include center;
}
```

## 색상 내장 함수

내장 함수는 이미 SCSS 내부에 구현이 되어져 있어서 별도의 선언 없이도 바로 사용할 수 있는 함수를 말한다.

- `mix()`
    - 첫 번째 인수의 색상과 두 번째 인수의 색상을 섞어서 새로운 색상을 만들어준다.
    - 두개의 인수를 필요로 한다.
- `lighten()`
    - 첫 번째 인수로는 특정 색상을 추가하고, %를 넣어 더 밝게 만들어준다.
    - 두개의 인수를 필요로 한다.
- `darken()`
    - 첫 번째 인수로는 특정 색상을 추가하고, %를 넣어 더 어둡게 만들어준다.
    - 두개의 인수를 필요로 한다.
- `saturate()`
    - 첫 번째 인수에 해당하는 색상의 채도를 %만큼 더 올려준다.
    - 두개의 인수를 필요로 한다.
- `desaturate()`
    - 첫 번째 인수에 해당하는 색상의 채도를 %만큼 더 낮춰준다.
    - 두개의 인수를 필요로 한다.
- `rgba()`
    - 첫 번째 인수로는 기본적인 색상을 추가하고 두 번째 인수로는 투명도를 입력한다.
    - 두개의 인수를 필요로 한다.
- `grayscale()`
    - 주어진 색상을 회색으로 만들어 준다.
    - 한개의 인수만 있으면 된다.
- `invert()`
    - 색상을 반전 시키는 효과를 준다.
    - 한개의 인수만 있으면 된다.

## 가져오기

`@import` 키워드를 사용하여 다른 SCSS를 가져올 수 있다.

**적용 방법**

```scss
@import url("./sub.scss");

// url을 사용하지 않아도 됨
@import "./sub.scss";

// 여러개 동시 사용 및 확정자 사용X
@import "./sub", "./sub2";
```

## 데이터 종류

**SCSS 코드**

```scss
$number: 1; // .5, 100px, 1em
$string: bold; // relative, "../images/a.png"
$color: red; // blue, #FFFF00, rgba(0, 0, 0, .1)
$boolean: true; // false
$null: null;
$list: orange, royalblue, yellow;
$map: (
    o: orange,
    r: royalblue,
    y: yellow
);

.box {
    width: 100px;
    color: red;
    position: relative;
}
```

## 반복문 @each

`list`나 `map`을 반복문으로 다루기 위해서는 `@each` 키워드를 사용한다.

**list 코드**

```scss
$list: orange, royalblue, yellow;

@each $c in $list {
    .box {
        color: $c;
    }
}
```

**map 코드**

```scss
$map: (
    o: orange,
    r: royalblue,
    y: yellow
);
@each $k, $v in $map {
    .box-#{$k} {
        color: $v;
    }
}
```

## 재활용 @Content

`@mixin`을 제공을 할 때 `@content`라는 부분의 영역을 하나 잡아줄 수 있다. 그 해당 부분에 사용자가 상황에 맞게 추가적인 내용들을 작성해서 삽입을 해줄 수 있다.

**CSS 코드**

```css
.container {
  width: 100px;
  height: 100px;
  position: absolute;
  top: 0;
  left: 0;
}

.box {
  width: 200px;
  height: 300px;
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  margin: auto;
}
```

**SCSS 코드**

```scss
@mixin left-top {
    position: absolute;
    top: 0;
    left: 0;
    @content;
}
.container {
    width: 100px;
    height: 100px;
    @include left-top;
}
.box {
    width: 200px;
    height: 300px;
    @include left-top {
        bottom: 0;
        right: 0;
        margin: auto;
    }
}
```

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
