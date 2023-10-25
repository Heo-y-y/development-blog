# HTTP 기본

## HTTP(HyperText Transfer Protocol)

지금은 HTTP에 모든 프로토콜을 전송합니다.

심지어 서버간에 데이터를 주고 받을 때도 HTTP를 사용합니다.

- HTML, TEXT
- IMAGE, 음성, 영상, 파일
- JOSN, XML(API)
- 거의 모든 형태의 데이터를 전송 할 수 있다.
- 서버간에 데이터를 주고 받을 때도 대부분 HTTP를 사용합니다.

즉, 현재 HTTP가 없는 세상은 상상할 수가 없습니다!

### HTTP 역사

- HTTP/0.9: 1991년 출시하고 GET 메서드만 지원, HTTP 헤더 X
- HTTP/1.0: 1996년 출시하고 메서드, 헤더가 추가됩니다.
- **HTTP/1.1:** 1997년 출시하고 **가장 많이 사용하는 버전**이며, **중요한 버전**입니다.
    - RFC2068(1997) → RFC2616(1999) → RFC7230~7235(2014)
- HTTP/2: 2015년 출시하고 성능 개선 위주입니다.
- HTTP/3: 진행중이며, TCP 대신에 UDP 사용, 성능 개선 중 입니다.

### 기반 프로토콜

- **TCP**: HTTP/1.1, HTTP
- **UDP**: HTTP
- 현재 HTTP/1.1를 주로 사용합니다.
    - HTTP/2, HTTP/3도 점점 증가하는 중 입니다.

### HTTP 특징

- 클라이언트 서버 구조
- 무상태 프로토콜(스테이스리스), 비연결성
- HTTP 메시지를 통해서 통신을 합니다.
- 단순함, 확장 가능

## 클라이언트 서버 구조

![스크린샷 2023-10-25 오후 2.01.12.png](https://github.com/Heo-y-y/development-blog/assets/112863029/2996772a-0c92-4da4-a72d-f682a2d270de)

1. HTTP는 클라이언트가 HTTP 메시지를 통해서 서버에 보냅니다.
2. 클라이언트는 서버에 응답이 올 때까지 기다립니다.
3. 서버가 요청에 대한 결과를 만들어서 응답을 보냅니다.

클라이언트와 서버를 개념적으로 분리를 합니다.

비즈니스로직이랑 데이터들은 서버에다가 할당합니다.

클라이언트는 UI, UX에 집중합니다.

즉, 역할을 분리하여 더 집중할 수 있게 하는 것 입니다.

## 무상태 프로토콜(Stateless)

서버가 클라이언트의 상태를 보존하지 않습니다.

### Stateful, Stateless 차이

**Stateful**은 **서버의 상태를 유지**해주는 것 입니다. 중간에 다른 점원으로 바뀌면 안됩니다. 즉, 중간에 다른 점원으로 바뀔 때 상태 정보를 다른 점원에게 미리 알려줘야 합니다.

반대로 **Stateless**는 **서버의 상태를 유지하지 않습니다**. 중간에 다른 점원으로 바뀌어도 됩니다. 즉, 갑자기 클라이언트 요청이 증가해도 서버를 대거 투입할 수 있습니다.

정리하면, 무상태는 응답 서버를 쉽게 바꿀 수 있습니다. → **무한한 서버 증설 가능**

### Stateful - 상태 유지

![스크린샷 2023-10-25 오후 3.45.49.png](https://github.com/Heo-y-y/development-blog/assets/112863029/33ea7c59-70ef-4f3a-944a-7f1626178c90)

![스크린샷 2023-10-25 오후 3.46.42.png](https://github.com/Heo-y-y/development-blog/assets/112863029/2a25de3d-ab54-4c6b-b7e0-37bc3572ff9e)

1. 클라이언트A가 요청을 하면, 서버A가 응답을 합니다.
2. 하지만, 상태 유지일 경우 서버A랑 통신할 경우 무조건 서버A랑 진행을 해야 합니다.
3. 만약 서버A가 터질 경우 처음부터 다시 요청해야 합니다.

### Stateless - 무상태

![스크린샷 2023-10-25 오후 3.58.15.png](https://github.com/Heo-y-y/development-blog/assets/112863029/db5e8bda-e7f2-4668-acf0-726cae1aab9a)

![스크린샷 2023-10-25 오후 3.58.56.png](https://github.com/Heo-y-y/development-blog/assets/112863029/bf967a7d-583c-46d0-8f65-12f8a41f0051)

1. 애초에 요청할 때부터 필요한 데이터를 다 보냅니다.
2. 서버는 상태를 보관하지 않고, 필요한 응답만 합니다.
3. 만약 중간에 서버가 장애가 나면, 중계서버가 다른 서버에게 요청합니다.

즉, 아래 그림처럼 **스케일 아웃 - 수평 확장**에 유리합니다.

![스크린샷 2023-10-25 오후 4.00.08.png](https://github.com/Heo-y-y/development-blog/assets/112863029/67721a8f-106b-4e92-89a3-19962bc38f7e)

- **Stateless 실무 한계**
    - 모든 것을 무상태로 설계 할 수 있는 경우도 있고 없는 경우도 있습니다.
    - 단점으로는 한번에 데이터를 많이 보냅니다.
    - 무상태: 로그인이 필요 없는 단순한 서비스 소개 화면
    - 상태 유지: 로그인
        - 로그인한 사용자의 경우 로그인 했다는 상태를 서버에 유지해줘야 합니다.
        - 일반적으로는 브라우저 쿠키와 서버 세션등을 사용해서 상태를 유지합니다.
        - 상태 유지는 최소한만 사용하는게 좋습니다.

## 비 연결성(connectionless)

- **HTTP**는 **기본이 연결을 유지하지 않는 모델**입니다.
- 일반적으로는 초 단위 이하의 빠른 속도로 응답합니다.
- 1시간 동안 수천명이 서비스를 사용해도 실제 서버에서 동시에 처리하는 요청은 수십개 이하로 매우 작습니다.
    - 예시) 웹 브라우저에서 계속 연속해서 검색 버튼을 누르지 않습니다.
- 서버 자원을 매우 효율적으로 사용할 수 있습니다.

### 연결을 유지하는 모델

![스크린샷 2023-10-25 오후 5.37.38.png](https://github.com/Heo-y-y/development-blog/assets/112863029/be20eb2c-a72e-4c00-a9dc-3022c181d920)

클라이언트의 연결들을 유지하는 동안 서버의 자원은 계속 소비하고 있어야 합니다.

즉, 연결시켜 놓고 있기 때문에 사용하지 않아도 계속 서버가 유지하고 있어야 합니다.

### 연결을 유지하지 않는 모델

![스크린샷 2023-10-25 오후 5.39.26.png](https://github.com/Heo-y-y/development-blog/assets/112863029/17fd6105-1afd-44a3-8702-a9504b5b9c71)

이렇게 요청이 끝나면 서버와의 연결은 종료됩니다.

즉, 서버 입장에서는 서로 주고 받을 때만 자원을 소비하기 때문에 **서버가 유지하는 자원을 최소한으로 소비**할 수 있습니다.

### 비 연결성 한계와 극복

- TCP / IP 연결을 새로 맺어야 합니다.(3 way handshake 시간 추가)
- 웹 브라우저로 사이트를 요청을하면, HTML 뿐만 아니라 자바스크립트, css, 추가 이미지 등 등 수 많은 자원이 함께 다운로드가 됩니다.

![스크린샷 2023-10-25 오후 5.45.12.png](https://github.com/Heo-y-y/development-blog/assets/112863029/f448e629-56b8-40be-b82c-779874f2fa54)

- 현재는 HTTP 지속 연결(Persistent Connections)로 문제를 해결했습니다.
- HTTP/2, HTTP/3에서 더 많은 최적화가 됐습니다.

![스크린샷 2023-10-25 오후 5.45.25.png](https://github.com/Heo-y-y/development-blog/assets/112863029/709a23fe-af4f-463d-ad4c-13fabcd5b5b4)

### 스테이스리스

- 같은 시간에 딱 맞추어 발생하는 대용량 트래픽
    - 예시) 선착순 이벤트, 명절 KTX 예약, 학과 수업 등록 등
    - 예시) 저녁 6:00 선착순 100명 치킨 할인 이벤트 → 수만명 동시 요청

## HTTP 메시지

### HTTP 메시지 구조

![스크린샷 2023-10-25 오후 5.51.43.png](https://github.com/Heo-y-y/development-blog/assets/112863029/fe4de778-2ab2-4218-8553-5d040e884bd2)

여기서 공백은 무조건 사용해야 합니다.

요청 메시지도 본문에 필요한 message body를 사용할 수 있습니다.

![스크린샷 2023-10-25 오후 5.53.27.png](https://github.com/Heo-y-y/development-blog/assets/112863029/5eebd6c1-87eb-4cf2-b6ba-fbc84522a02a)

### 시작 라인

- **요청 메시지**
    
    ![스크린샷 2023-10-25 오후 6.00.11.png](https://github.com/Heo-y-y/development-blog/assets/112863029/f75207db-bd4e-42e9-9809-c8cada218721)
    
    - start-line = **request-line** / status-line
    - **reqeust-line** = method SP(공백) request-target SP HTTP-version CRLF(엔터)
    - HTTP 메서드
        - 종류: GET, POST, PUT, DELETE 등
        - 서버가 수행해야 할 동작 지정
    - 요청 대상(/search?q=hello&hl=ko)
        
        ![스크린샷 2023-10-25 오후 5.59.30.png](https://github.com/Heo-y-y/development-blog/assets/112863029/4afe09a9-cf9e-43fd-a423-2c54af99aa54)
        
        - absolute-path[?query](절대경로[?쿼리]
        - 절대경로= “/”로 시작하는 경로
        - 참고로 *, http://…?x=y와 같이 다른 유형의 경로 지정 방법도 있습니다.
    - HTTP Version
        
        ![스크린샷 2023-10-25 오후 5.59.50.png](https://github.com/Heo-y-y/development-blog/assets/112863029/2ed0bcce-262f-4482-adbb-25e9a39513e9)
        
- **응답 메시지**
    
    ![스크린샷 2023-10-25 오후 6.03.36.png](https://github.com/Heo-y-y/development-blog/assets/112863029/b83432a0-372f-4887-9801-854db181b32f)
    
    - start-line = request-line / **status-line**
    - **status-line** = HTTP-version SP status-code SP reson-phrase CRLF
    - HTTP 버전
    - HTTP 상태 코드: 요청 성공, 실패를 나타냅니다.
        - 200 = 성공 / 400 = 클라이언트 요청 오류 / 500 = 서버 내부 오류
    - 이유 문구: 사람이 이해할 수 있는 짧은 상태 코드 설명 글

### HTTP 헤더

- header-field = field-name “:” OWS field-value OWS (OWS: 띄어쓰기 허용)
- field-name은 대소문자 구문 X

![스크린샷 2023-10-25 오후 6.06.10.png](https://github.com/Heo-y-y/development-blog/assets/112863029/da8024a7-71bc-4280-96fa-81ddf5f12f6e)

- **용도**
    
    ![스크린샷 2023-10-25 오후 6.09.22.png](https://github.com/Heo-y-y/development-blog/assets/112863029/0d953b06-4503-49b6-a25b-4f4d0fe33bd3)
    
    - HTTP 전송에 필요한 모든 부가 정보(**메시지 바디 빼고 필요한 모든 정보**)
        - 예시) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트(브라우저) 정보, 서버 애플리케이션 정보, 캐시 관리 정보 등
    - 표준 헤더가 엄청 많습니다.
    - 필요시 임의의 헤더를 추가할 수 있습니다.

### HTTP 메시지 바디

![스크린샷 2023-10-25 오후 6.10.00.png](https://github.com/Heo-y-y/development-blog/assets/112863029/3d344840-0ca3-4ef2-9c92-fa7a254b275b)
- **용도**
    - 실제 전송할 데이터
    - HTML 문서, 이미지, 영상, JSON 등 byte로 표현할 수 있는 모든 데이터를 전송할 수 있습니다.

**참고 자료**

- [https://www.inflearn.com/course/http-웹-네트워크/dashboard](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard)
