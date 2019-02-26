---
layout: default
title: 읽어보기
parent: 개발환경
nav_order: 1
---
## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
문서버전 ver:2.0

# 샘플 요구사항
* JDK : JAVA 8 이상
* OS : Windows 7 64bit 이상 

# 디렉터리 구성
## camunda-modeler
Service bpmn 모델링 도구

## eclipse
IDE

## HeidiSQL_9.5_64_Portable
SQL Tool

## mysql-5.7.24-winx64
MySql Server

## apache-tomcat-8.5.37
WAS
## workspace
* mes_ref

> MES 구축을 위한 템플릿 프로젝트입니다. 이 프로젝트를 복사해서 새 프로젝트를 생성하세요. MES에서 공통적으로 적용해야하는 기능이 있으면 이 프로젝트에 추가해서 배포하겠습니다.(예-message, security 등)
> oasis 기능 구현 샘플이 필요한 경우 이 프로젝트의 test에 작성해서 배포하겠습니다.

* oasis_egov

> egov project sample 생성시 만들어진 파일 구조위에 OASIS 만 올려놓은 프로젝트입니다. egov 에서 재공하는 component를 추가하여 테스트하고자 하는 경우 사용하시면 편리합니다. 이 프로젝트에서 충분히 검증이되면 본 프로젝트에만 별도로 적용하셔도 됩니다. 공통적용이 필요한 경우는 공유해주시면 mes_ref 프로젝트에도 적용하겠습니다.


# 프로젝트의 테스트
프로젝트의 테스트들을 성공시키기 위해서는 DB Server를 먼저 시작해야 합니다. 필요한 데이터는 이미 준비되어 있습니다.
## MySQL Server 시작 및 종료
- 시작 : mysql-5.7.24-winx64 디렉토리의 "start_mysql.bat" 실행

    실행하면 명령 프롬프트 창이 열리고 아무 메시지 없이 커서만 깜박입니다.(창을 닫으면 서비스가 종료되니 닫지 마세요)

- 종료 : mysql-5.7.24-winx64 디렉토리의 "stop_mysql.bat" 실행

    실행하면 시작시 열렸던 명령 프롬프트 창이 닫힙니다.

## MySQL 계정 및 Database
- 계정

    com/com01
- Database

    testdb

    => 차후 테스트들은 메모리 DB를 사용하도록 변경하겠습니다.


# DBMS Tool
SQL 서버 구동 후 배포된 \HeidiSQL_9.5_64_Portable\heidisql.exe 을 실행하여 이상이 없는지 확인해 볼 수 있습니다.

Heidi SQL은 가벼운 SQL Tool입니다. 별도 설정 없이 heidisql.exe 을 실행하면 미리 설정해놓은 프로파일들을 볼 수 있습니다.

* 접속이 안되면 MySql Server의 기본 포트인 3306 을 이미 사용하고 있는지 확인해보세요.

    개발환경의 DB Server 포트를 바꾸려면 \mysql-5.7.24-winx64\my.ini 파일을 열어서 3306으로 되어 있는 것을 모두 다른 것으로 바꿉니다.

    개발환경의 DB Server 포트를 바꾸면 src/main/resources/application.yml 에 DB연결주소에 포트번호를 명시해야 합니다.


# 의존성 다운로드
프로젝트를 최초 실행하기 전에 maven의 의존성을 리프레시 해야 합니다. 

## Refresh 방법
- 이클립스의 프로젝트 익스플로러에서 프로젝트를 오른쪽 마우스 클릭
- Maven -> Update Project 선택


# 프로젝트 실행
IDE에서 프로젝트를 실행하실 때에는 프로젝트 오른쪽 마우스 클릭 "Run AS -> Run on Server" 클릭하여 tomcat 서버를 하세요.

## 실행 후 요청 URL 샘플
- 서비스 요청

	http://localhost:8080/service/sample1/find

- 페이지 요청

	http://localhost:8080/page/sample1

	http://localhost:8080/page/sample2


  
# mes_ref 프로젝트 디렉토리 구조			
```
├─main
│  ├─java
│  │  └─com
│  │      └─dkunc
│  │          └─mes
│  │              └─ref
│  │                  └─controllers     : SpringMVC Controller 패키지
│  ├─resources
│  │  ├─contexts                        : 프로젝트 환경 설정 XML 예)DB설정정보
│  │  ├─messages                        : Spring Message 설정 파일
│  │  ├─persistence                     : MyBatis Mapper xml 저장 위치
│  │  │  └─mappers
│  │  └─services                        : BPMN 서비스 파일 저장 위치		
│  └─webapp
│      ├─META-INF
│      └─WEB-INF
│          ├─config                     : root context 위치
│          └─views                      : View 파일(예-jsp) 저장위치
│              └─common                 : 공통뷰 저장위치(예-에러메시지  표시, 초기화면)
└─test                                  : 테스트 코드
    ├─java
    │  └─com
    │      └─dkunc
    │          └─mes
    │              └─ref
    └─resources					
```				

# 추천 사항
개발 시 모든 테스트와 서비스에 대해서 Unit Test 하는 것을 추천해 드립니다.
샘플 코드에는 Task 별로 테스트하는 것과 Service 전체를 테스트하는 코드를 넣어놨습니다. 이것을 응용해서 테스트코드를 만드시면 됩니다.


# BPMN Modeler 설정
Service Modeling tool인 Camunda Modeler는 배포한 디렉토리에 포함되어 있으므로 별도 설치하지 않으셔도 됩니다. 
다만, 이클립스에서 bpmn 파일을 외부 도구로 열기 위해서 external tool로 등록을 해야합니다.
## 외부 도구 등록 방법
1. 프로젝트에서 src/main/resources/services 디렉토리의 sample1.bpmn 파일을 마우스 오른쪽 클릭합니다.
2. open with -> other
3. External programe 라디오 버튼 선택
4. Browse... 버튼 클릭
5. 배포한 디렉터리에서 \camunda-modeler\camunda-modeler.exe 선택
6. Use it for all '*.bpmn' files 체크박스 체크박스
7. OK 버튼 클릭

# BPMN Modeler 프로젝트 환경으로 변경
Camunda modeler는 프로젝트별로 개발 편의 사항을 설정할 수 있습니다. 예를 들어 CommonDbTask 를 선택하면 이에 필요한 필드들이 활성화 되는 것을 볼 수 있습니다.
이는 언제든지 프로젝트 상황에 맞게 camunda-modeler\resources\element-templates 디렉터리 파일들을 추가, 수정하여 변경할 수 있습니다.


# 형상관리
프로젝트의 코드는 git으로 버전 관리되고 있습니다. 개인적으로 추가 코드 테스트가 필요한 경우에는 별도 브랜치를 만들어서 작업하시는 것을 권장합니다.
새로운 기능이나 샘플 코드가 추가되면 master branch에 공개하고 별도 알림 드리겠습니다.
## 브랜치 만드는 방법
```sh
git branch newbranch
git checkout newbranch
```
> git에 관련 정보는 별도 문서를 참고하시기 바랍니다.

## mes-ref clone 하는 방법(참고만 하세요)
```sh
git clone ssh://gituser@10.110.1.12/~/mes-ref.git
```

# 앞으로 변경 될 것
* Test 코드 Memory DB 참조하도록 변경
* Security 설정
* Message(전문) 처리 모듈 추가