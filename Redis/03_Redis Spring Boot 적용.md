![redis](https://github.com/user-attachments/assets/b7b855de-7bcc-403e-9e5f-f11b0ac8a277)

# Redis Spring Boot 적용하기

## 1. Spring Boot 설정
- build.gradle
    ```
    dependencies {
        ...
        implementation 'org.springframework.boot:spring-boot-starter-data-redis'
        ...
    }
    ```
    - Redis를 추가한다.


- RedisConfig.java
    ```
    @Configuration
    public class RedisConfig {

        @Value("${spring.data.redis.host}")
        private String host;

        @Value("${spring.data.redis.port}")
        private int port;

        @Bean
        public LettuceConnectionFactory redisConnectionFactory() {
            return new LettuceConnectionFactory(new RedisStandaloneConfiguration(host, port));
        }
    }
    ```

- RedisCacheConfig.java
    ```
    @Configuration
    @EnableCaching
    public class RedisCacheConfig {

        @Bean
        public CacheManager boardCacheManager(RedisConnectionFactory connectionFactory) {
            RedisCacheConfiguration cacheConfiguration = RedisCacheConfiguration
                    .defaultCacheConfig()
                    // Redis에 Key를 저장할 때 String으로 직렬화해서 저장
                    .serializeKeysWith(
                            RedisSerializationContext.SerializationPair.fromSerializer(
                                    new StringRedisSerializer())
                    )
                    // Redis에 Value를 저장할 때 JSON으로 직렬화해서 저장
                    .serializeValuesWith(
                            RedisSerializationContext.SerializationPair.fromSerializer(
                                    new Jackson2JsonRedisSerializer<Object>(Object.class))
                    )
                    // 캐시 만료 시간 설정(TTL)
                    .entryTtl(Duration.ofMinutes(1L));

            return RedisCacheManager
                    .RedisCacheManagerBuilder
                    .fromConnectionFactory(connectionFactory)
                    .cacheDefaults(cacheConfiguration)
                    .build();
        }

    }

    ```

- BoardService.java
    ```
    @Service
    public class BoardService {

        private final BoardRepository boardRepository;

        public BoardService(BoardRepository boardRepository) {
            this.boardRepository = boardRepository;
        }

        @Cacheable(value = "getBoards", key = "'boards:page:' + #page + ':size:' + #size", cacheManager = "boardCacheManager")
        public List<Board> getBoards(int page, int size) {
            Pageable pageable = PageRequest.of(page-1, size);
            Page<Board> pageOfBoards = boardRepository.findAllByOrderByCreatedAtDesc(pageable);
            return pageOfBoards.getContent();
        }
    }
    ```

## 2. 결과 보기
- Redis Cache 저장 전
![alt text](<img/03_Redis Spring Boot 값 넣기 전.png>)

- Redis Cache 저장 후
![alt text](<img/03_Redis Spring Boot 값 넣기 후.png>)

- Redis 결과  
![alt text](<img/03_Redis Spring Boot 값 결과.png>)