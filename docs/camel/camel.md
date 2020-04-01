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


# 현재 버전
1.0.0-SNAPSHOT
{: .fs-6 .fw-300 }
