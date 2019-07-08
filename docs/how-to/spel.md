---
layout: default
title: Task에서 SpEL(Spring Expression Language) 
parent: How-to
nav_order: 1
---
OASIS SpEL
============

**SpEL**은 런타임시 객체를 조작하는 강력한 표현언어이다.(참조 : https://blog.outsider.ne.kr/835)

OASIS에서는 `com.dkunc.oasis.process.ProcessController`에서 Task 수행 전, 수행 후 SpEL을 수행 할 수 있으며 모든 Task 수행이 끝난 후 다음 Branch를 결정하기 위한 return 값 처리를 위한 SpEL까지 총 3개의 SpEL 입력 항목을 지원한다.

여기서는 기본적인 OASIS를 위한 SpEL의 문법과 bpmn modeler에서 사용하기 위한 설정에 대해 설명한다.

# 기본 SpEL 설명
Javascript에서 배열기호(`[]`)를 써서 Json 다루는 방법과 동일한 방법으로 참조한다.
 1. 객체형 : Java의 Map형태 
 ```
	value = Obj['key']
	Obj['key'] = 1000
```	  
2. 배열형 : Java의 List형태
```
   value = Obj[0]
   Obj[10] = 1000
```	

 **SpEL 참고**
> https://blog.outsider.ne.kr/835  
> https://blog.outsider.ne.kr/837

## Map
```java
mapVar['PROC_CD'] = '공정1'
```
## List / Array

```java
listVar.add('공정1') 
listVar[0] = '공정2' // List의 첫번째 항목의 값을 교체
```

# OASIS를 위한 SpEL

OASIS의 Task내에 사용하는 SpEL은 context 내부의 변수를 처리 위해 사용된다.  
SpEL은 실시간으로 자바 코드로 변경되어 실행된다.

## context내 변수 처리
context는 Map과 동일한 방법으로 사용한다.[^1]  
>**note:**  
context 자체는 `#root` 표현식을 사용한다. `#root`는 생략 가능하다.
```java
#root[i] = 10
or
['i'] = 10
or
["i"] = 10
or
[i] = new Integer(10)

```
위의 표현은 모두 같은 코드로 실행 되며 아래의 java코드로 실행이 된다.
``` java
context.put("i", Integer(10));
```

## Map생성 및 사용
```java
[subMap] = new java.util.HashMap() // context에 Map추가 , <String, Object>를 지정할 수 없음
[subMap][sub1] = 'String Value1' 
#root[subMap][sub2] = 20		   // #root는 생략 가능

```

## List생성 및 사용(배열 포함)
```java
[list] = new java.util.ArrayList() // context에 List 추가
[list].add('List value1') 
[list].add(0)
[list].add(4.5)
[list][0] = 15                      // 0번재에 15를 재할당
[list][10] = 15                     // 10번째 변수가 없으면 exception 발생 
[arr] = new Integer[8]              // 배열 생성
[arr][0] = 0
[arr][1] = 1
[arr][2] = [arr][0] + [arr][1]
```

## 조건
```java
['j'] <= 9? 'loop' : 'end' 
```

## function 사용
function은 해당 class의 function을 모두 사용할 수 있다.

```java
['substr'] = 'abc'.substring(2, 3)  // bc
['substr'] = ['substr'].replaceAll('b', 'x')    // xc
```

### 연산자 사용

```java
[subMap][sub3] = 20 + [i] ++       // context.get("subMap").set("sub3", 20 + context.get("i"));
                                   // context.set("i", context.get("i") + 1);
```
# bpmn modeler
Task에서 SpEL을 사용하기 위해서는 beforeSpel, afterSpel, nextBranchSpel 프로퍼티가 추가 되어야 한다. OASIS-CORE의 기본 기능으로 등록되어 각각 Task를 변경하지 않아도 모든 Task에 적용이 가능하다.
모델러에서 입력을 쉽게 하려면 아래의 코드를 Task 템플릿의 properties 항목에 넣으면 입력이 가능하다. 템플릿에 추가하지 않는다면 속성창의 `Extensions` 항목에 직접 넣으면 된다.
## element-templates
```json
    {
        "properties": [
          ...
            {
                "label": "beforeSpel", "type": "Text","value": "",
                "description": "전처리 Spel을 입력하세요.\n 예) ['i'] = 10",
                "binding": {
                "name": "beforeSpel", "type": "camunda:property"
                },
                "constraints": {"notEmpty": false }
            },
            {
                "label": "afterSpel", "type": "Text","value": "",
                "description": "후처리 Spel을 입력하세요.",
                "binding": {
                "name": "afterSpel", "type": "camunda:property"
                },
                "constraints": {"notEmpty": false }
            },
            {
                "label": "return(분기결정문)", "type": "String","value": "success",
                "description": "여기서 선택된 값으로 다음 분기를 결정 할 수 있습니다.",
                "binding": {
                "name": "nextBranchSpel", "type": "camunda:property"
                },
                "constraints": {"notEmpty": false }
            }	
        ],
        ...
    }
```

[^1]: OASIS의 context는 Map interface를 구현하여 SpEL에서는 Map과 동일하게 동작한다.