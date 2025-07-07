# 상세 작업은 추후 정리예정

## 0. route53 생성
- route53으로 dns 생성 및 등록

## 1. eks 생성
- eks에서 my-cluster 생성
- 노드그룹 my-nodes 생성
- 이후 aws cli 다운로드 후 명령어, 엑세스키, 시크릿키 입력
    ```
    $ aws configure
    ```
- kubectl 로컬 세팅
    ```
    $ aws eks update-kubeconfig --region ap-northeast-2 --name my-cluster
    ```

## 2. ingress 로드밸런서 생성
- ingress-controller설치
    ```
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
    ```
    $ aws ecr get-login-password --region ap-northeast-2 | docker login --username AWS --password-stdin <ecr-uri>
    ```

## 추가작업
1. https 설정