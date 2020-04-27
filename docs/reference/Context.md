---
layout: default
title: Context 알아보기
parent: Reference
nav_order: 1
---

# 역할
서비스 내에서 공유되는 Value Object 입니다. 

`Map<K,V>` Interface를 구현한 구현체이며 내부적으로 Data파트와 Property 파트로 분리하여 저장합니다.

# 생성
`com.dkunc.oasis.domain.ContextFactory` 의 `createContext()` 메소드를 이용해서 `Context` 인스턴스를 생성해야합니다.

# 기능
## 데이터 넣기
```java
Object put(String key, Object value)
```

### 단일 Task 내에서만 참조

`put` 메소드를 호출할 때 키가 `~` 로 시작하면 입력한 키는 해당 Task 내에서만 사용할 수 있습니다.

만약 해당키가 `Context` 내에 이미 존재한다면 기존 값은 백업되었다가 Task이 끝날 때 **복원**됩니다.



## Dao 설정
## 기본 서비스 Dao 설정
```java
void setDefaultServiceDao(String daoName)
```

### Dao와 TransactionanlDao 간 Mappeer 설정
실제 Db접근에 사용하는 `dao`와 트랜젝션 처리를 위한 기능을 포함하고 있는 `TransactionalDao`와 연결정보를 설정합니다.
```java
void setDaoAndTransactionalDaoMapper(DaoAndTransactionalDaoMapper daoAndTransactionalDaoMapper)
```

### TransactionalDynamicDao 가져오기
트랜잭션 관리를 자동으로하면서 dao를 동적으로 변경할 할 수 있는 `TransactionalDynamicDao` 를 가져옵니다.
defaultDaoName이 먼저 설정되어 있어야 합니다.
```java
TransactionalDynamicDao getDynamicDao()
```

### TransactionalDao 가져오기
트랜잭션 관리를 자동으로하는 dao를 가져옵니다. `defaultDaoName`과 `DaoAndTransactionalDaoMapper`이 먼저 설정되어 있어야 합니다.
`DaoAndTransactionalDaoMapper` 는 Bean 정의에 있으면 자동으로 설정해줍니다. 

```java
void setDaoAndTransactionalDaoMapper(DaoAndTransactionalDaoMapper daoAndTransactionalDaoMapper)
```

를 이용해서 수동으로 설정할 수도 있습니다.

```java
TransactionalDao getDao()
```

메소드는 설정된 `defaultDaoName` 의 `dao`를 가져옵니다.

```java
TransactionalDao getDao(String daoName) 
```

메소드는 지정한 이름의 dao를 가져옵니다. `DaoAndTransactionalDaoMapper`가 설정되어 있어야 동작합니다.



## 클래스이름 바인딩 소스
Taks에 지정한 Class 이름에 `#{key}`이 포함되어 있으면 해당 key와 Context내 클래스이름 바인딩 소스와 매핑합니다.

### 클래스이름 바인딩 소스 항목 추가
```java
void putClassNameBindingSource(String key, String value)
```

### 클래스이름 바인딩 소스 항목 삭제
```java
void removeClassNameBindingSource(String key)
```

### 클래스이름 바인딩 소스 항목값 가져오기
```java
String getClassNameBindingSource(String key)
```

### 클래스이름 바인딩 소스 항목 전체 가져오기
```java
Map<String, String> getClassNameBindingSources()
```
