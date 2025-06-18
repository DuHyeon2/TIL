![Kubernetes](https://github.com/user-attachments/assets/3ec2d35d-184a-480a-878f-1f89f9547880)

# Kubernets Object/Controller
- Kubernetes 오브젝트는 Kubernetes 클러스터에서 실행되는 애플리케이션의 상태를 정의하는 API 객체
- Kubernetes 오브젝트는 클러스터의 상태를 선언적으로 정의하고, Kubernetes가 이를 관리하도록 함
- Object는 하나의 인프라 개념으로 단독 기능을 함
  - Pod, Service, ConfigMap, Secret 등
- Controller는 Object를 관리하는 기능을 함
  - Deployment, HPA 등

## 1. labels
- Kubernetes 오브젝트에 대한 메타데이터로, 오브젝트를 식별하고 그룹화하는 데 사용
- 특정 애플리케이션의 버전, 환경(개발, 테스트, 프로덕션 등), 팀 등을 나타낼 수 있음
- 네이밍 규칙은 따로 정해져 있지 않음
- selector와 매칭되서 두 오브젝트를 연결하는 역할을 함

## 2. selector
- labels를 기반으로 오브젝트를 선택하는 데 사용

## 3. probe
- 애플리케이션의 상태를 모니터링하고, 문제가 발생했을 때 자동으로 조치를 취할 수 있도록 함
- startup probe: 애플리케이션이 시작 중인지 확인
- liveness probe: 애플리케이션이 정상적으로 실행되고 있는지 확인
- readiness probe: 애플리케이션이 요청을 처리할 준비가 되었는지 확인

## 4. ConfigMap, Secret
- ConfigMap: 애플리케이션의 설정 정보를 저장하는 데 사용
  - 예: 데이터베이스 연결 정보, API 키 등
  - YAML 파일로 정의 가능
- Secret: 민감한 정보를 저장하는 데 사용
  - 예: 비밀번호, 인증 토큰 등
  - Base64로 인코딩되어 저장됨
  - YAML 파일로 정의 가능
  - docker-registry: 도커 레지스트리 인증 정보를 저장하는 데 사용
  - tls: TLS 인증서를 저장하는 데 사용

## 5. Persistent Volume, Persistent Volume Claim
- Persistent Volume(PV): 클러스터 내에서 데이터를 저장하는 볼륨
  - local : 로컬 디스크를 사용하는 볼륨
  - hostPath: 호스트 머신의 디렉토리를 사용하는 볼륨
- Persistent Volume Claim(PVC): 사용자가 PV에 대한 요청을 도와주는 오브젝트

## 6. Deployment
- 애플리케이션의 배포 및 관리를 위한 오브젝트
- Recreate : 기존 파드를 삭제하고 새로운 파드를 생성
- RollingUpdate : 기존 파드를 점진적으로 업데이트하여 새로운 버전의 애플리케이션을 배포
  - maxUnavailable: 업데이트 중에 사용할 수 없는 파드의 최대 수를 지정
- maxSurge: 업데이트 중에 추가로 생성할 수 있는 파드의 최대 수를 지정
- replicas: 파드의 복제본 수를 지정

## 7. Service
- 외부에서 Pod에 접근할 수 있도록 하는 오브젝트
- ClusterIP: 클러스터 내부에서만 접근 가능한 서비스
- NodePort: 클러스터 외부에서 접근 가능한 서비스
- port: 쿠버네티스 내부에서 Service에 접근하기 위한 포트 번호
- targetPort: 매핑을 위한 Pod의 포트 번호
- 서비스 디스커버리: 서비스 이름을 사용하여 Pod에 접근할 수 있도록 함

## 8. HPA(Horizontal Pod Autoscaler)
- 애플리케이션의 부하에 따라 Pod의 수를 자동으로 조정하는 오브젝트