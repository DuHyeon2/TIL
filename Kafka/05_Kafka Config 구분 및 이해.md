![Image](https://github.com/user-attachments/assets/44775098-769e-4d9c-9f2e-66d54322dc8f)

# Kafka Config 구분 및 이해

## 1. Broker와 Topic Config
- Broker와 Topic의 설정을 구분하는 방법
- Broker는 Kafka 서버의 설정
- Topic은 Kafka 서버에 생성된 Topic의 설정
- Topic의 설정은 Broker의 설정을 상속받음
- Topic의 설정은 Broker의 설정을 덮어씀

## 2. Producer 와 Consumer Config
- Producer와 Consumer의 설정을 구분하는 방법
- Producer는 Kafka 서버에 메세지를 보내는 설정
- Consumer는 Kafka 서버에 메세지를 받는 설정
- Producer의 설정은 Broker의 설정을 상속받음
- Consumer의 설정은 Broker의 설정을 상속받음
- Producer의 설정은 Broker의 설정을 덮어씀
- Consumer의 설정은 Broker의 설정을 덮어씀
