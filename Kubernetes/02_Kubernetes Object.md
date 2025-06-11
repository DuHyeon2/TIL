![Kubernetes](https://github.com/user-attachments/assets/3ec2d35d-184a-480a-878f-1f89f9547880)

# Kubernets Object
- Kubernetes 오브젝트는 Kubernetes 클러스터에서 실행되는 애플리케이션의 상태를 정의하는 API 객체
- Kubernetes 오브젝트는 클러스터의 상태를 선언적으로 정의하고, Kubernetes가 이를 관리하도록 함

## 1. labels
- Kubernetes 오브젝트에 대한 메타데이터로, 오브젝트를 식별하고 그룹화하는 데 사용
- 특정 애플리케이션의 버전, 환경(개발, 테스트, 프로덕션 등), 팀 등을 나타낼 수 있음
- 네이밍 규칙은 따로 정해져 있지 않음
- selector와 매칭되서 두 오브젝트를 연결하는 역할을 함
## 2. selector
- labels를 기반으로 오브젝트를 선택하는 데 사용