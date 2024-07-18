![도커 기본이미지](https://github.com/user-attachments/assets/90b27680-a3da-473c-bc18-62e8f993a28f)

# Docker Node.js 어플 만들기

## 1. Node.js 파일 생성

1. package.json 생성
    ```
    {
    "name": "nodejs-docker-app",
    "version": "1.0.0",
    "description": "",
    "main": "server.js",
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
    },
    "dependencies": {
        "express": "^4.16.1"
    },
    "author": "",
    "license": "ISC"
    }
    ```
2. server.js 생성
    ```
    const express = require('express');

    const PORT = 8080;

    const app = express();
    app.get('/', (req, res) => {
        res.send('Hello World\n');
    });

    app.listen(PORT);
    console.log(`Running on http://localhost:${PORT}`);
    ```

## 2. DockerFile 생성
1. DockerFile 생성
    ```
    FROM node:10

    WORKDIR /usr/src/app

    COPY ./ ./

    RUN npm install

    CMD ["node", "server.js"]

    ```
- FROM : Node.js의 버전을 베이스 이미지로 지정
- WORKDIR : 이미지안에서 어플리케이션 소스 코드를 갖고있을 디렉토리를 생성하는것
    ![Alt text](<img/도커 파일 실행하기-1.png>)

- COPY : 컨테이너에 필요한 파일을 컨테이너 안으로 가져옴
- RUN : package.json에 있는 종속성들 설치
- CMD : 컨테이너가 실행됐을때 nodejs의 server.js가 실행된다


## 3. Docker 컨테이너 실행
1. Docker 컨테이너 실행
- Docker 컨테이너의 8080 포트를 로컬의 5001의 포트로 매핑하여 접근
![Alt text](<img/도커 파일 실행하기-2.png>)


## 4. Docker 컨테이너 실행 결과
![Alt text](<img/도커 파일 실행하기-3.png>)