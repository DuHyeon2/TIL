# Jenkins 설정

## 1. Jenkins 설치
- ubuntu
    ```bash
    ```

- docker
    ```yml
    ```

- kube
    ```yaml
    ```

## 2. Jenkins install
- Jenkins 설치 후 plugins install은 suggest install 로 하면 된다
    - 어짜피 나중에 필요한거 install 할 수 있다.

- 이후 위에 말했듯이 필요한 plugin들을 설치
    - Jenkins관리 - Plugins - Available Plugins
    - Spring 파일을 배포하기 위해 Maven/Gradle, GitHub(이건 깔려있을수도 없을수도) 정도를 설치

- Jenkins credentials 설정
    - Jenkins관리 - Credentials - System - Global credentials - Add Credentials
    - Username : GitHub 아이디
    - Password : 아까 token
    - ID : 젠킨스에서 credentials 설정하면서 부를 식별자(?)
    - Description: 설명
    - 양식에 맞게 넣어준다

## 3. GitHub Token 생성
- GitHub 계정을 jenkins에 token으로 등록 해줘야함
    - GitHub - setting - Personal access tokens>Tokens 로 token 생성
- 나는 repo 랑 project 권한만 줬음(project는 이유없이 일단 줘봄)
- token값은 한번만 보여주니 안전한곳에 잘 보관하기

## 4. Jenkins credentials 설정
- Jenkins관리 - Credentials - System - Global credentials - Add Credentials
    - Username : GitHub 아이디
    - Password : 아까 token
    - ID : 젠킨스에서 credentials 설정하면서 부를 식별자(?)
    - Description: 설명
- 양식에 맞게 넣어준다

## 5. Jenkins Webhook 설정
- Jenkins관리 - Security
- Git Hooks 부분에 Allow on Controller, Allow on Agents 체크 

## 6. Jenkins 프로젝트 생성
- New Item - Pipeline으로 생성
- GitHub Project 체크 후 Project url 넣어주기
    - 해당 GitHub Repository url 넣어주면 된다
- GitHub hook trigger for GITScm polling 체크
- Pipeline에 Definition을 Pipeline script from SCM 선택
    ![alt text](<scm 설정.png>)

## 6. GitHub Webhook 설정
- 해당 Jenkins에 등록된 Repository 이동
- Settings - Webhooks - Add webhook 클릭
    ![alt text](<GitHub webhook 설정.png>)

## 7. 