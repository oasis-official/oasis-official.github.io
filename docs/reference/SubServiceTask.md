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
부모 컨텍스트를 서브 서비스에서 사용하고자 한다면 서브서비스 Task 에 `useParentContext` 프로퍼티를 `true` 로 설정해야 합니다.(기본값은 `false`)
이 프로퍼티가 설정되면 컨텍스트가 서브서비스에 전달됩니다.
서브서비스의 `action` 값을 설정하려면 서브서비스 Task에 `action` 프로퍼티를 설정합니다. 설정하지 않으면 부모 컨텍스트의 `action` 값을 사용합니다.

## 트랜잭션 공유
트랜잭션은 원 서비스가 종료될 때까지 공유합니다. 따라서 원 서비스에서 예외가 발생하면 서브서비스에서 실행했던 DB 작업은 같이 Rollback 됩니다.

## 새 트랜잭션으로 서브서비스를 시작할 때(`useParentContext`가 `false`) 데이터 전달 방법
### 부모 서비스의 컨텍스트 데이터를 서브 서비스 컨텍스트로 전달
새로운 서비스 컨텍스트로 넘길 값은 `passKey` 프로퍼티에 `,(Comma)` 로 구분하여 지정합니다.
키 값을 바꿔서 전달하고자 하는 경우는 `originalKey as newKey` 형태로 지정하면 됩니다.
> `dsGrid1 as dsNewGri`

### 서브 서비스의 컨텍스트를 부모 서비스 컨텍스트로 전달
서브서비스 컨텍스트에 저장된 값을 부모 서비스 컨텍스트로 가져오기 위해서는 `returnKey` 프로퍼티에 `,(Comma)` 로 구분하여 지정합니다.
키 값을 바꿔서 전달하고자 하는 경우는 `originalKey as newKey` 형태로 지정하면 됩니다.
>`gridResult as subResult`

### ResponseTarketKey(조회 결과키) 가져오기
서브서비스의 DbTask 등에서 `isServiceResult` 프로퍼티로 설정한 `DaoResult` 의
키 들은 부모 서비스 컨텍스트의 `serviceResponseTargetKeys` 정보에 추가되고, 데이터 정보도 가져옵니다.
만약, **같은 키가 이미 있는 경우는 덮어쓰니 유의하시기 바랍니다.**
단, `useFullNameReturnKey` 프로퍼티가 `true` 인 경우 `subServiceId_key` 형태로 부모 컨텍스트에 저장됩니다.


# 속성
## serviceId (필수)
실행할 서비스 ID

## action (옵션)
서브서비스에서 사용할 `action` 값(브랜치 이름)

## useParentContext (옵션)
### value
* `true` : 부모 서비스 컨텍스트를 상속
* 기본값 : `false`

## passKey (옵션)
* 서브 서비스로 전달할 키 목록. ,(comma)로 구분
* `useParentContext`가 `true`일 때

## returnKey (옵션)
* 서브 서비스 컨텍스트에서 가져올 Key 목록. ,(comma)로 구분
* `useParentContext`가 `true`일 때

## useFullNameReturnKey (옵션)
### value
* `true` : 서비스 결과 키의 이름을 서비스 이름과 붙여서 부모 서비스 컨텍스트에 저장
* 기본값 : `false`
