![도커 기본이미지](https://github.com/user-attachments/assets/90b27680-a3da-473c-bc18-62e8f993a28f)

## 1. Docker 설치
1. ### Docker 삭제
    - 혹시 기존에 도커가 있다면 완전 삭제 하자
        ```
        # sudo yum remove docker docker-common docker-selinux docker-engine

        # sudo yum remove -y docker-ce docker-ce-cli

        # rm -rf /var/lib/docker
        ```
2. ### Docker 설치
    ```
    # yum install -y https://download.docker.com/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.6-3.3.el7.x86_64.rpm

    # yum install docker-ce docker-ce-cli
    ```

## 2. Dockerfile 작성
1. ### Dockerfile 작성
    - 우선 Dockerfile작성 경로에 배포할 jar도 함께 있어야 한다.
        ```
        # Dockerfile
        FROM openjdk:11-jdk-slim # Openjdk 11이미지를 사용하겠다고 베이스 이미지 지정

        WORKDIR /app # 컨테이너 내부에서 작업 디렉토리를 지정

        COPY ./test-1.0.0.jar ./test-1.0.0.jar # 컨테이너 내부의 /app 디렉토리에 ./test-1.0.0.jar 를 복사한다.

        CMD ["java", "-jar", "./test-1.0.0.jar"] # 컨테이너가 시작될 때 실행할 기본 명령어를 지정

        ```

## 3. docker-compose.yml 작성
1. ### docker-compose.yml 작성
    - Dockerfile과 같은 경로에 만들어준다.
        ```
        version: "3"
        services:
            app:
                container_name: test
                build: .
                restart: always
                environment:
                - TZ=Asia/Seoul
                ports:
                - "8050:8050"
                privileged: true
                stdin_open: true
                tty: true
        ```

## 4. docker compose up 
1. ### docker compose up
    - docker compose를 실행한다.
        ```
        # docker compose up
        ```

2. ### docker compose down
    - docker compose를 종료한다
        ```
        # docker compose down
        ```


