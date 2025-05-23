![Image](https://github.com/user-attachments/assets/44775098-769e-4d9c-9f2e-66d54322dc8f)

# Kafka Producer 실습
- Client java JDK 17
- Kafka JDK 11

## 1. 의존성 추가
```gradle
...

dependencies {
    implementation 'org.apache.kafka:kafka-clients:3.1.0'
    implementation 'org.slf4j:slf4j-api:1.7.36'
    implementation 'org.slf4j:slf4j-simple:1.7.36'
}

...
```
-  `kafka-clients` : 카프카 클라이언트 라이브러리(3.1.0)
-  `slf4j-api` : 로깅 API
-  `slf4j-simple` : SLF4J Simple 로깅 구현체

## 2. 실습
```java
import org.apache.kafka.clients.producer.KafkaProducer;
import org.apache.kafka.clients.producer.ProducerConfig;
import org.apache.kafka.clients.producer.ProducerRecord;
import org.apache.kafka.common.serialization.StringSerializer;

import java.util.Properties;

public class SimpleProducer {
    public static void main(String[] args) {

        String topicName = "simple-topic";

        // Kafka producer configuration
        Properties props = new Properties();

        props.setProperty(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "카프카 서버 주소");
        props.setProperty(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());
        props.setProperty(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());


        // KafkaProducer 객체 생성
        KafkaProducer<String, String> kafkaProducer = new KafkaProducer<String, String>(props);

        // ProducerRecord 객체 생성
        ProducerRecord<String, String> producerRecord = new ProducerRecord<>(topicName, "hello world");

        // KafkaProducer message 전송
        kafkaProducer.send(producerRecord);

        kafkaProducer.flush();
        // close하면 flush가 자동으로 호출됨
        kafkaProducer.close();
    }
}
```
- `ProducerConfig` : Producer 설정을 위한 클래스
- `BOOTSTRAP_SERVERS_CONFIG` : 카프카 서버 주소
- `KEY_SERIALIZER_CLASS_CONFIG` : Key 직렬화 클래스
- `VALUE_SERIALIZER_CLASS_CONFIG` : Value 직렬화 클래스
- `KafkaProducer` : 카프카 Producer 객체
- `ProducerRecord` : Producer가 전송할 메세지 객체
- `send` : Producer가 메세지를 전송하는 메소드
- `close` : Producer 객체를 닫는 메소드

- 결과
    ``` bash
    $ ./kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic simple-topic --from-beginning
    hello world
    ```
## 3. Producer - Broker 동기화/비동기화
- Producer가 메세지를 전송할 때 Broker와 동기화/비동기화 방식으로 전송할 수 있다.
- 동기화 방식
    - Producer가 메세지를 전송할 때 Broker로부터 응답(`Ack 메세지`)을 받을 때까지 대기하는 방식
    - 메세지 전송 후 Broker로부터 응답을 받을 때까지 대기
    - 메세지 전송 후 Broker로부터 응답을 받을 때까지 대기하기 때문에 메세지 전송 속도가 느려질 수 있다.
    - 동기화 방식 예제
        ```java
        try {
            /*
            Future<RecordMetadata> future = kafkaProducer.send(producerRecord);
            RecordMetadata recordMetadata = future.get();
            */
            RecordMetadata recordMetadata = kafkaProducer.send(producerRecord).get();
            logger.info("Record sent to topic {} partition {} offset {}", recordMetadata.topic(), recordMetadata.partition(), recordMetadata.offset());
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        } finally {
            kafkaProducer.close();
        }

        ```
        - `Future` : Producer가 메세지를 전송한 후 Broker로부터 응답(`Ack 메세지`)을 받을 때까지 대기하는 클래스
        - `RecordMetadata` : Producer가 전송한 메세지의 메타데이터를 담고 있는 클래스
        - `get()` : 메세지 전송 후 Broker로부터 응답을 받을 때까지 대기하는 메소드
        - `RecordMetadata`를 통해 메세지가 전송된 Topic, Partition, Offset을 확인할 수 있다.


- 비동기화 방식
    - Producer가 메세지를 전송할 때 Broker로부터 응답(`Ack 메세지`)을 기다리지 않고 다음 메세지를 전송하는 방식
    - 메세지 전송 후 Broker로부터 응답을 받을 때까지 대기하지 않기 때문에 메세지 전송 속도가 빠르다.
    