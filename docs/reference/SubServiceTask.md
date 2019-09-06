---
layout: default
title: SubServiceTask 알아보기
parent: Reference
nav_order: 1
---

# 역할
이 테스크는 새로운 Context 를 생성하여 Task의 serviceId 프로퍼티에 지정한 서비스를 실행합니다.

# 사용방법 및 유의사항
## 부모 컨텍스트 사용
부모 컨텍스트를 그대로 유지하고자 한다면 Task 에 `newContext` 프로퍼티를 `false` 로 설정해야 합니다.
이 프로퍼티가 설정되면 아래에 언급한 `DefaultServiceDao` 와 `action` 프로퍼티를 제외한 다른 속성은 무시합니다.
Task에 `action` 이 설정되어 있으면 해당 값을 사용하고, 없으면 부모 컨텍스트의 `action` 값을 사용합니다.

## DefaultServiceDao 는 자동 상속
부모 서비스가 가지고 있는 `DefaultServiceDao` 는 서브서비스로 자동 **상속**합니다.

## 트랜잭션 공유
트랜잭션은 원 서비스가 종료될 때까지 공유합니다. 따라서 원 서비스에서 예외가 발생하면 서브서비스에서 실행했던 DB 작업은 같이 Rollback 됩니다.

## 부모 서비스의 컨텍스트를 서브 서비스 컨텍스트로 전달
새로운 서비스 컨텍스트로 넘길 값은 `passKey` 프로퍼티에 `,(Comma)` 로 구분하여 지정합니다.
키 값을 바꿔서 전달하고자 하는 경우는 `originalKey as newKey` 형태로 지정하면 됩니다.
> `dsGrid1 as dsNewGri`

## 서브 서비스의 컨텍스트를 부모 서비스 컨텍스트로 전달
서브서비스 컨텍스트에 저장된 값을 부모 서비스 컨텍스트로 가져오기 위해서는 `returnKey` 프로퍼티에 `,(Comma)` 로 구분하여 지정합니다.
키 값을 바꿔서 전달하고자 하는 경우는 `originalKey as newKey` 형태로 지정하면 됩니다.
>`gridResult as subResult`

## ResponseTarketKey(조회 결과키) 가져오기
서브서비스의 DbTask 등에서 `isServiceResult` 프로퍼티로 설정한 `DaoResult` 의
키 들은 부모 서비스 컨텍스트의 `serviceResponseTargetKeys` 정보에 추가되고, 데이터 정보도 가져옵니다.
만약, 같은 키가 이미 있는 경우는 덮어쓰니 유의하시기 바랍니다.
단, `fullPathKey` 프로퍼티가 `true` 인 경우 `subServiceId_key` 형태로 부모 컨텍스트에 저장됩니다.


# 속성
## serviceId (필수)
실행할 서비스 ID
## action
서비스 분기를 위한 Branch Name
## newContext
false 이면 부모 서비스 컨텍스트를 상속
## passKey
서브 서비스로 전달할 Key 목록. ,(comma)로 구분
## returnKey
서브 서비스 컨텍스트에서 가져올 Key 목록. ,(comma)로 구분
## fullPathKey
true 이면 서비스 결과 키의 이름을 서비스 이름과 붙여서 부모 서비스 컨텍스트에 저장
