# TypeScript 설치 Error(Not Found)

## **TypeScript not found**

TypeScript를 잘 사용하다가 갑자기 `tsc -w`로 실행을 하니 찾을 수 없다는 문구가 나온다.

![스크린샷 2024-01-16 오전 11.53.50.png](https://github.com/Heo-y-y/development-blog/assets/112863029/896f6529-446e-4b7c-8cfd-27efefb17e87)

```jsx
npm list -g
```

위 명령어를 통해 폴더에 글로벌로 설치 되어 있는지 확인하면

![스크린샷 2024-01-16 오전 11.57.17.png](https://github.com/Heo-y-y/development-blog/assets/112863029/6e40f441-ab5c-40ab-8a15-f87f12382eeb)

정상적으로 나오는 걸 확인할 수 있다.

하지만 아래 명령어로 타입스크립트 버전을 확인하면,

```jsx
tsc -version
```

![스크린샷 2024-01-16 오전 11.58.57.png](https://github.com/Heo-y-y/development-blog/assets/112863029/b1a7346c-9497-43b2-aaff-85dda5a44db8)

찾을 수 없다고 나온다.

그러면 tsc를 다시 지웠다가 설치하고 npx를 해보자.

참고로 **npx**는 **새로운 패키지 관리 모듈이 아니다.** **자바스크립트 패키지 관리 모듈인 npm(Node Package Module)의 5.2.0 버전부터 새로 추가된 도구이**다. 따라서 npm과 비교대상이 아닌 npm을 좀 더 편하게 사용하기 위해서 npm에서 제공해주는 하나의 도구이다.

```jsx
npm uninstall tsc

npm uninstall -g tsc

npx --package typescript tsc --init

npx --package typescript tsc --version
```

하지만 위와 같이 해도 결과가 같아.

Homebrew로 설치를 진행해봤다.

```jsx
brew install typescript
```

![스크린샷 2024-01-16 오후 12.27.30.png](https://github.com/Heo-y-y/development-blog/assets/112863029/d44c9a76-f681-4d6d-8447-1df6cb7b72e3)

정상적으로 설치가 완료되었고, 버전을 확인하니 올바르게 나온다.

프로젝트에서 아래 명령어를 통해 TypeScript를 실행하면 잘 동작하는 것을 확인할 수 있다.

```jsx
tsc -w
```

![스크린샷 2024-01-16 오후 12.30.02.png](https://github.com/Heo-y-y/development-blog/assets/112863029/0b618c08-289c-435f-b1da-47574290dddf)
