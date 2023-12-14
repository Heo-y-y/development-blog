# Styling

Styling은 웹사이트 전체 레이아웃 구성을 포함하여 버튼의 색깔, 테두리, 폰트의 크기, 색상 등을 모두 포함하는 개념이라고 생각하면 된다.

## **CSS와 selector**

### CSS

웹 개발을 할 때 스타일링을 하기 위해 가장 대표적으로 사용되는 것이 바로 CSS이다. **CSS**는 **Cascading Style Sheets**의 약자로써 **스타일링을 위한 일종의 언어**라고 생각하면 된다. 

여기서 **Cascading**이라는 단어는 계단식이라는 뜻을 가지고 있다. CSS에는 여러가지 스타일이 정의되어 있는데, 각 스타일은 해당 스타일이 적용되는 규칙을 가지고 있다. **하나의 element가 여러 개의 스타일 규칙을 만족할 경우에 해당 스타일들을 마치 계단을 한 칸씩 내려가는 것처럼 우선순위를 가지고 하나씩 적용**하게 된다. 결과적으로 **하나의 스타일이 여러 개의 element에 적용될 수 있고, 하나의 element에도 여러 개의 스타일이 적용될 수 있다**.

그리고 **element에 스타일이 적용되는 규칙**을 **Selector**라고 부른다. 단어 그대로 해석하면 **선택자**라고 할 수 있는데, **스타일을 어떤 element에 적용할지를 선택하게 해주는 것**이다. CSS는 크게 Selector와 실제 스타일로 이루어져 있다.

그러면 Selector와 스타일을 카테고리별로 나눠서 알아보자.

먼저 **CSS의 기본적인 문법**을 살펴보면,

![스크린샷1 2023-12-14 오후 1 26 28](https://github.com/Heo-y-y/development-blog/assets/112863029/7182ed08-3b75-41f3-9162-c5e57f9d812d)

CSS는 크게 Selector와 스타일로 구성되어 있다. 위 그림에 나온 것처럼 Selector를 먼저 쓰고, 이후에 적용할 스타일을 중괄호 안에 세미콜론(;)으로 구분하여 하나씩 기술한다.

Selector에는 해당 스타일이 적용될 HTML element를 넣게 된다. 위 그림처럼 HTML 태그를 직접 넣거나 다른 조건들을 조합하여 Selector를 작성할 수 있다.

Selector 다음에는 중괄호 안에 적용할 스타일을 선언하게 된다. 각 스타일은 CSS 속성과 값으로 이뤄진 키, 값 쌍이며 CSS 속성의 이름과 값을 콜론(:)으로 구분한다. 그리고 중괄호로 묶여있는 하나의 스타일 블록에는 스타일이 여러 개가 들어갈 수 있는데, 여기에서 각 스타일은 세미콜론(;)으로 구분한다. 

### Selector의 유형

- **Element Selector**
    
    Element Selector는 단순하게 특정 HTML 태그를 선택하기 위해 쓴다. 이 경우에는 선택자에 HTML 태그의 이름을 써주면 된다.
    
    **Element Selector를 사용한 예시**
    
    ```css
    h1 {
        color: green;
    }
    ```
    
    위 코드는 h1 태그의 글자 색깔을 녹색으로 바꾸기 위한 CSS 속성이다. 여기서 Element Selector를 사용한 것을 볼 수 있다.
    
- **ID Selector**
    
    HTML에서는 element의 ID를 정의할 수 있는데, ID Selector는 이 ID를 기반으로 선택하는 형태이며, # 뒤에 ID를 넣어 사용한다.
    
    **ID Selector를 사용한 예시**
    
    ```css
    <div id="section">
        ...
    </div>
    ```
    
    ```css
    #section {
        background-color: black;
    }
    ```
    
    위 코드는 `id`가 `section`인 element를 정의한 HTML 코드와 해당 element의 배경 색깔을 검은색으로 바꾸기 위한 CSS 속성이다.
    
- **Class Selector**
    
    id는 고유하다는 성질을 갖고 있기 때문에 하나의 element에 사용해야 하지만, 클래스는 여러 개의 element를 분류하기 위해 사용한다. 영어로는 classification이라고 한다. 클래스 선택자는 점(.) 뒤에 클래스명을 넣어서 사용한다. 
    
    **Class Selector를 사용한 예시**
    
    ```css
    <span class="medium">
        ...
    </span>
    
    <p clzss"=medium">
        ...
    </p>
    ```
    
    ```css
    .medium {
        font-size: 20px;
    }
    
    p.medium {
        font-size: 20px;
    }
    ```
    
    위 코드는 클래스가 `medium`인 element들을 정의한 HTML 코드와 해당 element의 글자 크기를 `20px`로 바꾸기 위한 CSS 속성이다.
    
    여기에서 두번째에 나오는 스타일의 selector는 `p.medium`이라고 되어 있다. 이것은 Element Selector와 Class Selector를 함께 사용한 것인데, 해당 HEML 태그에 클래스가 있는 경우에만 클래스가 적용된다. 이렇게 사용하고 싶을 경우 HTML 태그명 뒤에 점(.)을 찍고 클래스 이름을 넣어주면 된다.
    
    **Element Selector와 Class Selector를 함께 사용한 예시**
    
    ```css
    h1.medium {
        font-size: 20px;
    }
    ```
    
    예를 들어 `h1` 태그에 클래스가 `medium`인 경우에만 스타일을 적용하고 싶다면 selector는 `h1.medium`이 된다.
    
- **Universal Selector**
    
    Universal이라는 단어는 전세계적인, 보편적인 이라는 뜻을 가지고 있다. Universal Selector는 의미 그대로 특정 element에만 선택적으로 적용하는 것이 아니라 전체 element에 적용하기 위한 selector이며 한국에서는 흔히 별표라고 부르는 `*`를 사용한다.
    
    **Universal Selector를 사용한 예시**
    
    ```css
    * {
        font-size: 20px;
        color: blue;
    }
    ```
    
    위 코드는 모든 element의 글자 크기를 20px로 하고 글자 색깔을 파란색으로 바꾸기 위한 CSS 속성이다.
    
- Grouping Selector
    
    Grouping은 그룹으로 묶는다는 뜻을 가지고 있다. Grouping Selector는 말 그대로 여러가지 선택자를 그룹으로 묶어 하나의 스타일을 적용하기 위해 사용하는 selector이다.
    
    **예시 코드**
    
    ```css
    h1 {
        color: black;
        text-align: center;
    }
    
    h2 {
        color: black;
        text-align: center;
    }
    
    p {
        color: black;
        text-align: center;
    }
    ```
    
    만약 위 코드와 같이 똑같은 스타일들이 각기 다른 selector로 나눠져 있다고 가정하자. 이렇게 반복되는 것을 여러 곳에 나눠서 쓰게 되면 유지 보수도 힘들고, 굉장히 비효율적일 것이다.
    
    **Grouping Selector를 사용한 예시**
    
    ```css
    h1, h2, p {
        color: black;
        text-align: center;
    }
    ```
    
    그래서 Grouping Selector를 사용하여 위 코드와 같이 바꿔주면 굉장히 간결해지고 유지 보수도 쉬워지게 된다. 이처럼 같은 스타일을 여러 조건의 선택자에 적용하고 싶을 때에는 각 선택자를 콤마(,)로 구분하여 Grouping Selector를 적용하면 된다.
    
- **Element의 상태와 관련된 Selector**
    
    여기서 상태라는 것은 마우스 커서가 element 위에 올라오거나 element가 활성화되어 있는 경우 등을 의미한다.
    
    - **상태와 관련된 대표적인 selector**
        - **:hover**
            - 마우스 커서가 element 위에 올라왔을 떄
        - **:active**
            - 주로 `<a>` 태그(link)에 사용되는데, element가 클릭됐을 때를 의미
        - **:focus**
            - 주로 `<input>` 태그에서 사용되는데, element가 초점을 가지고 있을 경우를 의미
        - **:checked**
            - radio button이나 chekbox 같은 유형의 `<input>` 태그가 체크되어 있는 경우를 의미
        - **:first-child, :last-child**
            - 상위 element를 기준으로 각각 첫 번째 child, 마지막 child일 경우를 의미
    
    **상태와 관련된 selector를 사용한 예시**
    
    ```css
    button:hover {
        font-weight: bold;
    }
    
    a:active {
        color: red;
    }
    
    input:focus {
        color: #000000;
    }
    
    option:checked {
        background: #00ff00;
    }
    
    p:first-child {
        background: #ff0000;
    }
    
    p:last-child {
        background: #0000ff;
    }
    ```
    
    위 코드는 상태와 관련된 selector들을 사용하는 예시이다.
    

## 레이아웃과 관련된 CSS 속성

### Layout

일반적으로 Layout이라고 하면 정해진 공간에 가구나 물건들을 어떻게 배치할지를 의미한다. 웹 사이트에서도 Layout이 비슷한 의미로 사용되는데, 화면에 element들을 어떻게 배치할 것인지를 의미한다. 그래서 Layout과 관련된 CSS 속성들은 화면상의 배치와 관련이 있다고 생각하면 된다.

### Display

Layout과 관련해서 가장 중요한 속성은 Display이다. Display 속성은 element를 어떻게 표시할지 그 방법에 관한 속성이다.

**Display 속성**

```jsx
div {
    display: none | block | inline | flex;
}
```

모든 element는 그 종류에 따라서 기본적으로 정해진 Display 속성 값을 가지고 있다. 대부분의 element는 block 또는 inline 값을 가진다.

Display 속성의 값으로는 굉장히 많은 종류가 있는데, 대표적으로 위 코드에 나온 값들이 가장 많이 사용된다.

**Display 속성의 대표적인 값들**

- **display: none;**
    - element를 화면에서 숨기기 위해 사용
    - `<script>` 태그의 display 속성 기본값은 display: none;
- **display: block;**
    - 블록 단위로 element를 배치
    - `<p>`, `<div>`, `<h1> ~ <h6>` 태그의 display 속성 기본값이 display: block;
- **display: inline;**
    - element를 라인 안에 넣는 것
    - `<span>` 태그의 display 속성 기본값이 display: inline;
- **display: flex;**
    - element를 블록 레벨의 flex container로 표시
    - container이기 때문에 내분에 다른 element들을 포함

### Visibility

Visibility는 우리말로 눈에 잘 보이는 성질, 가시성이라는 뜻을 가지고 있다.

**Visibility 속성**

```css
div {
    visibility: visible | hidden;
}
```

CSS에서는 element를 화면에 보여주거나 감추기 위해 사용하는 속성이다. 대표적으로 visible과 hidden 이 두가지 값을 가장 많이 쓴다.

**Visibility 속성의 대표적인 값들**

- **visibility: visible;**
    - element를 화면에 보이게 하는 것
- **visibility: hidden;**
    - 화면에서 안 보이게 감추는 것
    - element를 안보이게만 하는 것이고, 화면에서의 영역은 그대로 차지

### Position

Position 속성은 element를 어떻게 위치시킬 것인지 즉, 포지셔닝을 어떻게 할 것인지를 정의하기 위해서 사용한다.

**Position 속성**

```css
div {
    position: static | fixed | relative | absolute;
}
```

**Position 속성의 대표적인 값들**

- **static**
    - 기본값으로 element를 원래의 순서대로 위치시킴
- **fixed**
    - element를 브라우저 window에 상대적으로 위치시킴
- **relative**
    - element를 보통의 위치에 상대적으로 위치시킴
- **absolute**
    - element를 절대 위치에 위치시킴

### 가로, 세로 길이와 관련된 속성들

```css
div {
    width: auto | value;
    height: auto | value;
    min-width: auto | value;
    min-height: auto | value;
    max-width: auto | value;
    max-height: auto | value;
}
```

위 속성들은 값으로 보통 실제 픽셀 값을 넣거나 상대값인 `%`를 사용한다. 또한 픽셀 단위가 아닌 m, rem 등의 단위도 사용할 수 있다.

값으로 `auto`를 사용하면 브라우저에서 길이를 계산하며 실제 값을 사용하면 해당 값만큼의 길이를 가지게 된다.

### Flexbox

Layout에서 가장 많이 쓰이고 중요한 Flexbox를 알아보자.

Flexbox는 기존 CSS Layout 사용의 불편한 부분을 개선하기 위해 등장했다. Flexbox가 나오기 전에는 앞에서 설명한 display block이나 display inline 등의 속성을 활용해서 Layout을 구성했다. 하지만 이런 속성들은 다양한 Layout을 자유롭게 구성하는 데 불편한 부분이 있고, 이러한 문제를 해결하기 위해서 Flexbox가 나오게 된 것이다.

Flexbox는 크게 컨테이너와 아티템으로 구성된다.

**flex container와 flex item을 표현한 그림**

![스크린샷2 2023-12-14 오후 3 04 44](https://github.com/Heo-y-y/development-blog/assets/112863029/aafd4f50-7cbf-4c56-a701-8ba0fc945dd2)

위에서 display flex를 사용하면 element가 flex container가 된다고 말했는데, 이것이 바로 flex container가 된다. 그리고 flex container는 내부에 여러 개의 element를 포함할 수 있다.

이때 컨테이너에 포함되는 element들이 바로 Flexbox의 flex item이 된다.

컨테이너 안에 여러 개의 아이템이 존재할 경우 컨테이너에 들어있는 flex와 관련된 CSS 속성은 이 아이템들을 어떤 방향과 어떤 순서로 배치할 것인지를 정의하게 된다.

**Flex와 관련된 대표적인 속성**

```css
div {
    display: flex;
    flex-direction: row | column | row-reverse | column-reverse;
    align-items: stretch | flex-start | center | flex-end | baseline;
    justify-content: flex-start | center | flex-end | space-between| space-around;
}
```

먼저 element를 flex 컨테이너로 사용하기 위해서 display flex를 써주어야 한다. 그렇지 않으면, display 속성의 값이 flext가 아닌 element의 기본 값으로 지정되기 때문이다.

그리고 이후에는 `flex-directio` 속성을 사용하여 아이템들이 어떤 방향으로 배치될 것인지를 지정한다.

**flex-direction 속성의 대표적인 값들**

- **row**
    - 기본값이며 아이템을 행(row)을 따라 가로 순서대로 왼쪽부터 배치
- **column**
    - 아이템을 열(column)을 따라 세로 순서대로 위쪽부터 배치
- **row-reverse**
    - 아이템을 행(row)의 역(reverse)방향으로 오른쪽부터 배치
- **column-reverse**
    - 아이템을 열(column)의 역(reverse)방향으로 아래쪽부터 배치

**flex-direction 속성의 값에 따른 아이템들의 배치 순서 그림**

![스크린샷3 2023-12-14 오후 3 15 42](https://github.com/Heo-y-y/development-blog/assets/112863029/7fe32eee-3e2c-47f9-b1ef-a059de7bb14e)

아이템 배치와 관련해서 두 개의 중요한 축이 나오는데, 바로 main axis와 corss axis이다.

**main axis와 cross axis 그림**

![스크린샷4 2023-12-14 오후 3 17 09](https://github.com/Heo-y-y/development-blog/assets/112863029/86a58003-d351-4163-bb17-43a8017d71e5)

flex direction으로 지정된 방향으로 향하는 축을 main axis라고 부르고, main axis를 가로지르는 방향으로 향하는 축을 가로지른다는 의미에서 cross axis라고 한다.

이 main axis와 cross axis의 방향에 따라서 aline items와 justify content 속성의 값의 의미가 달라지게 된다.

### align-items

align-items는 말 그대로 컨테이너 내에서 아이템을 어떻게 정렬, align 할 것인지를 결정한다. 이때 정렬은 cross axis 기준으로 하게 된다.

**align-items 속성의 대표적인 값들**

- **stretch**
    - 기본값으로써 아이템을 늘려서(stretch) 컨테이너를 가득 채움
- **flex-start**
    - cross axis의 시작 지점으로 아이템을 정렬
- **center**
    - cross axis의 중앙으로 아이템을 정렬
- **flex-end**
    - cross axis의 끝 지점으로 아이템을 정렬
- **baseline**
    - 아이템을 baseline 기준으로 정렬

**align-items 속성의 값에 따른 아이템의 정렬 결과**

![스크린샷5 2023-12-14 오후 3 24 15](https://github.com/Heo-y-y/development-blog/assets/112863029/50aeb76c-435b-49c2-816c-98774f4374dc)

### justify-content

justify-content는 컨테이너 내에서 아이템들을 어떻게 나란히 맞출 건인지를 결정한다. 이때 맞추는 기준은 align items와 반대로 main axis를 기준으로 하게 된다.

**justify-content 속성의 대표적인 값들**

- flex-start
    - main axis의 시작 지점으로 아이템을 맞춤
- center
    - main axis의 중앙으로 아이템을 맞춤
- flex-end
    - main axis의 끝 지점으로 아이템을 맞춤
- space-between
    - main axis를 기준으로 첫 아이템은 시작 지점에 맞추고 마지막 아이템은 끝 지점에 맞추며, 중간에 있는 아이템들 사이(between)의 간격(space)이 일정하게 되도록 맞춤
- space-around
    - main axis를 기준으로 각 아이템의 주변(around) 간격(space)을 동일하게 맞춤

**justify-content 속성의 값에 따른 아이템들의 정렬 결과**

![스크린샷6 2023-12-14 오후 3 28 54](https://github.com/Heo-y-y/development-blog/assets/112863029/81f0d22c-68bd-4f4a-9cbe-544ebef7e10d)

## Font와 관련된 CSS 속성, 기타 많이 사용하는 CSS 속성

**font와 관련된 대표적인 속성**

```css
#title {
    font-family: "사용할 글꼴 이름", <일반적인 글꼴 분류>;
    font-size: value;
    font-weight: normal | bold;
    font-style: normal | italic | oblique;
}
```

### font-family

font-family는 어떤 글꼴을 사용할 것인지를 결정하는 속성이다.

font-family 속성의 값으로는 사용할 글꼴의 이름을 적어주면 되는데, 여기서 주의할 점은 글꼴의 이름에 띄어쓰기가 들어갈 경우 `“”`로 묶어 주어야 한다.

**font-family 속성 사용 예시**

```css
#title1 {
    font-family: "Times New Roman", Times, serif;
}

#title2 {
    font-family: Arial, Verdana, sans-serif;
}

#title3 {
    font-family: "Courier New", Monaco, monospace;
}
```

그런데 여기서 사용할 글꼴의 이름이 하나가 아니라 `,` 로 구분하여 여러 개의 글꼴이 쓰여있는 것을 볼 수 있다. 이것은 `font-family` 속성의 펄백 시스템 때문이다. 펄백이라는 단어는 대비책이라는 뜻을 가지고 있다. 따라서 `font-family` 속성에서의 펄백 시스템은 지정한 글꼴을 찾지 못했을 경우를 대비하여 사용할 글꼴을 순서대로 지정해 주는 것이다.

이것은 최대한 많은 브라우저와 운영체제에서 글자가 깨지지 않고, 나올 수 있도록 하기 위함이다.

**일반적인 글꼴 분류 - Generic font family**

- **serif**
    - 각 글자의 모서리에 작은 테두리를 갖고 있는 형태의 글꼴
- **sans-serif**
    - 모서리에 테두리가 없이 깔끔한 선을 가진 글꼴
    - 컴퓨터 모니터에서는 serif보다 가독성이 좋음
- **monospace**
    - 모든 글자가 같은 가로 길이를 가지는 글꼴. 코딩에서 주로 사용
- **cursive**
    - 사람이 쓴 손글씨 모양의 글꼴
- **fantasy**
    - 장식이 들어간 형태의 글꼴

이처럼 font-family 속성을 사용할 때 사용하고 싶은 글꼴의 이름을 `,` 로 분류하여 순서대로 적고, 가장 마자막에 일반적인 글꼴 분류를 적어준다. 이렇게 하는 이유는 만약 사용하고자 하는 글꼴들이 모두 없을 경우 브라우저가 일반적인 글꼴 분류에서 가장 유사한 글꼴을 선택하도록 하기 위함이다.

### font-size

font-size는 글꼴의 크기와 관련된 CSS 속성이다.

**font-size의 값**

- px
- em
- rem
- vw

픽셀은 고정된 값이기 때문에 브라우저를 통해 크기를 바꿀 수 없지만 m이라는 단위는 사용자가 브라우저에서 글꼴의 크기를 변경할 수 있게 해준다. 브라우저의 기본 글꼴 크기 1m은 16픽셀과 동일하다. 그래서 아래와 같은 공식이 성립한다.

```css
16 * em = pixels
```

**font-size 속성 사용예시**

```css
#title1 {
    font-size: 16px;
}

#title2 {
    font-size: lem;
}

#title3 {
    font-size: 10vw; /* viewport 가로 길이의 10%를 의미 */
}
```

### font-weight

font-weight은 글꼴의 두께와 관련된 속성이다.

값으로는 normal, bold를 사용하거나 100 ~ 900까지의 100 단위의 숫자로 된 값을 사용할 수 있다. 숫자가 클수록 글자의 두께가 두꺼워진다.

**font-weight 속성 사용 예시**

```css
#title1 {
    font-weight: bold;
}

#title2 {
    font-weight: 500;
}
```

### font-style

font style은 글꼴의 스타일을 지정하기 위한 속성이다.

**font-style 속성의 값들**

- **normal**
    - 일반적인 글자의 형태를 의미
- **italic**
    - 글자가 기울어진 형태로 나타남
    - 별도로 기울어진 형태의 글자들을 직접 디자인해서 만든 것
- **oblique**
    - 글자가 비스듬한 형태로 나타남
    - 그냥 글자를 기울인 것

**font-style 속성 사용 예시**

```css
#title1 {
    font-style: itlic;
}

#title2 {
    font-style: oblique;
}
```

### background-color

background-color는 element의 배경색을 지정하기 위한 속성이다.

값으로 색상의 값이 들어가게 되는데 CSS에서 색상 값으로 사용할 수 있는 것과 예제 값들은 아래와 같다.

**CSS의 색상 값**

- **16진수 컬러 값**: #ff0000
- **투명도를 가진 16진수 컬러 값**: #ff000055
- **RGB 컬러 값**: rgb(255, 0, 0)
- **RGBA 컬러 값**: rgba(255, 0, 0, 0.5)
- **HSL 컬러 값**: hsl(120, 100%, 25%)
- **HSLA 컬러 값**: hsla(120, 100%, 50%, 0.3)
- **미리 정의된 색상의 이름**: red
- **currentcolor 키워드**: 현재 지정된 생상 값을 사용

**background-color 속성 사용 예시**

```css
div {
    background-color: color | transparent;
}
```

element 배경색을 지정하기 위해서는 `background-color` 속성의 값으로 `color` 값을 넣어주면 된되고, 투명하게 만들고 싶으면 `transparent` 키워드를 넣어주면 된다.

### border

border 속성은 테두리를 위한 속성이다.

**border 속성 사용 예시**

```css
div {
    border: border-width border-style border-color;
}
```

**background-color 와 border 속성 실제 사용 예시**

```css
#title1 {
    background-color: red;
}

#title2 {
    border: 1px solid black;
}
```

## styled-components

**styled-components**는 **CSS 문법을 그대로 사용하면서 결과물을 스타일링된 컴포넌트 형태로 만들어주는 오픈소스 라이브러리**이다.

**컴포넌트 개념을 사용**하기 때문에 **리액트와 굉장히 궁합이 잘 맞**으며 전세계적으로 리액트 개발에 굉장히 많이 사용되고 있다.

**styled-components 설치**

```css
# npm을 사용하는 경우 
npm install --save styled-components

# yarn을 사용하는 경우
yarn add styled-components
```

### styled-components의 기본 사용법

**tagged template literal**

styled-components는 tagged template literal을 사용하여 구성요소의 스타일을 지정한다. 여기서 template literal이라는 것이 등장하는데 이것은 자바스크립트에서 제공하는 문법 중 하나이다.

먼저 **literal**에 대해 알아보자.

프로그래밍에서 **literal**은 **소스 코드에 고정된 값**을 의미한다. 흔히 상수와 헷갈리는 경우가 잇는데 둘은 다른 개념이다.

**예제 코드**

```jsx
let number = 20;
```

위 코드를 보면 대입 연산자의 왼쪽에는 `number`라는 변수가 등장하고 오른쪽에는 정수 20이 등장한다. 여기서 오른쪽에 있는 정수 20이 바로 **literal**이다.

**소스 코드 상에 있는 고정된 값을 의미**하는 것이다.

**다른 종류의 literal**

```jsx
// 정수 리터럴 (Integer literal)
const myNumber = 10;

// 문자열 리터럴 (String literal)
const myStr = 'Hello';

// 배열 리터럴 (Array literal)
const myArray = [];

// 객체 리터럴 (Object literal)
const myObject = {};
```

변수를 선언할 때 `var`나 `let`을 사용하지 않고, 상수를 의미하는 `const`를 사용했는데, 이렇게 선언하게 되면 해당 변수들이 모두 상수가 된다.

**상수**는 변하지 않는 수를 의미하는데 **한 번 선언된 이후에는 값을 바꿀 수 없는 것**이다.

그리고 **대입 연산자의 오른쪽에 있는 값들이 모두 리터럴**이 된다.

그렇다면 **template literal**은 뭘까?

template literal은 말 그대로 literal을 template 형태로 사용하는 자바스크립트 문법인데, 우리가 흔히 역다운표라고 부르는 백틱스(`)를 사용하여 문자열을 작성하고 그 안에 대체 가능한 expression을 넣는 방법이다. 여기서 이 expression을 대체라는 뜻을 가진 substiution이라고 부른다.

template literal은 또 크게 untagged template literal과 tagged template literal로 나뉜다.

**Untagged template literal과 Tagged Template literal**

```jsx
// Untagged template literal
// 단순한 문자열
`string text`

// 여러 줄(Multi-line)에 걸친 문자열
`string text line 1
 string text line 2`

 // 대체 가능한 expression이 들어있는 문자열
 `string text ${expression} string text`

 // Tagged template literal
 // myFunction의 파라미터로 expression으로 구분된 문자열 배열과 expresstion이 순서대로 들어간 형태로 호출됨
 myFunction`string text ${expression} string text`
```

코드에서 보이는 것처럼 Untagged template literal은 보통 문자열을 여러 줄에 걸쳐서 작성하거나, 포매팅을 하기 위해서 사용한다.

그리고 Tagged Template literal은 앞에 나와 있는 태그 Function을 호출하여 결과를 리턴한다. 여기서 Tag Function의 파라미터는 expression으로 구분된 문자열 배열과 expression이 순서대로 들어가게 된다.

**Tagged Template literal을 사용한 예제 코드**

```jsx
const name = '윤영';
const region = '서울';

function myTagFunction(string, nameExp, regionExp) {
    let str0 = string[0]; // "제 이름은 "
    let str1= string[1]; // "이고, 사는 곳은 "
    let str2 = string[2]; // "입니다."

    // 여기에서도 template literal을 사용하여 리턴할 수 있음
    return `${str0}${nameExp}${str1}${region}${str2}`;
}

const output = myTagFunction`제 이름은 ${name}이고, 사는 곳은 ${region}입니다.`;

// 출력 결과
// 제 이름은 윤영이고, 사는 곳은 서울입니다.
console.log(output);
```

`myTagFunction`이라는 이름의 TagFunction에 파라미터와 expression이 각각 어떻게 들어가는지 쉽게 파악된다.

이처럼 Tagged Template literal을 사용하면 문자열과 expression을 TagFunction의 파라미터로 넣어 호출한 결과를 받게 된다.

그러면 다시 styled-components로 돌아와 보자.

**styled-components 사용 예시**

```jsx
import React from "react";
import styled from "styled-components";

const Wrapper = styled.div`
    padding: lem;
    background: grey;
`;
```

**styled-components**는 **Tagged Template literal을 사용하여 CSS 속성이 적용된 리액트 컴포넌트를 만들어준다**. styled-components를 사용하는 기본적인 방법은 위와 같이 배틱스(`)으로 둘러싸인 문자열 부분에 CSS 속성을 넣고 태그 함수 위치에는 `styled.html` 태그 형태로 사용한다.

이렇게 하면 해당 element 태그에 CSS 속성들이 적용된 형태의 리액트 컴포넌트가 만들어진다.

styled-components에서는 조건이나 동적으로 변하는 값을 사용해서 스타일링 할 수는 없을까?

물론 가능하다 이것을 위해 제공하는 기능이 바로 **props**이다.

### styled-components의 props 사용하기

리액트 컴포넌트의 props와 같은 개념으로 이해하면 된다.

**예제 코드**

```jsx
mport React from "react";
import styled from "styled-components";

const Button = styled.button`
    color: ${props => props.dark ? "white" : "dark"};
    background: ${props => props.dark ? "black" : "white"};
    border: 1px solid black;
`;

function Sample(props) {
    return (
        <div>
            <Button>Normal</Button>
            <Button dark>Dark</Button>
        </div>
    )
}

export default Sample;
```

위 코드에서 styled-components를 사용하는 부분의 CSS 속성을 보면 내부에 `props`가 사용된 것을 볼 수 있다. 여기에서의 `props`는 해당 컴포넌트에 사용된 `props`를 의미한다.

따라서 실제 `Button` 컴포넌트를 사용하는 부분의 코드를 보면 `props`로 `dark`를 넣어주는 것을 볼 수 있다. 그리고 이렇게 들어간 `props`는 그대로 styled-components에 전달된다.

이 기능을 사용하면 styled-components를 사용하여 다양한 스타일을 자자재로 구현할 수 있다.

앞에서 styled-components를 사용하면 리액트 컴포넌트가 생성된다고 설명했다. 그렇다면 이렇게 생성된 컴포넌트를 기반으로 추가적인 스타일을 적용하고 싶을 경우에는 어떻게 할까?

styled-components에서는 이를 위한 스타일 확장 기능을 제공한다.

### styled-components의 스타일 확장하기

**예제 코드**

```jsx
// Button 컴포넌트
const Button = styled.button`
    color: grey;
    border: 2px solid palevioletred;
`;

// Button에 style이 추가된 RoundedButton 컴포넌트
const RoundedButton = styled(Button)`
    border-radius: 16px;
`;

function Sample(props) {
    return (
        <div>
            <Button>Normal</Button>
            <RoundedButton>Rounded</RoundedButton>
        </div>
    )
}

export default Sample;
```

먼저 `Button` 컴포넌트는 HTML의 `Button` 태그를 기반으로 만들어진 단순한 버튼이다.

그리고 `RoundedButton` 컴포넌트를 만드는 부분을 자세히 보면 HTML 태그가 빠져 있고, Button 괄호로 둘러싸인 채로 들어가 있는 것을 볼 수 있다. 이것이 바로 다른 컴포넌트의 스타일을 확장해서 사용하는 부분이다.

`RoundedButton` 컴포넌트는 `Button` 컴포넌트의 모서리를 둥글게 만든 컴포넌트이다.

**참고 자료**

- [https://www.inflearn.com/course/lecture?courseSlug=처음-만난-리액트&unitId=113248&tab=curriculum](https://www.inflearn.com/course/lecture?courseSlug=%EC%B2%98%EC%9D%8C-%EB%A7%8C%EB%82%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8&unitId=113248&tab=curriculum)
