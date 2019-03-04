---
layout: default
title: Task에서 다른 Task 사용하기
parent: How-to
nav_order: 1
---

Task를 실행하는 순서는
1. Context를 만들고
2. Task 인스턴스를 만들어서
3. 필요한 프로퍼티를 넣고
4. 실행
입니다.

```java
public class RunAnotherTask implements Wow {
    @Override
    public String run(Context context, Task task) {
        //1. Context를 만들고
        Context context = ContextFactory.createContext();

        //2. Task 인스턴스를 만들어서
        Task task = new Task();

        //3. 필요한 프로퍼티를 넣고
        task.putProperty("sqlKey", sqlKey);
        task.putProperty("dao", "commonDao");

        //4. 실행
        wow.run(context, task);
    }
}
```

여기서 1번 `Context`는 `RunAnotherTask` 이전에 실행했던 결과나 요청정보가 **필요없는 경우**만 새로 만드세요.



