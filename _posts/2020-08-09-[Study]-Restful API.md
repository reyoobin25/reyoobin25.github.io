---
layout: post
title: "[Study] Rest API"
date: 2020-08-09 14:00:00
categories: Study
permalink: /study/RestAPI
---

# Rest API

1. API

   - **A**pplication **P**rogramming **I**nterface

   - 소프트웨어가 다른 소프트웨어로부터 **지정된 형식**으로 **요청, 명령**을 받을 수 있는 수단

   - 예를들어 지하철 어플리케이션 을 만들기 위해 해당 서버에 요청을 할 때,  아래와 같이 요청, 응답에 대한 지정된 형식이 있다면 쉽게 사용이 가능할 것이다. 이것이 API이다.

     **요청** : "날짜 : 20200809, 라인 : 7호선, 조회내용 : 막차시간" 

     **응답** : "23시" 

     

2. Rest API

   - 각 요청이 어떤 동작을 하고 어떤 정보를 위한 것인지 요청주소 자체로 추론이 가능하다.

   - 서버에 REST API로 요청을 보낼때는 HTTP 규약에 따라 신호를 전송한다.

   - HTTP로 요청을 보낼때 다양한 메소드를 사용하며 REST API에서는 주로 GET, POST, DELETE, PUT, PATCH를 많이 사용한다.

     - GET, DELETE < POST, PUT, PATCH  : POST, PUT, PATCH의 경우 body를 통해 내용을 전달하기 때문에 더 많이, 좀 더 안전하게 전달가능하다.

     - GET 혹은 POST로도 Read, Update, Create, Delete모두 가능하긴 하나 각 요청별로 쉽게 구분하기 위해 각각의 메소드를 적절하게 사용하는것이다.

       **GET** : Read(내용 조회) 

       **POST** : Create(내용 추가)

       **PUT** : Update(내용 수정 -> 전체 내용 수정)

       **PATCH** : Update(내용 수정 -> 특정 부분 수정)

       **DELETE** : Delete(내용 삭제)

       

3. 현재 사용하고 있는 방식

   - GET : 조회 
   - POST : 조회, 수정, 삭제
   - DELETE : 삭제
   - 조회는 GET, DELETE는 삭제, 그 외 대부분 POST로 사용 중에 있으면 가끔씩 삭제 부분에 POST로 사용하고 있는 부분도 존재한다. (비고.. 삭제)
   - PUT도 존재 하나 거의 존재하지 않는다. (상태값.. 수정, 제조사..수정 등..)
   - PATCH는 사용하지 않고 있다.

   

4. 생각해볼 점

   - 5가지 함수보단 지금처럼 4가지의 함수를 사용하는게 좋다고 생각된다. (PUT과 PATCH 모호함)
   - POST로 수정, 삭제하고 있는 부분은 확인 후 수정해도 좋을것 같다.

참고할만한 블로그 : <https://meetup.toast.com/posts/92>