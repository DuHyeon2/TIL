# Spring Batch 시작

## 1. EnableBatchProcessing
- 스프링 배치가 작동하기 위해 선언해야 하는 어노테이션
- 스프링 배치의 모든 초기화 및 실행 구성이 이루어지다
    1. BatchAutoConfiguration
        - Job을 수행하는 JobLauncherApplicationRunner 빈 생성
    2. SimpleBatchConfiguration

    3. BatchConfigurerConfiguration
        - BasicBatchConfigurer
        - JpaBatchConfigurer
- 2 -> 3 -> 1 순으로 초기화