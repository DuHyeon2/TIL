![Image](https://github.com/user-attachments/assets/44775098-769e-4d9c-9f2e-66d54322dc8f)

# Kafka Producer

## 1. Producer란
- Topic에 메세지를 보내는 역할
- 메세지를 보낼때 **Topic**과 **Value**는 무조건 지정해줘야함
- 메세지를 직렬화 시켜서 Topic에 보냄


## 2. Partitioner
- Producer가 메세지를 보낼때 메세지가 어디 Partition으로 갈지 정해준다.
- 동작 방식
    1. Key가 있으면
        - Key를 해시(hash) → 파티션 인덱스 결정
        - 항상 같은 Key → 같은 파티션으로 라우팅 → 파티션 내 순서 보장
    2. Key가 없으면
        - **kafka 2.4 이하 (Round-robin)**
            - 각 메시지를 순서대로 다음 파티션에 순환 배분  
            - 부하 분산은 가능하나, 메시지 그룹화나 순서 보장이 어려움
        - **Kafka 2.4 이상 (Sticky Partitioner)**  
            - 한 번 선택된 파티션을 일정 배치(batch.size)만큼 “고정(Sticky)” 사용  
            - 배치가 가득 차거나 타임아웃(`linger.ms`)이 지나면 다른 파티션으로 전환  
            - 대량 전송 시 네트워크 효율(배치 처리) 개선  
             

## 3. Key
- Producer가 메세지를 보낼 때 Key를 지정할수도 하지 않을수도 있다.
- 여기서 Key는 메세지를 찾는 Key라기 보다 어떤 Patition으로 가야할지 정해주는거
- **Key 미지정 시**  
    - 파티션 전략이 선택되어 파티션 별로 메세지가 전송될 수 있음
    - Topic이 여러개의 파티션을 가질 때 메세지의 전송 순서가 보장되지 않을 채로 Consumer에서 읽혀질 수 있음
- **Key 지정 시**
    - 메세지 Key는 업무 로직이나 메세지 Produce/Consume시 분산 성능 영향을 고려하여 생성
    - 특정 Key값을 가지는 메세지는 특정 파티션으로 고정되어 전송된다.
- Kafka는 하나의 파티션 내에서만 메세지의 순서 보장

## 4. 전송방법
```bash
$ kafka-console-producer --bootstrap-server localhost:9092 --topic <토픽명> --property key.separator=: --property parse.key=true
```
- `--property key.separator=:`
    - 메시지 입력 시 key:value 형태로 구분

- `--property parse.key=true`
    - 입력값 앞부분을 Key로 파싱