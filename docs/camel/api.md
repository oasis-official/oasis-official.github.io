---
layout: default
title: API Doc
nav_order: 1
parent: 메시지 전송 중계 시스템
---
# API

## 시스템 시작
대기열에 있는 Message를 전송하는 루프를 시작합니다.

GET/POST
```
/api/v1/start
```

## 시스템 정지
대기열에 있는 Message를 전송하는 루프를 정지합니다.

GET/POST
```
/api/v1/stop
```

## 시스템 현재상태
시스템의 현재 상태를 조회합니다.

GET
```
/api/v1/status
```
#### 응답 예
```json
{
    "status": "SUCCESS",
    "message": "current system status",
    "responseHeaders": {
        "Content-Type": [
            "application/json; charset=UTF-8"
        ]
    },
    "report": {
        "status": "Started"
    }
}
```


## Protocol 등록
신규 Protocol을 등록합니다.

```json
{
    "protocolId" : "99",
    "topicId" : "1",
    "protocolName" : "99name",
    "sender" : "mock",
    "protocolType" : "BlockingExclusive",
    "url" : "http://test"
}
```

POST
```
/api/v1/register
```

## Message 전송 대기열에 추가
요청한 Protocol ID 에 해당하는 Message를 만들어 대기열에 추가합니다.

Query string이나 POST 요청의 Payload에 Json 형태로 파라미터를 줄 수 있습니다. 파라미터는 문자열 형태만 지원합니다.

```json
{
    "sender":"송신자",
    "params":{
        "from":"20200101",
        "to":"20200102"
    }
}
```


GET/POST
```
/api/v1/add/{protocolId}
```

#### 응답 예
```json
{
    "status": "SUCCESS",
    "message": "Protocol 3 has added",
    "messageId": "3_20200327172844_I_87494c1e-3271-43aa-9c46-9d40f1054558",
    "protocolId": "3"
}
```



## Message 재전송
Message 전송을 했으나 실패하여 대기열을 막고 있는 Message을 재시작합니다.


GET/POST
```
/api/v1/retry/{messageId}
```

## Message 전송 취소
전송을 시작했으나 실패하여 대기열을 막고 있는 Message를 취소합니다. 취소되는 즉시 그 다음 Message 전송을 시작합니다.

GET/POST
```
/api/v1/drop/{messageId}
```

## Message 삭제
대기열에 있는 Message를 삭제합니다. 대기열을 막고있거나 실행중인 Message는 삭제할 수 없습니다.

GET/POST
```
/api/v1/remove/{messageId}
```

## 동시 전송 개수 변경
동시에 전송할 수 있는 개수를 변경합니다.

GET/POST
```
/api/v1/size/{size}
```

## 동시 전송 개수 조회
동시에 전송할 수 있는 개수를 조회합니다.

GET/POST
```
/api/v1/report/size
```
#### 응답 예
```json
{
    "status": "SUCCESS",
    "message": "max queue size",
    "report": {
        "maxSlotSize": 3
    }
}
```

## Message 대기열 조회
Message 전송 대기열에 있는 Message를 조회합니다.

GET/POST
```
/api/v1/report/front
```
#### 응답 예
```json
{
    "status": "SUCCESS",
    "message": "front queue report",
    "report": {
        "frontQueuedMessages": []
    }
}
```

## 전송 중인 Message 조회
전송 중인 Message를 조회합니다.


GET/POST
```
/api/v1/report/sending
```
#### 응답 예
```json
{
    "status": "SUCCESS",
    "message": "sending transaction report",
    "report": {
        "sendingTransactions": []
    }
}
```


## 전송 대기 중인 Message 조회
메인 큐에서 대기중인 Message를 조회합니다.

GET/POST
```
/api/v1/report/waiting
```
#### 응답 예
```json
{
    "status": "SUCCESS",
    "message": "waiting transaction report",
    "report": {
        "mainMessageQueues": [
            [],
            [],
            []
        ],
        "waitingMessageQueues": []
    }
}
```


## 현재 전체 대기 상태 조회
현재 대기 상태를 조회합니다.


GET/POST
```
/api/v1/report/current
```

#### 응답 예

```json
{
    "status": "SUCCESS",
    "message": "current status",
    "report": {
        "frontQueuedMessages": [],
        "mainMessageQueues": [
            [],
            [],
            []
        ],
        "waitingMessageQueues": [],
        "sendingTransactions": []
    }
}
```

## Protocol 조회
시스템에 등록되어 있는 Protocol을 전체 조회합니다.

GET/POST
```
/api/v1/report/protocols
```

#### 응답 예
```json
{
  "responseHeaders" : {
    "Content-Type" : [ "application/json; charset=UTF-8" ]
  },
  "report" : {
    "registeredProtocols" : [ {
      "protocolId" : "6",
      "topicId" : "$Unlimited",
      "protocolName" : "UnlimitedConcurrent_6",
      "sender" : "com.dkss.camel.sender.MockTransactionSender",
      "protocolType" : "com.dkss.camel.protocol.UnlimitedConcurrentProtocol",
      "maxSlot" : 0,
      "url" : "http://localhost:8080"
    }]
  },
  "message" : "protocols",
  "status" : "SUCCESS"
}

```


## Topic 조회
시스템에 등록되어 있는 Topic을 전체 조회합니다.

GET/POST
```
/api/v1/report/topics
```

#### 응답 예
```json
{
  "responseHeaders" : {
    "Content-Type" : [ "application/json; charset=UTF-8" ]
  },
  "report" : {
    "registeredTopics" : [ {
      "topicId" : "$Unlimited",
      "maxSlot" : 0
    } ]
  },
  "message" : "topics",
  "status" : "SUCCESS"
}
```

## 실행 실적 조회
전송 실적을 조회합니다.

조회할 기간을 파라미터로 전달해야 합니다.
* **from** 시작시각
* **to** 종료시각
* **형태** yyyymmddhh24mi
* 예시 202003310900

GET/POST
```
/api/v1/report/transactions/{from}/{to}
```

#### 응답 예

```json
{
    "status": "SUCCESS",
    "message": "transaction result",
    "report": {
        "transactionResult": [
            {
                "transactionId": "1_E_8400b2e1-25e8-417c-b03a-9b1a5179a878",
                "messageId": "1_20200327172013_I_fc8d8998-d472-4f2e-99ea-7f62b2d903a5",
                "protocolId": "1",
                "topicId": null,
                "type": null,
                "status": "DONE",
                "result": "FAILURE",
                "runtime": 1434,
                "creationTime": "2020-03-27T17:20:25.422",
                "startTime": "2020-03-27T17:20:26.052",
                "endTime": "2020-03-27T17:20:27.486"
            }
        ]
    }
}
```

## 메시지 상태 조회
메시지의 현 상태를 조회합니다.

조회할 기간을 파라미터로 전달해야 합니다.
* **from** 시작시각
* **to** 종료시각
* **형태** yyyymmddhh24mi
* 예시 202003310900

GET/POST
```
/api/v1/report/messages/{from}/{to}
```

#### 응답 예

```json
{
  "responseHeaders" : {
    "Content-Type" : [ "application/json; charset=UTF-8" ]
  },
  "report" : {
    "messageStatus" : [ {
      "messageId" : "9_20200406151227_I_c75f4ccf-acce-444c-9ee0-e93200056483",
      "protocolId" : "9",
      "topicId" : "4",
      "type" : "com.dkss.camel.protocol.LimitedConcurrentProtocol",
      "date" : "2020-04-06T15:12:27.503",
      "status" : "DONE",
      "parmas" : "a=3;b=5"
    } ]
  },
  "message" : "message result",
  "status" : "SUCCESS"
}
```

# 현재 버전
1.0.0-SNAPSHOT
{: .fs-6 .fw-300 }
