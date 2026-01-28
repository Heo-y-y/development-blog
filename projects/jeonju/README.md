# 전주 에너지 정보 플랫폼

## Overview

| 항목 | 내용 |
|------|------|
| 기간 | 2024.04 ~ 2024.07 (개발) / 현재 운영 중 |
| 역할 | 풀스택 단독 개발 |
| 규모 | 365세대 / 18단지 공동주택 에너지 플랫폼 |
| 링크 | [jeonju.myenergy.co.kr](https://jeonju.myenergy.co.kr/) |

---

## Tech Stack

| 분류 | 기술 |
|------|------|
| Frontend | Next.js 13 / React 18 / TailwindCSS |
| Backend | Next.js API Routes |
| Database | MySQL / Prisma ORM |
| 시각화 | ApexCharts |
| 지도 | 카카오맵 API |
| 외부 API | 전력거래소(KPX) 공공데이터 |

---

## Architecture

> 아키텍처 다이어그램 추가 예정

---

## What I Did

- 전주시 공동주택 에너지 사용량 시각화 대시보드 풀스택 단독 개발
- 공공데이터포털 API 연동 + ApexCharts/Kakao Maps 기반 데이터 시각화
- 부분 문자열 조합 기반 검색 알고리즘 구현 (매칭 횟수 정렬)
- Prisma ORM 다중 조건 쿼리 + SWR 클라이언트 상태 관리

---

## Key Features

- 에너지 데이터 시각화 대시보드 (도넛/바/라인/스택드 컬럼 차트)
- 탄소 절감 캠페인 - 탄소 절감량을 나무 그루 수로 환산
- 실시간 전력수급 현황 (5분 단위 갱신)
- 지역별 에너지 분석 (행정구역별 소비량 + 태양광 설비 지도)

---

## Metrics

| 지표 | 수치 |
|------|------|
| 운영 규모 | 365세대 / 18단지 |
| DR 성공률 | 43.7% |
| 개발 기간 | 6개월 |
| 데이터 갱신 | 5분 (실시간 전력수급) |

---

[← Back to Portfolio](../../README.md)
