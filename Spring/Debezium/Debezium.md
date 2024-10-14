![debezium](https://github.com/DuHyeon2/DailyStudy/assets/83499405/8272a82d-9b52-4af3-b912-45d7022c5f38)

# Spring CDC(Change Data Capture)
spring에서 실시간 알람 기능을 구현하던 도중 DB에 새로운 값이 들어오는걸 실시간으로 감지를 해야했다. <br>
찾던 도중 데이터베이스에서 변경된 데이터를 캡쳐하는 CDC라는 것을 알게 되었다. <br>
CDC란 Change Data Capture의 약자로 변경된 내용을 골라내는 패턴이다. <br>


## 1. Debezium
- Debezium은 Kafka기반의 변경된 데이터 캡쳐를 위한 오픈 소스 분산 플랫폼이다.  <br>
- 데이터가 변경되면 Debezium은 변경된 데이터를 캡쳐 해 Kafka에 전달한다. <br>

## 2. Embedded Debezium 설정하기(Spring)
- pom.xml 설정
```
<dependency>
  <groupId>io.debezium</groupId>
  <artifactId>debezium-core</artifactId>
  <version>1.8.0.Final</version>
</dependency>
<dependency>
  <groupId>io.debezium</groupId>
  <artifactId>debezium-embedded</artifactId>
  <version>1.8.0.Final</version>
</dependency>
<dependency>
  <groupId>io.debezium</groupId>
  <artifactId>debezium-connector-postgres</artifactId>
  <version>1.8.0.Final</version>
</dependency>
```
- Spring 버전에 맞게 설정(본인은 Spring 4.3.30.RELEASE)
- DB는 PostgreSQL 11

## 3. 구현하기

- # DebeziumConfig
  ```
  import org.springframework.beans.factory.annotation.Value;
  import org.springframework.context.annotation.Bean;
  import org.springframework.context.annotation.Configuration;


  //TODO: DB wal_level 이 logical 일때만 사용가능(현재 replica)
  @Configuration
  public class DebeziumConfig {

      @Value("${spring.datasource.db1.url}")
      private String url;

      @Value("${spring.datasource.db1.username}")
      private String username;

      @Value("${spring.datasource.db1.password}")
      private String password;

      @Value("${spring.datasource.db1.url}")
      private String dburl;

      @Value("${spring.datasource.db1.port}")
      private String port;

      @Value("${spring.datasource.db1.host}")
      private String host;

      @Value("${spring.datasource.db1.dbname}")
      private String dbname;

      @Bean
      public io.debezium.config.Configuration debeziumConnector(){
          return io.debezium.config.Configuration.create()
                  .with("name", "임의 커넥터 이름")
                  .with("connector.class", "io.debezium.connector.postgresql.PostgresConnector")
                  .with("database.hostname", host)
                  .with("database.port", port)
                  .with("database.user", username)
                  .with("database.password", password)
                  .with("database.dbname", dbname)
                  .with("database.server.name", "임의 설정")
                  .with("table.include.list", "감지할 테이블")
                  .with("plugin.name", "pgoutput")
                  .with("slot.name", "슬롯 이름")
                  .with("slot.drop.on.stop", "false")
                  .with("offset.storage", "org.apache.kafka.connect.storage.FileOffsetBackingStore")
                  .with("offset.storage.file.filename", "./offsets.dat")
                  .with("offset.flush.interval.ms", "60000")
                  .build();
      }
  }
  ```

- # DebeziumListener
  ```
  import io.debezium.config.Configuration;
  import io.debezium.embedded.Connect;
  import io.debezium.engine.DebeziumEngine;
  import io.debezium.engine.RecordChangeEvent;
  import io.debezium.engine.format.ChangeEventFormat;
  import lombok.extern.slf4j.Slf4j;
  import org.apache.kafka.connect.source.SourceRecord;
  import org.springframework.stereotype.Component;

  import javax.annotation.PostConstruct;
  import javax.annotation.PreDestroy;
  import java.io.IOException;
  import java.util.concurrent.Executor;
  import java.util.concurrent.Executors;

  @Slf4j
  @Component
  public class DebeziumListener {
      private final Executor executor = Executors.newSingleThreadExecutor();
      private final DebeziumEngine<RecordChangeEvent<SourceRecord>> debeziumEngine;

      public DebeziumListener(Configuration config) {
          System.out.println("DebeziumListener Start");
          this.debeziumEngine = DebeziumEngine.create(ChangeEventFormat.of(Connect.class))
                  .using(config.asProperties())
                  .notifying(this::handleRecord)
                  .build();
      }

      private void handleRecord(RecordChangeEvent<SourceRecord> record) {
          System.out.println(record);
          // 감지한 데이터를 처리하는 로직 작성
      }

      @PostConstruct
      public void start() {
          this.executor.execute(debeziumEngine);
      }

      @PreDestroy
      public void stop() throws IOException {
          if (this.debeziumEngine != null) {
              this.debeziumEngine.close();
          }
      }
  }
  ```

- Spring에서 자신이 연결한 DB를 Debezium에서도 설정
- 이후 쓰레드 형식을 이용해 변경되는 데이터를 실시간으로 감지

## 4. 주의사항
```
SHOW wal_level;
```
![image](https://github.com/DuHyeon2/DailyStudy/assets/83499405/a7451684-60cc-4886-a8c4-872bbec8b2b5)

- wal_level은 PostgreSQL에서 WAL에 기록되는 정보의 양을 결정하는데, replica와 logical, minimal 이 있다. <br>
- **replica**는 물리적 디코딩을 지원하는 데 필요한 정보만 제공 <br>
- **logical**은 논리적 디코딩을 지원하는 데 필요한 정보를 추가 <br>
- **minimal**은 복구하기위해 필요한 정보만 기록 <br>
- 본인은 PostgreSQL을 사용했는데 wal level 이 replica였는데 Debezium은 wal_level이 logical이여야만 지원을 해 준다. <br>

