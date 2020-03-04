---
layout: default
title: API Doc
nav_order: 1
parent: 작업실행관리 시스템
---
# API

## 시스템 시작
Job Instance 대기열에 있는 작업을 실행하는 루프를 시작합니다.

GET/POST
```
/api/v1/start
```

## 시스템 정지
Job Instance 대기열에 있는 작업을 실행하는 루프를 정지합니다.

GET/POST
```
/api/v1/stop
```

## 요청 작업 시작
요청한 Job ID 에 해당하는 작업의 JobInstance를 만들어 실행대기열에 추가합니다.

Query string이나 POST 요청의 Payload에 Json 형태로 파라미터를 줄 수 있습니다. 파라미터는 문자열 형태만 지원합니다.

```
{
    "sender":"송신자",
    "from":"20200101",
    "to":"20200102"
}
```


GET/POST
```
/api/v1/job/{jobId}
```


## 작업 재시작
작업을 시작했으나 실패하여 대기열을 막고 있는 잡을 재시작합니다.


GET/POST
```
/api/v1/retry/{jobInstanceId}
```

## 작업 취소
작업을 시작했으나 실패하여 대기열을 막고 있는 잡을 취소합니다. 취소되는 즉시 그 다음 작업을 시작합니다.

GET/POST
```
/api/v1/drop/{jobInstanceId}
```

## 작업 삭제
작업 대기열에 있는 잡을 삭제합니다. 대기열을 막고있거나 실행중인 잡은 삭제할 수 없습니다.

GET/POST
```
/api/v1/remove/{jobInstanceId}
```

## 동시 실행 개수 변경
동시에 실행할 수 있는 개수를 변경합니다.

GET/POST
```
/api/v1/size/{size}
```

## 잡 인스턴스 대기열 조회
잡 인스턴스 대기열에 있는 작업리스트를 조회합니다.

GET/POST
```
/api/v1/report/front
```

## 실행 중인 잡 인스턴스 조회
실행중인 잡 인스턴스를 조회합니다.


GET/POST
```
/api/v1/report/execution
```

## 실행 대기 중인 잡 인스턴스 조회
메인 큐에서 대기중인 잡 인스턴스를 조회합니다.

GET/POST
```
/api/v1/report/waiting
```


## 현재 전체 대기 상태 조회
현대 대기 상태를 조회합니다.


GET/POST
```
/api/v1/report/current
```

## 실행 실적 조회
실행실적을 조회합니다.


GET/POST
```
/api/v1/report/result/{from}/{to}
```



# 현재 버전
1.0.0-SNAPSHOT
{: .fs-6 .fw-300 }
