# (Error) npm run deploy &  vite.config.ts 설정

Git으로 배포를 진행하던 중 build 디렉토리를 찾을 수 없다는 내용이 나왔다.

```bash
Error: ENOENT: no such file or directory, stat '/Users/heoyun-yeong/Desktop/react-note-app/build'

```

다시 한번 `npm run build`를 진행했지만. 찾을 수 없다는 내용과 실제로 생성이 되지 않았다.

그래서 먼저 build 디렉토리가 존재하고, 올바른 위치에 생성되었는지 확인하기 위해 아래 명령어를 실행했다.

```bash
ls -l dist
```

해당 명령어는 Vite가 기본적으로 빌드 출력을 저장하는 dist 디렉토리의 내용을 보여준다. 여기에 예상되는 빌드 파일이 표시되면 올바른 디렉토리를 가리키도록 gh-pages 배포 스크립트를 조정해야 한다.

```bash
heoyun-yeong@heoyun-yeong-ui-MacBookPro react-note-app % ls -l dist
total 16
drwxr-xr-x  5 heoyun-yeong  staff   160  2  6 16:28 assets
-rw-r--r--  1 heoyun-yeong  staff   464  2  6 16:28 index.html
-rw-r--r--  1 heoyun-yeong  staff  1497  2  6 16:28 vite.svg
```

dist 디렉토리에 index.html 및 asseets 디렉토리를 포함하여 예상되는 빌드 파일이 포함되어 있는 것을 학인할 수 있다.

따라서 dist 디렉토리를 사용하도록 `packge.json`의 `deploy`를 수정했다.

```bash
"deploy": "gh-pages -d dist",
```

변겅 후 다시 `npm run deploy` 를 진행하니 성공하는 것을 볼 수 있다.

![스크린샷1 2024-02-06 오후 4 38 30](https://github.com/Heo-y-y/react-budget-deploy-test-app/assets/112863029/e02b1377-1054-4f8a-ba86-7df30d4cc0ad)

그런데 문제는 배포 사이트는 열렸으나, 화면이 빈 화면으로 나오는 것이다.

![스크린샷2 2024-02-06 오후 5 37 00](https://github.com/Heo-y-y/react-budget-deploy-test-app/assets/112863029/6c4c6197-6aaa-4de0-9e95-0595fd4f624d)

이유를 살펴보니 프로덕션 배포(`npm run build`)의 경우 웹 서버에서 제공할 수 있는 최적화된 정적 파일을 생성해야 한다고 한다. 현재 프로덕션 배포가 정적 파일을 올바르게 생성하지 못하는 것 같다.

구글링을 통해 찾아보니 `vite.config.ts` 파일을 사용하여 빌드 프로세스의 특정 측면을 실제로 구성할 수 있다고 한다.

그래서 다음과 같이 설정을 해주었다.

**vite.config.ts**

```tsx
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  base: '',
  build: {
    outDir: 'dist',
    assetsDir: 'assets',
  },
});
```

![스크린샷3 2024-02-06 오후 5 38 04](https://github.com/Heo-y-y/react-budget-deploy-test-app/assets/112863029/4a9eab84-3ea9-4567-865d-b392ff51a815)

설정을 하여 사이틀 확인해보니 이제는 만든 페이지가 보이지만 `package.json`에서 설정한  url이 아니라 바로 해당 url로 이동하며 위 그림처럼 404 에러가 발생한다.

그래서 `base` 값에 `/keep-note-app/` **으로 설정하여 빌드 후 다시 진행하니 정상 동작하는 것을 확인 할 수 있다.**

![스크린샷4 2024-02-06 오후 6 29 26](https://github.com/Heo-y-y/react-budget-deploy-test-app/assets/112863029/5cabaa54-8f5d-446a-9f1c-d30594779a6f)
