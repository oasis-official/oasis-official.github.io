---
layout: default
title: 프로젝트에 의존성 추가 방법
parent: Cactus
nav_order: 1
---
## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
문서버전 ver:1.1
# 프로퍼티 추가
`cactus.version` 프로퍼티를 기존 프로젝트에 추가합니다.
# 의존성 교체
`oasis-core` artifact를 삭제하고 `cactus`를 추가합니다.

# 참고 POM
```xml
<project>
    <properties>
        <cactus.version>1.0.2-SNAPSHOT</cactus.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.dkunc</groupId>
            <artifactId>cactus</artifactId>
            <version>${cactus.version}</version>
        </dependency>
    </dependencies>
</project>
```

