---
layout: post
title: "[Spring] SpringBoot XSS 방지"
date: 2021-07-03 14:00:00
categories: Java&Jsp&Spring
permalink: /Java&Jsp&Spring/xss
---

# Why?

웹 애플리케이션을 개발한다면 기본적으로 생각해봐야할 보안으로 XSS(Cross Site Scripting) 방지가 있다. 어떤 방법들로 XSS방지를 할 수 있는지 확인해보고 아래 두 블로그를 참고하여 로컬에서 직접 적용해본 내용을 정리하고 넘어가기 위해 글을 작성하게 되었다.

1) [이동욱님 블로그](https://jojoldu.tistory.com/470)

2) [오명운님 블로그](https://homoefficio.github.io/2016/11/21/Spring%EC%97%90%EC%84%9C-JSON%EC%97%90-XSS-%EB%B0%A9%EC%A7%80-%EC%B2%98%EB%A6%AC-%ED%95%98%EA%B8%B0/)

## XSS?

XSS(Cross-Site Scripting) 이란 웹 애플리케이션에서 일어나는 취약점으로 관리자가 아닌 권한이 없는 사용자가 웹 사이트에 스크립트를 삽입하는 공격 기법이다. 구체적인 내용은 위키백과를 참고해보자 ([위키백과](https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9D%B4%ED%8A%B8_%EA%B0%84_%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8C%85))

## lucy-xss-servlet-filter

XSS(Cross Site Scripting) 방지를 위해 널리 쓰이는 오픈소스 중 하나인 lucy-xss-servlet-filter는 Servlet Filter 단에서 '<' 등의 태그나 특수 문자를 &lt 등으로 변환해주며 여러 가지 관련 설정을 편리하게 설정할 수 있도록 도와준다. 이 필터를 프레임워크에 등록하는 하나의 방법이 있다.

그러나 그 처리가 form-data형식에서만 적용이 되고 그 외 데이터 형태(application-Json, text data..) 대해서는 처리해주지 않는다. 그렇기 때문에 JSON을 주고 받는 API 서버의 경우에는 직접 처리가 필요하다.

JSON데이터의 처리가 필요했기 때문에 위 필터는 적용하지 않고 ObjectMapper를 통해 처리할 수 있도록 적용해보았다.


## ObjectMapper에 Convert추가

Json 데이터 처리하는 내용은 이미 Bean에 등록된 ObjectMapper에 설정이 있으니 이곳에 특수문자를 변환하는 내용을 추가하면 된다고 생각하여 처리해본 내용을 적어본다.

먼저, 태그 변환을 위한 CharacterEscapes를 상속받은 XssCharacterEscapes 클래스를 생성한다. (이 클래스에 대해서는 오명운님의 블로그를 참조하면 좋을것 같다.)

```java
import com.fasterxml.jackson.core.SerializableString;
import com.fasterxml.jackson.core.io.CharacterEscapes;
import com.fasterxml.jackson.core.io.SerializedString;

import io.micrometer.core.instrument.util.StringEscapeUtils;
import lombok.extern.slf4j.Slf4j;

@Slf4j
public class XssCharacterEscapes extends CharacterEscapes {
   private final int[] asciiEscapes;

    public XssCharacterEscapes() {
       log.info("XssCharacterEscapes 생성");
        // 1. XSS 방지 처리할 특수 문자 지정
        asciiEscapes = CharacterEscapes.standardAsciiEscapesForJSON();
        asciiEscapes['<'] = CharacterEscapes.ESCAPE_CUSTOM;
        asciiEscapes['>'] = CharacterEscapes.ESCAPE_CUSTOM;
        asciiEscapes['\"'] = CharacterEscapes.ESCAPE_CUSTOM;
        asciiEscapes['('] = CharacterEscapes.ESCAPE_CUSTOM;
        asciiEscapes[')'] = CharacterEscapes.ESCAPE_CUSTOM;
        asciiEscapes['#'] = CharacterEscapes.ESCAPE_CUSTOM;
        asciiEscapes['\''] = CharacterEscapes.ESCAPE_CUSTOM;
    }

    @Override
    public int[] getEscapeCodesForAscii() {
        return asciiEscapes;
    }

    @Override
    public SerializableString getEscapeSequence(int ch) {
       log.info("XssCharacterEscapes 동작");
       return new SerializedString(StringEscapeUtils.escapeJson(Character.toString((char) ch)));
    }  
}
```



HttpMessageConverter는 POST나 PUT등 Body로 데이터가 들어오는 내용을 변환시킬때 사용되는 클래스이다.
HttpMessageConverter를 Bean으로 등록한다.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.http.converter.json.MappingJackson2HttpMessageConverter;

import com.fasterxml.jackson.databind.ObjectMapper;

@Configuration
public class ObjectMapperConfig {

   @Bean
    public MappingJackson2HttpMessageConverter jsonEscapeConverter(ObjectMapper objectMapper) {
        ObjectMapper copy = objectMapper.copy();
        copy.getFactory().setCharacterEscapes(new XssCharacterEscapes());
        return new MappingJackson2HttpMessageConverter(copy);
    }
}
```



StringEscapeUtils를 사용하기 위해 pom.xml에 common-text 의존성을 추가한다.

```java
<!-- https://mvnrepository.com/artifact/org.apache.commons/commons-text -->
<dependency>
     <groupId>org.apache.commons</groupId>
     <artifactId>commons-text</artifactId>
     <version>1.9</version>
</dependency>
```



파라미터에 치환할 부분을 아래와 같이 추가하여 작성한다.

```java
Product.setDesc(StringEscapeUtils.escapeHtml4(Product.getDesc()));
logger.info("desc = {}", Product.getDesc());
```



**테스트 결과)**

requestBody에 아래와 같은 내용을 추가해서 보낼 경우,

<img src = "/img/xss_postman1.png" class="middle-image"/>

변환되는 모습을 확인할 수 있다.

<img src = "/img/xss_re.png" class="middle-image"/>



## 정리

- lucy-xss-servlet-filter의 경우 form data에서만 XSS처리를 해준다.
- Json 형태에서 XSS처리를 하기위해 ObjectMapper에서 변환할 수 있도록 변경한다.