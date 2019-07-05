---
layout: default
title: DbAccessTemplate Class 알아보기
parent: Reference
nav_order: 1
---

# 역할
`com.dkunc.oasis.task.commonDbTask.DbAccessTemplate` Class는 파라미터로 넘겨받은 속성들을 이용해서 Database에 접근해 Query를 수행하는 Templete Task Class 입니다. 

이 Task는 전달받은 속성들이 유효한지 검증하고 쿼리를 수행한뒤에 결과를 Context에 담는 일을 합니다.

이 클래스 자체는 추상 클래스여서 직접 사용할 수는 없고 상속하여 
```java
protected abstract Object query(Context context, Task task, Object param, CommonDao dao);
```
메소드를 재정의해야 사용할 수 있습니다.


# 구현체들
```java
CommonMultiSaveTask (com.dkunc.oasis.task.commonDbTask)
DynamicQueryDbAccessTemplate (com.dkunc.oasis.task.commonDbTask)
    CommonEhUpdateTask (com.dkunc.oasis.task.commonDbTask)
    CommonEhSelectModifiedTask (com.dkunc.oasis.task.commonDbTask)
    CommonEhGridSaveTask (com.dkunc.oasis.task.commonDbTask)
    CommonEhInsertTask (com.dkunc.oasis.task.commonDbTask)
    CommonEhSelectTask (com.dkunc.oasis.task.commonDbTask)
    CommonEhDeleteTask (com.dkunc.oasis.task.commonDbTask)
MapperBaseDbAccessTemplate (com.dkunc.oasis.task.commonDbTask)
    CommonDeleteTask (com.dkunc.oasis.task.commonDbTask)
    CommonSelectTask (com.dkunc.oasis.task.commonDbTask)
    CommonUpdateTask (com.dkunc.oasis.task.commonDbTask)
    IterativeInsertTask (com.dkunc.oasis.task.commonDbTask)
    CommonInsertTask (com.dkunc.oasis.task.commonDbTask)
```

# 속성
## dao
dao 이름을 지정합니다. null이면 `com.dkunc.oasis.exception.MissingDefaultValueException` 예외가 발생합니다.
### value
dao 명

## paramKey
* 쿼리에 사용할 파라미터 Key를 지정합니다. null이면 `defaultParam` 으로 자동 설정됩니다. 만약 paramKey에 null이 아닌 값이 설정됐는데 실행할 떄 그 값을 찾을 수 없는 경우 `com.dkunc.oasis.exception.MissingDefaultValueException` 예외가 발생합니다.
* paramKey를 `,` 를 구분자로 복수개를 입력할 수 있습니다.
* paramKey들은 Map으로 묶인뒤 query의 parameter로 전달됩니다.

### value
파라미터 key명

## resultKey
쿼리를 수행한 결과를 담을 Key를 지정합니다. null이면 `resultKey` 로 자동 설정됩니다.
### value
결과를 담을 key명

## isServiceResult
쿼리를 수행한 결과인 `resultKey`를 `Context`에 저장합니다. 저장된 key는 `Context`의 `getServiceResponseTargetKeys()` 메소드를 이용해서 추출할 수 있습니다. 주로 WebController에서 응답을 위한 결과 뷰를 만들 때 유용합니다.
### value
* true : Result key를 Context에 담음

## isSingleRowOnly
조회된 결과가 1건 이하임을 보장합니다. 2건 이상이 조회됐을 경우 `com.dkunc.oasis.exception.TooManyRowsException` 예외가 발생합니다.

### Values
* `true` : 조회된 결과가 1건 이하임
* `false`(defalut) : 제약 없음

## isResultTypeMap
* 조회된 결과를 Map으로 반환합니다. `isSingleRowOnly` property 가 `true` 인 경우만 사용할 수 있습니다. 
### Values
* `true` : 조회된 결과를 Map으로 반환
* `false`(defalut) : 제약 없음

