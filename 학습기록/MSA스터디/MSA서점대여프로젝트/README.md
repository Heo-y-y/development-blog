## 학습 설명
사전에 알고리즘API를 이용한 [Spring Cloud OpenFeign](../오픈페인알고리즘.md)을 학습하고, 스터디원들과 간단한 서점 대여 서비스를 만들어보는 프로젝트를 진행했습니다.

우선 인원이 3명이여서 `LoanService`을 빼고 나머지 3가지 서비스를 각각 맡아서 진행하기로 했었는데, 제가 `HistoryService`를 담당하기로 했었지만, 스터디원 1명이 중간에 빠져야하는 상황이 생겨 추가적으로 `LoanService`와 `BookService`까지 구현하게 됐습니다.

### Repository
📎 **[GitHub](https://github.com/Heo-y-y/study_toy_MSA)**

### 서비스 관계 흐름
![](https://github.com/heo-mewluee-Study-Group/cs-study/assets/112863029/a79613cc-4aff-4309-97a8-7e1ff44f8f0a)

그림을 보면, 과제의 **메인 흐름을 담당**하는 `LoanService`를 통해 대여 서비스를 주고 받습니다.

그리고 **대여 기록을 담당**(**거의 저장소 역할**)을 하는 `HistoryService`를 통해 실시간으로 변경되는 데이터를 기록하여 **다른** `Service`**가 의 데이터를 사용하는 흐름**입니다.

일단 `UserService`와 `BookService`가 `HistoryService`에서 조회해야하기 때문에 제가 대여 횟수와 책 잔여 개수를 보내주는 mock test용 server를 만들기로 했습니다.

그 뒤에 `HistroyService` → `BookService`를 수정하고 → `LoanService`를 구현했습니다.

### API 요구사항 명세서 및 테이블 구조
- [API 요구사항 명세서 및 테이블 구조](요구사항명세서및테이블구조.md)

### Study Content
- [LoanService](LoanService.md)
- [BookService](BookService.md)
- [HistoryService](history/README.md)
- [MockTestService](MockTestService.md)
