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
- 전자정부프레임워크 기반이라 *.do 서블릿 매필이 기본이였기에 .do까지 적용했다.
