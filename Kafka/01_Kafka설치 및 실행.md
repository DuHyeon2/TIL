# Kafka 설치 및 실행

## 1. Kafka 설치
- java 11 설치
    ```bash
    $ sudo apt-get update

    $ sudo apt install openjdk-<자바 버전>-jdk
    ```
    - java 11을 설치했음

- Kafka 설치
    ```bash
    $ wget https://packages.confluent.io/archive/7.1/confluent-community-7.1.2.tar.gz

    $ tar -xvf confluent-community-7.1.2.tar.gz

    $ mv confluent-community-7.1.2 confluent
    ```
    - 버전은 골라서 사용 가능
    - 이후 편의성을 위해 디렉토리 이름 변경

- 환경변수 설정
    ```bash
    $ vi .bashrc

    # 이후 접속 편집기 맨 아래에 설정
    export CONFLUENT_HOME=<kafka설치경로>/confluent
    export PATH=.:$PATH:$CONFLUENT_HOME/bin
    ```


## 2. Kafka 설정
- Kafka로그 저장 디렉토리 만들기
    ```bash
    $ mkdir data

    $ cd data

    $ mkdir kafka-logs
    $ mkdir zookeeper
    ```

- 로그 경로 변경
    ```bash 
    $ cd $CONFLUENT_HOME/etc/kafka

    $ vi server.properties

    # 이후 접속 편집기에서 /log.dir 검색 후 경로 수정 후 저장
    log.dir = <Kafka log 경로>

    $ vi zookeeper.properties
    # 이후 접속 편집기에서 /dataDir 검색 후 경로 수정 후 저장
    dataDir = <zookeeper경로>
    ```

## 3. Kafka 실행
- kafka는 zookeeper -> kafka 순으로 실행
- 종료는 역순으로 kafka -> zookeper 종료
- zoo_start.sh 생성
    ```bash
    $ vi zoo_start.sh

    export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64 # java 11 기반으로 실행
    $CONFLUENT_HOME/bin/zookeeper-server-start $CONFLUENT_HOME/etc/kafka/zookeeper.properties
    ```
- kafka_start.sh 생성
    ```bash
    $ vi kafka_start.sh

    export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64 # java 11 기반으로 실행
    $CONFLUENT_HOME/bin/kafka-server-start $CONFLUENT_HOME/etc/kafka/server.properties
    ```
