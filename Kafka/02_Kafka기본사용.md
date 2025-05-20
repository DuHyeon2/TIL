![Image](https://github.com/user-attachments/assets/44775098-769e-4d9c-9f2e-66d54322dc8f)

# Kafka 기본 사용

## 1. 토픽 생성
```bash
$ kafka-topics --bootstrap-server localhost:9092 --create --topic <토픽명> <파티션 개수>
```
- --bootstrap-server <주소:포트>
    - kafka에 브로커 접속 명령어

- --create <토픽명> <파티션 개수>
    - 토픽을 만들고 파티션의 개수를 지정함
    - 파티선 개수를 지정하지 않으면 기본으로 1개(이 기본값도 프로퍼티에서 따로 설정할 수 있음)

## 2. 토픽 리스트 확인
```bash
$ kafka-topics --bootstrap-server localhost:9092 --list
```
- Kafka 브로커에 있는 Topic 리스트

## 3. 토픽 삭제
```bash
$ kafka-topic --bootstrap-server localhost:9092 --delete --topic <토픽명>
```

## 4. 토픽의 상세 정보 보기
```bash
$ kafka-topic --bootstrap-server localhost:9092 --describe <토픽명>
```