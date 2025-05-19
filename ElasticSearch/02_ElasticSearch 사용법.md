# ElasticSearch 사용법

## 1. 인덱스 생성/조회/삭제
```bash
# users라는 인덱스를 생성
PUT /users 

# users라는 인덱스 조회
GET /users 

# users라는 인덱스 삭제
DELETE /users
```

## 2. 매핑 정의
```bash
# users의 매핑 정의
PUT /users/_mappings
{
    "properties": 
    {
        "name": { "type": "keyword" },
        "age": { "type": "integer" },
        "is_active": { "type": "boolean" }
    }
}

# 정의한 users의 매핑 조회
GET /users 
```

## 3. 도큐먼트 삽입/조회/수정
- 도큐먼트 삽입
    ```bash
    # users의 도큐먼트 삽입
    POST /users/_doc
    {
        "name": "Alice",
        "age": 28,
        "is_active": true
    }

    POST /users/_doc
    {
        "name": "Bob",
        "age": 30,
        "is_active": false
    }
    ```

- 모든 도큐먼트 조회
    ```bash
    # users의 도큐먼트 조회
    GET /users/_search
    ```

- 도큐먼트 저장 (id 직접 지정)
    ```bash
    # users의 인덱스에 id=1인 도큐먼트 저장
    POST /users/_create/1
    {
        "name": "jscode",
        "age": 30,
        "is_active": true
    }
    ```

- 도큐먼트 upsert
    ```bash
    # users의 인덱스에 id=2인 도큐먼트 저장(이미 있다면 수정)
    PUT /users/_doc/2
    {
        "name": "jason",
        "age": 30,
        "is_active": true
    }
    ```

- 특정 도큐먼트 조회
    ```bash
    # users의 id=1인 도큐먼트 조회
    GET /users/_doc/1
    ```

- 특정 도큐먼트 수정
    - 수정 시 통째로 덮어씌워짐
    ```bash
    # 이 API는 데이터 저장 시에도 사용하는 API이다. (바로 위에서 언급했었음)
    # users의 id=1인 도큐먼트 수정
    PUT /users/_doc/1
    {
        "name": "new"
    }
    ```

    - 일부 필드만 수정됨
    ```bash
    # users의 id=2인 도큐먼트 수정
    POST /users/_update/2
    {
        "doc": 
        {
            "age": 10,
            "is_active": false
        }
    }
    ```

- 도큐먼트 삭제하기
    ```bash
    # users의 id=2인 도큐먼트 삭제
    DELETE /users/_doc/2
    ```