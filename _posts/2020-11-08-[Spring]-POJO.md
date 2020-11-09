---
layout: post
title: "[Spring] POJO"
date: 2020-11-08 14:00:00
categories: Java&Jsp&Spring
permalink: /Java&Jsp&Spring/pojo
---

## POJO

스프링 책을 읽다보면 많이 나오는 단어임에도 불구하고 "POJO(Plain Old Java Object) - 오래된 방식의 단순 자바 객체" 와 같이 추상적인 표현으로 정의되어 있어 이해하기가 어려운 경우가 있다. 다시 한번 생각해 볼 수 있도록 하자.



## 정의

순수하게 Setter, Getter 메소드로 이루어진 단순한 자바 객체를 말한다. 스프링 5 레시피에서는 빈(Bean)과 POJO 인스턴스 모두 자바 클래스로 생성한 객체 인스턴스를 의미하며 같은 의미로 혼용하여 사용하고 있다. 또한 POJO의 경우 미리 지정된 클래스를 extends하거나 인터페이스를 implement하거나 혹은 미리 지정된 어노테이션을 포함하는 행위는 하면 안된다.



## 지향점

자바의 객체지향적인 특징을 살려 비즈니스 로직에 충실한 개발을 할 수 있도록 하는것이다. 특정 기술규약과 환경에 종속되지 않도록 개발하는것을 목표로 하는것이다. 이렇게 개발하면서 얻는 이점은 **특정 규약에 종속되지 않고 설계가 가능**하고, 특정 환경에 종속되지 않아 **테스트**하기 좋으며, 특정 규약에 종속되지 않아 **깔끔한 코드작성**이 가능하다.



## 생각해 볼 점

1. 진정한 POJO는 무엇일까?

> 그럼 특정 기술규약과 환경에 종속되지 않으면 모두 POJO라고 말할 수 있는가? 많은 개발자가 크게 오해하는 것 중의 하나가 바로 이것이다. ... 진정한 POJO란 객체지향적인 원리에 충실하면서, 환경과 기술에 종속되지 않고 필요에 따라 재활용될 수 있는 방식으로 설계된 오브젝트를 말한다. - by.토비의 스프링

2. POJO / DTO, VO의 차이는 무엇일까?

> VO : A Value Object is an object such as java.lang.Integer that hold values, hence value objects **(Data Structure)** 값을 보유한 객체
>
> POJO : collection of value objects **(Data Structure)** 값 객체 컬렉션
>
> DTO : It is a POJO that is serializable, it provides for storage and retrieval of its own data (accessors and mutators) via save and find methods. it has a no-argument constructor and allows access to properties using getter and setter methods **(Data Structure)** 직렬화 가능한 POJO이며 저장 및 찾기 메소드를 통해 자체 데이터의 저장 및 검색을 제공, 인수가없는 생성자가 있으며 getter 및 setter 메서드를 사용하여 속성에 액세스 가능 - by.클린코드



참고할만한 블로그

1.https://itewbm.tistory.com/entry/POJOPlain-Old-Java-Object

2.https://blog.naver.com/writer0713/222097381699

3.https://docs.spring.io/spring-framework/docs/5.1.x/spring-framework-reference/core.html