# DR 수요자원 서비스

## 📋 Overview

| 항목 | 내용 |
|------|------|
| 기간 | 2024.04 ~ 현재 |
| 역할 | 백엔드 개발 |
| 규모 | 11개 마이크로서비스 |

---

## 🛠️ Tech Stack

| 분류 | 기술 |
|------|------|
| Backend | Java 17 / Spring Boot 3 / JPA / QueryDSL / Spring Cloud |
| Database | MariaDB / PostgreSQL / TimescaleDB / InfluxDB |
| Messaging | Kafka / ActiveMQ |
| Infra | Docker / Kubernetes / GitLab CI/CD |

---

## 🏗️ Architecture

> 아키텍처 다이어그램 추가 예정

---

## 💪 What I Did

### 1. MSA 아키텍처 설계 및 운영
- 11개 마이크로서비스 설계 및 운영
- Spring Cloud 기반 서비스 간 통신 구현

### 2. 대용량 데이터 파이프라인 구축
- 5종 에너지 데이터 수집·적재 시스템 구축
- 전기 데이터 140GB+ (7개월) 처리

### 3. DB 성능 튜닝
- innodb_buffer_pool 128MB → 20GB 최적화
- 파티셔닝 / 콜드존 / 인덱싱 적용
- 조회 성능 대폭 개선

### 4. 실시간 이벤트 처리
- Kafka 기반 이벤트 스트리밍
- 외부 시스템 연동 (KPX, 한전 등)

### 5. 레거시 시스템 리팩토링
- 기존 모놀리식 시스템의 MSA 전환
- 코드 품질 개선 및 테스트 커버리지 향상

---

## 📊 Metrics

| 지표 | 수치 |
|------|------|
| 처리 데이터량 | 140GB+ (7개월) |
| 마이크로서비스 수 | 11개 |
| DB 버퍼 풀 최적화 | 128MB → 20GB |

---

[← Back to Portfolio](../../README.md)
