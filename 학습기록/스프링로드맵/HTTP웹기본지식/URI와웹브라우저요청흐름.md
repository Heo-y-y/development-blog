# URI와 웹 브라우저 요청 흐름

## URI(Uniform Resource Identifier)

URI는 **locator**, **name** 또는 둘다 추가로 분류될 수 있습니다.

![스크린샷 2023-10-20 오후 12.13.28.png](https://github.com/Heo-y-y/development-blog/assets/112863029/c505754e-1be5-4fc5-b51d-76d0c5554aad)

즉, 리소스릉 식별한다는 의미인데, 자원이 어디에 있는지, 자원 자체를 식별하는 것 입니다.

- URL: 리소스가 해당 위치에 있다는 뜻
- URN: 리소스의 이름

![스크린샷 2023-10-20 오후 12.14.49.png](https://github.com/Heo-y-y/development-blog/assets/112863029/d808fcf7-4cc5-4e83-90f7-38c6658395ea)

우리가 평소 웹 브라우저에서 주소를 적는게 URL이라고 생각하면 됩니다.

URN은 진짜 이름을 딱 부여해버리는 것 입니다.(찾기 힘듬)

### URI 의미

- Uniform: 리소스 식별하는 통일된 방식
- Reasource: 자원, URI로 식별할 수 있는 모든 것(제한 없음)
- Identifier: 다른 항복과 구분하는데 필요한 정보

URL: Uniform Resource Locator

URN: Uniform Resource Name

### URL / URN 의미

- URL - Locator: 리소스가 있는 위치를 지정
- URN - Name: 리소스에 이름을 부여
- 위치는 변할 수 있지만, 이름은 변하지 않음.
- URN 이름만으로 실제 리소스를 찾을 수 있는 방법이 보편화 되지 않음

### URL 전체 문법

![스크린샷 2023-10-20 오후 1 14 06](https://github.com/Heo-y-y/development-blog/assets/112863029/e6de766b-928d-4c9e-a8c9-df25b370c361)

- 프로토콜(https)
- 호스트명(www.google.com)
- 포트 번호(443)
- 패스(/search)
- 쿼리 파라미터(q=hello&hl=ko)

### URL scheme

![스크린샷 2023-10-20 오후 1 14 18](https://github.com/Heo-y-y/development-blog/assets/112863029/4140a4f8-c604-4db5-b24c-1dc03cbe5a3c)

- 주로 프로토콜 사용
- 프로토콜: 어떤 방식으로 자원에 접근할 것인가 하는 약속
    - ex) http, https, ftp 등
- http는 80 포트, https는 443 포트를 주로 사용, 포트는 생략 가능합니다.
- https는 http에 보안을 추가한 것 입니다.

### URL userinfo

![스크린샷 2023-10-20 오후 1 14 26](https://github.com/Heo-y-y/development-blog/assets/112863029/9ae9fd44-acc5-4d20-8eff-86af0bf7dfb0)

- URL에 사용자 정보를 포함해서 인증
- 거의 사용하지 않습니다.

### URL host

![스크린샷 2023-10-20 오후 1 14 35](https://github.com/Heo-y-y/development-blog/assets/112863029/5c9d0f7a-e2f6-4969-be67-1296c262f762)

- 호스트명
- 도메인 명 또는 IP 주소를 직접 사용 가능합니다.

### URL PORT

![스크린샷 2023-10-20 오후 1 14 44](https://github.com/Heo-y-y/development-blog/assets/112863029/5d96c0e9-6760-4ba5-892a-bc04456d5768)

- 접속 포트
- 일반적으로 생략, 생략시 http는 80, https는 443으로 합니다.

### URL path

![스크린샷 2023-10-20 오후 1 14 51](https://github.com/Heo-y-y/development-blog/assets/112863029/62a12a32-232d-4993-b1ed-bc520214a092)

- 리소스가 있는 경로이고, 계층적 구조를 가지고 있습니다.

### URL query

![스크린샷 2023-10-20 오후 1 14 59](https://github.com/Heo-y-y/development-blog/assets/112863029/a9a234ee-1e02-49b9-8b6c-0f2ad946cf31)

- key=value 형태
- ?로 시작, &로 추가 가능 ?keyA=valueA&keyB=valueB
- query parameter, query string 등으로 불립니다.
    - 웹 서버에 제공하는 파라미터, 문자 형태

### URL fragment

![스크린샷 2023-10-20 오후 1 15 06](https://github.com/Heo-y-y/development-blog/assets/112863029/a2adf0ad-f1f0-4c95-a315-4992d589413a)

- html 내부에서 북마크 등에 사용됩니다.
- 서버에 전송하는 정보는 아닙니다.

## 웹 브라우저 요청 흐름

만약 이러한 식으로 보내면,

![스크린샷 2023-10-20 오후 12.38.39.png](https://github.com/Heo-y-y/development-blog/assets/112863029/564165f9-f7e3-48d0-8b20-185b869a821f)

아래 그림과 같이 웹 브라우저가 DNS 서버를 조회하고, PORT를 정보를 찾아냅니다.

그다음 HTTP 요청 메시지를 생성합니다.

![스크린샷 2023-10-20 오후 12.39.10.png](https://github.com/Heo-y-y/development-blog/assets/112863029/203e7e60-c23e-4d80-9636-3b7c38d4c684)

HTTP 요청 메시지 형식은 아래 그림과 같은 형태입니다.

![스크린샷 2023-10-20 오후 12.40.00.png](https://github.com/Heo-y-y/development-blog/assets/112863029/212cfdf8-f506-4fc6-a3de-408d77a17e6c)

### HTTP 메시지 전송

![스크린샷 2023-10-20 오후 12.43.20.png](https://github.com/Heo-y-y/development-blog/assets/112863029/d3dc1840-8bac-4352-990e-6c6cb3af7f9d)

웹 브라우저가 HTTP 메시지를 생성하고,

**SOCKET 라이브러리를 통해서 TCP/IP 계층에 전달**합니다.(SYN, ACK)

그 다음 데이터에 한번 패킷 정보를 씌웁니다.

그러면 아래와 같이 패킷이 생성됩니다.

![스크린샷 2023-10-20 오후 12.44.28.png](https://github.com/Heo-y-y/development-blog/assets/112863029/4043667e-e33c-41a5-89a1-87bbe16911cb)

그다음 요청 패킷을 전달하면 받을 때 TCP/IP 패킷을 싹 버리고 HTTP 데이터만 가져옵니다.

그 다음 아래 그림처럼 응답 메시지를 만들어냅니다.

![스크린샷 2023-10-20 오후 12.45.34.png](https://github.com/Heo-y-y/development-blog/assets/112863029/d1e335a2-7be6-464b-af43-2d414ac8aa6f)

**응답 메시지**는 응답 메시지, **Content-Type**으로 형식과 언어의 타입을 보내주고,

**Content-Length**로 언어의 길이를 전달합니다.

그리고 html의 데이터를 보내고 그 결과를 받으면, 아래 그림처럼 요청자에게 보여지게 되는 것 입니다.

![스크린샷 2023-10-20 오후 12.47.19.png](https://github.com/Heo-y-y/development-blog/assets/112863029/620196f5-3eff-4b20-ba9a-ccb23e8b868d)

**참고 자료**

- [https://www.inflearn.com/course/http-웹-네트워크/dashboard](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard)
