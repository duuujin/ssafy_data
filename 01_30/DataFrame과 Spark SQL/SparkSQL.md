# SparkSQL 이란 
- Spark SQL은 구조화된 데이터를 SQL처럼 처리할 수 있도록 해주는 스파크 모듈
- 내부적으로는 DataFrame/Dataset API와 동일한 엔진(Catalyst)을 사용하여 처리
- DataFrame과 Dataset을 SQL처럼 다룰 수 있게 해주는 분산 SQL 쿼리 엔진
- Spark SQL은 RDD보다 더 높은 수준의 추상화와 자동 최적화를 제공
-> DataFrame이 중심이고, Spark SQL은 그것을 SQL 방식으로 접근하게 해주는 방법 중 하나

# SparkSQL 역할
- SQL 같은 질의 수행
- 스파크 컴포넌트들을 통합하고, DataFrame, Dataset가 java, scala, python, R 등 여러 프로그래밍 언어로 정형화 데이터 관련 작업을 단순화 할 수 있도록 추상화
- 정형화 된 파일 포맷(JSON, CSV, txt, avro, parquet, orc 등)에서 스키마와 정형화 데이터를 읽고 쓰며, 데이터를 임시 테이블로 변환
- 빠른 데이터 탐색을 할 수 있도록 대화형 스파크 SQL 셸을 제공
- 표준 데이터베이스 JDBC/ODBC 커넥터를 통해, 외부의 도구들과 연결할 수 있는 중간 역할 제공
- 최종 실행을 위해 최적화 된 질의 계획과 JVM을 위한 최적화 된 코드를 생성

# Spark SQL의 내부 동작
- SQL 쿼리를 실행하는 역할 + 사용자가 입력한 쿼리나 DataFrame의 명령을 가장 빠르고 효율적인 방식으로 처리
- Spark SQL이 내부에서 데이터를 효율적으로 처리하는 핵심적인 엔진이 Catalyst Optimizer

### Spark SQL의 내부는 어떻게 작동 ?
- 최적화 과정
    1. SQL Parser & DataFrame API 해석 관계
    2. Logical Plan(논리적 계획) 생성
    3. Optimized Logical Plan (최적화된 논리 계획) 생성
    4. Physical Plan (물리적 실행 계획) 생성
    -> Catalyst Optimizer가 내부적으로 복잡환 최적화 과정을 자동으로 처리

# Spark SQL과 DataFrame API의 관계
- Spark SQL과 DataFrame API는 서로 완전히 독립된 별개의 것이 아님
- 동일한 최적화 엔진(Catalyst Optimizer)을 공유하고, 내부적으로 통합된 구조를 가짐
-> Spark에서는 DataFrame API를 이용해 작성된 데이터 처리 명령을 내부적으로 Spark SQL의 엔진으로 최적화해 실행

# Dataset API
- 스파크 2.0에서, 개발자들이 한 종류의 API만 알면 되도록, Dataframe, Dataset API를 하나로 합침
- Dataset은 정적 타입(typed) API와 동적 타입(untyped) API의 두 특성을 모두 가짐
- Java, Scala(타입 안전을 보장하는 언어)에서만 사용이 가능하고, Python, R(타입 안전을 보장하지 않는 언어)에서는 사용이 불가능, DataFrame API만 사용 가능

