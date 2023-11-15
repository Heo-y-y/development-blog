# **PAINTING BOARD**

이번에는 만들어 놓은 그림판을 클릭할 경우 여러가지 색이 랜덤으로 선이 만들어지는 작업을 경험해봤다.

```jsx
const canvas = document.querySelector("canvas");
const ctx = canvas.getContext("2d");
canvas.width = 800;
canvas.height = 800;
ctx.lineWidth = 2;

// 실제 색
const colors = [
    "#ff3838",
    "#ffb8b8",
    "#c56cf0",
    "#ff9f1a",
    "#fff200",
    "#32ff7e",
    "#7efff5",
]

function onClick(event){
    ctx.beginPath();
		// 어디서부터 시작될지 설정
    ctx.moveTo(0, 0);
		// color 변수 색상 랜덤 적용
    const color = colors[Math.floor(Math.random() * colors.length)];
    ctx.strokeStyle = color;
		// 실제 마우스 위치 좌표
    ctx.lineTo(event.offsetX, event.offsetY);
    ctx.stroke();
}

canvas.addEventListener("mousemove", onClick)
```

![스크린샷 2023-11-15 오후 10 33 58](https://github.com/Heo-y-y/development-blog/assets/112863029/b17ef69d-b6a3-4339-94c4-06fe81534472)

마우스를 클릭 후 0, 0 좌표를 시작으로 마우스의 위치만큼 색상이 랜덤으로 선을 만든다.

이번에는 유저가 클릭을 할 때 선을 만들어주는 작업을 해보자.

```jsx
let isPainting = false;

function onMove(event) {
    if (isPainting) {
        ctx.lineTo(event.offsetX, event.offsetY);
        ctx.stroke();
        return;
    }
    ctx.moveTo(event.offsetX, event.offsetY);
}
function startPainting() {
    isPainting = true;
}
function cacelPainting() {
    isPainting = false;
}

canvas.addEventListener("mousemove", onMove);
canvas.addEventListener("mousedown", startPainting);
canvas.addEventListener("mouseup", cacelPainting);
canvas.addEventListener("mouseleave", cacelPainting);
```

`startPainting()`과 `cacelPainting()`를 만들어 각 상황에 맞게 `true, false`를 반환시킨다.

그리고 `addEventListener()` 각 상황에 맞는 값을 넣어주고 동작하게 한다.

이렇게 하면 실제 그림판 처럼 마우스를 클릭하고 있는 경우에만 그림을 그릴 수 있다!

![스크린샷 2023-11-15 오후 10 34 05](https://github.com/Heo-y-y/development-blog/assets/112863029/37793201-6bd3-470b-be2e-61acb24e2ede)

이제 여기서 조금 업그레이드 시켜보자.

선의 굵기가 일정한데 선 굵기를 조절할 수 있는 line-Wdith을 추가하면,

```html
<input id="line-width" type="range" min="1" max="10" value="5" step="0.5" />
```

```jsx
const lineWidth = document.getElementById("line-width");

function onLineWidthChange(event) {
    ctx.lineWidth = event.target.valu
}

lineWidth.addEventListener("change", onLineWidthChange);
```

코드 결과를 보면 선 굵기를 다양하게 조절할 수 있는 것을 볼 수 있다.

![스크린샷 2023-11-15 오후 10 34 14](https://github.com/Heo-y-y/development-blog/assets/112863029/3c9565da-e1f1-4b27-b1d2-c52ae62aae6a)

HTML 코드에서 보면 `step`을 이용해 선 굵기를 0.5 단위로 조절할 수 있게 설정했다.

![스크린샷 2023-11-15 오후 10 34 46](https://github.com/Heo-y-y/development-blog/assets/112863029/8ae07970-860b-40e2-a583-d1ef750c5f72)

그다음에는 색상을 변경할 수 있게 HTML에 input을 추가하자.

```jsx
<input type="color" id="color" />
```

```jsx
const color = document.getElementById("color");

function onColorChange(event) {
    ctx.strokeStyle = event.target.value;
    ctx.fillStyle = event.target.value;
}

color.addEventListener("change", onColorChange);
```

![스크린샷 2023-11-15 오후 10 34 46](https://github.com/Heo-y-y/development-blog/assets/112863029/1fd75810-05b2-4e7c-9c54-2e9008b6489c)

위 그림처럼 색 별로 그림을 그릴 수 있게 됐다.

이제는 특정 색들을 클릭하여 선택할 수 있게 만들어보겠다.

```html
<input type="color" id="color" />
    <div 
     class="color-option"
     style="background-color: #1abc9c"
     data-color="#1abc9c"
    ></div>
    <div
     class="color-option"
     style="background-color: #3498db"
     data-color="#3498db"
    ></div>
    <div
     class="color-option"
     style="background-color: #34495e"
     data-color="#34495e"
    ></div>
    <div
     class="color-option"
     style="background-color: #27ae60"
     data-color="#27ae60"
    ></div>
    <div
     class="color-option"
     style="background-color: #8e44ad"
     data-color="#8e44ad"
    ></div>
    <div
     class="color-option"
     style="background-color: #f1c40f"
     data-color="#f1c40f"
    ></div>
    <div
     class="color-option"
     style="background-color: #e74c3c"
     data-color="#e74c3c"
    ></div>
    <div
     class="color-option"
     style="background-color: #95a5a6"
     data-color="#95a5a6"
    ></div>
    <div
     class="color-option"
     style="background-color: #d35400"
     data-color="#d35400"
    ></div>
    <div
     class="color-option"
     style="background-color: #bdc3c7"
    data-color="#bdc3c7"
    ></div>
    <div
     class="color-option"
     style="background-color: #2ecc71"
     data-color="#2ecc71"
    ></div>
    <div
     class="color-option"
     style="background-color: #e67e22"
     data-color="#e67e22"
    ></div>
```

```css
.color-option {
    width:50px;
    height:50px;
    cursor: pointer;
}
```

```jsx
const colorOptions = Array.from(
    document.getElementsByClassName("color-option")
);

function onColorClick(event) {
    const colorValue = event.target.dataset.color;
    ctx.strokeStyle = colorValue;
    ctx.fillStyle = colorValue;
    color.value = colorValue;
}

colorOptions.forEach(color => color.addEventListener("click", onColorClick));
```

코드를 실행시키면, 해당 색을 클릭하면 현재 색으로 바뀐다.

![스크린샷 2023-11-15 오후 10 34 58](https://github.com/Heo-y-y/development-blog/assets/112863029/cc41b595-7952-4057-a0ee-a7bfe8b63ddc)

그러면 이제 클릭하는 색과 선택된 색상을 나타내는 것을 세로로 바꾸고, Fill, Draw의 버튼을 만들어서 바탕색과 그림의 색상을 같이 적용할 수 있게 만들어보겠다.

```html
<canvas></canvas>
    <input id="line-width" type="range" min="1" max="10"
    value="5" step="0.5" />
    <div>
        <input type="color" id="color" />
        <div 
        class="color-option"
        style="background-color: #1abc9c"
        data-color="#1abc9c"
       ></div>
       <div
        class="color-option"
        style="background-color: #3498db"
        data-color="#3498db"
       ></div>
       <div
        class="color-option"
        style="background-color: #34495e"
        data-color="#34495e"
       ></div>
       <div
        class="color-option"
        style="background-color: #27ae60"
        data-color="#27ae60"
       ></div>
       <div
        class="color-option"
        style="background-color: #8e44ad"
        data-color="#8e44ad"
       ></div>
       <div
        class="color-option"
        style="background-color: #f1c40f"
        data-color="#f1c40f"
       ></div>
       <div
        class="color-option"
        style="background-color: #e74c3c"
        data-color="#e74c3c"
       ></div>
       <div
        class="color-option"
        style="background-color: #95a5a6"
        data-color="#95a5a6"
       ></div>
       <div
        class="color-option"
        style="background-color: #d35400"
        data-color="#d35400"
       ></div>
       <div
        class="color-option"
        style="background-color: #bdc3c7"
       data-color="#bdc3c7"
       ></div>
       <div
        class="color-option"
        style="background-color: #2ecc71"
        data-color="#2ecc71"
       ></div>
       <div
        class="color-option"
        style="background-color: #e67e22"
        data-color="#e67e22"
       ></div>
    </div>
    <button id="mode-btn">Fill</button>
```

버튼을 만들고, `div`로 `color`를 한번 더 감싸준다.

그리고 JS에 적용한다.

```jsx
const modeBtn = document.getElementById("mode-btn");

function onModeClick() {
    if(isFilling) {
        isFilling = false
        modeBtn.innerText = "Fill";
    } else {
        isFilling = true
        modeBtn.innerText = "Draw";
    }
}

function onCanvasClick() {
    if (isFilling) {
        ctx.fillRect(0, 0, 800, 800);
    }
}
color.addEventListener("change", onColorChange);

modeBtn.addEventListener("click", onModeClick);
```

![스크린샷 2023-11-15 오후 10 35 22](https://github.com/Heo-y-y/development-blog/assets/112863029/7c0ad7f8-4aa9-46de-a73b-a7722951f861)

 Fill(바탕)과 Draw(선)를 선택하여 색을 지정할 수 있다.

이제 전체 지우기와 지우개 기능을 만들어보겠다.

먼저 `Destory`버튼과 `Eraser`버튼을 2개 만들고, `Destory` 는 전체를 흰색으로 채우고, `Eraser`는 `isFilling`을 `false`로 두고, `Draw`로 설정과 흰색으로 적용했다.

```html
<button id="destory-btn">Destory</button>
<button id="eraser-btn">Eraser</button>
```

```jsx
const destoryBtn = document.getElementById("destory-btn");
const eraserBtn = document.getElementById("eraser-btn");

const CANVAS_WIDTH = 800;
const CANVAS_HEIGHT = 800;

canvas.width = CANVAS_WIDTH;
canvas.height = CANVAS_HEIGHT;

function onDestoryClick() {
    ctx.fillStyle = "white";
    ctx.fillRect(0, 0, CANVAS_WIDTH, CANVAS_HEIGHT);
}

function onEraserClick() {
    ctx.strokeStyle = "white";
    isFilling = false;
    modeBtn.innerText = "Draw";
}

destoryBtn.addEventListener("click", onDestoryClick);
eraserBtn.addEventListener("click", onEraserClick);
```

![스크린샷 2023-11-15 오후 10 35 32](https://github.com/Heo-y-y/development-blog/assets/112863029/db863fa8-3f27-465d-b4ce-6551f99d93e0)

**참고 자료**

- <https://nomadcoders.co/?gad_source=1&gclid=CjwKCAiA9dGqBhAqEiwAmRpTC-cZUkTXw06mRgUb66aphBQDg53RXl3UK4UEsCfgkiQjhv5559r5thoCbDEQAvD_BwE>
