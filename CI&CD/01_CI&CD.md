![ci-cd](https://github.com/user-attachments/assets/20f71753-d6da-457e-8dab-62bb79cb998e)

# CI/CD란
- CI/CD란 테스트(Test), 통합(Merge), 배포(Deploy)의 과정을 자동화 하는것

## 1. CI/CD가 왜 필요한지?
- 서비스 운영 중 새로운 기능을 추가하기의 과정이 있다.
- 커밋-빌드-테스트-배포
- 위 과정이 반복되면 귀찮아지기에 이런 박복적인 과정을 자동화 하기 위해 필요하다!
- Github Actions, Jenkins, Circle CI, Travis CI 등이 있다.

## 2. Github Actions 
- 로직을 실행시킬 수 있는 일종의 컴퓨터
- Commit -> Push -> Push감지 후 로직(Build, Test, Deploy) 실행 -> 서버에 배포된 최신 코드로 서버 재실행

## 3. 예제
- 디렉터리 구조

    ![Alt text](img/01_%EB%94%94%EB%A0%89%ED%86%A0%EB%A6%AC%EA%B5%AC%EC%A1%B0.png)

    - 최상단/.github/workflows/[이름].yml 형식

- yml 파일
    ```
    # Workflow의 이름
    name: Github Actions 실행시켜보기

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
        My-Deploy-Job:
            # Job을 실행하는 환경 설정
            runs-on: ubuntu-latest

            # Step: 특정 작업을 수행하는 가장 작은 단위
            # Job은 여러 Step으로 구성
            steps:
            - name: Hello World 찍기
                run: echo "Hello World"

            - name: 여러 명령어 문장 작성하기
                run: |
                    echo "First"
                    echo "Second"
                    echo "Third"

            - name: Github Actions에서 환경변수 사용하기
                run: |
                    echo $GITHUB_SHA
                    echo $GITHUB_REPOSITORY

            - name: 아무한테도 노출되면 안되는 값
                run: |
                    echo ${{ secrets.MY_NAME }}
                    echo ${{ secrets.MY_HOBBY }}
    ```
    