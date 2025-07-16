# Terraform이란?
- Terraform은 인프라를 코드로 관리할 수 있게 해주는 오픈소스
- Terraform을 사용하면 인프라 리소스를 선언적으로 정의하고, 이를 버전 관리할 수 있음
- 사용자는 HCL(HashiCorp Configuration Language)을 사용하여 인프라를 정의

## 1. Terraform의 구성요소
- **프로바이더(Provider)**: 테라폼으로 생성할 인프라의 종류를 정의
- **리소스(Resource)**: 테라폼으로 실제로 생성할 인프라 자원
- **스테이트(State)**: 테라폼을 통해 생성한 자원의 상태 의미
- **아웃풋(Output)**: 테라폼으로 만든 자원을 변수 형태로 State에 저장하는 것을 의미
- **모듈(Module)**: 공통적으로 활용할 수 있는 코드를 문자 그대로 모듈 형태로 정의하는 것을 의미
- **리모트(remote)**: 다른 경로의 state를 참조하는것 

