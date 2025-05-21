![Image](https://github.com/user-attachments/assets/44775098-769e-4d9c-9f2e-66d54322dc8f)

# Kafka 기본 사용
다음은 Kafka 브로커에서 토픽(topic)을 생성·조회·삭제·정보 확인하는 기본 명령어입니다.

## 1. 토픽 생성
```bash
$ kafka-topics --bootstrap-server localhost:9092 --create --topic <토픽명> --partitions <파티션 개수>
```
- `--bootstrap-server <주소:포트>`  
  Kafka 브로커에 연결할 서버 주소와 포트  
- `--create`  
  토픽 생성 명령어  
- `--topic <토픽명>`  
  생성할 토픽 이름  
- `--partitions <파티션 개수>`  
  파티션 수 지정 (지정하지 않으면 기본값 1)

## 2. 토픽 리스트 확인
```bash
$ kafka-topics --bootstrap-server localhost:9092 --list
```
- Kafka 브로커에 있는 Topic 리스트

## 3. 토픽 삭제
```bash
$ kafka-topics --bootstrap-server localhost:9092 --delete --topic <토픽명>
```

## 4. 토픽의 상세 정보 보기
```bash
$ kafka-topics --bootstrap-server localhost:9092 --describe --topic <토픽명>
```
- 파티션 개수, 리더(leader), ISR(동기 복제본) 등 토픽의 상세 메타데이터 확인