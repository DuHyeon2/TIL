# PostgreSQL의 비동기 작업
spring에서 실시간 알람 기능을 구현하던 도중 DB에 새로운 값이 들어오는걸 실시간으로 감지를 해야했다. <br>
찾던 도중 PostgreSQL의 Trigger를 기반으로 한 Listen & Notify라는게 있다는걸 알았다.


## 1. PostgreSQL Listen & Notify
- PostgreSQL Listen & Notify란 Trigger를 기반으로 한 비동기 작업 <br>
- DB의 테이블 row가 변경될때마다 공지하는 Notify <br>
- 공지하는 내용을 듣는 채널인 Listen <br>

## 2. Notify 설정
```
CREATE OR REPLACE FUNCTION public.notify_alarm()
 RETURNS trigger
 LANGUAGE plpgsql
AS $function$
begin
	RAISE NOTICE '%', NEW.alm_info;	
  PERFORM pg_notify('alarm_watcher', 
                  NEW.alm_pk || ',' || NEW.alm_info);
  RETURN NEW;
END;
$function$
```

- pg_notify로 공지 내용을 설정하고 'alarm_watcher' 라는 채널 생성 <br>
- 알람PK와 알람 내용을 공지 <br>

## 3. 트리거 등록
```
CREATE TRIGGER 
	ALARM_TRIGGER
AFTER INSERT 
	ON ALARM_TABLE
FOR EACH ROW
EXECUTEFUNCTION NOTIFY_ALARM()
```
- ALARM_TABLE에 Trigger 생성 <br>

## 4. Listen 채널 감지
```
import org.postgresql.PGConnection;
import org.postgresql.PGNotification;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.jdbc.core.JdbcTemplate;

import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;
import javax.annotation.Resource;
import java.sql.Connection;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

public class PostgreSQLNotificationListener {

    private final Logger LOGGER = LoggerFactory.getLogger(this.getClass());
    private static final String LISTEN_CHANNEL = "alarm_watcher";
    private static final int SLEEP_INTERVAL = 100;

    private JdbcTemplate jdbcTemplate;

    private ExecutorService executorService;
    private volatile boolean isRunning = true;
    private Connection connection;

    public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    @PostConstruct
    public void initialize() {
        listenForNotifications();
    }

    public void listenForNotifications() {
        try (Connection connection = jdbcTemplate.getDataSource().getConnection();
             Statement stmt = connection.createStatement()) {
            PGConnection pgConnection = connection.unwrap(PGConnection.class);
            stmt.execute("LISTEN " + LISTEN_CHANNEL);

            executorService = Executors.newSingleThreadExecutor();
            executorService.submit(() -> {
                try {
                    while (isRunning) {
                        PGNotification[] notifications = pgConnection.getNotifications();
                        if (notifications != null) {
                            for (PGNotification notification : notifications) {
                                handleNotification(notification);
                            }
                        }
                        Thread.sleep(SLEEP_INTERVAL);
                    }
                } catch (SQLException | InterruptedException e) {
                    LOGGER.error("Error in notification listener", e);
                    Thread.currentThread().interrupt();
                }
            });
        } catch (SQLException e) {
            LOGGER.error("Error setting up database connection", e);
        }
    }

    private void handleNotification(PGNotification notification) {

            LOGGER.info("Notification received: {}", notification.getParameter());
        
    }

    @PreDestroy
    public void destroy() {
        isRunning = false;

        if (executorService != null) {
            executorService.shutdown();
            try {
                if (!executorService.awaitTermination(10, TimeUnit.SECONDS)) {
                    LOGGER.warn("Executor service did not terminate gracefully");
                }
            } catch (InterruptedException e) {
                LOGGER.error("Error during executor service termination", e);
                Thread.currentThread().interrupt();
            }
        }

        try {
            if (connection != null && !connection.isClosed()) {
                connection.close();
            }
        } catch (SQLException e) {
            LOGGER.error("Error closing database connection", e);
        }
    }
}
```
- Java Spring의 SingleThread를 이용해 Listen채널 감지<br>
- Listen "채널이름" 으로 채널 감지<br>
