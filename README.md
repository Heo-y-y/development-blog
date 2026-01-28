# Heo-y-y | Backend Developer

> 데이터 기반 시스템 설계와 AI 자동화로 개발 생산성을 혁신하는 백엔드 개발자

---

## Skills

| 분류 | 기술 |
|------|------|
| Backend | Java / Spring Boot / JPA / MyBatis |
| Frontend | TypeScript / Next.js / Tailwind CSS / Prisma |
| Data | MySQL / MariaDB / Redis / Kafka / RabbitMQ |
| DevOps | Git / GitLab / Docker |
| AI & Automation | Claude / Gemini / MCP / Antigravity / AI Agent / Multi-Agent |

---

## Work Experience

### 누리플렉스

#### [생거진천 에너지 정보 시스템](projects/sjc/README.md)
2025.01 ~ 2025.03 (개발 중) | 진천군 + 한전 협력

🛠️ Java 8, Spring Boot 2.2.6, MyBatis, Spring Security, Next.js 15, React 19, Vue 3, TypeScript, TailwindCSS, MariaDB, Redis, Docker, GitLab CI/CD

- 아토믹 디자인 시스템 적용: Figma MCP로 디자인 토큰 연동, 컴포넌트 기반 UI 아키텍처로 일관된 디자인 시스템 구축
- 한전 요금 계산 인터페이스 개발: 6개 요금종별(주택용 저압/고압, 일반용 고압 A/B, 심야전력, EV충전) Strategy 패턴 기반 계산 엔진 연동
- 권한 기반 접근 제어 시스템: auth/user/login 서비스에 AOP 기반 권한 관리 구현, 역할별 페이지 접근 및 API 필터링
- LwM2M 기반 장치 제어 화면 기획: TOU(시간대별 요금) 장치 연동을 위한 Vue 프론트엔드 화면 기획 및 백엔드 연결
- 풀스택 개발: Next.js 15 기반 관리자 웹 + Spring Boot Admin Service 개발, JWT 인증 및 React Query 기반 데이터 페칭
- API 문서 생성 자동화: Claude Code 에이전트 설계로 기획서→Excel 문서화 워크플로우 구축

---

#### [DR 수요자원 서비스](projects/dr/README.md)
2024.08 ~ 현재 (운영 중) | 2,365세대 / 18단지 | 전주시 DR 성공률 43.7%

🔗 [전자신문 기사](https://www.etnews.com/20260120000365) | [Google Play](https://play.google.com/store/apps/details?id=com.nuriflex.myenergy&hl=ko)

🛠️ Java 17/21, Spring Boot 3.x, JPA/MyBatis, Next.js 15, MySQL, Redis, Kafka, RabbitMQ, Docker

- 11개 마이크로서비스 아키텍처 설계: KPX 연동, 데이터 수집, 계산, 메시지 발송 등 도메인별 독립 서비스로 분리, OADR 5.1~5.4 표준 프로토콜 직접 구현
- 대용량 데이터 파이프라인: 5종 에너지 데이터(전기 140GB/7개월) 수집·적재 시스템 구축, DB 튜닝(innodb_buffer_pool 128MB→20GB)으로 조회 성능 대폭 개선, 파티셔닝/콜드존/인덱싱 적용
- 실시간 이벤트 처리 및 외부 연동: Kafka로 서울시 DR 발령 이벤트 실시간 수신, RabbitMQ로 2,365세대 메시지 발송, 한전·PST·GND 등 3개 외부 시스템 15분 단위 양방향 연계
- 1인 배치 서비스 설계/구현: CBL 계산, 감축량 정산, 적립금 발송, DR 발령 예약 등 핵심 비즈니스 로직 배치 시스템 개발
- 레거시 시스템 리팩토링: JPA→MyBatis 전환으로 복잡 쿼리 성능 개선, HTML/JS→Next.js 15 마이그레이션, Redis 기반 세션/토큰 및 역할별 접근 제어

---

#### [전주 에너지 플랫폼](projects/jeonju/README.md)
2024.04 ~ 2024.07 (개발) | 현재 운영/유지보수 중

🔗 [jeonju.myenergy.co.kr](https://jeonju.myenergy.co.kr/)

🛠️ Next.js, TypeScript, Prisma, MySQL, Tailwind CSS

- 전주시 공동주택 에너지 사용량 시각화 대시보드 풀스택 단독 개발
- 공공데이터포털 API 연동 + ApexCharts/Kakao Maps 기반 데이터 시각화
- 부분 문자열 조합 기반 검색 알고리즘 구현 (매칭 횟수 정렬)
- Prisma ORM 다중 조건 쿼리 + SWR 클라이언트 상태 관리

---

## Contact

[![GitHub](https://img.shields.io/badge/GitHub-181717.svg?&style=flat-square&logo=github&logoColor=white)](https://github.com/Heo-y-y)
[![Gmail Badge](https://img.shields.io/badge/Gmail-d14836?style=flat-square&logo=Gmail&logoColor=white&link=mailto:localhost8586@gmail.com)](mailto:localhost8586@gmail.com)

**Resume**: [Notion](https://common-mouth-660.notion.site/620dcd4ee97c4f9f9611771b65c7794d)
