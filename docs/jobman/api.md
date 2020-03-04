---
layout: default
title: API Doc
nav_order: 1
parent: 작업실행관리 시스템
---
# API

## 시스템 시작
```url
/api/v1/start
```

## 시스템 정리
```url
/api/v1/stop
```

## 요청 작업 시작
```url
/api/v1/job/{jobId}
```

## 작업 재시작
```url
/api/v1/retry/{jobInstanceId}
```

## 작업 취소
```
/api/v1/drop/{jobInstanceId}
```

## 작업 삭제
```url
/api/v1/remove/{jobInstanceId}
```

## 동시 실행 개수 변경
```
/api/v1/size/{size}
```

## 잡 인스턴스 대기열 조회
```
/api/v1/report/front
```

## 실행 중인 잡 인스턴스 조회
```
/api/v1/report/execution
```

## 실행 대기 중인 잡 인스턴스 조회
```
/api/v1/report/waiting
```


## 현재 전체 대기 상태 조회
```
/api/v1/report/current
```

## 실행 실적 조회
```
/api/v1/report/result/{from}/{to}
```



# 현재 버전
1.0.0-SNAPSHOT
{: .fs-6 .fw-300 }
