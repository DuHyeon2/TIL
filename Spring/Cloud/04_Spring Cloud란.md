# Spring cloud 란?
- Spring Cloud는 마이크로서비스 아키텍처를 구축하기 위한 도구 모음
- Spring Framework를 기반으로 하며, 분산 시스템에서의 서비스 간 통신, 구성 관리, 서비스 디스커버리, 로드 밸런싱, 회로 차단기, API 게이트웨이 등 다양한 기능을 제공

## 1. Spring Cloud의 주요 기능
- **서비스 디스커버리**
    - 서비스 인스턴스를 등록하고 검색할 수 있는 기능 제공
    - Eureka, Consul, Zookeeper 등을 사용하여 서비스 인스턴스를 등록하고 검색
    - 서버
        ```java
        @SpringBootApplication
        @EnableEurekaServer
        ```
        
    - 클라이언트
        ```java
        @SpringBootApplication
        @EnableEurekaClient
        ```
- **로드 밸런싱**
    - 클라이언트가 여러 서비스 인스턴스 중 하나를 선택할 수 있도록 지원
    - Ribbon, Spring Cloud LoadBalancer 등을 사용하여 로드 밸런싱 구현
- **클라이언트**
    - 마이크로서비스 자체를 의미
    - Eureka 같은 디스커버리 서버에 자신을 등록하고, 다른 서비스도 탐색할 수 있도록 함
- **API 게이트웨이**
    - 마이크로서비스 진입점
    - 클라이언트(사용자) 요청을 적절한 서비스로 라우팅
    - 인증, 라우팅, 로깅, 속도 제한 등의 기능을 제공
    - Spring Cloud Gateway, Zuul 등을 사용하여 API 게이트웨이 구현
- **Netflix Ribbon**
    - 클라이언트(서비스 자체) 측 로드 밸런싱 라이브러리
    - 서비스 인스턴스의 목록을 가져와서 요청을 분산
    - 현재는 Spring Cloud LoadBalancer로 대체 가능
- **Netflix Zuul**
    - API 게이트웨이로 사용되는 라이브러리
    - 클라이언트(사용자) 요청을 적절한 서비스로 라우팅하고, 필터링, 로깅 등의 기능 제공
- **Spring Cloud Gateway**
    - 새로운 API 게이트웨이 구현
    - 비동기 요청 처리 및 필터링 기능 제공