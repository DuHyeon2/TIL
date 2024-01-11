![websocket](https://github.com/DuHyeon2/DailyStudy/assets/83499405/ea728c5b-72a0-460e-9083-a0904fe1bbfd)

# Spring WebSocket
Websocket이란 양방향으로 통신을 지원하는 Transport Protocol의 일종이다. <br>
서버와 클라이언트 간 양방향 통신을 자유롭게 가능하도록 하는 통신 방법이다. <br>
본인은 실시간 알람 기능을 구현하고 알람을 읽으면 알람이 사라지도록 하기 위해 Spring과 Websocket을 이용한 알람기능을 구현하였다.<br>

## 1. Spring Websocket 설정
```
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-websocket</artifactId>
  <version>4.3.30.RELEASE</version>
</dependency>
```
- Websocket을 사용하기위해 pom.xml 에 dependency를 설정해주었다.

```
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.socket.config.annotation.*;
import org.springframework.web.socket.server.standard.ServerEndpointExporter;
import org.springframework.web.socket.server.support.HttpSessionHandshakeInterceptor;

@Configuration
@EnableWebSocket
public class WebSocketConfig implements WebSocketConfigurer {

    private WebSocketHandler webSocketHandler;

    public WebSocketConfig(WebSocketHandler webSocketHandler) {
        this.webSocketHandler = webSocketHandler;
    }

    @Override
    public void registerWebSocketHandlers(WebSocketHandlerRegistry webSocketHandlerRegistry) {
        webSocketHandlerRegistry.addHandler(webSocketHandler, "/endpoint/websocket.do")
                .setAllowedOrigins("*")
                .addInterceptors(new HttpSessionHandshakeInterceptor());
    }

    @Bean
    public ServerEndpointExporter serverEndpointExporter() {
        return new ServerEndpointExporter();
    }
}
```
- 이후 서버와 클라이언트를 연결해 줄 EndPoint를 "/endpoint/websocket.do" 로 지정하였다
- 전자정부프레임워크 기반이라 *.do 서블릿 매핑이 기본이였기에 .do까지 적용했다.
- sockJS 사용시 .withSockJS() 를 registerWebSocketHandlers에 설정 해줘야 한다.

## 2. Spring Websocket Handler 설정
```
import org.springframework.stereotype.Component;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.socket.TextMessage;
import org.springframework.web.socket.WebSocketSession;
import org.springframework.web.socket.handler.TextWebSocketHandler;
import org.springframework.web.socket.CloseStatus;

import java.util.List;
import java.util.ArrayList;

@Component
@RequestMapping("/endpoint")
public class WebSocketHandler extends TextWebSocketHandler {

    private List<WebSocketSession> sessions = new ArrayList<>();

    public void afterConnectionEstablished(WebSocketSession session) throws Exception {
        super.afterConnectionEstablished(session);
        System.out.println("WebSocket Connection Established");
        sessions.add(session);
        String message = "Hello, Client!";
        session.sendMessage(new TextMessage(message));
    }

    protected void handleTextMessage(WebSocketSession session, TextMessage message) throws Exception {
        System.out.println("Received Message: " + message.getPayload());
        String responseMessage = "Server received your message: " + message.getPayload();
        session.sendMessage(new TextMessage(responseMessage));
    }

    public void handleTransportError(WebSocketSession session, Throwable exception) throws Exception {
        System.err.println("WebSocket Transport Error: " + exception.getMessage());
    }

    public void afterConnectionClosed(WebSocketSession session, CloseStatus status) throws Exception {
        super.afterConnectionClosed(session, status);
        sessions.remove(session);
        System.out.println("WebSocket Connection Closed");
    }
}
```

- **afterConnectionEstablished** : 웹소켓 연결시 실행
- **handleTextMessage** : 웹소켓에 연결된 클라이언트에게 메시지를 보냄
- **handleTransportError** : 웹소켓 에러시 실행
- **afterConnectionClosed** : 웹소켓 종료시 실행
- session에 websocket에 연결된 클라이언트 정보를 담고, 연결이 끊길때마다 session을 제거한다.
- **sendMessage**로 해당 session의 클라이언트에게 메시지를 보낸다.

## 3.클라이언트 설정
```
$(document).ready(function() {
    /* webSocket */
    let host = location.host;
    let protocol = location.protocol === "https:" ? "wss:" : "ws:";
    let url = protocol + "//" + host + "/endpoint/websocket.do";

    var websock = new WebSocket(url);
    websock.onopen = function(e) {
        console.log("open");
        console.log(e)
    };
    websock.onmessage = function(e) {
        console.log("Message is received");
        console.log(e);
    };
    websock.onclose = function(e) {
        console.log("close");
        console.log(e);
    };
    websock.onerror = function(e) {
        console.log("error");
        console.log(e);
    };
});
```
- http 환경에선 ws로 https 환경에선 wss로 protocol을 연결 해 줘야 한다.
- **onopen** : 웹소켓 연결될때 실행
- **onmessage** : 서버로부터 메시지를 받을때 사용
- **onclose** : 웹소켓 연결이 끊어질때 실행
- **onerror** : 웹소켓 연결이 에러가 발생할때 실행
- 메시지를 서버로 보내고 싶으면 **websock.send("메시지")** 를 이용하면 된다.

## 4.Apache 설정
```
<VirtualHost *:443>
        ...

        ProxyRequests Off
        SSLProxyEngine on
        ProxyPreserveHost On

        #websocket settgins
        <Location /endpoint>
                ProxyPass ws://0.0.0.0:8080/endpoint
                ProxyPassReverse ws://0.0.0.0:8080/endpoint
        </Location>
        ...
</VirtualHost>
```
- apache ssl 이 설정되어 있을땐 proxy 서버 설정을 해 주어야 한다
- **(이부분은 추후 업데이트 예정)**
