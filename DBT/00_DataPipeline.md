# DataPipeline

## 1. DataPipeline 란
- 여러가지 소스에서 데이터를 모으는 것
- 한 저장소에 데이터를 저장 함
- 분석을 위해 데이터를 변형 
- 예시
    - 어플리케이션의 DB
    - log나 event
    - SaaS tools (payments, marketing)

## 2. Traditional ETL
- 데이터 -> 변환 -> Data Warehouse 저장(ETL)
- 변환 후 로딩
- Spark나 custom code를 사용한 무거운 파이프라인
- 스키마를 일찍 지정함
- 문제점
    - 바꾸는데 오래걸림
    - 비용이 많이 듬
    - 분석가 입장에선 바로바로 보여지기 힘듬

## 3. Modern ELT 
- 데이터 -> Data Warehouse 저장 -> 변환(ELT)
- Raw data를 먼저 로딩 함
- 이후 warehouse에서 변환을 함
- 이점
    - 바꾸는데 빠름
    - 비용도 쌈
    - More flexible analytics


