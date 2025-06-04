# Microservice 특징

## 1. Challenges
- 서비스 간 데이터 일관성 유지

## 2. Small Well Chosen Deployable Units
- **작고 잘 선택된 배포 단위**로 구성되어 있습니다.

## 3. Bounded Context
- **경계가 명확한 컨텍스트**로 나누어져 있습니다
- **gRPC, 메시지 큐** 등 다양한 방식이 사용될 수 있습니다.

## 4. RESTful
- **RESTful API**를 통해 서비스 간 통신을 합니다.

## 5. Configuration Management
- **구성 관리**가 중요합니다. 각 서비스는 독립적으로 구성될 수 있습니다.

## 6. Cloud Enabled
- **클라우드 환경에서 실행**되도록 설계되어 있습니다. 이는 확장성과 유연성을 제공합니다.

## 7. Dynamic Scale Up And Scale Down
- **동적 확장 및 축소**가 가능합니다. 필요에 따라 서비스 인스턴스를 추가하거나 제거할 수 있습니다.

## 8. CI/CD
- **지속적 통합 및 배포(CI/CD)**를 통해 빠른 개발과 배포가 가능합니다. 이는 자동화된 테스트와 배포 파이프라인을 포함합니다.

## 9. Visibility
- **가시성**이 중요합니다. 각 서비스의 상태와 성능을 모니터링할 수 있는 도구가 필요합니다.
