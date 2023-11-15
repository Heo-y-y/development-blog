# **THE CANVAS API**

**Canvas API**는 JS 및 HTML 요소를 통해 그래픽을 그리는 수단을 제공한다.

예를 들면 애니메이션, 게임 그래픽, 데이터 시각화 등 여러가지 기능을 제공한다.

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
    <canvas></canvas>
    <script src="app.js"></script>
  </body>
</html>
```

위 코드와 같이 HTML에 `<canvas>`를 적용하면 된다.

나머지 작업은 대부분 JS에서 이루어진다.

```jsx
const canvas = document.querySelector("canvas"); //1
const ctx = canvas.getContext("2d");
canvas.width = 800;
canvas.height = 800;

ctx.fillRect(50, 50, 100, 200);
```

- `document.querySelector()` 메서드는 JS에서 `canvas` 불러온다.
- `context`는 캔버스에 그림을 그릴 때 사용하는 붓인데, `getContext()`를 사용하여 붓을 가져온다.

**캔버스 크기 설정**

- CSS에서 캔버스 크기 설정을 한 후 JS에서도 작성해준다.
- 이후에는 `width` 와 `height`를 JS에서만 수정할 한다.

**캔버스 위에 사각형 그리기**

- `fillRect()` 메서드는 사각형을 채우는 함수이다.
- 코드에서 사용된 값들은 `x, y, w, h` 로 적용된다.

먼저 테스트하기에 앞서서 **Live Server**라는 도구가 있다.

![스크린샷 2023-11-15 오전 1 19 06](https://github.com/Heo-y-y/development-blog/assets/112863029/f7b6467b-039b-4cf0-ac3c-c0d715b29299)

이 도구는 **실시간으로 코드의 변화에 따라 화면을 보여준다**.

만약 Live Server를 다운 받고, 실시간으로 화면이 보이지 않는다면 아래 사진처럼 설정하면 된다.

![스크린샷 2023-11-15 오전 1 14 55](https://github.com/Heo-y-y/development-blog/assets/112863029/5c9a4526-5715-4a5b-8967-4554374142f6)

다시 본론으로 들어가서

위 코드를 실행하면 아래 그림과 같이 보여진다.

![스크린샷 2023-11-15 오전 12 53 13](https://github.com/Heo-y-y/development-blog/assets/112863029/5faf1f79-dd5b-4629-af56-603c365dd722)

사각형을 그리려면 기본적인 단계들을 하나씩 거쳐야 한다.

- 사각형 선 그리기: `ctx.rect(x, y, w, h);`
- `ctx.stroke()` 또는 `ctx.fill()` 를 사용해서 테두리만 그리거나 채울 수 있다.
- 끊어가기를 원하는 곳 맨 앞에 `ctx.beginPath();`추가해 새 경로를 만들어 다르게 적용할 수 있다.

`canvas`에서 그림을 그릴 때는 단계별로 진행할 필요가 있다.

위 설명을 이용하여 아래 코드를 작성했다.

```jsx
const canvas = document.querySelector("canvas");
const ctx = canvas.getContext("2d");
canvas.width = 800;
canvas.height = 800;

ctx.rect(50, 50, 100, 100);
ctx.rect(150, 150, 100, 100);
ctx.rect(250, 250, 100, 100);
ctx.fill();

ctx.beginPath();
ctx.rect(350, 350, 100, 100);
ctx.rect(450, 450, 100, 100);
ctx.fillStyle = "red";
ctx.fill();
```

위 코드를 실행시키면, 아래 그림처럼 바뀐다.

![스크린샷 2023-11-15 오전 1 08 28](https://github.com/Heo-y-y/development-blog/assets/112863029/59170b88-f36e-4034-8709-b13f25d2538c)

사실 `rect` 조차 `shortcut functiond`이다.

더 작게 나눠진 기능을 써보자.

- moveTo(x, y): 브러쉬의 좌표를 움직여준다.
- lineTo(x, y): 라인을 그려준다.
- 수평인 직선을 그리려면 두 y값이 같아야 한다.
- 라인이 끝난 점이 다음에 시작하는 브러쉬 좌표이다.

정리하자면, `fillRect = fill+Rect = fill + (moveTo+lineTo)`

그러면 직점 코드로 사각형을 그려보자.

```jsx
ctx.moveTo(50, 50);
ctx.lineTo(150, 50);
ctx.lineTo(150, 150);
ctx.lineTo(50, 150);
ctx.lineTo(50, 50);
ctx.fill();
```

코드를 실행하면 아래 그림처럼 사각형이 완성된다.

![스크린샷 2023-11-15 오후 4 58 52](https://github.com/Heo-y-y/development-blog/assets/112863029/d99017ca-d5fc-48b2-91e7-1d2a2766d4d4)

이 방법을 활용해서 간단한 집을 만들어보자.

```jsx
ctx.fillRect(200, 200, 50, 200); // 왼쪽 벽 만들기
ctx.fillRect(400, 200, 50, 200); // 오른쪽 벽 만들기
ctx.lineWidth = 2; // 선 굵기 조절
ctx.fillRect(300, 300, 50, 100); // 문 만들기
ctx.fillRect(200, 200, 200, 20); // 천장 만들기
ctx.moveTo(200, 200); // 지붕을 만들기 위해 좌표 이동
ctx.lineTo(325, 100);
ctx.lineTo(450, 200);
ctx.fill(); // 지붕 채우기
```

![스크린샷 2023-11-15 오후 5 11 13](https://github.com/Heo-y-y/development-blog/assets/112863029/f9ba0e73-5079-43b7-acd6-001c7d1f4ce3)

이렇게 해서 집이 완성됐다.

연습으로 사람 모양을 만들어보자.

```jsx
// 몸통, 팔
ctx.fillRect(210 - 40, 200 - 30, 15, 100);
ctx.fillRect(350 - 40, 200 - 30, 15, 100);
ctx.fillRect(260 - 40, 200 - 30, 60, 200);
// 얼굴
ctx.arc(250, 100, 50, 0, 2 * Math.PI);
ctx.fill();

// 눈
ctx.beginPath();
ctx.fillStyle = "white";
ctx.arc(260 + 10, 80, 8, Math.PI, 2 * Math.PI);
ctx.arc(220 + 10, 80, 8, Math.PI, 2 * Math.PI);
ctx.fill();
```

![스크린샷 2023-11-15 오후 5 25 45](https://github.com/Heo-y-y/development-blog/assets/112863029/08690ac8-9067-487d-af01-66efe5f574e2)

Canvas API를 사용하여 실제 그림을 그리는 것처럼 작업을 할 수 있다는 것을 경험했다.

[Math.PI](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/PI)가 궁금하면 해당 공식 문서를 살펴보자.

**참고 자료**

- <https://nomadcoders.co/?gad_source=1&gclid=CjwKCAiA9dGqBhAqEiwAmRpTC-cZUkTXw06mRgUb66aphBQDg53RXl3UK4UEsCfgkiQjhv5559r5thoCbDEQAvD_BwE>
