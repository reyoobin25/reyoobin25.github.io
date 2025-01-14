---
layout: post
title: "[SpringBoot] springBoot"
date: 2020-08-02 14:00:00
categories: Java&Jsp&Spring
permalink: /Java&Jsp&Spring/springBoot
---

# SpringBoot

1. SpringBoot가 나온 배경
   - EJB(Enterprise Java Bean)라는 무겁고 복잡한 플랫폼에서 POJO(Plain Old Java Object)를 기반으로 하는 경량의 환경을 제공한다.
   - 스프링 프레임워크가 처음 등장했을 당시 단순히 애플리케이션 운용에 필요한 객체들을 생성하고, 객체들 사이에서 의존성을 주입해주는 단순한 컨테이너로서의 기능만 제공했지만 발전을 거듭한 현재의 스프링은 다양한 엔터프라이즈 시스템 개발에 필요한 모든 분야를 지원하는 하나의 플랫폼으로 자리잡았다.
   - 다양한 프레임워크와 기술들을 지원하면서 동시에 개발자가 처리해야하는 설정도 많아지고 복잡해졌다.
   - 복잡한 설정에서 발생한 문제들을 해결하려는 노력의 일환으로 '**스프링 부트**'가 탄생하게 되었다.

2. SpringBoot의 장점
   - 라이브러리 관리 자동화 (Starter를 통해 간단히 처리가 가능하다.)
   - 설정의 자동화 
   - 라이브러리 버전 자동 관리
   - JUnit이 기본적으로 포함, Tomcat서버 내장(실행결과를 빠르게 확인할 수 있다.)
   - 독립적으로 실행 가능한 JAR파일
   - (  WAR 파일은 Java EE Web Profile 호환 응용 프로그램 서버 만 실행하면되고 JAR 파일은 Java 설치 만 필요)
   - 추가적 : 구조파악, 현재 우리 서비스에 필요한게 무엇인지 확인