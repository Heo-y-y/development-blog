# **MEME MAKER**

이번에는 이미지 파일을 업로드 하는 작업을 진행보려고 한다.

먼저 파일 `input`을 HTML에 만들어준다.

```html
<input type="file" accept="image/*" id="file" />
```

우선 `type`은 `file`로 설정하고, `accept`로 접근 권한을 설정한다.

그리고 JS에 코드를 추가해준다.

```jsx
const fileInput = document.getElementById("file");

function onFileChange(event) {
    const file = event.target.files[0];
    const url = URL.createObjectURL(file);
    const image = new Image()
    image.src = url;
    image.onload = function() {
        ctx.drawImage(image, 0, 0, CANVAS_WIDTH, CANVAS_HEIGHT);
        fileInput.value = null
    }
}

fileInput.addEventListener("change", onFileChange);
```

HTML에서 설정한 `id`를 넣어주고, `addEventListener`에서 사용 하는 `onFileChange`를 정의해준다.

여기서 `URL.createObjectURL(file)`를 통해 이미지의 `url`을 받아온다.

`drawImage`를 통해 이미지의 위치와 크기를 설정한다.

![스크린샷 2023-11-16 오후 4 27 24](https://github.com/Heo-y-y/development-blog/assets/112863029/ff7da4ba-7cca-4e32-898c-d700f5ef527d)

![스크린샷 2023-11-16 오후 4 27 36](https://github.com/Heo-y-y/development-blog/assets/112863029/0f52bdf5-85cd-4143-a45c-f811569eb65b)

이제는 텍스트를 작성하여 그림판에 더블클릭을 하면 작성한 텍스트가 해당 위치에 생성되는 작업을 해보자.

마찬가지로 HTML에 `input`을 만들어 준다.

```html
<input type="text" placeholder="Write and then douvle click" id="text" />
```

그리고 만든 `text`를 JS에 적용시킨다.

```jsx
const textInput = document.getElementById("text");

function onDoubleClick(event) {
    const text = textInput.value;
    if (text !== "") {
     ctx.save(); // 현재 상태 저장
     ctx.lineWidth = 1;
     ctx.font = "68px 'Press Start 2P"; // 폰트 스타일
     ctx.fillText(text, event.offsetX, event.offsetY);
     ctx.restore(); // 저장한 상태로 되돌린다.
    }
} 

canvas.addEventListener("dblclick", onDoubleClick);
```

![스크린샷 2023-11-16 오후 4 27 45](https://github.com/Heo-y-y/development-blog/assets/112863029/947d56d3-a1cf-4c19-9263-6327b4e6d593)

그리고 현재 선은 엄청 날카롭다.

조금더 둥글게 선을 만들고 싶으면 `ctx.lineCap = "round"` 해당 코드를 사용하면 된다.

`lineCap` 에서 지원하는 기능이다.

- 적용하지 않은 선 상태
    
    ![스크린샷 2023-11-16 오후 4 27 59](https://github.com/Heo-y-y/development-blog/assets/112863029/b64546e1-2b94-45c1-93b7-859d42372247)
    
- 적용한 선 상태
    
    ![스크린샷 2023-11-16 오후 4 28 06](https://github.com/Heo-y-y/development-blog/assets/112863029/1a726db1-4da6-4006-a26a-6dbecf4e276f)
    

확실히 더 둥글게 선이 적용이 되는 것을 확인할 수 있다.

이번에는 내가 그린 이미지를 저장하는 기능을 추가하려고 한다.

마찬가지로 이미지를 저장할 때 사용할 버튼을 만들어주고,

```html
<button id="save">Save image</button>
```

JS에 적용시켜 준다.

```jsx
const saveBtn = document.getElementById("save");

function onSaveClick() {
    const url = canvas.toDataURL();
    const a = document.createElement("a");
    a.href = url;
    a.download = "myDrawing.png";
    a.click();
}

saveBtn.addEventListener("click", onSaveClick); // 이미지 저장
```

그림을 완성 시키고 저장 버튼을 누르면 아래와 같이 뜨고,

![스크린샷 2023-11-16 오후 4 28 17](https://github.com/Heo-y-y/development-blog/assets/112863029/cfad3055-494e-4550-908d-2a02551e9c13)

![스크린샷 2023-11-16 오후 4 28 28](https://github.com/Heo-y-y/development-blog/assets/112863029/fc4be1bb-af86-4bf1-baa2-642b18473ecc)

저장까지 잘 되는 것을 확인할 수 있다.

이제는 CSS를 적용하여 그림판을 좀더 이쁘게 수정해보자.

```css
@import "reset.css";

body {
    display: flex;
    gap: 20px;
    justify-content: space-between;
    align-items: flex-start;
    background-color: gainsboro;
    padding: 20px;
    font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}
canvas {
  width: 800px;
  height: 800px;
  background-color: white;
  border-radius: 10px;
}
body {
  display: flex;
  justify-content: center;
  align-items: center;
}
.btns {
    display: flex;
    flex-direction: column;
    gap: 20px;
}
.color-options {
    display: flex;
    flex-direction: column;
    gap: 20px;
    align-items: center;
}
.color-option {
    width:50px;
    height:50px;
    border-radius: 50%;
    cursor: pointer;
    border: 5px solid white;
    transition: transform ease-in-out .1s;
}
.color-option:hover {
    transform: scale(1.2);
}
input#color {
    background-color: white;
}
button, 
label {
    all: unset;
    padding: 10px 0px;
    text-align: center;
    background-color: royalblue;
    color: white;
    font-weight: 500;
    cursor: pointer;
    border-radius: 10px;
    transition: opacity linear .3s;
}
button:hover {
    opacity: 0.85;
}
input#file {
    display: none;
}
input#text {
    all:unset;
    padding: 10px 0px;
    border-radius: 10px;
    font-weight: 500;
    text-align: center;
    background-color: white;
}
```

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Meme Maker</title>
    <link rel="stylesheet" href="styles.css" />
  </head>
  <body>
    <div class="color-options">
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
        style="background-color: #2ecc71"
        data-color="#2ecc71"
       ></div>
       <div
        class="color-option"
        style="background-color: #e67e22"
        data-color="#e67e22"
       ></div>
    </div>
    <canvas></canvas>
    <div class="btns">
        <input id="line-width" type="range" min="1" max="10"
    value="5" step="0.5" />
    <button id="mode-btn">🩸 Fill</button>
    <button id="destory-btn">⬜️ Destory</button>
    <button id="eraser-btn">🔘 Eraser</button>
    <label for="file">
        📩 Add Photo
        <input type="file" accept="image/*" id="file" 
    /></label>
    <input type="text" id="text" placeholder="Add text here... :)" />
    <button id="save">⚙️ Save image</button>
    </div>
    <script src="app.js"></script>
  </body>
</html>
```

reset.css 파일에는 [Reset CSS](https://meyerweb.com/eric/tools/css/reset/)에서 제공하는 코드를 사용했다.

그림판 완성이다!

![스크린샷 2023-11-16 오후 4 28 39](https://github.com/Heo-y-y/development-blog/assets/112863029/622c828c-3d6f-4298-a416-00d85bf717e8)


**참고 자료**

- <https://nomadcoders.co/?gad_source=1&gclid=CjwKCAiA9dGqBhAqEiwAmRpTC-cZUkTXw06mRgUb66aphBQDg53RXl3UK4UEsCfgkiQjhv5559r5thoCbDEQAvD_BwE>
