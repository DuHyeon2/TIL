![Kubernetes](https://github.com/user-attachments/assets/3ec2d35d-184a-480a-878f-1f89f9547880)

# Kubernets란
- 다수의 컨테이너를 효율적으로 배포, 확정 및 관리하기 위한 오픈 소스 시스템
- 컨테이너 오케스트레이션
- Docker Compose의 확장판이라 생각!
- 장점
    - 컨테이너 관리 자동화
    - 부하 분산(로드 밸런싱)
    - 쉬운 스케일링(트레픽 정도에 따라 서버 늘림)
    - 셀프 힐링(특정 프로그램이 죽을 때 다시 재시작 시켜줌)

## 1. Linux OS
 - 주로 사용하는 리눅스에는 Debian과 Redhat 걔열이 있다.
 - 쿠버네티스 설치도 크게 이 두 가지 계열을 지원한다.
 - Redhat 계열의 하나인 CentOS는 종료가 됐다.
 - Rocky Linux는 CentOS의 대체재 중 하나다
 - Rocky Linux는 쿠버네티스 설치할 때 Redhat 계열을 보면 된다.

## 2. Container
 - 실행환경을 독립적으로 운영할 수 있게 해주는 것
 - 커널레벨 기술 : chroot, cgroup, namespace
 - 도커는 High Level 기반의 기술
 - Container는 ContainerD를 사용해서 만들어짐
 - ContainerD는 Container runtime(container를 생성해줌)이다.

## 3. Container Orchestration
 - App을 컨테이너에 담아서 배포한다.
 - 시스템 운영 노하우를 많이 가지고 있다.
 - 컨테이너들을 지휘하는 메인 컨트롤러가 있고 그 지휘에 맞춰 컨테이너의 배포, 관리, 확장, 네트워킹을 자동화하는 것

## 4. Kubernetes
 - Container Orchestration를 사용하는 툴
 - Kubernetes는 현재 표준을 넘어 여러 분야에서 활용되고 있다.
 - Kubernetes는 컨테이너를 더 쉽게 사용 할 수 있게 해준다.
 - 컨테이너를 관리해주는 역할을 한다.
 - 컨테이너는 Kubernetes와의 인터페이스가 중요하다.
 - kube-apiserver : 쿠버네티스로 보내지는 모든 API를 받는다
 - kubelet : Container runtime이 받을 수 있는 형태에 API를 호출
 - 1.5~1.23버전은 kubelet Container Runtime Interface(CRI)를 통해 Container runtime에 전달했다
 - 1.24버전 이후론 kubelet에서 바로 Container runtime으로 전달(pod 만들어라 라는 명령어로 행동된다)

 ## 5. Cluster
- 쿠버네티스는 클러스터 기반으로 구성되어 있음
- 컨테이너화된 애플리케이션을 실행하기 위한 컴퓨터(노드)의 집합
- 클러스터는 마스터 노드와 워커 노드로 구성됨
- 마스터 노드: 클러스터의 제어 및 관리
    - API Server: 워커 노드에게 명령을 내림
    - Scheduler: 워커 노드에 파드를 균등하게 할당
    - Controller-Manager: 워커노드의 파드 상태를 모니터링하고 조정
    - etcd: 클러스터 상태를 저장하는 분산 키-값 저장소
- 워커 노드: 애플리케이션을 실행하는 노드
    - kubelet: 마스터 노드의 명령을 받아서 컨테이너를 실행
    - kube-proxy: 사용자 요청을 적절한 파드로 연결시켜줌(라우터 역할)

## 6. namespace
- 쿠버네티스 클러스터 내의 리소스를 논리적으로 분리하는 방법