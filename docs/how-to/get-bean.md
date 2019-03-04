---
layout: default
title: Task에서 Spring Bean 가져오는 방법
parent: How-to
nav_order: 1
---


`Task` Interface 인 `Wow`는 `run(Context context, Task task)` 메소드를 하나 가지고 있고, 파라미터로 `Context`와 `Task`를 넘겨받습니다.
넘겨받은 이 `Context`를 이용해서 __Spring Application Context__ 에 접근할 수 있습니다.

아래는 MasterManager bean을 가져오는 코드입니다.
```java
public class GetSpringBeanTask implements Wow {
    @Override
    public String run(Context context, Task task) {
        MasterManager masterManager = 
            context.getApplicationContext().getBean("masterManager", MasterManager.class);
        return null;
    }
}
```