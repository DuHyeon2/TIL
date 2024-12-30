![ci-cd](https://github.com/user-attachments/assets/20f71753-d6da-457e-8dab-62bb79cb998e)

# CI&CD + Spring

## 1. EC2 제작 및 설정 
- EC2 ubuntu 서버 설정
    ```
    $ sudo apt update
    $ sudo apt install openjdk-17-jdk -y # java 17 버전 다운로드
    $ git config --global credential.helper store # 깃허브 아이디와 토큰 저장하는 명령어
    $ git clone [repository 주소]
    
    $ github id와 token을 입력하라고 함
    ```

## 2. deploy.yml 설정
- deploy.yml
    ```
    # Workflow의 이름
    name: Github Actions

    # Event: 싥행되는 시점을 설정
    # main 브랜치에 push가 되면 실행
    on:
    push:
        branches:
        - main

    # Job: 실행할 작업을 설정
    # 하나의 Workflow는 1개 이상의 Job으로 구성
    # 여러 Job은 병렬로 실행
    jobs:
    # Job을 식별하기 위한 이름
    Deploy:
        # Job을 실행하는 환경 설정
        runs-on: ubuntu-latest

        # Step: 특정 작업을 수행하는 가장 작은 단위
        # Job은 여러 Step으로 구성
        steps:
        - name: SSH로 EC2에 접속하기
            user: appleboy/ssh-action@v1.0.3 # SSH로 EC2에 접속하기 위한 라이브러리
            with:
            host: ${{ secrets.EC2_HOST }} # EC2 인스턴스의 IP 주소
            username: ${{ secrets.EC2_USERNAME }} # EC2 인스턴스의 사용자 이름(ubuntu)
            key: ${{ secrets.EC2_PRIVATE_KEY }} # EC2 인스턴스의 개인 키(key.pem) cat으로 열어서 복사(% 앞까지)
            script_stop: true
            script: | # 소스 코드 커밋하고 빌드 및 실행하는 스크립트를 전부 작성한다 
                cd /home/ubuntu/[프로젝트 경로]
                git pull origin main
                ./gradlew clean build
                sudo fuser -k -n tcp 808 || true
                nphub java -jar build/libs/*SNAPSHOT.jar > ./output.log 2>&1 &
    ```

## 3. clone받은 repository에서 secret 값을 각각 설정
![Alt text](<img/02_Repository Secret.png>)