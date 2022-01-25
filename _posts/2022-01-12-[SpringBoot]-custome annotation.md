---
layout: post
title: "[SpringBoot] custom annotation으로 검증하기"
date: 2022-01-12 14:00:00
categories: Java&Jsp&Spring
permalink: /Java&Jsp&Spring/custom-validation
---



# Why?

SpringBoot에서 어노테이션을 이용해 validation하는 방법을 적어 보았었다.  제공해주는 어노테이션 외 별도 체크할 validator가 필요할 때 커스텀해서 검증할 수 있는 방법을 공유해보려고 한다.



## 직접 만들어 보기

### 메타 어노테이션

아래 메타 어노테이션들을 이용해 커스텀 어노테이션을 만들어낼 수 있다.

- @Retention : 어노테이션의 범위, 어떤 시점까지 어노테이션이 영향을 미치는지 결정한다. RetentionPolicy의 값을 넘겨주는 것으로 어노테이션의 메모리 보유 범위가 결정하며, RetentionPolicy는 enum 클래스로 선언되어 있다.
- @Target : 어노테이션이 적용할 위치를 결정한다.
- @Repeatable : 반복적으로 어노테이션을 선언할 수 있게한다.
- @Constraint : validatedBy 값으로 우리가 아래에서 정의할 validator를 전달해주면 Spring Boot는 우리가 API를 호출할 때 전달한 값을 가져오면서 validation을 수행한다.



### 어노테이션 선언

```java
public @interface testAnnotation {}
```

```java
@Retention(RetentionPolicy.RUNTIME) // 어노테이션을 런타임시에까지 사용할 수 있다. JVM이 자바 바이트코드가 담긴 class 파일에서 런타임환경을 구성하고 런타임을 종료할 때까지 메모리는 살아있다.
@Target(value = {ElementType.FIELD}) // 필드에 붙일 수 있음을 설명한다.
```



### 테스트

- 핸드폰 번호를 검증하는 validation을 만들어 적용해보려고 한다.
- 어노테이션 선언)

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(value = {ElementType.FIELD})
@Constraint(validatedBy = PhoneValidator.class)
public @interface Phone {
    String message() default "{javax.validation.constraints.NotBlank.message}";
    ...
}
```

```java
public class PhoneValidator implements ConstraintValidator<Phone, String> {

    private static Pattern pattern = Pattern.compile("(\\d{3})-(\\d{4})-(\\d{4})");

    @Override
    public boolean isValid(String value, ConstraintValidatorContext context) {
        // 빈 값 체크
        if (Strings.isBlank(value)) {
            return false;
        }

        // 숫자 11자리 체크.
        Matcher matcher = pattern.matcher("");
        matcher.reset(value);
        return matcher.matches();
    }
}
```

- DTO에 적용)

```java
@Getter
@Setter
public class TestRequestDto {
    @Phone(message = "올바른 형식의 휴대폰 번호를 입력해주세요.")
    private String phoneNumber;
}
```

- 간단한 테스트 API 작성)

```java
@PostMapping("/user")
    public ResponseEntity<TestResponseDto> signup(@RequestBody @Valid TestRequestDto request) {
        logger.info("request check {}", request.getPhoneNumber());
        return ResponseEntity.ok(new TestResponseDto(true, "가입완료"));
    }
```



### 검증 확인

- ExceptionHandling은 이전 @Valid 포스트에 적은 내용과 동일하게 진행한다.
- 핸드폰 번호 요청이 제대로 들어갔을 때,

<img src = "/img/220113_s1.png" class="left-image"/>

- 핸드폰 번호 요청이 제대로 들어가지 않았을 때,

<img src = "/img/220113_f1.png" class="left-image"/>



## 참고

[Baeldung 문서](https://www.baeldung.com/spring-mvc-custom-validator)