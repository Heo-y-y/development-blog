# HTTP 메서드 활용

## 클라이언트에서 서버로 데이터 전송

데이터를 전달하는 방식은 크게 2가지가 있습니다.

- **쿼리 파라미터를 통한 데이터 전송**
    - GET
    - 주로 정렬 필터(검색어)
- **메시지 바디를 통한 데이터 전송**
    - POST, PATCH, PUT
    - 회원가입, 상품 주문, 리소스 등록, 리로스 변경

클라이언트에서 서버로 데이터를 전송하는 상황을 4가지로 정리하면,

### **정적 데이터 조회**

- 이미지, 정적 텍스트 문서
- 조회는 GET 사용
- 정적 데이터는 일반적으로 쿼리 파라미터 없이 리소스 경로로 단순하게 조회 가능
    
    ![스크린샷 2023-11-01 오후 4.26.29.png](https://github.com/Heo-y-y/development-blog/assets/112863029/409347fc-53b6-4ae8-90e0-93bd085af662)
    

### **동적 데이터 조회**

- 주로 검색, 게시팔 목록에서 정렬 필터(검색어)
- 조회 조건을 줄여주는 필터, 조회 결과를 정렬하는 정렬 조건에 주로 사용
- 조회는 GET 사용
- GET은 쿼리 파라미터를 사용해서 데이터를 전달
    
    ![스크린샷 2023-11-01 오후 4.26.44.png](https://github.com/Heo-y-y/development-blog/assets/112863029/74b9d062-db14-4404-86ab-c531d38d53b3)
    
### **HTML Form 데이터 전송**

- HTML Form submit 시 POST 전송	
    
    ![스크린샷 2023-11-01 오후 4.34.10.png](https://github.com/Heo-y-y/development-blog/assets/112863029/c0a048ba-7f77-48d2-9138-e8bbf99628c7)
    
    - ex) 회원가입, 상품 주문, 데이터 변경
- Content-Type: `application/x-www-form-urlencoded` 사용
    - form의 내용을 메시지 바디를 통해서 전송(`key=value`, 쿼리 파라미터 형식)
    - 전송 데이터를 url encoding 처리
        - ex) abc김 → abc%EA%B9%80
- HTML Form은 GET 전송도 가능
    
    ![스크린샷 2023-11-01 오후 4.36.00.png](https://github.com/Heo-y-y/development-blog/assets/112863029/670609f9-5b8d-48f1-a47b-b1fdd4a2ff1e)
    
   
- Content-Type: `multipart/form-data`
    
    ![스크린샷 2023-11-01 오후 4.36.58.png]( https://github.com/Heo-y-y/development-blog/assets/112863029/deaa4068-6fc6-4c0c-8aaf-c68d4d927282)
    
    - 파일 업로드 같은 바이너리 데이터 전송시 사용
    - 다른 종류의 여러 파일과 폼의 내용 함께 전송 가능
- 참고로 HTML Form 전송은 **GET, POST만 지원**합니다.

### **HTTP API 데이터 전송**

![스크린샷 2023-11-01 오후 4.41.33.png](https://github.com/Heo-y-y/development-blog/assets/112863029/98f78f00-1c18-4762-b72f-a80cd8eabf77)

- 서버 to 서버
    - 백엔드 시스템 통신
- 앱 클라이언트
    - 아이폰, 안드로이드
- 웹 클라이언트
    - HTML에서 Form 전송 대신 자바 스크립트를 통한 통신에 사용(AJAX)
    - ex) React, VueJs 같은 웹 클라이언트와 API 통신
- POST, PUT, PATCH: 메시지 바디를 통해 데이터 전송
- GET: 조회, 쿼리 파라미터로 데이터 전달
- Content-Type: `application/json`을 주로 사용(사실상 표준)
    - TEXT, XML, JSON 등

## HTTP API를 설계하는 방법

### **HTTP API - 컬렉션**

- **POST 기반 등록**
- ex) 회원 관리 API 제공
    - **회원** 목록 /members → **GET**
    - **회원** 등록 /members → **POST**
    - **회원** 조회 /members/{id} → **GET**
    - **회원** 수정 /members/{id} → **PATCH, PUT, POST**
    - **회원** 삭제 /members/{id} → **DELETE**
- **POST 신규 자원 등록 특징**
    - 클라이언트는 등록될 리소스의 URI를 모릅니다.
    - 서버가 새로 등록된 리소스 URI를 생성해줍니다.
        - HTTP/1.1 201 Created
        Location: **/members/100**
    - 컬렉션(Collection)
        - 서버가 관리하는 리소스 디렉토리
        - 서버가 리소스의 URI를 생성하고 관리
        - 여기서 컬렉션은 /members

- **HTTP API - 스토어**
    - **PUT 기반 등록**
    - ex) 정적 컨텐츠 관리, 원격 파일 관리
        - **파일** 목록 /files → **GET**
        - **파일** 조회 /files/{filename} → **GET**
        - **파일** 등록 /files/{filename} → **PUT**
        - **파일** 삭제 /files/{filename} → **DELETE**
        - **파일** 대량 등 /files → **POST**
    - PUT 신규 자원 등록 특징
        - 클라이언트가 리소스 URI를 알고 있어야 합니다.
            - 파일 등록 / files{filename} → PUT
            - PUT **/files/star.jpg**
        - 클라이언트가 직접 리소스의 URI를 지정합니다.
        - 스토어(Store)
            - 클라이언트가 관리하는 리소스 저장소
            - 클라이언트가 리소스의 URI를 알고 관리
            - 여기서 스토어는 /files

- **HTML FORM 사용**
    - HTML FORM은 **GET, POST**만 지원합니다.
    - **컨트롤 URI**
        - GET, POST만 지원하므로 제약이 있습니다.
        - 이런 제약을 해결하기 위해 동사로 된 리소스 경로를 사용
        - POST의 /new, /edit, /delete가 컨트롤 URI
        - HTTP 메서드로 해결하기 애매한 경우(HTTP API 포함)
    - AJAX 같은 기술을 사용해서 해결 가능
    - 여기서는 순수 HTML + HTML form 사용
        - **회원** 목록 /members → **GET**
        - **회원** 등록 폼 /members/new → **GET**
        - **회원** 등록 /members/new, /members → **POST**
        - **회원** 조회 /members/{id} → **GET**
        - **회원** 수정 폼 /members/{id}/edit → **GET**
        - **회원** 수정 /members/{id}/edit, /members/{id} → **POST**
        - **회원** 삭제 /members/{id}/delete → **POST**

**참고 자료**

- [https://www.inflearn.com/course/http-웹-네트워크/dashboard](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard)
