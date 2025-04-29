# 카프카란
- 대용량 데이터 처리를 하는 메세징 시스템
- Stream 처리와 실시간 데이터 파이프라인 및 대규모 데이터 통합을 위한 시스템
- 이벤트 스트림을 게시(쓰기)하고 구독(읽기)한다.

## 1. Producer

## 2. Topic
- Partition으로 구성된 로그 파일
- 하나의 Topic은 여러개의 Partition으로 구성되어 있다. 
- RDBMS의 Table과 유사한 기능
- Key-Value 기반 메시지 구조
- Value는 모든 타입의 메시지 가능
- 시간의 흐름에 따라 메시지가 순차적으로 offset값이 증가하면서 물리적인 파일에 Write된다.

## 3. Partition
- Kafka의 병렬 분산처리의 핵심

## 4. Consumer
