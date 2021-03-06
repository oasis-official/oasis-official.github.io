---
layout: default
title: 메시지 전송 중계 시스템
nav_order: 1
has_children: true
permalink: /docs/camel
---
# 메시지 전송 중계 시스템 - camel
* 수신받은 메시지를 목표 시스템으로 전달하는 시스템
* 동시간에 많은 메시지를 목표 시스템으로 전달하여 목표 시스템이 과부하가 걸리지 않도록 
제어하고, 메시지간의 순서 보장, 최대 동시 전달 횟수 보장 등을 수행한다.


# 주요 용어

### Protocol
* 메시지 전송을 위해 필요한 정보를 정의함
* ID, 이름, 주소, 전송방식 등을 정의함

### Topic
* Protocol의 주제별 그룹정보
* ID, 최대 실행개수 정보 정의함

### Message
* 메시지 전송 요청에 의해서 만들어진 개체
* Protocol 정보와 메시지 전송 요청시에 받은 파라미터 정보를 가지고 있음
* 클라이언트에서 받은 **_전송 요청_** 마다 생성됨

### Transaction
* Message를 전송하기 위해 만들어진 개체
* 시작시간, 종료시간, 실행시간 등의 정보를 가지고 있음
* 시스템에서 Message를 목표위치로 **_전송 시도 마다_** 생성됨

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

## Protocol Type별 동작 방법

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

# 시스템 자원 구성
### camel-core.jar
- 메시지 전달을 위한 핵심 큐를 관리하는 프레임워크

### camel-persistence-jpa.jar
- camel-core에서 데이터 저장기능을 구현한 구현체
- hibernate(JPA)를 사용함
- 의존성
 * camel-core.jar

### camel-sender.jar
- camel-core에서 메시지 전달기능을 구현한 구현체
- HTTP 기반 구현
- 의존성
 * camel-core.jar

### camel-api.jar
- camel-core 기능을 웹으로 노출시키기 위한 REST API Library
- spring-mvc 사용
- 의존성
 * camel-core.jar
 * camel-persistence-jpa.jar

### camel-config-spring-jpa.jar
- spring을 이용하여 필요한 의존관계를 설정
- 의존성
 * camel-api.jar
 * camel-core.jar
 * camel-persistence-jpa.jar
 * camel-sender.jar

### dmes-cm-api.war
- DMes-plate 를 위한 웹 구현
- DB 설정정보, Table 이름 규칙 등 정의
- 의존성
 * camel-config-spring-jpa.jar



# 시스템 설정
### 초기 큐 크기 설정
 - 프로퍼티 : camel.initialQueueSize
 - 기본값 : 20
 - 예) -Dcamel.initialQueueSize=10

### 전송 타임아웃 설정
 - 프로터티 : camel.sendTimeoutInSecond
 - 기본값 : 600
 - 예) -Dcamel.sendTimeoutInSecond=300


# 현재 버전
2.4.4-SNAPSHOT
{: .fs-6 .fw-300 }
