# dbt

## 1. dbt란?
- data build tool의 줄임말
- 분석 엔지니어가 SQL과 소프트웨어 엔지니어링을 사용해서 데이터 웨어하우스 내부에서 데이터를 변환할 수 있게 해줌
- 한마디로 dbt는 변환을 함
- 추출(Extract)이나 적재(Load)는 dbt 역할이 아님
- ELT에서 T 담당!
- 변환 작업은 웨어하우스 내에서 실행
    - 예 : BigQuery, Snowflake, Redshiftm Databricks, Postgres

## 2. DataLake?
- 원본(Raw) 데이터를 원래 형태대로 보관하는 중앙 집중형 스토리지
- 정형, 비정형, 반정형 데이터 모두 저장 가능
- 데이터를 저장할 때 스키마를 미리 정의하지 않음(Schema-on-read)
- 확장성이 뛰어나고 비용 효율적
- 데이터 예 : JSON, CSV, 이미지, 비디오, 로그 파일 

## 3. Data Warehouse?
- 분석(Analytics) 과 리포팅(Reporting)에 최적화된 데이터 저장소
- 원본을 가공(Processed), 정제(Cleansed), 구조화(Structured)된 형태로 저장
- 대부분 정형(Structured) 데이터
- 데이터가 적재되기 전에 스키마를 정의함(Schema-on-write)
- 빠른 SQL 쿼리를 위해 최적화되어 있다
- 데이터 예 : 매출 Fact Table(숫자가 들어간 사실 기반 데이터), 고객 차원 데이터, 재고 데이터
