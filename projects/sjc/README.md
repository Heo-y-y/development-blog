# 생거진천 에너지 정보 시스템

## 📋 Overview

| 항목 | 내용 |
|------|------|
| 기간 | 2025.01 ~ 현재 (개발 중) |
| 역할 | 풀스택 개발 + 화면 기획 |
| 협력사 | 진천군 + 한전 |
| 대상 | 진천시장 상가 에너지 관리 |

---

## 🛠️ Tech Stack

| 분류 | 기술 |
|------|------|
| Backend | Java 8 / Spring Boot 2.2.6 / MyBatis / Spring Security |
| Frontend | Next.js 15 / React 19 / Vue 3 / TypeScript / TailwindCSS |
| Database | MariaDB |
| Infra | Docker / GitLab CI/CD |

---

## 🏗️ Architecture

> 아키텍처 다이어그램 추가 예정

---

## 💪 What I Did

### 1. 아토믹 디자인 시스템 적용
- Figma MCP로 디자인 토큰 연동
- 컴포넌트 기반 UI 아키텍처로 일관된 디자인 시스템 구축

### 2. 한전 요금 계산 인터페이스 개발
- 6개 요금종별 Strategy 패턴 기반 계산 엔진 연동
  - 주택용 저압/고압
  - 일반용 고압 A/B
  - 심야전력
  - EV충전

### 3. 권한 기반 접근 제어 시스템
- auth/user/login 서비스에 AOP 기반 권한 관리 구현
- 역할별 페이지 접근 및 API 필터링

### 4. LwM2M 기반 장치 제어 화면 기획
- TOU(시간대별 요금) 장치 연동을 위한 Vue 프론트엔드 화면 기획
- 백엔드 연결 설계

### 5. 풀스택 개발
- Next.js 15 기반 관리자 웹 개발
- Spring Boot admin-api 개발
- JWT 인증 및 React Query 기반 데이터 페칭

### 6. API 문서 생성 자동화
- Claude Code 에이전트 설계로 기획서→Excel 문서화 워크플로우 구축

---

## 📊 Metrics

> 프로젝트 완료 후 성과 지표 추가 예정

---

[← Back to Portfolio](../../README.md)
