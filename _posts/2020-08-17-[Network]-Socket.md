---
layout: post
title: "[Network] Socket"
date: 2020-08-17 14:00:00
categories: Network
permalink: /Network/Socket
---



# Socket

> - Web polling : Polling, Long Polling, Streaming
> - Web push : WebSocket, Server Sent Events(SSE)
> - SpringBoot 웹소켓 테스트

HTTP 프로토콜로 통신하는 경우, 연결이 유지되지 않아 서버에서 먼저 요청을 보내는 것이 불가능하다. Polling, Long Polling, Streaming 방식을 이용해 실시간인 것처럼 구현할 수 있다.

**Web polling**

1. Polling
   - **Client에서 일정 주기마다 요청**을 보내고, 서버는 현재 상태를 응답해주는 방식이다. 
   - 이 방식의 경우, 실시간으로 반영이 되어야 하는 서비스에서는 좋지 않으며, 변경된 것이 없더라도 매 요청마다 응답을 내려줘야 하기때문에 불필요한 트래픽이 다량으로 발생하게 된다. (주기 ↑, 요청 ↑)
   - 실시간 처럼 보이기 위해서는 주기를 늘려야 하는데, 주기를 늘린 만큼의 불필요한 트래픽이 많아지기 때문에 실시간 서비스에 사용하기는 어렵다.
   - ![polling01](..\img\polling01.JPG)

2. Long Polling
   - Client에서 요청을 보내고 서버에서는 **이벤트가 발생했을 때** 응답을 내려주고 Client가 응답을 받았을 때 다시 다음 응답을 기다리는 요청을 보내는 방식이다.
   - 실시간 반응이 가능하고  Polling에 비해 불필요한 트래픽은 덜 발생하나 이벤트가 많을 경우 순간적인 과부하가 걸릴 수 있다.
   - 이때, 대기시간은 무한정이 아닌 구현에 따라 시간을 정할 수 있다.
   - ![polling02](..\img\polling02.JPG)

3. Streaming

   - long polling과 마찬가지로 Client에서 서버로 일단 http request를 날린다. 서버에서 Client로이벤트를 전달할때 해당 요청을 끊지 않고 필요한 메시지만 보내기를(flush) 반복하는 방식이다.
   - 이벤트가 발생했을 때 응답을 내려주는데 응답을 완료시키지 않고 계속 연결을 유지한다.
   - Long Polling에 비해 응답마다 다시 요청을 하지 않아도 되므로 효율적이지만, 연결 시간이 길어질수록 연결의 유효성 관리의 부담이 생길 수 있다.

   

이것에서 벗어나 Client와 서버간 양방향 통신이 가능하게 하기 위해 HTML5 표준의 일부로 webSocket이 만들어지게 되었다. 

**Web push**

1. WebSocket
   - 웹 서버와 웹 브라우저간 실시간 양방향 통신환경을 제공해주는 실시간 통신기술이다. 위 Polling 방식(요청 - 응답의 형식)과 다르게 양방향으로 원할때 요청을 보낼 수 있으며 오버헤드가 적다.
   - ![polling03](..\img\polling03.JPG)
   - http에서 ws로 전환한다.
   - WS는 모듈을 이용해 쉽게 구현이 가능하다. (<https://github.com/websockets/ws/blob/HEAD/doc/ws.md>)
   - **webSocket을 지원하지 않는 브라우저**에 대한 **솔루션**으로 아래와 같은 라이브러리를 제공한다.
   - **Socket.io(http://socket.io) / SockJS(http://scokjs.org), STOMP (Long Polling방식)**
     - Socket.io :  node.js 기반으로 만들어진 기술로 자체 스팩으로 만들어진 socket.io 서버를 만들고 socket.io 클라이언트와 브라우저에 구애받지 않고 실시간 통신이 가능하다. socket.io는 [node.js](https://nodejs.org/) 기반이기때문에 모든 코드가 javascript로 작성되어 있다.
     - SockJS :   springframework에서 WebSocket을 지원한다. 스프링 메뉴얼에 webSocket 부분을 보면 위와 같은 미지원하는 브라우저 문제를 해결하기 위한 방법으로 SockJS를 지원한다. 
     - STOMP : springframework에서 제공하는 라이브러리이다. (*https://spring.io/guides/gs/messaging-stomp-websocket/*)
2. Server Sent Events(SSE)
   - HTTP 연결을 통해 서버에서 Client로 데이터를 수신가능하게 한 방식이다.
   - 연결된 상태를 유지하고 서버가 일방적으로 데이터를 전송한다. 사용자는 메시지를 확인하는 과정이 필요하다.
   - WebSocket과는 다르게 자동으로 재 연결을 한다.

참고 사이트 : <https://m.mkexdev.net/98>



SpringBoot 웹소켓 테스트

1. 라이브러리 사용하지 않고 webSocket만 이용

   - 필요한 dependency 추가, Bean객체 등록, Socket 클래스 작성

     ![socket01](..\img\socket01.JPG)

     ![socket02](..\img\socket02.JPG)

     ![socket06](..\img\socket06.JPG)

     ![socket03](..\img\socket03.JPG)

   - 확인 테스트(*https://chrome.google.com/webstore/detail/advanced-rest-client/hgmloofddffdnphfgcellkdfbfbjeloo*)

     ![socket04](..\img\socket04.JPG)

     ![socket05](..\img\socket05.JPG)

2. STOMP 사용하기(spring 공식 홈페이지에서 제공)

   <https://spring.io/guides/gs/messaging-stomp-websocket/>

   <https://www.tutorialspoint.com/spring_boot/spring_boot_web_socket.htm>



참고 블로그 : <https://adrenal.tistory.com/20>
