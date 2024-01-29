# [Error] **A complete log of this run can be found in:**

에러 페이지를 만드는 도중 확인을 위해 리액트를 실행했는데, 아래와 같은 오류를 만났다.

```bash
npm ERR! A complete log of this run can be found in :
C : 파일경로-debug.log
```

구글링을 했지만, 결국에는 내가 잘못 명령어를 치고 있었다.

**package.json** 

```jsx
"scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "preview": "vite preview"
},
```

`npm start`를 아무리 남발해도 위 에러가 떠 알아보니 React 앱에서는 일반적으로 "start" 스크립트가 **`react-scripts start`**로 설정되어 있는데, Vite를 사용하면 "start" 스크립트 대신 "dev" 스크립트를 사용하는 경우가 많아 "dev" 스크립트를 실행하여 개발 서버를 시작해야 한다는 것이다.

```bash
npm run dev
```

![스크린샷 2024-01-29 오후 11.13.59.png](https://github.com/Heo-y-y/development-blog/assets/112863029/9260cf80-a963-476f-9f73-3b0150232477)

잘 실행되는 것을 볼 수 있다.
