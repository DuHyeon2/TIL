![Image](https://github.com/user-attachments/assets/44775098-769e-4d9c-9f2e-66d54322dc8f)

# Kafka Consumer

## 1. Consumer란
- Topic에서 메세지를 읽어 들임
- 모든 Consumer들은 단 하나의  Consumer Group에 소속되어야 한다.
- Consumer Group은 1개 이상의 Consumer를 가질 수 있다.
- 파티션의 레코드들은 단 하나의 Consumer에만 할당

## 2. Consumer Group
- 하나 이상의 Consumer로 구성되어 있는 그룹
- 고유 그룹 id를 가지고 있음
- 그룹 내에선 **Rebalancing**이 일어남
- 동일 그룹 안에 있는 Consumer들은 최대한 균등하게 Partition을 배분
- 서로 다른 Cousumer Group 내에선 분리되어 독립적으로 동작

## 3. Rebalancing

































```
kafka-console-consumer --bootstrap-server localhost:9092 --group group_01 --topic multipart-topic --property print.key=true --property print.value = true --property print.partition=true
```