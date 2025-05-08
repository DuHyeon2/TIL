# Kafka Producer

## 1. Producer란
- Topic에 메세지를 보내는 역할
- 메세지를 보낼때 Topic과 Value는 무조건 지정해줘야함
- 메세지를 직렬화 시켜서 Topic에 보냄

## 2. Partitioner
- Producer가 메세지를 보낼때 메세지가 어디 Partition으로 갈지 정해준다.

## 3. Key
- Producer가 메세지를 보낼 때 Key를 지정할수도 하지 않을수도 있다.
- 여기서 Key는 메세지를 찾는 Key라기 보다 어떤 Patition으로 가야할지 정해주는거
- Key를 지정하지 않았을때
    - 파티션 전략이 선택되어 파티션 별로 메세지가 전송될 수 있음
    - Topic이 여러개의 파티션을 가질 때 메세지의 전송 순서가 보장되지 않을 채로 Consumer에서 읽혀질 수 있음
- Key를 지정할 때
    - 메세지 Key는 업무 로직이나 메세지 Produce/COnsume시 분산 성능 영향을 고려하여 생성
    - 특정 Key값을 가지는 메세지는 특정 파티션으로 고정되어 전송된다.
- Kafka는 하나의 파티션 내에서만 메세지의 순서 보장