---
layout: post
title: "[SpringBoot] @Valid로 객체 검증"
date: 2022-01-10 14:00:00
categories: Java&Jsp&Spring
permalink: /Java&Jsp&Spring/valid
---

# Why?

웹 개발 시 입력값이 제대로 들어왔는지 확인을 하기 위해서 보통 웹 브라우저에서는 스크립트를 통해 유효성 체크를 하고, 웹 서버에서는 요청 파라미터를 검사한다. Spring에서는 서버 측의 요청 파라미터를 검사할 수 있는 기능을 제공하는데 validation, validator 인터페이스를 통해 검증이 가능하다. 

현재 진행중인 프로젝트는 SpringBoot로 진행하고 있어 어노테이션을 이용해 validation하는 방법을 적어보려고 한다. 



## @Valid

### 의존성 추가

```java
implementation 'org.springframework.boot:spring-boot-starter-validation'
```



### 테스트 API작성

- 파라미터로 @RequestBody 어노테이션 옆에 @Valid를 작성하면, RequestBody로 들어오는 객체에 대한 검증을 수행한다. 이 검증의 세부적인 사항은 객체 안에 정의를 해두어야 한다.

```java
@RestController
public class TestApiController {

    private final Logger logger = LoggerFactory.getLogger(this.getClass().getName());

    @PostMapping("/check-email")
    public ResponseEntity<EmailCheckResponseDto> checkEmail(
            @RequestBody @Valid EmailCheckRequestDto request
    ) {
        logger.info(request.getEmail());
        return ResponseEntity.ok(new EmailCheckResponseDto(true, "이메일이 맞습니다."));
    }
}
```

- EmailCheckRequestDto 객체를 정의해 준다.

```java
@Getter
@Setter
public class EmailCheckRequestDto {
    @Email(message = "올바른 이메일 형식을 입력해주세요")
    @NotBlank(message = "값이 빈 상태로 들어왔습니다.")
    private String email;
}
```

- @NotBlank :  `null` 과 `""` 과 `" "` 모두 허용하지 않는다.
- @Email : 인자로 들어온 값을 Email 형식을 갖추어야 한다.



### 검증 확인

- email 요청이 제대로 들어갈 때

<img src = "/img/220110_s1.png" class="left-image"/>

<img src = "/img/220110_s2.png" class="left-image"/>

- email 요청이 제대로 들어가지 않았을 때

<img src = "/img/220110_f1.png" class="left-image"/>

<img src = "/img/220110_f2.png" class="left-image"/>




## Exception Handling

BadRequest의 경우, custom 한 errorhandling도 할 수 있다. 

<img src = "/img/220110_f3.png" class="middle-image"/>

잘못된 객체 값이 나갔을 때 Log를 읽어보면, MethodArgumentNotValidException이 발생했음을 알 수 있다. 이 Exception을 사용하여 custom한 ErrorMessage를 response로 내보낼 수도 있다.



### handler 작성

```java
@ExceptionHandler(value = MethodArgumentNotValidException.class)
public ResponseEntity<MethodNotValidResponse> handleArgNotValid(MethodArgumentNotValidException e) {
        FieldError fieldError = e.getBindingResult().getFieldError();
        String fieldName = fieldError.getField();
        String message = fieldError.getDefaultMessage();
        MethodNotValidResponse body = new MethodNotValidResponse(fieldName, "400", message);
        logger.warn("Input Not valid Arg {}", body);
        return ResponseEntity.badRequest().body(body);
    }
```

- error가 난 fieldName을 내려줄 수 있도록 했다.



### 확인

- 위 핸들러를 추가하여 아까와 동일하게 email에 빈 값을 요청해보자.

<img src = "/img/220110_f4.png" class="middle-image"/>

- Response값을 살펴보면, @Valid를 통과하지 못한 모든 필드 이름을 내려주는것을 볼 수 있다. 에러 내용을 커스텀하게 내려준 것이 잘 반영되었음을 알 수 있다.



## 정리

위 작성한 내용은 이메일만 확인해보았지만, @Valid검증에는 더 많은 내용들이 포함되어있다. 아래 문서와 책을 참조해서 필요한 부분에 사용하면 좋을 듯하다.

[Baeldung 문서](https://www.baeldung.com/spring-boot-bean-validation)

[최범균 Spring 4.0](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=44345991)