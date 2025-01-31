![redis](https://github.com/user-attachments/assets/b7b855de-7bcc-403e-9e5f-f11b0ac8a277)

# Redis 설치 및 명령어
- MacOS와 Windows 설치법으로 나뉜다.

## 1. Redis 설치(MacOS)
- MacOS
    ```
    $ brew install redis # redis 설치

    $ brew services info redis # redis 설치 확인

    $ brew services start redis # redis 시작

    $ brew services stop redis # redis 종료

    $ redis-cli # redis 접속

    > ping # redis 연결 확인 
    PONG
    ```
    ![alt text](<img/01_Redis 실행.png>)

- Windows

## 2. Redis 기본 명령어
- redis 데이터 넣기, 가져오기
    ```
    > set [키] [값]
    OK
    > get [키]
    [값]
    ```
    ![alt text](<img/01_Redis 데이터 넣기.png>)

- redis 데이터 넣기(TTL 지정)
    - Redis는 많은 양의 데이터를 저장하기에 무리가 있기에 만료시간(TTL, Time To Live)을 지정해서 저장하는것이 일방적이다.
    ```
    > set [키] [값] ex [시간(초단위)]
    OK
    > ttl [키]
    [데이터의 만료 시간 출력]
    ```
    ![alt text](<img/01_Redis 데이터 넣기(TTL).png>)

- 등록된 데이터 모두 보기
    ```
    > keys *
    [등록된 데이터 리스트]
    ```
    ![alt text](<img/01_Redis 데이터 키 모두보기.png>)

- 데이터 삭제
    ```
    > del [키]
    ```
    ![alt text](<img/01_Redis 데이터 삭제.png>)

- 데이터 모두 삭제
    ```
    > flushall
    OK
    ```
    ![alt text](<img/01_Redis 데이터 모두 삭제.png>)

## 3. Redis Key 네이밍 컨벤션
- 콜론( : ) 을 활용해 계층적 의미로 구분해서 사용
    - ex) users:100:profile : 사용자 중 pk가 100인 사용자의 프로필