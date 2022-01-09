---
layout: post
title: "[SpringBoot] flyway 사용기"
date: 2022-01-08 14:00:00
categories: Java&Jsp&Spring
permalink: /Java&Jsp&Spring/flyway
---

# Why?

최근에 프로젝트를 진행하면서 flyway란걸 처음 사용해보게 되었다. flyway를 사용해본 경험을 정리해보고자 한다.



## flyway?

**flyway는 데이터베이스의 형상관리를 목적으로 하는 툴** 이다. git을 통해 소스코드를 관리하는 것의 데이터베이스 버전으로 볼 수 있다. flyway는 데이터베이스의 DDL의 이력을 쌓아 DDL이 어떻게 변화되었는지 관리하는 툴로 사용되고 있다. 또한 flyway는 CLI, API, Maven, Gradle 4가지 방법으로 컨트롤이 가능하다고 한다.

[Flyway 공식 사이트](https://flywaydb.org/documentation/getstarted/how)



## SpringBoot에서 사용방법

### 의존성 추가

```java
implementation'org.flywaydb:flyway-core:7.15.0'
```



### Properties 또는 yml설정

```java
flyway:
  locations: classpath:db/migration/common, classpath:db/migration/dev # migration 파일들이 위치하는 directory
  baseline-on-migrate: true # flyway_schema_history 테이블을 자동으로 생성할지 여부 
  baseline-version: 0  # 최초 버전 정보
```

- 현재 진행중인 프로젝트는 멀티모듈구조여서 flyway 마이그레이션 파일을 각 모듈에 동일하게 계속 생성해주어야 하는 번거로움이 있어 flyway 파일을 common에 집중화하고 모든 모듈들이 common 안에 있는 마이그레이션 파일을 사용하도록 하기 위해 common에 위치 시켰다. dev의 경우, 개발서버와 나눠서 관리하기 위해 추가된 것으로 위에 디렉토리를 따라 하지 말고, 각자 본인의 상황에 맞게 위치를 설정하면 된다. 

- 필요 시 아래 spring에서 제공하고 있는 공식문서를 참조하면 된다.

  [spring 공식 docs](https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#application-properties.data-migration)

  [Baeldung 문서](https://www.baeldung.com/database-migrations-with-flyway)



### 마이그레이션 파일

flyway에서는 데이터베이스에 일어나는 모든 행위를 마이그레이션(migration) 이라고 표현하고 있다. 이러한 마이그레이션은 파일로 관리되며, 파일의 이름은 지정하는 형식을 따라야 한다.

```java
<Prefix><Version>__<Description>.sql
```

<img src = "/img/flyway_0.png" class="middle-image"/>



### 스크립트 실행 방법

스크립트를 실행하기 위해서는 인자가 총 3개가 필요하다.

- module : 어느 프로젝트에 생성할 것인지 ex) user-api 에 생성하고 싶다면 "user"
- environment : 해당 sql을 어떤 환경에 적용할 것인지 ex) "dev"를 입력 => 개발환경에서만 실행이 되기를 원하는 sql
- description : 이 sql에 대한 설명 ex) 유저 테이블을 생성하는 sql이라면 => "create_user_table" 와 같은 이름을 만든다.

```java
sh create_migration.sh user dev create_user_table 
```

위의 명령어를 실행하게 되면 
user-api 모듈의 resources/db/migration/dev 폴더에 `V{timestamp}__create_user_table.sql`파일이 생성된다.




## 정리

- 장점)
  - 이력 확인 용이 : 프로젝트를 진행하다보면 DB의 이력이 궁금해질 때가 있다. 이 컬럼은 왜 생긴거지? 언제, 누가 삭제한거지? 라고 의문이 생길때가 있는데 flyway를 사용한다면, 이러한 이력들을 빠른 시간에 찾아 볼 수 있어 이점이라 생각된다.
  - 사용성이 좋음 : 전반적으로 문서가 잘 되어 있으며 사용이 간단해서 쉽게 적용할 수 있다는 장점이 있다고 생각된다.
- 한계)
  - 유료 버전 : rollback같은 기능들은 유료버전을 사용해야 하며 비싼 편이다.
  - RDBMS별 동작 상이 : 표준이 아닌 구문을 사용하면 다른 RDBMS 에서는 작동 안될 수도 있다. 여러 RDBMS 에서 공통적으로 동작하게 할 수 없다.