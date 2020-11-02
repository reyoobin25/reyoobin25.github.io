---
layout: post
title: "[Spring] error"
date: 2020-11-01 14:00:00
categories: Java&Jsp&Spring
permalink: /Java&Jsp&Spring/error
---

# mvc:annotation-driven

- Spring MVC 컴포넌트들을 디폴트 설정을 통해 활성화한다.
- Spring MVC @Controller에 요청을 보내기 위해 필요한 HandlerMapping과 HandlerAdapter를 Bean으로 등록한다.
  - HandlerMapping : HTTP 요청정보를 이용해서 컨트롤러를 찾아주는 기능
  - HandlerAdapter : HandlerMapping을 통해 찾은 컨트롤러를 직접 실행하는 기능을 수행

- Bean을 생성하기 위해 xml 파일에 context:component-scan을 명시하면 이 태그를 포함하지 않아도 MVC 어플리케이션을 작동한다.



## MediaType.APPLICATION_JSON

 [@produces](https://github.com/produces)(MediaType.APPLICATION_JSON)

ResponseEntity.ok().body(dto)

xmlns:mvc="http://www.springframework.org/schema/mvc"