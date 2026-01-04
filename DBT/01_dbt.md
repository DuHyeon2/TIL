# dbt

## 1. dbt란?
- data build tool의 줄임말
- 분석 엔지니어가 SQL과 소프트웨어 엔지니어링을 사용해서 데이터 웨어하우스 내부에서 데이터를 변환할 수 있게 해줌
- 한마디로 dbt는 변환을 함
- 추출(Extract)이나 적재(Load)는 dbt 역할이 아님
- ELT에서 T 담당!
- 변환 작업은 웨어하우스 내에서 실행
    - 예 : BigQuery, Snowflake, Redshiftm Databricks, Postgres
