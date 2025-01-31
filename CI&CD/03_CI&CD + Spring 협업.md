![ci-cd](https://github.com/user-attachments/assets/20f71753-d6da-457e-8dab-62bb79cb998e)

# CI&CD + Spring 협업

## 1. deploy.yml 설정
- deploy.yml
    ```
    name: Deploy to EC2

    on:
    push:
        branches:
        - main

    jobs:
    Deploy:
        runs-on: ubuntu-latest
        steps:
            - name : Github Repository에 올린 파일들 불러오기
                uses : actions/checkout@v4 # Github Repository에 있는 파일들을 가져옴

            - name : JDK 17 설치
                uses : actions/setup-java@v4
                with:
                distribution: temurin
                java-version: 17

            - name : Gradle Build
                run : ./gradlew clean build

            - name : 빌드 파일 이름 변경
                run : mv ./build/libs/*SNAPSHOT.jar ./project.jar

            - name : SCP로 EC2로 파일 전송
                uses : appleboy/scp-action@v0.1.7
                with:
                host: ${{ secrets.EC2_HOST }}
                username: ${{ secrets.EC2_USERNAME }}
                key: ${{ secrets.EC2_PRIVATE_KEY }}
                source: project.jar
                target: /home/ubuntu/instagram-server/tobe

            - name: SSH to EC2
                uses: appleboy/ssh-action@v1.0.3
                with:
                host: ${{ secrets.EC2_HOST }}
                username: ${{ secrets.EC2_USERNAME }}
                key: ${{ secrets.EC2_PRIVATE_KEY }}
                script_stop: true
                script: |
                    rm -rf /home/ubuntu/instagram-server/current
                    mkdir /home/ubuntu/instagram-server/current
                    mv /home/ubuntu/instagram-server/tobe/project.jar /home/ubuntu/instagram-server/current/project.jar
                    cd /home/ubuntu/instagram-server/current
                    sudo fuser -k -n tcp 8080 || true
                    nohup java -jar project.jar > ./output.log 2>&1 &
                    rm -rf /home/ubuntu/instagram-server/tobe
    ```
