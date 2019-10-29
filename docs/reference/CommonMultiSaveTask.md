---
layout: default
title: CommonMultiSaveTask 알아보기
parent: Reference
nav_order: 1
---

# 역할
이 테스크는 저장 모드에 따라 삽입, 갱신, 삭제를 합니다.

# 사용방법 및 유의사항
## 파라미터
`List` 또는 `Map` 타입을 파리미터로 사용할 수 있습니다. `List`가 가지고 있는 요소는 `Map` 타입이어야 합니다.

## 저장 타입
한 행을 표현하는 Map 에는 저장 타입를 표시하는 `!nativeeditor_status` 키와 값의 쌍이 존재해야합니다. 만약 없으면 아무일도 하지 않습니다.
### 값
- `inserted`
- `updated`
- `deleted`


# 속성
## insertSqlKey
insert Sql Key

## updateSqlKey
update Sql Key

## deleteSqlKey
delete Sql Key

## skip
무시할 DML을 설정합니다. DML은 `,` 로 구분하여 복수로 지정할 수 있습니다.
### 값
- `insert`
- `update`
- `delete`

## saveMode
delete, update 구문이 영향받은 수에 제약조건을 추가합니다. 지정하지 않으면 one 이 기본값으로 설정됩니다.
### 값
- `one` : 영향받은 행 수가 1행이 아니면 예외를 발생합니다.
- `belowOne` : 영향받은 행 수가 0 또는 1행이 아니면 예외를 발생합니다.

