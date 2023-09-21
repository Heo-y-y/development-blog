![](https://github.com/Heo-y-y/development-blog/assets/112863029/a597076e-53a8-4535-a094-af1d2849a180)

![](https://github.com/Heo-y-y/development-blog/assets/112863029/e555fdbd-5735-4adf-9fe0-93f52b657c74)

### 작업 기간 및 인원
*팀 프로젝트(백엔드 3명, 프론트엔드 3명), 2023.05 ~ 2023.06*

### 사용 Skill
- **Backend**

![](https://img.shields.io/badge/SpringBoot-6DB33F.svg?&style=for-the-badge&logo=SpringBoot&logoColor=white)
<img src="https://img.shields.io/badge/Java11-007396?style=for-the-badge&logo=Conda-Forge&logoColor=white">
![](https://img.shields.io/badge/MySQL-4479A1.svg?&style=for-the-badge&logo=MySQL&logoColor=white)
![](https://img.shields.io/badge/SpringSecurity-6DB33F.svg?&style=for-the-badge&logo=SpringSecurity&logoColor=white)
![](https://img.shields.io/badge/AmazonS3-569A31.svg?&style=for-the-badge&logo=amazons3&logoColor=white)
<img src="https://img.shields.io/badge/jsonwebtokens-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white">

- **Tools**

![](https://img.shields.io/badge/GitHub-181717.svg?&style=for-the-badge&logo=github&logoColor=white)
![](https://img.shields.io/badge/git-F05032?style=for-the-badge&logo=git&logoColor=white)
![](https://img.shields.io/badge/intellij-000000.svg?&style=for-the-badge&logo=intellijidea&logoColor=white)
![](https://img.shields.io/badge/Swagger-85EA2D.svg?&style=for-the-badge&logo=swagger&logoColor=black)
![](https://img.shields.io/badge/Postman-ff6c37.svg?&style=for-the-badge&logo=Postman&logoColor=white)
![](https://img.shields.io/badge/discord-5865F2?style=for-the-badge&-logo=discord&logoColor=white)
![](https://img.shields.io/badge/notion-000000?style=for-the-badge&logo=notion&logoColor=white)


### 링크
📎 **[Github](https://github.com/Heo-y-y/cocktail_project) / [Demo](http://resevilleage-bukit.s3-website.ap-northeast-2.amazonaws.com)** **/ [시연 영상](https://youtu.be/hv4089oai4o)**

### 진행 목적
여러 가지 술이나 음료를 Mix 해서 만든 술을 칵테일이라고 합니다. 우리가 흔하게 마시는 소맥도 칵테일에 속하고, 요즘 유행하는 하이볼도 마찬가지입니다.

혼술 하는 사람들이 많이 늘어나고 있는데, 여러 가지 레시피를 제공해 주는 곳이 생각보다 없고 정보도 제각각이라 정확한 정보를 구하기가 쉽지 않습니다.

그래서 정규 레시피를 제공해 주며, 각자의 개성 있는 칵테일 레시피를 서로 공유할 수 있는 사이트를 만들어보기로 했습니다.

### 구현한 기능

- **[정규 레시피 도수 별 Pagination](정규레시피.md)**
- **[커스텀 레시피 상세 조회](커스텀레시피.md)**
- **[Swagger UI를 통한 프론트엔드 팀과 백엔드 팀간의 손쉬운 협업 활성화](Swagger.md)**
- **[백엔드, 프론트엔드 서버 분리와 S3를 통한 이미지 저장 아키텍팅](S3.md)**
- **[안전한 인증 흐름, 토큰 관리 및 데이터 정보 보호를 보장하여 사용자 경험 향상 및 프로세스 단순화](인증흐름.md)**

### 프로젝트 진행 후 느낀점

- **Pagination**
    
    왜 대량의 데이터를 보낼 때 Pagination을 선호하는지 알 수 있었고, 나중에 작업을 할 때도 사용자 측면에서 좀 더 생각하게 되었습니다.
    
- **S3**
    
    새로 적용해보는 기능이라 처음에는 어려웠지만, 이 작업을 맡아 진행한 덕분에 S3 버킷을 활용해 이미지를 가져오고 보내는 경험을 할 수 있었습니다.
    
- **JWT**
    
    토큰 인증 관련도 어려웠지만, 결국에는 무조건 웹을 만들 때 필수 요소라고 생각하고, 해당 작업을 경험하면서 전반적인 인증흐름에 대한 경험을 할 수 있었고, 특히나 보안 측면에서 더 좋은 방법이 없는지, 생각해보는 시간이였습니다.

### 아쉬웠던 점

- **access 토큰 재발급 부분 - jwt의 이해도 부족**
    
    큰 아쉬운 부분이 **리플레시 토큰을 사용하지 못한 부분**입니다. 엑세스 토큰이 만료 된다면 클라이언트의 리플래시 토큰을 받아서 검사 후 **엑세스 토큰을 재발급** 해주는 기능을 구현하고 싶었지만, 토큰에 대한 이해도와 jwt의 구현 코드들을 완전히 파악하지 못해 진행하지 못했습니다.
    
- **이메일 인증 - 일정 관리 실패**
    
    현재 프로잭트는 아이디를 이메일로 사용하고 있는데, 이것이 **실제 사용하고 있는 이메일인지에 대해 이메일 인증을 구현하지 못한 부분**입니다.
    
- **OAuth2 인증 - 기술 이해도 부족**
    
    **리플레시 토큰이 제대로 구현되어있지않으니 자연스럽게 같이 구현하지 못한 점** 입니다.

하지만 알딸딸 프로젝트로 토큰 인증에 대한 지식을 습득한 경험으로 인해,

지금 팀장으로 진행하고 있는 **[인스타 클론 코딩](https://github.com/Instagram-clone-project-team/Instamram-clone)** 에서는 이메일 인증 코드를 사용한 회원가입 로직도 만들 수 있었고, **[원티드 프리온보딩 인턴쉽 과제](https://github.com/Heo-y-y/wanted-pre-onboarding-backend)** 에서 레디스를 활용해 토큰 재발급을 만들 수 있었습니다.
