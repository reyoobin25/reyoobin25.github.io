---
layout: post
title: "[SpringBoot] @DataJpaTest"
date: 2021-10-01 14:00:00
categories: JAVA&JSP&SPRING
permalink: /springboot/datajpatest
---

## Why?

테스트 코드를 짜면서 이해없이 사용하다보니 에러를 만났던 경험이 있어 정리하고 넘어가기 위함이다.



## @DataJpaTest

@DataJpaTest는 JPA 테스트 관련 컴포넌트만 Import하여 빠르게 테스트하기 위한 어노테이션이다. 관련 컴포넌트들은 아래와 같다. ([JAVA DOCS](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/test/autoconfigure/orm/jpa/DataJpaTest.html))

```java
 @Target(value=TYPE)
 @Retention(value=RUNTIME)
 @Documented
 @Inherited
 @BootstrapWith(value=org.springframework.boot.test.autoconfigure.orm.jpa.DataJpaTestContextBootstrapper.class)
 @ExtendWith(value=org.springframework.test.context.junit.jupiter.SpringExtension.class)
 @OverrideAutoConfiguration(enabled=false)
 @TypeExcludeFilters(value=DataJpaTypeExcludeFilter.class)
 @Transactional
 @AutoConfigureCache
 @AutoConfigureDataJpa
 @AutoConfigureTestDatabase
 @AutoConfigureTestEntityManager
 @ImportAutoConfiguration
```



### @AutoConfigureTestDatabase

- 해당 어노테이션의 경우, replace=AutoConfigureTestDatabase.Replace 가 디폴트로 설정되어 있어 설정해 놓은 DB가 아니라 in-memory DB를 활용해서 테스트가 실행된다. (default : Replace.Any)
- replace, connection에 대해서 변경이 가능하며, replace = AutoConfigureTestDatabase.Replace.NONE 으로 덮어씌워, 설정해놓은 실제 DB를 사용할 수 있다.
- EmbeddedDatabaseConnection을 통해 사용 가능한 in-memory DB에 자동으로 커넥션을 설정하는것을 확인 할 수 있다.

```java
@Target({ ElementType.TYPE, ElementType.METHOD })
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@ImportAutoConfiguration
@PropertyMapping("spring.test.database")
public @interface AutoConfigureTestDatabase {
  @PropertyMapping(skip = SkipPropertyMapping.ON_DEFAULT_VALUE)
  Replace replace() default Replace.ANY;

  EmbeddedDatabaseConnection connection() default EmbeddedDatabaseConnection.NONE;

  enum Replace {
    ANY,
	AUTO_CONFIGURED,
	NONE
  }
}

public enum EmbeddedDatabaseConnection {
  NONE(null, null, null),
  H2(EmbeddedDatabaseType.H2, "org.h2.Driver", "jdbc:h2:mem:%s;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE"),
  DERBY(EmbeddedDatabaseType.DERBY, "org.apache.derby.jdbc.EmbeddedDriver", "jdbc:derby:memory:%s;create=true"),
  HSQL(EmbeddedDatabaseType.HSQL, "org.hsqldb.jdbcDriver", "jdbc:hsqldb:mem:%s");
}
```



### @Transactional

- @DataJpaTest는 기본적으로 @Transactional 어노테이션을 포함하고 있다. 그래서 테스트가 완려되면, 롤백하기 때문에 직접 선언적 트랜잭션 어노테이션을 달아 옵션을 설정해주어야 한다.
- @Transactional(propagation = Propagation.NOT_SUPPORTED)을 통해 기능을 제외시킬 수 있다.



## 	해결?

- 문제 해결은 간단했다. 결국 위 Java docs를 읽어보면, 'When using JUnit 4, this annotation should be used in combination with `@RunWith(SpringRunner.class)`.' 라는 말이 써있다. @RunWith을 붙여주면 되는 문제 였음을 깨달았다.
- 결국 나는 이런저런 어노테이션을 붙이기 싫어 아래 두 어노테이션을 사용하여 테스트를 진행하게 되었다.
  - @RunWith(SpringRunner.class)
  - @SpringBootTest : 통합테스트를 위한 어노테이션