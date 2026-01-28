# DR 수요자원 서비스

## Overview

| 항목 | 내용 |
|------|------|
| 기간 | 2024.08 ~ 현재 (운영 중) |
| 역할 | 백엔드 개발 |
| 규모 | 11개 마이크로서비스 |
| 설명 | 전력 피크 시간대 시민 참여형 수요자원(DR) 절감 서비스 플랫폼 |

---

## Tech Stack

| 분류 | 기술 |
|------|------|
| Backend | Java 17/21 / Spring Boot 3 / JPA / MyBatis / Spring Cloud |
| Database | MariaDB / PostgreSQL / TimescaleDB / InfluxDB |
| Messaging | Kafka / ActiveMQ |
| Infra | Docker / Kubernetes / GitLab CI/CD |

---

## Architecture

> 아키텍처 다이어그램 추가 예정

---

## What I Did

- 11개 마이크로서비스 아키텍처 설계: KPX 연동, 포인트, 알림 등 도메인별 서비스 분리
- 대용량 데이터 파이프라인: 5종 에너지 데이터(전기 140GB/7개월) 수집·적재 시스템 구축
- 실시간 이벤트 처리 및 외부 연동: Kafka로 서울시 DR 발령 이벤트 실시간 수신
- 1인 배치 서비스 설계/구현: CBL 계산, 정산, 리포트 생성 자동화
- 레거시 시스템 리팩토링: JPA→MyBatis 전환으로 복잡 쿼리 성능 개선

---

## Metrics

| 지표 | 수치 |
|------|------|
| 처리 데이터량 | 140GB+ (7개월) |
| 마이크로서비스 수 | 11개 |

---

[← Back to Portfolio](../../README.md)
