---
layout: post
title: "[Study] Controller Service Dao"
date: 2020-08-07 14:00:00
categories: Study
permalink: /study/mvc
---

# Controller / Service / Dao

1. 각각의 역할

   - **Controller** : 요청을 받아서 결과를 리턴
   - **Service** : Dao에서 얻어온 결과를 가지고 적절한 로직을 수행 후 Controller에 리턴, 각 Dao들을 조립하는 것
   - **Dao** : DB처리와 관련된 작업, Service로 리턴

   

2. 현재 사용하고 있는 모습
   - 요청 > controller > ManagerImpl > Dao

   

3. 현재 상황

   - Controller단에 서비스 로직이 많이 포함되어 있는 경우가 종종 있다.
   - 그러다 보면 기능 변경 시 공통적인 부분을 서비스로 빼기 어렵고 같은 기능이여도 호출하는 부분에 따라  변경해야 해서 반복되는 로직이 생길 수 있다.

   

4. 변경점

   - Service단으로 뺄 수 있는 부분 확인 후 변경해도 좋을 것 같다.
   - 오히려 dao를 더욱 세분화해도 좋을것 같다.
   - 함수별 반복적으로 사용하는 부분을 확인해서  서비스로 만들어도 좋을것 같다.
