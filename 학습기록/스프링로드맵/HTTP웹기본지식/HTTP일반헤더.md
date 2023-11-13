## 용도

- HTTP 전송에 필요한 모든 부가정보
- ex) 메시지 바디 내용, 메시지 바디 크기, 압축, 인증, 요청 클라이언트, 서버 정보, 캐시 관리 등
- 표준 헤더가 엄청 많습니다.
- 필요시 임의의 헤더를 추가할 수 있습니다.

## 분류 - RFC2616(과거)

### 헤더 분류

![스크린샷 2023-11-13 오후 4.32.16.png](https://github.com/Heo-y-y/development-blog/assets/112863029/8917cc45-4b6f-4684-9d13-6f150e4de0cf)

- **General 헤더**: 메시지 전체에 적용되는 정보
- **Request 헤더**: 요청 정보
- **Response 헤더**: 응답 정보
- **Entity 헤더**: 엔티티 바디 정보

### message body

![스크린샷 2023-11-13 오후 4.34.19.png](https://github.com/Heo-y-y/development-blog/assets/112863029/2debd324-ad81-40b0-939a-1386f4ad47d0)

- 메시지 본문(message body)은 엔티티 본문(entity body)를 전달하는데 사용
- 엔티티 본문은 요청이나 응답에서 전달할 실제 데이터
- **엔티티 헤더**는 **엔티티 본문**의 데이터를 해석할 수 있는 정보 제공
    - 데이터 유형(html, json), 데이터 길이, 압충 정보 등

하지만 1999년 HTTP 표준에서 RFC2616이 폐기됐습니다.

2014년 RFC7230 ~ 7235가 등장합니다.

## RFC723x  변화

- 엔티티(Entity) → 표현(Representation)
- Representation = Representation Metadata + Representation Data

### message body

![스크린샷 2023-11-13 오후 4.38.36.png](https://github.com/Heo-y-y/development-blog/assets/112863029/0224d747-7e58-42d0-9490-33c999b65bc9)

- 메시지 본문(message body)을 통해 표현 데이터 전달
- 메시지 본문 = 페이로드(payload)
- **표현**은 요청이나 응답에서 전달할 실제 데이터
- **표현 헤더는 표현 데이터**를 해석할 수 있는 정보 제공
    - 데이터 유형(html, json), 데이터 길이, 압축 정보 등
- 참고로 표현 헤더는 표현 메타데이터와 페이로드 메시지를 구분해야 합니다.

## 표현

- 표현 헤더는 전송, 응답 둘다 사용

![스크린샷 2023-11-13 오후 4.41.44.png](https://github.com/Heo-y-y/development-blog/assets/112863029/220a3dab-edf7-4686-87af-3cb8e81ef5db)

### Content-Type: 표현 데이터의 형식

![스크린샷 2023-11-13 오후 4.44.03.png](https://github.com/Heo-y-y/development-blog/assets/112863029/c0a8d7c9-79c4-4d90-b384-61875b32a302)

- 미디어 타입, 문자 인코딩

### Content-Encoding: 표현 데이터의 압축 방식

![스크린샷 2023-11-13 오후 4.44.48.png](https://github.com/Heo-y-y/development-blog/assets/112863029/df47fa85-f5b5-4db6-8162-d32e6c511afe)

- 표현 데이터를 압축하기 위해 사용
- 데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가
- 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제

### Content-Language: 표현 데이터의 자연 언어

![스크린샷 2023-11-13 오후 4.45.04.png](https://github.com/Heo-y-y/development-blog/assets/112863029/804e9e2c-5637-4ef2-9c48-de57ea44b15d)

- 표현 데이터의 자연 언어를 표현

### Content-Length: 표현 데이터의 길이

![스크린샷 2023-11-13 오후 4.45.58.png](https://github.com/Heo-y-y/development-blog/assets/112863029/8f67fc01-e03e-405d-bd54-69010e59db7f)

- 바이트 단위
- Transfer-Encoding(전송 코딩)을 사용하면 Content-Length를 사용하면 안됨

## 협상(콘텐츠 네고시에이션)

### 클라이언트가 선호하는 표현 요청

- 협상 헤더는 요청시에만 사용
- Accept: 클라이언트가 선호하는 미디어 타입 전달
- Accept-Charset: 클라이언트가 선호하는 문자 인코딩
- Accept-Encoding: 클라이언트가 선호하는 압축 인코딩
- Accept-Language: 클라이언트가 선호하는 자연 언어

### 협상과 우선순위1 (Quality Values(q))

![스크린샷 2023-11-13 오후 4.51.05.png](https://github.com/Heo-y-y/development-blog/assets/112863029/087e863d-159e-482a-b283-9e28c7644fa5)

- Quality Values(q) 값 사용
- **0~1, 클수록 높은 우선순위**
- 생략하면 1
- Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
    1. ko-KR;q=1 (q생략)
    2. ko;q=0.9
    3. en-US;q=0.8
    4. en;q=0.7

### 협상과 우선순위2 (Quality Values(q))

![스크린샷 2023-11-13 오후 4.53.49.png](https://github.com/Heo-y-y/development-blog/assets/112863029/d282773b-b3aa-4bcd-8bf2-16d0b2712b70)

- 구체적인 것이 우선합니다.
- Accept: **text/***, **text/plain**, **text/plain;format=flowed**, ***/***
    1. text/plain;format=flowed
    2. text/plain
    3. text/*
    4. */*

### 협상과 우선순위3 (Quality Values(q))

- 구체적인 것을 기준으로 미디어 타입을 맞춥니다.
- Accept: **text/***;q=0.3, **text/html**;q=0.7, **text/html;level=1**, **text/html;level=2**;q=0.4, ***/***;q=0.5
    
    ![스크린샷 2023-11-13 오후 4.59.36.png](https://github.com/Heo-y-y/development-blog/assets/112863029/ab4986aa-e197-4fbe-b8a4-075e1dcebae9)
    

## 전송 방식

- Transfer-Encoding
- Range, Content-Range

### 단순 전송

![스크린샷 2023-11-13 오후 5.02.07.png](https://github.com/Heo-y-y/development-blog/assets/112863029/b6e45689-e6db-444c-81d1-d5f8b9310e8c)

- 한번에 전송하고, 한번에 쭈욱 받습니다.

### 압축 전송

![스크린샷 2023-11-13 오후 5.02.22.png](https://github.com/Heo-y-y/development-blog/assets/112863029/0df02725-d957-4b61-94f8-0de6a5936886)

### 분할 전송

![스크린샷 2023-11-13 오후 5.02.44.png](https://github.com/Heo-y-y/development-blog/assets/112863029/0ff84690-1700-4137-a7d9-9a288449318a)

- Content-Length를 넣으면 안됩니다.
- 분할로 쪼개어 전송합니다.

### 범위 전송

![스크린샷 2023-11-13 오후 5.04.38.png](https://github.com/Heo-y-y/development-blog/assets/112863029/38e67018-957d-47e9-919e-e7500eb2f8be)

## 일반 정보

### From: 유저 에이전트의 이메일 정보

- 일반적으로 잘 사용되지 않습니다.
- 검색 엔진 같은 곳에서 주로 사용됩니다.
- 요청에서 사용됩니다.

### Referer: 이전 웹 페이지 주소

- 현재 요청된 페이지의 이전 웹 페이지 주소
- A → B로 이동하는 경우 B를 요청할 때 Referer: A를 포함해서 요청합니다.
- Referer을 사용해서 유입 경로 분석 가능
- 요청에서 사용
- 참고로 referer는 단어 referrer의 오타입니다.

### User-Agent: 유저 에이전트 애플리케이션 정보

- user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7)AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.183 Safari/537.36
- 클라이언트의 애플리케이션 정보(웹 크라우저 정보 등)
- 통계 정보
- 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
- 요청에서 사용

### Server: 요청을 처리하는 ORIGIN 서버의 소프트웨어 정보

- Server: Apache/2.2.22(Debian)
- server: nginx
- 응답에서 사용

### Data: 메시지가 발생한 날짜와 시간

- Data: Tue, 15 Nov 1994 08:12:31 GMT
- 응답에서 사용

## 특별한 정보

### Host: 요청한 호스트 정보(도메인)

- 요청에서 사용
- 필수
- 하나의 서버가 여러 도메인을 처리해야 할 때
- 하나의 IP 주소에 여러 도메인이 적용되어 있을 때

### Location: 페이지 리다이렉션

- 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동(리다이렉트)
- 201(Created): Location 값은 요청에 의해 생성된 리소스 URI
- 3xx(Redirection): Location 값은 요청을 자동으로 리다이렉션하기 위한 대상 리소스를 가리킵니다.

### Allow: 허용 가능한 HTTP 메서드

- 405(Method Not Allowed)에서 응답에 포함해야합니다.
- Allow: GET, HEAD, PUT

### Retry-After: 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간

- 503(Service Unavailable): 서버가 언제까지 불능인지 알려줄 수 있습니다.
- Retry-After: Fri, 31 Dec 1999 23:59:59 GMT(날짜 표기)
- Retry-After: 120(초단위 표기)

## 인증

### Authorization: 클라이언트 인증 정보를 서버에 전달

- Authorization: Basic xxxxxxxxxxxx

### WWW-Authenticate: 리소스 접근시 필요한 인증 방법 저의

- 401 Unauthorized 응답과 함께 사용
- WWW-Authenticate: Newauth realm=“apps”, type=1, title=”Login to \ “apps\””, Basic realm=”simple”

## 쿠키

- Set-Cookie: 서버에서 클라이언트로 쿠키 전달(응답)
- Cookie: 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달

### 쿠키 미사용

쿠키를 사용하지 않으면, 서버 입장에서는 로그인한 사용자인지 아닌지 구분할 방법이 없습니다.

### Stateless

- HTTP는 무상태(Stateless) 프로토콜입니다.
- 클라이언트와 서버가 요청과 응답을 주고 받으면 연결이 끊어집니다.
- 클라이언트가 다시 요청하면 서버는 이전 요청을 기억하지 못합니다.
- 클라이언트와 서버는 서로 상태를 유지하지 않습니다.

대안으로는 모든 요청에 사용자 정보를 포함하는 방법이 있는데, 해당 방식에는 문제가 있습니다.

- 모든 요청에 사용자가 포함되도록 개발을 해야합니다.
- 브라우저를 완전히 종료하고 다시 열게되면 방법이 없습니다.

이 방식들을 해결하기 위해 **쿠키**를 사용합니다.

### 쿠키

쿠키는 모든 요청에 쿠키 정보를 자동으로 포함시킵니다.

하지만 보안에도 문제가 있기 때문에 제약을 둡니다.

- ex) set-cookie: **sessionId=abcde1234**; **expires**=Sat, 26-Dec-2020 00:00:00 GMT; **path**=/; **domain**=.google.com; **Secure**
- 사용처
    - 사용자 로그인 세션 관리
    - 광고 정보 트래킹
- 쿠키 정보는 항상 서버에 전송됩니다.
    - 네트워크 트래픽 추가 유발
    - 최소한의 정보만 사용(세션 id, 인증 토큰)
    - 서버에 전송하지 않고, 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지(localStorage, sessionStorage)
- 보안에 민감한 데이터는 저장하면 안됩니다.(주민번호, 신용카드 번호 등)

### 쿠키 - 생명주기(Expires, max-age)

- Set-Cookie: **expire**=Sat, 26-Dec-2023: 11:24:21 GMT
    - 만료일이 되면 쿠키 삭제
- Set-Cookie: **max-age**=3600(3600초)
    - 0이나 음수를 지정하면 쿠키 삭제
- 세션 쿠키: 만료 날짜를 생략하면 브라우저 종료시 까지만 유지
- 영속 쿠키: 만료 날짜를 입력하면 해당 날짜까지 유지

### 쿠키 - 도메인

- ex) domian=example.org
- **명시: 명시한 문서 기준 도메인 + 서브 도메인 포함**
    - domian=example.org를 지정해서 쿠키 생성
        - example.org는 물론이고, dev.example.org도 쿠키 접근
- **생략: 현재 문서 기준 도메인만 적용**
    - example.org에서 쿠키를 생성하고 domian 지정을 생략
        - example.org에서만 쿠키 접근
        - dev.example.org는 쿠키 미접근

### 쿠키 - 경로

- ex) path=/home
- **해당 경로를 포함한 하위 경로 페이지만 쿠키 접근**
- **일반적으로 path=/ 루트로 지정**

위의 예시를 기준으로 설명하면,

- /home 부터 시작하면 가능합니다.
- 그 이외에는 불가능합니다.

### 쿠키 - 보안

- **Secure**
    - 쿠키는 http, https를 구분하지 않고 전송
    - Secure를 적용하면 https인 경우에만 전송
- **HttpOnly**
    - XSS 공격 방지
    - 자바스크립트에서 접근 불가(document.cookie)
    - HTTP 전송에만 사용
- **SameSite**
    - XSRF 공격 방지
    - 요청 도메인과 쿠키에 설정된 도메인이 같은 경우에만 쿠키 전송

**참고 자료**

- [https://www.inflearn.com/course/http-웹-네트워크/dashboard](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard)
