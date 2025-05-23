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

## 2. SimpleProducer 설정
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
    ```