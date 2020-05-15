---
layout: default
title: 메시지 전송 중계 시스템
nav_order: 1
has_children: true
permalink: /docs/camel
---
# camel
메시지 전송 중계 시스템


# 실행방법
java -jar [배포한 jar 파일]


# 주요 용어

### Protocol
* 메시지 전송을 위한 메타 정보를 가지고 있는 주체
* ID, 이름, 주소, 전송방식 등을 정의하고 있음

### Topic
* Protocol의 주제별 그룹정보
* ID, 최대 실행개수 정보 정의하고 있음

### Message
* 메시지 전송 요청에 의해서 만들어진 주체
* Protocol 정보와 메시지 전송 요청시에 받은 파라미터 정보를 가지고 있음

### Transaction
* 메시지를 전송시도하면 생성됨
* 시작시간, 종료시간, 실행시간 등의 정보를 가지고 있음

# Protocol Type
대기열 사용 방법, 실패시 다음 메시지 전송 방법에 따라 4 종류의 프로토콜 타입이 있음

## 대기열 사용 방법에 따른 분류
* 빈 곳 사용
* 지정된 갯수 이하로 빈 곳 사용
* 하나만 점유

## 메시지 전송 실패시 처리 방법에 따른 분류
* 무시 - 실패한 메시지는 실패로 종료 처리하고 다음 메시지 전송
* 막음 - 실패한 메시지를 실패로 처리하고 대기열을 막음

|   |빈 곳 사용|지정된 갯수 이하<br>빈 곳 사용|하나만 점유|
|:-:|---|---|---|
|무시|UnlimitedConcurrent|limitedConcurrent|NonBlockingExclusive|
|막음|   |   |BlockingExclusive|

## Protocl type별 동작 방법

### BlockingExclusive
* 같은 Topic을 가지고 있는 Message를 **순차적**으로 하나씩 전송함
* Message 전송에 실패하면 같은 Topic 내의 메시지 전송은 **정지**됨

### NonBlockingExclusive
* 같은 Topic을 가지고 있는 Message를 **순차적**으로 하나씩 전송함
* Message 전송에 실패해도 바로 다음 Message를 전송함

### UnlimitedConcurrent
* Topic에 상관없이 빈 큐가 있으면 즉시 전송함

### limitedConcurrent
* Topic에 지정된 최대 전송갯수 이하로 메시지를 전송함






# 현재 버전
1.0.0-SNAPSHOT
{: .fs-6 .fw-300 }
