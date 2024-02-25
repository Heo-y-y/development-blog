# (ERROR) load build definition from Dockerfile

Dockerfile을 build하고 실행하려면 다음 명령어를 사용해 빌드해야 한다.

```bash
docker build . -t springbootapp
```

Mac M1을 사용하면 다음과 같은 오류가 발생한다.

![스크린샷 2024-01-31 오후 5.22.23.png](https://github.com/Heo-y-y/development-blog/assets/112863029/406580da-2696-4a3e-a4ec-22b0882058c1)

M1 운영체제는 기본적으로 arm 기반 아키텍처이기 때문에 M1으로 도커파일을 빌드하여 도커 이미지를 생성하면 platform이 linux/arm64/v8을 베이스로 생성된다.

그러나 아직 arm64에 해당하는 이미지가 없는 경우도 있기 때문에 이러한 이미지를 사용하려면 빌드 단계에서 —platform 옵션으로 linux/amd64로 지정해줘야 한다.

아래 명령어를 추가하여 진행해보자.

```bash
--platform linux/amd64
```

M1으로 진행하면 최종 빌드 명령어는 다음과 같다.

```bash
docker build -t 'instagramapp' --platform linux/amd64 .
```

![스크린샷 2024-01-31 오후 6.09.20.png](https://github.com/Heo-y-y/development-blog/assets/112863029/e2bd426e-98a0-4e62-8e7f-258c66427dee)

참고로 빌드를 진행할 때는 해당 프로젝트 경로에서 진행해야 한다.
