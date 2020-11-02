---
layout: post
title: "[SpringBoot] Lombok"
date: 2020-09-21 14:00:00
categories: Java&Jsp&Spring
permalink: /Java&Jsp&Spring/lombok
---

## Lombok

롬복을 사용할 때 @Getter, @Setter등 개별 어노테이션을 사용하기 보단 @Data를 쓰는 경우가 종종 있다. @Data는 하나의 어노테이션으로 @ToString, @EqualsAndHashCode, @Getter, @Setter, @RequiredArgsConstructor을 한번에 사용하 수 있다는 편리함도 있지만 그만큼 주의해서 사용해야하는 어노테이션이다.  



## toString() 무한루프 에러

JPA와 같이 ORM을 사용하고 있다면, 부모 객체와 자식 객체의 관계에서 toString()문제가 발생할 수 있다.  

```java
@Data
public class Member {
  private String id;
  private String pw;
  
  private Address addr;
}

@Data
public class Address {
  private String zipcode;
  private Member member;
}
```

Member 객체의 toString()을 호출하면 Address 객체의 toString()이 호출 되면서, 다시 Member 객체의 toString()을 호출하게되어 무한루프에 빠지게 되고 결국엔 StackOverflowError 오류가 발생하게 된다. 이럴 경우, exclude 속성을 이용해서 원하지 않는 속성을 출력하지 않도록 제어해야 한다. 그러나 @Data의 경우 exclude를 지원하지 않기 때문에 개별 어노테이션으로 작성하고, @ToString(exclude = "member")옵션을 추가할 수 있도록 한다.

```java
@Getter
@Setter
@ToString(exclude = "member")
public class Address {
  private String zipcode;
  private Member member;
}
```



## @EqualsAndHashCode 무한루프

eqauls() : 두 객체의 내용이 같은지 확인하는 메소드이다.

hashCode() : 두 객체가 같은 객체인지 확인하는 메소드이다.

@EqualsAndHashCode(of="id"): 연관 관계가 복잡해 질 때, @EqualsAndHashCode에서 서로 다른 연관 관계를 순환 참조하느라 무한 루프가 발생하고, 결국 stack overflow가 발생할 수 있기 때문에 id 값만 주로 사용한다.



참고

1.[권남 블로그](https://kwonnam.pe.kr/wiki/java/lombok/pitfall#tostring_equalsandhashcode_%ED%95%84%EB%93%9C%EB%AA%85_%EC%A7%80%EC%A0%95%EC%8B%9C_%EC%98%A4%ED%83%80_%EB%AC%B8%EC%A0%9C)

2.https://projectlombok.org/features/Data

