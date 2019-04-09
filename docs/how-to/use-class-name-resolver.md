---
layout: default
title: 클래스 이름을 짧게 사용하는 방법
parent: How-to
nav_order: 1
---

OASIS에는 `com.dkunc.oasis.task.support.TaskClassNameResolver` Interface를 제공하고 있습니다.
이 인터페이스는 `getClassName(String name)` 메소드를 제공하는데 OASIS 내부에서 Task Class 이름을 가져올 때 이 메소드를 사용합니다.

따라서 이 메소드를 구현하는 Class를 작성하고 **Component Scan 대상** 으로 등록하면 새로 정의한 클래스를 OASIS에서 사용합니다.


# 테스트

간단 TaskClassNameResolver 인터페이스를 간단하게 구현해서 등록 후 서비스를 테스트해보도록 하겠습니다.

## TaskClassNameResolver 구현
`TaskClassNameResolver`를 구현한 간단한 예시입니다.

```java
@Component
public class MesClassNameResolver implements TaskClassNameResolver {
	private final Map<String, String> data = new HashMap<>();

	public MesClassNameResolver() {
		data.put("selectTask", "com.dkunc.oasis.task.commonDbTask.CommonSelectTask");
	}

	@Override
	public String getClassName(String name) {
		return data.get(name);
	}
}
```

## 서비스에 짧은 이름 Task 추가
짧은 클래스 명을 사용하는 Task를 하나 추가합니다.

![](/assets/use-class-name-resolver/use-class-name-resolver_114638.png)

class 프로퍼티에 짧은 task 명을 입력합니다.

![](/assets/use-class-name-resolver/use-class-name-resolver_115033.png)

## 테스트 코드 작성

다음과 같이 서비스를 실행할 수 있는 테스트 코드를 작성하여 정상적으로 동작하는지 확인해 봅니다.

```java
@Slf4j
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = {AppConfig.class})
public class MesClassNameResolverTest {
    @Autowired
    ServiceControllerProxy serviceController;
    @Autowired
    ServiceResponseBuilder serviceResponseBuilder;

    @Test
    public void shortenClassNameUsage(){
        Context context = ContextFactory.createContext();
        context.put("action","short");

        serviceController.start("sample1", null, context);
        context.setServiceResponse(serviceResponseBuilder.makeServiceResponse(context));
        log.info(context.getServiceResponse().toString());
    }
}
```

