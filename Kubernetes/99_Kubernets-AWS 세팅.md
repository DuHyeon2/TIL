# 상세 작업은 추후 정리예정

## 0. route53 생성
- route53으로 dns 등록
- 가비아에서 구매하면 가비아에 route53 등록 해야함

## 1. eks 생성
- eks에서 my-cluster 생성
- 노드그룹 my-nodes 생성
- 이후 eksctl을 통해 kubectl 설정(windows)
    - aws cli 로컬 세팅
    ```bash 
    $ aws --version

    # 버전이 나오지 않는다면 설치
    $ msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi
    ```

    ```bash
    $ aws configure

    # 이후 발급받은 키 입력
    ```

    - kubectl 설치
    ```bash
    $ winget install -e --id Kubernetes.kubectl
    ```

    - kube config 세팅
    ```bash
    $ aws eks update-kubeconfig --region ap-northeast-2 --name my-cluster
    ```

## 2. ingress 로드밸런서 생성
- ingress-controller설치
    ```bash
    $ kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.8.1/deploy/static/provider/aws/deploy.yaml
    ```
    - 이후 EC2->로드밸런서 확인하면 생성된 걸 알 수 있다
- 로드밸런서 매핑
    - 이후 route53 접속
    - 레코드 생성 -> 레코드유형 A -> 트래픽 라우팅 대상 Network Load Balancer 생성 -> 로드밸런서 DNS서버 값 넣기

## 3. RDS 생성
- 손쉬운 생성 클릭
- db 종류 선택
- 프리티어 선택
- 이후 비밀번호 지정 후 생성
- 이후 생성되면 퍼블릭 액세스 가능 "예" 로 수정
- 보안그룹 -> 인바운드 규칙 편집 포트 범위 3306 허용(MySQL 기본 포트)

## 4. ECR 생성(Docker hub 느낌)
- 레포지토리 생성 클릭
- order-backend 레포지토리 이름으로 생성
- 이후 레포지토리 생성 후 ECR로 접속
    ```bash
    $ aws ecr get-login-password --region ap-northeast-2 | docker login --username AWS --password-stdin <ecr-uri>
    ```

## 추가작업
1. https 설정
    - 쿠버네티스로 쉽게 설정할 수 있게 소스가 이미 있음
    ```bash
    $ kubectl create namespace cert-manager
    
    $ kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.5.0/cert-manager.crds.yaml

    $ kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.5.0/cert-manager.yaml
    ```

2. argocd
    - argocd 설치
    ```bash
    $ kubectl create namespace argocd

    $ kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
    ```

    - 로컬로 접속
    ```bash
    $ kubectl port-forward svc/argocd-server -n argocd 8080:443
    ```

    - 초기 비밀번호 확인
    
    ```bash
    # id는 admin

    # 비밀번호는 디코딩 해야함
    $ kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"
    ```

    - public 대시보드 접속
    ```bash
    $ kubectl edit deployment argocd-server -n argocd

    # /usr/local/bin/argocd-server 에 --insecure 추가
          containers:
            - args:
                - /usr/local/bin/argocd-server
                - --insecure
    ```
