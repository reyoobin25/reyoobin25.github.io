---
layout: post
title: "[Spring] Retry"
date: 2020-08-31 14:00:00
categories: Java&Jsp&Spring
permalink: /Java&Jsp&Spring/retry
---

# Retry

서버를 운영하다보면 일시적인 오류가 종종 발생한다.  특히 큰 한덩어리로 관리되던 Monolithic Architecture 에서 Endpoint가 작은 단위로 분리되는 Micro Service Architecture(MSA) 로 바뀌면서 각 어플리케이션 간의 통신에서 이러한 일시적인 오류가 빈번하게 생기는 편이다.

이러한 네트워크 오류는 '재시도'를 함으로써 간단하고 강력하게 해결할 수 있다. 

Spring에서는 재시도와 관련해서 @Retryable이라는 어노테이션, RetryRestTemplate, Circuit Breaker을 제공한다.



## @Retryable 

-  Spring Application에 @EnableRetry 어노테이션을 추가한다.
- 재시도 하고 싶은 메소드에 @Retryable 어노테이션 추가한다.
  - value: 설정한 특정 Exception이 발생했을 경우 retry
  - maxAttempts : 최대 재시도 횟수 (기본 3회)
  - backoff : 재시도 pause 시간
- 정해진 실패 회수를 초과할 경우 @Recover함수가 불리게 되는데, exception + 파라미터가 동일하다는 점에 주의해야 한다.

```java
@Retryable(value = DataAccessResourceFailureException.class, maxAttempts = 2,
        backoff = @Backoff(2000))
public void checkAndProcess(String userKey, String itemId) {

@Recover
public void recover(DataAccessResourceFailureException e, String userKey, String itemId) {
 log.error("All retries have failed!, userKey:{} " + e.toString(), userKey);
}
```



## RetryTemplate

- 여기저기 @Retryable을 붙여 사용하고 싶지 않다면 RetryTemplate을 이용해 서비스로 구현할 수 있다.
- 필요한 dependency를 추가하고, Bean 등록 후 생성한 Bean을 가지고 서비스를 구현하면 된다.

```java
// dependency 추가
implementation 'org.springframework.retry:spring-retry'
    

@Bean 
public RetryTemplate RetryTemplate() { 
    FixedBackOffPolicy backOffPolicy = new FixedBackOffPolicy(); 
    backOffPolicy.setBackOffPeriod(1);//지정한 시간만큼 대기

    SimpleRetryPolicy retryPolicy = new SimpleRetryPolicy(); 
    retryPolicy.setMaxAttempts(2); // 재시도 max count
	
    RetryTemplate retryTemplate = new RetryTemplate(); 	
    retryTemplate.setBackOffPolicy(backOffPolicy); 
    retryTemplate.setRetryPolicy(retryPolicy); 
    return retryTemplate; 
}

@Service 
public class SomeClass { 
    @Autowired 
    private RetryTemplate someRetryTemplate; 
    
    @Autowired 
    private Something something; 
    
    private String apply(SomeEntity someEntity) { 
    	String result = someRetryTemplate.execute(context -> something.apply(someEntity)); 
        return result; 
    } 
}
```



## Circuit Breaker

- MSA구조내에서 일상적으로 발생할 수 있는 에러로 대응하기 위해 나온 개념이다.
- 자동으로 모듈간의 호출 에러를 감지하고 연쇄로 장애가 나는것을 사전에 막을 수 있는 기능이다.
- Netflix에서는 이미 Hystrix라는 이름으로 개발이 되어 있다. 
  - spring-cloud-starter-netflix-hystrix 라이브러리 추가
  - @EnableCircuitBreaker 어노테이션 추가
  - 재시도할 부분에 @HystrixCommand 어노테이션을 추가





> 참고할만한 사이트
>
> <https://www.baeldung.com/spring-retry>
>
> <https://github.com/Netflix/Hystrix>
>
> 참고한 블로그
>
> https://taetaetae.github.io/2020/03/29/better-rest-template-2-netflix-hystrix/
>
> <https://taetaetae.github.io/2020/03/29/better-rest-template-2-netflix-hystrix/>
>
> <https://jobc.tistory.com/211>