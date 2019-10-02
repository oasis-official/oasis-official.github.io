---
layout: default
title: Audit 사용하기
parent: How-to
nav_order: 1
---

Oasis내에서 Audit를 사용하기 위해서는 Audit 정보객체를 생성하고 AuditConverter를 구현해야 합니다.

## Audit 정보 객체

`com.dkunc.oasis.audit.Audit` 클래스는 6개 필드를 가지고 있습니다.
- Device : 서비스를 호출한 장치 정보
- User : 서비스를 호출한 사용자 정보
- Service : 실행된 Service
- Process : 실행된 Process
- Task : 실행된 Task
- Timestamp : 시각, Task 실행직전 시각

이 객체에 저장된 정보를 기준으로 서비스내에서 사용할 수 있습니다.

### Audit 정보 객체 생성
서비스를 실행하기위한 Client, 예를들면 Spring MVC Controller, 에서 Audit 객체를 생성 후 필요한 정보를 추가합니다.
**Device 또는 User 정보가 필요한 경우만 추가**하면 됩니다. 나머지 정보는 프레임워크 내에서 정보를 넣어줍니다.

```java
Audit audit = new Audit();
audit.setDevice(new Device());
audit.setUser(new User());
```
생성한 Audit 객체를 Context에 추가합니다.

```java
Context context = ContextFactory.createContext();
context.setAudit(audit);
```

## AuditConverter
Audit 객체와 Database를 연결하기 위한 Converter입니다.
`com.dkunc.oasis.audit.InsertAuditConverter` 와 `com.dkunc.oasis.audit.UpdateAuditConverter` 인터페이스를 구현하여 스프링빈으로 등록합니다.

```java
@Component
public class MESInsertAuditConverter implements InsertAuditConverter {
    @Override
    public Set<AuditValuePair> getAuditData(Audit audit) {
        if(audit == null) return null;
        String user = audit.getUser() == null ?
                audit.getService().getAttribute("serviceId") + "." + audit.getTask().getName()
                : audit.getUser().getId();
        Set<AuditValuePair> pairs = new HashSet<>();
        pairs.add(new AuditValuePair("created_by", user));
        pairs.add(new AuditValuePair("created_at", audit.getTimestamp()));
        pairs.add(new AuditValuePair("modified_by", user));
        pairs.add(new AuditValuePair("modified_at", audit.getTimestamp()));
        return pairs;
    }
}
```
```java
@Component
public class MESUpdateAuditConverter implements UpdateAuditConverter {
    @Override
    public Set<AuditValuePair> getAuditData(Audit audit) {
        if(audit == null) return null;
        String user = audit.getUser() == null ?
                audit.getService().getAttribute("serviceId") + "." + audit.getTask().getName()
                : audit.getUser().getId();
        Set<AuditValuePair> pairs = new HashSet<>();
        pairs.add(new AuditValuePair("modified_by", user));
        pairs.add(new AuditValuePair("modified_at", audit.getTimestamp()));
        return pairs;
    }
}
```