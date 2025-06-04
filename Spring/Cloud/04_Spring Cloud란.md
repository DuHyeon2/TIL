# Spring cloud 란?
- Spring Cloud는 마이크로서비스 아키텍처를 구축하기 위한 도구 모음
- Spring Framework를 기반으로 하며, 분산 시스템에서의 서비스 간 통신, 구성 관리, 서비스 디스커버리, 로드 밸런싱, 회로 차단기, API 게이트웨이 등 다양한 기능을 제공

## 1. Spring Cloud의 주요 기능
- **서비스 디스커버리**
    - 서비스 인스턴스를 등록하고 검색할 수 있는 기능 제공
    - Eureka, Consul, Zookeeper 등을 사용하여 서비스 인스턴스를 등록하고 검색
- **로드 밸런싱**
    - 클라이언트가 여러 서비스 인스턴스 중 하나를 선택할 수 있도록 지원
    - Ribbon, Spring Cloud LoadBalancer 등을 사용하여 로드 밸런싱 구현
