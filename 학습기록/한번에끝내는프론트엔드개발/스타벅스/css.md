# CSS

## CSS 기본 문법

스타일(CSS)을 적용할 대상을 **선택자**(**Selector**)라고 하고, 그리고 스타일(CSS)의 **종류**(**Property**)와 **값**(**Value**)를 넣어 적용한다.

```css
/* 선택자의 속성은 값이다. */
선택자{속성: 값;}
```

그리고 중괄호 사이에는 여러가지 속성과 값에 세트들을 넣을 수 가 있다.

```css
선택자{속성: 값; 속성: 값;}
```

즉, 중괄호가 범위를 잡아주는 것이다.

### 주석

단축키는 `cmd + /` 를 사용하여 할 수 있다.

```css
/* 설명 작성 */
```

### 선언 방식

- **내장 방식**
    - `<style></style>`의 내용으로 스타일을 작성하는 방식
    - CSS 내용을 HTML 요소 내부에다가 직접적으로 작성을 한다.
    
    ```html
    <style>
      div {
        color: red;
        margin: 20px;
      }
    </style>
    ```
    
- **인라인 방식**
    - 요소의 **style 속성**에 직접 스타일을 작성하는 방식(선택자 없음)
    
    ```html
    <div *style*="color: red; margin: 20px;"></div>
    ```
    
- **링크 방식**
    - `<link/>` 로 외부 CSS 문서를 가져와서 연결하는 방식
    - 병렬로 연결된다.
    
    ![스크린샷 2023-12-22 오후 3.50.50.png](https://github.com/Heo-y-y/development-blog/assets/112863029/c490851d-5282-492f-9846-5a87341d11c6)
    
- **@import 방식**
    - CSS의 `@import` 규칙으로 CSS 문서 안에서 또 다른 CSS 문서를 가져와 연결하는 방식
    - 직렬로 연결된다.
    
    ![스크린샷 2023-12-22 오후 3.49.36.png](https://github.com/Heo-y-y/development-blog/assets/112863029/0b527ee0-68b4-4359-8943-33b8e5c43c40)
    

### 기본 선택자

- **전체 선택자**

![스크린샷3 2023-12-22 오후 4 00 49](https://github.com/Heo-y-y/development-blog/assets/112863029/b28cb57a-3da8-4686-ac4e-5e60eb7b68d2)

`*`를 사용하면 **모든 요소를 선택**하겠다는 의미를 가지기 때문에 **전체 선택자**(**Universal Selector**)라고 한다.

위 그림처럼 사용하면 모든 CSS에 `red`를 적용하게 된다.

- **태그 선택자**

![스크린샷4 2023-12-22 오후 4 03 34](https://github.com/Heo-y-y/development-blog/assets/112863029/61b37560-24e4-4b32-88eb-7b7b838fb43e)

**태그 이름**인 요소를 선택한다. 그림에서는 `li` 부분만 `red`가 적용된다.

- **클래스 선택자**

![스크린샷5 2023-12-22 오후 4 05 30](https://github.com/Heo-y-y/development-blog/assets/112863029/6755d2cb-7a84-40ab-9e9a-de2dd4b0cd9f)

HTML **class 속성의 값**이 입력한 값인 요소를 선택한다.

그림에서 보는 것처럼 클래스 선택자는 `.`을 앞에 먼저 적고 `class` 이름을 작성해야 한다.

- **아이디 선택자**

![스크린샷6 2023-12-22 오후 4 07 59](https://github.com/Heo-y-y/development-blog/assets/112863029/33fdc1e6-8ee3-4cfa-a73f-8e46735855ac)

HTML id 속성의 값이 입력한 값인 요소를 선택한다.

그림에서 보는 것 처럼 `id`는 `#` 을 앞에 먼저 작성을 한다.

### 복합 선택자

- **일치 선택자**

![스크린샷 2023-12-22 오후 4.11.08.png](https://github.com/Heo-y-y/development-blog/assets/112863029/a913b738-e3f1-4499-a488-4ae7ffe7949b)

**선택자 두개를 동시에 만족**하는 요소를 선택한다.

적용할 때 **태그 선택자의 경우는 앞에** 적어야하고, `.`을 이용하여 다른 선택자를 추가한다.

- **자식 선택자**

![스크린샷8 2023-12-22 오후 4 15 06](https://github.com/Heo-y-y/development-blog/assets/112863029/19dcff3e-1206-4005-b411-f20eadfbd841)

선택자의 **자식** 요소를 선택하는데, `부모 > 자식` 형식으로 사용한다.

- **하위(후손) 선택자**

선택자의 **하위** 요소를 선택한다. **띄어쓰기**가 **선택자의 기호**로 사용된다.

![스크린샷 2023-12-22 오후 4.18.02.png](https://github.com/Heo-y-y/development-blog/assets/112863029/af946f4f-eb6b-45d2-bdda-5f523ed32fcc)

- **인접 형제 선택자**

![스크린샷 2023-12-22 오후 4.22.00.png](https://github.com/Heo-y-y/development-blog/assets/112863029/5375d784-4feb-4110-9838-b794481fed8e)

선택자의 다음(아래) 형제 요소 **하나**를 선택한다. 그림처럼 `+`를 사용한다.

- **일반 형제 선택자**

![스크린샷 2023-12-22 오후 4.25.48.png](https://github.com/Heo-y-y/development-blog/assets/112863029/eaa3e247-10d4-4a45-8ee0-0755619921ca)

선택자의 다음(아래) 형제 요소 **모두**를 선택한다. 그림처럼 `~`를 사용한다.

### 가상 클래스 선택자

가상 클래스란 **어떠한 행동을 했을 때 동작**하는 개념이다.

- **HOVER**

![스크린샷 2023-12-22 오후 4.33.16.png](https://github.com/Heo-y-y/development-blog/assets/112863029/ec53601c-7c4d-47df-9760-fabb9349194a)

선택자 요소에 **마우스 커서가 올라가 있는 동안**에만 선택한다. 그림처럼 `:`를 사용하여 적용한다.

- **ACTIVE**

![스크린샷 2023-12-22 오후 4.35.44.png](https://github.com/Heo-y-y/development-blog/assets/112863029/baee6317-32b4-4f2c-8cbb-8c2a6e77abcc)

선택자 요소에 **마우스를 클릭하고 있는 동안**에만 선택한다. 마찬가지로 `:`를 사용하여 적용한다.

- **FOCUS**

![스크린샷 2023-12-22 오후 4.38.24.png](https://github.com/Heo-y-y/development-blog/assets/112863029/e7d36d85-a1af-482e-9150-4a2af03f8958)

선택자 요소가 **포커스**되면 선택한다. 마찬가지로 `:`를 사용하여 적용한다.

여기서 포커스가 될 수 있는 요소는 **HTML 대화형 콘텐츠**가 해당되는데 `input`, `a`, `button`, `label`, `select` 등의 요소가 있다. 그리고 대화형 콘텐츠 요소가 아니라도, `tabindex` 속성을 사용한 요소도 포커스가 될 수 있다.

- **FIRST CHILD**

![스크린샷 2023-12-22 오후 4.46.40.png](https://github.com/Heo-y-y/development-blog/assets/112863029/05481514-90a6-4236-b4bd-17f8683fac2e)

선택자가 형제 요소 중 첫째라면 선택한다.

사용법은 그림처럼 `해당 태그:first-chil`로 사용한다.

- **LAST CHILD**

![스크린샷 2023-12-22 오후 4.50.52.png](https://github.com/Heo-y-y/development-blog/assets/112863029/4d651df7-ce17-459b-927f-87c3762ab7a8)

선택자가 형제 요소 중 막내라면 선택한다.

사용법은 그림처럼 `해당 태그:last-chil`로 사용한다.

- **NTH CHILD**

![스크린샷 2023-12-22 오후 4.53.52.png](https://github.com/Heo-y-y/development-blog/assets/112863029/8689d78e-f921-492d-a271-d598fb4fe272)

선택자가 형제 요소 중 (n)째라면 선택한다.

사용법은 그림처럼 `*:nth-chil(n번째)`로 사용한다.

그리고 `n`이라는 키워드를 사용해서 `n`번째부터 시작하게 하여 배수를 선택하게 할 수 있고, `+` 기호 등을 사용하여 홀수도 선택할 수 있다.

- **부정 선택자 - NOT**

![스크린샷18 2023-12-22 오후 5 01 50](https://github.com/Heo-y-y/development-blog/assets/112863029/ee5f3214-4c27-4360-b516-3eb007e78d17)

선택자가 아닌 요소를 선택한다. 사용법은 `선택자:not(선택자)`로 할 수 있다.

### 가상 요소 선택자

- **BEFORE**

![스크린샷 2023-12-22 오후 5.07.22.png](https://github.com/Heo-y-y/development-blog/assets/112863029/81ccac22-0f8d-44dd-940a-f00d2f0c5ddc)

선택자 요소의 **내부 앞**에 내용을 삽입한다.

사용법은 `선택자::before` 방식으로 사용한다. 그리고 `content`라는 속성을 무조건 작성해야 적용이 된다.

- **AFTER**

![스크린샷 2023-12-22 오후 5.11.06.png](https://github.com/Heo-y-y/development-blog/assets/112863029/bf98f909-2be1-419d-b83a-7aab2e888e86)

선택자 ABC 요소의 **내부 뒤**에 내용을 삽입한다.

사용법은 `선택자::after` 방식으로 사용한다. 그리고 `content`라는 속성을 무조건 작성해야 적용이 된다.

### 속성 선택자

- **ATTR**

![스크린샷21 2023-12-22 오후 5 19 11](https://github.com/Heo-y-y/development-blog/assets/112863029/cdedc1dc-cea5-4ab7-b3a1-2d5afe16ff43)

속성을 포함한 요소를 선택한다.

사용법은 `[속성 선택자]` 방식으로 사용하면 된다. 

- **VALUE**

![스크린샷22 2023-12-22 오후 5 23 34](https://github.com/Heo-y-y/development-blog/assets/112863029/302c8a18-0f0b-4786-8087-eeb150f89242)

속성을 포함하고 값이 입력한 값인 요소를 선택한다.

사용법은 `[속성 선택자=”값”]` 방식으로 사용한다.

### 스타일 상속

부모 클래스를 가지고 있는 요소에 CSS 내용을 적용을 했을 때 적용된 내용이 해당하는 요소의 자식 요소에 더 나아가서 기타 하위 요소들까지 영향을 미치는 것을 **스타일 상속**이라고 한다.

![스크린샷23 2023-12-22 오후 5 45 24](https://github.com/Heo-y-y/development-blog/assets/112863029/16071df4-3466-4585-a705-0a7e5f5b0423)

**상속되는 CSS 속성들**

모두 **글자/문자 관련 속성**들이 상속이되는데, 모든 글자/문자 속성이 전부다 상속되는 것은 아니다.

정리하자면, 어떤 요소에다가 글자와 관련된 CSS 속성을 지정을 할 때는 **그 속성이 그 해당하는 요소의 자식이라던가 기타 후손 요소에도 똑같이 적용**될 수 있다는 것을 기억해야 한다.

- **강제 상속**

강제 상속은 실제로 CSS 속성이 상속이되지 않는 속성도 **강제적으로 상속시킬수 있는 방법**이다.

방법은 속성 값에 `inherit`을 넣어주면 된다.

### 선택자 우선 순위

우선순위란 같은 요소가 여러 선언의 대상이 된 경우, 어떤 선언의 CSS 속성을 우선 적용할지 결정하는 방법이다.

1. 점수가 높은 선언이 우선한다.
2. 점수가 같으면, 가장 마지막에 해석된 선언이 우선한다.

점수는 아래와 같다.

![스크린샷24 2023-12-22 오후 6 59 14](https://github.com/Heo-y-y/development-blog/assets/112863029/504c896f-69b6-47f8-b5ae-8cbb9ddfba96)

## CSS 속성

## 박스 너비

### **width**: 가로 너비

- 기본값으로 auto가 있는데, 브라우저가 너비를 계산
- 단위: px, em, vw 등 단위로 지정

### **height**: 세로 너비

- 기본값으로 `auto`가 있는데, 브라우저가 너비를 계산
- 단위: `px`, `em`, `vw` 등 단위로 지정

참고로 `auto`라는 기본값이 사용된다면 **인라인 요소**는 **가로 너비가 최대한 줄어**들려고 하고, **블록 요소**는 **가로 너비가 최대한 늘어**나려고 한다.

- **max-width**: 최대 가로 너비
    - `none`: 최대 너비 제한 없음
    - 단위: `px`, `em`, `vw` 등 단위로 지정
- **max-height**: 최대 세로 너비
    - `none`: 최대 너비 제한 없음
    - 단위: `px`, `em`, `vw` 등 단위로 지정
- **min-width**: 최소 가로 너비
    - `0`: 최소 너비 제한 없음(양수만 사용 가능)
    - 단위: `px`, `em`, `vw` 등 단위로 지정
- **min- height**: 최소 세로 너비
    - `0`: 최소 너비 제한 없음(양수만 사용 가능)
    - 단위: `px`, `em`, `vw` 등 단위로 지정

### CSS 단위

- `px`: 픽셀
- `%`: 상대적 백분율
- `em`: 요소의 글꼴 크기
- `rem`: 루트 요소(html)의 글꼴 크기
- `vw`: 뷰포트 가로 너비의 백분율
- `vh`: 뷰포트 세로 너비의 백분율

### 외부 여백(margin)

- 요소의 외부 여백(공간)을 지정하는 단축 속성
- 음수 사용 가능
- 외부 여백이 없음
- `auto`: 브라우저가 여백을 계산(가로, 세로 너비가 있는 요소의 가운데 정렬에 활용)
- 단위: `px`, `em`, `vw` 등 단위로 지정
    - `%`: 부모 요소의 가로 너비에 대한 비율로 지정
- 방향 별로 적용 방법
    - 1개 적용: `top,right,bottom,left`
    - 2개 적용: `top,bottom` `left,right`
    - 3개 적용: `top` `left,right` `bottom`
    - 4개 적용: `top` `right` `bottom` `left`

### 내부 여백(padding)

- 요소의 내부 여백(공간)을 지정하는 단축 속성
- 요소의 크기가 커짐
- 내부 여백이 없음
- 단위: `px`, `em`, `vw` 등 단위로 지정
    - `%`: 부모 요소의 가로 너비에 대한 비율로 지정
- 방향 별로 적용 방법
    - 1개 적용: `top,right,bottom,left`
    - 2개 적용: `top,bottom` `left,right`
    - 3개 적용: `top` `left,right` `bottom`
    - 4개 적용: `top` `right` `bottom` `left`

### border

- 요소의 테두리 선을 지정하는 단축 속성
- 요소의 크기가 커짐
- 속성(작성 시 선-두께 → 선-종류 → 선-색상 순으로 관습)
    - `border-width`: 요소 테두리 선의 두께
        - `margin`, `padding` 처럼 방향 별로 적용 가능
        - 단위: `px`, `em`, `%` 등 단위로 지정
    - `border-style`: 요소 테두리 선의 종류
        - `margin`, `padding` 처럼 방향 별로 적용 가능
        - `none`: 선 없음
        - `solid`: 실선(일반 선)
        - `dashed`: 파선
    - `border-color`: 요소 테두리 선의 색상을 지정하는 단축 속성
        - `margin`, `padding` 처럼 방향 별로 적용 가능
        - `black`: 검정색
        - 색상: 선의 색상
        - `transparent`: 투명
- 방향까지 추가할 때는 `border-방향-속성`으로 작성 가능

### border-radius

- 요소의 모서리를 둥글게 깎음
- `0`: 둥글게 없음
- 단위: `px`, `em`, `vw` 등 단위로 지정

### box-sizing

- 요소의 크기 계산 기준을 지정
- `content-box`: 요소의 내용으로 크기 계산
- `border-box`: 요소의 내용 + `padding` + `border`로 크기 계산

### overflow

- 요소의 크기 이상으로 내용이 넘쳤을 때, 보여짐을 제어하는 단축 속성
- `visible`: 넘친 내용을 그대로 보여줌
- `hiden`: 넘친 내용을 잘라냄
- `scroll`: 넘친 내용을 잘라냄, 스크롤바 생성
- `auto`: 넘친 내용이 있는 경우에만 잘라내고 스크롤바 생성
- 요소의 크기 이상으로 내용이 넘쳤을 때, 보여짐을 제어하는 개별 속성들
    - `overflow-x`
    - `overflow-y`

### display

- 요소의 화면 출력(보여짐) 특성
- `block`: 상자(레이아웃) 요소
- `inline`: 글자 요소
- `inline-block`: 글자 + 상자 요소
- 따로 지정해서 사용하는 값
    - `flex`: 플렉스 박스(1차원 레이아웃)
    - `grid`: 그리드(2차원 레이아웃)
    - `none`: 보여짐 특성 없음, 화면에서 사라짐
- 기타: `table`, `table-row`, `table-cell` 등…

### opacity

- 요소 투명도
- `1`: 불투명
- `0~1`: 0부터 1사이의 소수점 숫자

### **font-style**

- 글자의 기울기
- `normal`: 기울기 없음
- `italic`: 이텔릭체
- `oblique`: 기울어진 글자

### font-weight

- 글자의 두께(가중치)
- `normal`, 400: 기본 두께
- `bold`, 700: 두껍게
- `bolder`: 상위(부모) 요소보다 더 두껍게
- `lighter`: 상위(부모) 요소보다 더 얇게
- 100~900: 100 단위의 숫자 9개, `normal`과 `bold` 이외 두께

### font-size

- 글자의 크기
- `16px`: 기본 크기
- 단위: `px`, `em`, `rem` 등 단위로 지정

### line-height

- 한 줄의 높이, 행간과 유사
- 숫자: 요소의 글꼴 크기의 배수로 지정
- 단위: `px`, `em`, `rem` 등 단위로 지정

### font-family

- 글꼴(서체) 지정
- 띄어쓰기 등 특수문자가 포함된 글꼴 이름은 큰 따움표로 묶어야 함
- 마지막에 필수로 글꼴계열을 작성해야 한다.

### color

- 글자의 색상
- `rgb(0, 0, 0)`: 검정색
- 색상: 기타 지정 가능한 색상

### text-align

- 문자의 정렬 방식
- `left`: 왼쪽 정렬
- `right`: 오른쪽 정렬
- `center`: 가운데 정렬
- `justify`: 양쪽 정렬

### text-decoration

- 문자의 장식(선)
- `none`: 장식 없음
- `underline`: 밑줄
- `overline`: 윗줄
- `line-through`: 중앙 선

### text-indent

- 문자 첫 줄의 들여쓰기
- `0`: 들여쓰기 없음
- 음수를 사용할 수 있음, 반대는 내어쓰기
- 단위: `px`, `em`, `rem` 등 단위로 지정

## 배경

### background-color

- 요소의 배경 색상
- `transparent`: 투명함
- 색상: 지정 가능한 색상

### background-image

- 요소의 배경 이미지 삽입
- `none`: 이미지 없음
- `url(”경로”)`: 이미지 경로

### background-repeat

- 요소의 배경 이미지 반복
- `repeat`: 이미지를 수직, 수평 반복
- `repeat-x`: 이미지를 수평 반복
- `repeat-y`: 이미지를 수직 반복
- `no-repeat`: 반복 없음

### background-position

- 요소의 배경 이미지 위치
- 방향: `top`, `bottom`, `left`, `right`, `center`
- 단위: `px`, `em`, `rem` 등 단위로 지정

### background-size

- 요소의 배경 이미지 크기
- `auto`: 이미지의 실제 크기
- 단위: `px`, `em`, `rem` 등 단위로 지정
- `cover`: 비율을 유지, 요소의 더 넓은 너비에 맞춤
- `contain`: 비율을 유지, 요소의 더 짧은 너비에 맞춤

### background-attachment

- 요소의 배경 이미지 스크롤 특성
- `scroll`: 이미지가 요소를 따라서 같이 스크롤
- `fixed`: 이미지가 뷰포트에 고정, 스크롤 X

## 배치

### pisition

- 요소의 위치 지정 기준
- `static`: 기준 없음
- `relative`: 요소 자신을 기준
- `absolute`: 위치 상 부모 요소를 기준
- `fixed`: 뷰포트(브라우저)를 기준
- 음수 사용 가능

### top, bottom, left, right

- 요소의 각 방향별 거리 지정
- `auto`: 브라우저가 계산
- 단위: `px`, `em`, `rem` 등 단위로 지정

### 요소 쌓임 순서(Stack order)

어떤 요소가 사용자와 더 가깝게 있는지(위에 쌓이는지) 결정

1. 요소에 `position` 속성의 값이 있는 경우 위에 쌓인다.(기본값 `static` 제외)
2. 1번 조건이 같은 경우, `z-index` 속성의 숫자 값이 높을 수록 위에 쌓인다.
3. 1번과 2본 조건까지 같은 경우, HTML의 다음 구조일 수록 위에 쌓인다.

### z-index

- 요소의 쌓임 정도를 지정
- `auto`: 부모 요소와 동일한 쌓임 정도
- 숫자: 숫자가 높을 수록 위에 쌓임

### 요소의 display가 변경됨

`position` 속성의 값으로 `absolute`, `fixed`가 지정된 요소는, `display` 속성이 `block`으로 변경된다.

## 플랙스(정렬) Container

### display

- Flex Container의 화면 출력(보여짐) 특성
- `felx`: 블록 요소와 같이 Flex Container 정의
- `inline-flex`: 인라인 요소와 같이 Flex Contianer 정의

### flex-direction

- 주 축을 설정
- `row`: 행 축(좌 → 우)
- `row-reverse`: 행 축(우 → 좌)
- `column`: 열 축(위 → 아래)
- `column`: 열 축(아래 → 위)

### flex-wrap

- Flex Items 묶음(줄 바꿈) 여부
- `nowrap`: 묶음(줄 바꿈) 없음
- `wrap`: 여러 줄로 묶음
- `wrap-reverse`: `wrap`의 반대 방향으로 묶음

### justify-content

- 주 축의 정렬 방법
- `flex-start`: Flex Items를 시작점으로 정렬
- `flex-end`: Flex Items를 끝점으로 정렬
- `center`: Flex Items를 가운데 정렬
- `space-between`: 각 Flex Item 사이를 균등하게 정렬
- `space-around`: 각 Flex Item의 외부 여백을 균등하게 정렬

### align-content

- 교차 축의 여러 줄 정렬 방법
- `stetch`: Flex Items를 시작점으로 정렬
- `flex-start`: Flex Items를 시작점으로 정렬
- `flex-end`: Flex Items를 끝점으로 정렬
- `center`: Flex Items를 가운데 정렬
- `space-between`: 각 Flex Item 사이를 균등하게 정렬
- `space-around`: 각 Flex Item의 외부 여백을 균등하게 정렬

### align-items

- 교차 측의 한 줄 정렬 방법
- `stretch`: Flex Items를 교차 축으로 늘림
- `flex-start`: Flex Items를 시작점으로 정렬
- `flex-end`: Flex Items를 끝점으로 정렬
- `center`: Flex Items를 가운데 정렬
- `baseline`: Flex Items를 각 줄의 문자 기준선에 정렬

## 플랙스(정렬) Items

### order

- Flex Item의 순서
- `0`: 순서 없음
- 숫자: 숫자가 작을 수록 먼저

### flex-grow

- Flex Item의 증가 너비 비율
- `0`: 증가 비율 없음
- 숫자: 증가 비율

### flex-shrink

- Flex Item의 감소 너비 비율
- `1`: Flex Container 너비에 따라 감소 비율 적용
- 숫자: 감소 비율

### flex-basis

- Flex Item의 공간 배분 전 기본 너비
- `auto`: 요소의 Content 너비
- 단위: `px`, `em`, `rem` 등 단위로 지정

## transition - 전환

요소의 전환(시작과 끝) 효과를 지정하는 단축 속성이다.

### transition-property

- 전환 효과를 사용할 속성 이름을 지정
- `all`: 모든 속성에 적용
- 속성이름: 전환 효과를 사용할 속성 이름 명시

### transition-duration

- 전환 효과의 지속시간을 지정
- `0s`: 전환 효과 없음
- 시간: 지속시간(s)를 지정

### transition-timing-function

- 전환 효과의 타이밍(Easing) 함수를 지정
- `else`: 느리게 - 빠르게 - 느리게
- `linear`: 일정하게
- `ease-in`: 느리게 - 빠르게
- `ease-out`: 빠르게 - 느리게
- `ease-in-out`: 느리게 - 빠르게 - 느리게

### transition-delay

- 전환 효과가 몇 초 뒤에 시작할지 대기시간을 지정
- `0s`: 대기시간 없음
- 시간: 대기시간(s) 지정

## transform - 변환

요소의 변환 효과를 준다.

![스크린샷25 2023-12-24 오후 8 49 34](https://github.com/Heo-y-y/development-blog/assets/112863029/64699e0b-3818-4e76-8017-d88674e42372)

![스크린샷26 2023-12-24 오후 8 54 03](https://github.com/Heo-y-y/development-blog/assets/112863029/abbe8120-c8cc-481e-8775-f3a5291a7589)

### perspective

- 하위 요소를 관찰하는 원근 거리를 지정
- 단위: `px` 등 단위로 지정

**Perspective 속성과 함수 차이점**

| 속성/함수 | 적용 대상 | 기준점 설정 |
| --- | --- | --- |
| perspectivce: 600px; | 관찰 대상의 부모 | perspective-origin |
| transform: perspective(600px) | 관찰 대상 | transform-origin |

### backface-visibility

- 3D 변환으로 회전된 요소의 뒷면 숨김 여부
- `visible`: 뒷면 보임
- `hidden`: 뒷면 숨김

---
**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
