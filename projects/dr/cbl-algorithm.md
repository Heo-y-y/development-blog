# CBL(소비 기준선) 알고리즘

## Overview

CBL(Consumption Baseline, 소비 기준선)은 DR(Demand Response) 이벤트 시 전력 절감량을 산정하기 위한 기준 사용량을 계산하는 알고리즘입니다.

| 항목 | 설명 |
|------|------|
| 목적 | DR 이벤트 절감량 산정을 위한 기준 사용량 계산 |
| 방식 | Mid 8/10 (중간값 추출 방식) |
| 데이터 | 15분 단위 전력 사용량 데이터 |

---

## 7가지 핵심 지표

| 지표 | 설명 | 계산 방식 |
|------|------|----------|
| **CBL** | 소비 기준선 | Mid 8/10 방식으로 계산된 기준 사용량 |
| **발령날 사용량** | DR 발령일 실제 전력 사용량 | 발령 시간대 실측 데이터 |
| **절감량** | 기준 대비 줄인 전력량 | CBL - 발령날 사용량 |
| **절감률** | 절감 비율 (%) | (절감량 / CBL) × 100 |
| **증감량** | 사용량 변동폭 | 발령날 사용량 - CBL |
| **증감률** | 변동 비율 (%) | (증감량 / CBL) × 100 |
| **표준편차** | 데이터 분산도 | 참조 기간 데이터의 통계적 분산 |

---

## Mid 8/10 계산 방식

### 알고리즘 플로우

```
┌─────────────────────────────────────────────────────────────┐
│                    CBL 계산 프로세스                          │
├─────────────────────────────────────────────────────────────┤
│  1. 발령일 포함 최근 10일 데이터 조회                          │
│                    ↓                                         │
│  2. 발령일 데이터 제외 (9일 데이터)                            │
│                    ↓                                         │
│  3. 내림차순 정렬                                             │
│                    ↓                                         │
│  4. 상위 2개 제거 (이상치 제거)                               │
│                    ↓                                         │
│  5. 하위 2개 제거 (이상치 제거)                               │
│                    ↓                                         │
│  6. 중간 6개 값의 평균 = CBL                                  │
└─────────────────────────────────────────────────────────────┘
```

### 핵심 계산 로직

```java
public double calculateCbl(List<UsageData> usageDataList, NiaCblInfo cblConfig) {
    List<Double> finalValues = usageDataList.stream()
        .skip(1)  // 발령일 제외
        .map(UsageData::getUsed)
        .sorted(Collections.reverseOrder())  // 내림차순 정렬
        .skip(cblConfig.getRmTop())          // 상위 이상치 제거
        .limit(cblConfig.getUsedDays())      // 사용할 일수만큼 추출
        .collect(Collectors.toList());

    return finalValues.stream()
        .mapToDouble(Double::doubleValue)
        .average().orElse(0.0);
}
```

### 설정 파라미터

| 파라미터 | 기본값 | 설명 |
|----------|--------|------|
| TOTAL_DAYS | 10 | 총 조회 일수 |
| USED_DAYS | 6 | 실제 계산에 사용할 일수 |
| RM_TOP | 2 | 상위 제거 개수 |
| RM_BOTTOM | 2 | 하위 제거 개수 |

---

## 데이터베이스 스키마

### CBL 설정 테이블

```sql
CREATE TABLE nia_cbl_info (
    CBL_TYPE varchar(15) PRIMARY KEY,  -- CBL 타입 코드
    CBL_NAME varchar(20),              -- CBL 명칭
    TOTAL_DAYS int(11),                -- 총 조회 일수
    USED_DAYS int(11),                 -- 실제 사용 일수
    RM_BOTTOM int(11),                 -- 하위 제거 개수
    RM_TOP int(11),                    -- 상위 제거 개수
    RM_TYPE varchar(10)                -- 제거 방식 타입
);
```

### 시간대별 전력 사용량 테이블

```sql
CREATE TABLE nia_elec_hour (
    STORE_SEQ int(11),                 -- 수용가 고유번호
    ELEC_DATE date,                    -- 측정 일자
    ELEC_TIME varchar(4),              -- 측정 시간 (HHMM)
    USED double,                       -- 사용량 (kWh)
    PRIMARY KEY (STORE_SEQ, ELEC_DATE, ELEC_TIME)
);
```

### 관련 테이블 구조

| 테이블 | 용도 |
|--------|------|
| **nia_store_info** | 수용가(세대) 기본 정보 |
| **nia_dr_info** | DR 이벤트 발령 정보 |
| **nia_cbl_info** | CBL 계산 설정 |
| **nia_holidays** | 공휴일 정보 (CBL 계산 시 제외) |
| **nia_elec_hour** | 시간대별 전력 사용량 |

---

## 최적화 전략

### 1. CBL 타입별 그룹화

```java
// CBL 타입별로 수용가를 그룹화하여 일괄 처리
Map<String, List<Store>> storesByCblType = stores.stream()
    .collect(Collectors.groupingBy(Store::getCblType));

storesByCblType.forEach((cblType, storeGroup) -> {
    NiaCblInfo cblConfig = cblConfigMap.get(cblType);
    // 동일 설정으로 일괄 계산
    calculateBatch(storeGroup, cblConfig);
});
```

**효과**: 설정 조회 95% 감소

### 2. 배치 쿼리 최적화

```java
// Before: N+1 쿼리 문제
stores.forEach(store -> {
    List<UsageData> data = repository.findByStoreSeq(store.getSeq());
});

// After: 단일 배치 쿼리
List<Integer> storeSeqs = stores.stream()
    .map(Store::getSeq)
    .collect(Collectors.toList());
Map<Integer, List<UsageData>> dataMap = repository.findByStoreSeqIn(storeSeqs);
```

**효과**: 쿼리 횟수 99% 감소

### 3. Stream API 활용

```java
// 함수형 프로그래밍으로 가독성 및 성능 향상
double cbl = usageDataList.stream()
    .skip(1)
    .map(UsageData::getUsed)
    .sorted(Collections.reverseOrder())
    .skip(rmTop)
    .limit(usedDays)
    .mapToDouble(Double::doubleValue)
    .average()
    .orElse(0.0);
```

---

## 특수 케이스 처리

| 케이스 | 처리 방식 |
|--------|----------|
| **공휴일** | CBL 계산 시 제외 (nia_holidays 테이블 참조) |
| **주말** | 별도 CBL 타입 적용 (주말용 설정) |
| **데이터 부족** | 최소 필요 일수 미달 시 계산 스킵 |
| **이상치** | 상/하위 제거로 자동 필터링 |
| **미세먼지 이벤트** | 서울시 자체 발령, 별도 보상 체계 |

---

## 아키텍처

```
┌─────────────────────────────────────────────────────────────┐
│                     CBL 계산 서비스                          │
├─────────────────────────────────────────────────────────────┤
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐     │
│  │   Batch     │───→│  Calculator │───→│   Store     │     │
│  │   Service   │    │   Engine    │    │   (DB)      │     │
│  └─────────────┘    └─────────────┘    └─────────────┘     │
│         │                  │                  │             │
│         ↓                  ↓                  ↓             │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐     │
│  │    DR       │    │    CBL      │    │   Usage     │     │
│  │   Event     │    │   Config    │    │   Data      │     │
│  └─────────────┘    └─────────────┘    └─────────────┘     │
└─────────────────────────────────────────────────────────────┘
```

### 계층 구조

| 계층 | 역할 | 구성요소 |
|------|------|----------|
| **Boot** | 애플리케이션 진입점 | Spring Boot Application |
| **Domain** | 비즈니스 로직 | CblCalculationLogic, CblCalculationUtil |
| **Service** | 서비스 레이어 | CblBatchService, DrEventService |
| **Store** | 데이터 접근 | MyBatis Mapper, Repository |

---

## 성과 지표

| 지표 | 수치 |
|------|------|
| 계산 대상 | 2,363세대 |
| 데이터 갱신 | 15분 단위 |
| 배치 실행 | DR 이벤트 발령 시 |
| 쿼리 최적화 | 99% 감소 |
| 설정 조회 최적화 | 95% 감소 |

---

[← Back to DR Service](./README.md) | [← Back to Portfolio](../../README.md)
