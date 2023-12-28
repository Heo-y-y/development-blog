# Git을 활용한 버전관리

## 버전 관리

대표적인 버전 관리 시스템으로 **Git**이 있다.

**Git**은 컴퓨터 파일의 **변경사항을 추적**하고, **여러 사용자들 간에 해당 파일 작업을 조율**하기 위한 대표적인 **버전 관리 시스템**(VCS)이다.

## 버전 생성과 업로드의 이해

### 로컬 환경

**Git 기본 구성 설정**

![스크린샷1 2023-12-28 오후 2 11 59](https://github.com/Heo-y-y/starbucks/assets/112863029/2aeeaa2d-2064-43d0-9fa0-5a6adac5df68)

**현재 프로젝트에서 버전 관리 시작**

![스크린샷2 2023-12-28 오후 2 12 23](https://github.com/Heo-y-y/starbucks/assets/112863029/bcb125a1-0618-4bd2-a1c4-af30461a2e6c)

**추적할 특정 파일 지정**

![스크린샷3 2023-12-28 오후 2 14 46](https://github.com/Heo-y-y/starbucks/assets/112863029/b0e218a8-f727-4a90-8fd4-ada3a5d0490b)

**모든 파일의 변경사항을 추적하도록 지정**

![스크린샷4 2023-12-28 오후 2 15 35](https://github.com/Heo-y-y/starbucks/assets/112863029/952109ab-3145-42cd-b0da-a29fb3c17707)

**프로젝트 버전 생성**

![스크린샷5 2023-12-28 오후 2 16 36](https://github.com/Heo-y-y/starbucks/assets/112863029/d708573c-2203-4b4b-abb2-b4d38328ef07)

### 외부 환경

**원격 저장소에 등록**

![스크린샷6 2023-12-28 오후 2 22 37](https://github.com/Heo-y-y/starbucks/assets/112863029/79354ada-ebb6-4885-b4ed-699298963be8)

**원격 저장소에 버전 내역 전송**

![스크린샷7 2023-12-28 오후 2 23 26](https://github.com/Heo-y-y/starbucks/assets/112863029/415f4e7b-0ef9-477f-a9b0-047944b41f02)

## Netlify 지속적인 배포

**Netlify**에 저장소를 연결해 놓을 경우 컴퓨터 환경에서 수정을 하여 다시 GitHub 저장소에 업로드하면 Netlify가 자동으로 그 내용을 확인하여 새로운 버전을 사이트에도 업데이트 시켜준다.

이러한 자동화되는 시스템이 바로 **지속적인 배포** **Continuous Deployment**라고 한다.

[Netlify](https://www.netlify.com/)에서 가입하고 사용하면 된다.

순차적으로 Git과 연결하여 진행하고, 배포할 저장소를 선택하여 배포하면 아래와 같이 작업한 저장소를 연결하여 실제 사이트로 만들 수 있다.

https://superlative-cocada-16c2e2.netlify.app/

하지만 도메인이 이쁘지 않은 단점이 있다. 도메인은 따로 구입하여 사야한다.

## Git Branch

**branch**는 main이라는 하나의 줄기에서 다른 가지들로 만들어 작업하는 것을 말하는데, 이러한 가지들을 메인의 줄기로 합쳐서 실제 사이트에서 사용할 수 있도록 만드는 것이다.

**사용 방법**

```java
// 브랜치 확인
git branch

// 브랜치 생성
git branch "브랜치 이름"

// 브랜치 이동
git checkout "브랜치 이름"
```

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
