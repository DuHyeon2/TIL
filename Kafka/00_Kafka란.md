![Image](https://github.com/user-attachments/assets/44775098-769e-4d9c-9f2e-66d54322dc8f)

# Kafka 란
- 대용량 데이터 처리를 하는 메세징 시스템
- Stream 처리와 실시간 데이터 파이프라인 및 대규모 데이터 통합을 위한 시스템
- 이벤트 스트림을 게시(쓰기)하고 구독(읽기)한다.

## 1. Producer
- Topic에 메세지를 보내는 역할
- 메세지를 보낼때 Topic과 Value는 무조건 지정해줘야함

## 2. Topic
- Partition으로 구성된 로그 파일
- 하나의 Topic은 여러개의 Partition으로 구성되어 있다. 
- RDBMS의 Table과 유사한 기능
- Key-Value 기반 메시지 구조
- Value는 모든 타입의 메시지 가능
- 시간의 흐름에 따라 메시지가 순차적으로 offset값이 증가하면서 물리적인 파일에 Write된다.

## 3. Partition
- Kafka의 병렬 분산처리의 핵심 
- Kafka가 유명해진 이유가 병렬 성능이 좋아서
- Partition떄문에 Kafka가 유명해졌다 봐도 된다

## 4. Offset
- 토픽 내 파티션에 추가된 메세지의 일련 번호

## 5. Consumer
- Topic에서 메세지를 읽어 들임
- Topic에 메세지를 보내는 Producer와 반대의 역할

## 6. Broker
- Producer가 메세지를 보내고 Consumer가 메세지를 읽는 서버
- Kafka Cluster를 구성하는 서버