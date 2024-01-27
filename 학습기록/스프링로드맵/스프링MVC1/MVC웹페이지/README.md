# 스프링 MVC - 웹 페이지 만들기

## 요구사항

상품을 관리하는 서비스를 만들어보자.

**상품 도메인 모델**

- 상품 ID
- 상품명
- 가격
- 수량

**상품 관리 기능**

- 상품 목록
- 상품 상세
- 상품 등록
- 상품 수정

### **서비스 화면**

![스크린샷1 2024-01-27 오후 3 26 07](https://github.com/Heo-y-y/development-blog/assets/112863029/e7470170-5130-4fea-9cde-1b8eec528432)

![스크린샷2 2024-01-27 오후 3 26 18](https://github.com/Heo-y-y/development-blog/assets/112863029/cd3af269-e90a-41f3-a0b7-6f0be7203867)

![스크린샷3 2024-01-27 오후 3 26 29](https://github.com/Heo-y-y/development-blog/assets/112863029/320c24d7-82cf-4c35-a3ab-60cecd4cbb23)

![스크린샷4 2024-01-27 오후 3 26 46](https://github.com/Heo-y-y/development-blog/assets/112863029/b825757c-a556-40f0-ab51-568319d1afed)

### 서비스 제공 흐름

![스크린샷5 2024-01-27 오후 3 27 05](https://github.com/Heo-y-y/development-blog/assets/112863029/4c098e8d-fa6b-485b-bace-e241f9c80938)

요구사항이 정리되소 디자이너, 웹 퍼블리셔, 벡엔드 개발자가 업무를 나누어 진행된다고 가정하자.

- **디자이너**: 요구사랑에 맞도록 디자인하고, 디자인 결과물을 웹 퍼블리셔에게 전달
- **웹 퍼블리셔**: 디자이너에게 받은 디자인을 기반으로 HTML, CSS를 만들어 개발자에게 제공
- **백엔드 개발자**: 디자이너, 웹 퍼블리셔를 통해서 HTML 화면이 나오기 전까지 시스템을 설계하고, 핵심 비즈니스 모델을 개발, 이후 HTML이 나오면 이 HTML을 뷰 템플릿으로 변환해서 동적으로 화면을 그리고, 또 웹 화면의 흐름을 제어

---

**참고 자료**

- [https://www.inflearn.com/course/스프링-mvc-1/dashboard](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard)
