---
layout: post
title: "[Spring] Transaction"
date: 2020-11-02 14:00:00
categories: Java&Jsp&Spring
permalink: /Java&Jsp&Spring/transaction
---

## Transaction

두 개의 테이블의 데이터를 삭제해야 하는데 on delete cascade 설정이 되어 있지 않은 경우, 하나의 쿼리로 해결할 수 없는 로직을 처리할때 트랜잭션을 사용한다. spring에서는 Spring API 를 사용하는 방법, 트랜잭션 템플릿을 사용하는 방법, @Transactional을 제공한다. 



## @Transactional

@Transactional을 붙여 선언적으로 트랜잭션을 사용하는 경우, 주의할 점이 몇가지 있다. 스프링 AOP가 프록시 기반으로 움직이는 한계 때문에, public 메소드에서만 이런 방법이 통한다. (프록시를 사용해 @Transactional 메서드를 가져와야 하는데 private, ptortected등에 붙이게 되면 가져올 수 없기 때문에 에러는 나지 않지만 무시가 된다.)

@Transactional은 메서드/클래스 레벨에 적용가능한 애너테이션이며 인터페이스에서도 붙일 순 있지만 클래스 기반 프록시에서는 제대로 동작하지 않을 수 있다.