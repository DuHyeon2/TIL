# ElasticSearch란?

## 1. ElasticSearch
- 검색, 데이터 분석에 최적화된 데이터베이스이다

## 2. ElasticSearch의 주요 활용 사례
- 데이터 수집 및 분석
- 검색 최적화

## 3. ElasticSearch의  작동 방식
- MySQL은 SQL문으로 통신
- 그렇지만 ElasticSearch는 REST API 형식으로 통신
- 예시

    ```sql
    INSERT INTO users(name, email) VALUES ('Elastic', 'test@test.com')
    ```

    ```powershell
     curl -X POST "localhost:9200/user/_doc" -H 'Content-Type: applycation/json' -
     {
        "name": "Elastic",
        "email": "test@test.com"
     }
    ```