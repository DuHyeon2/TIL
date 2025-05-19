# ElasticSearch + Spring 사용

## 1. yml 설정
- elasticsearch의 url 값을 넣어준다
    ```yml
    spring:
        elasticsearch:
            uris: indie7942.store:9210

    logging:
        level:
            org.elasticsearch.client: TRACE
    ```
- 편의를 위해 로그도 추가

##  2. Repository 설정
- elasticsearch JPA를 만들어 준다
    ```java
    import org.springframework.data.elasticsearch.repository.ElasticsearchRepository;

    public interface UserDocumentRepository extends <UserDocument, String> {
    
    }
    ```
