# RDD API의 문제점
- 스파크가 RDD API 기반의 연산, 표현식을 검사하지 못해 최적화할 방법이 없음
    - RDD API 기반 코드에서 어떤 일이 일어나는지 스파크는 알 수 없음
    - Join, filter, group by 등 여러 연산을 하더라도 스파크에서는 람다 표현식으로만 보임
    - 특히 PySpark의 경우, 연산 함수 Iterator 데이터 타입을 제대로 인식하지 못함
        - 스파크에서는 단지 파이썬 기본 객체로만 인식

- 스파크는 어떠한 데이터 압축 테크닉도 적용하지 못함
    - 객체 안에서 어떤 타입의 컬럼에 접근한다고 해도, 스파크는 알 수 없음
    - 결국 바이트 형태로 직렬화해 사용할 수 밖에 없음

- -> 스파크가 연산 순서를 재정렬해서 효과적인 질의 계획으로 바꿀 수 없음

# DataFrame
- 스키마(schema)를 가진 분산 데이터 컬렉션
- 데이터를 행(row)과 열(column)로 구성된 표 형태로 관리
- 각 열은 명확한 데이터 타입과 메타 데이터(schema)를 가지고 있음
- Spark SQL이 제공하는 구조화된 데이터 모델로서 RDD의 한계를 보완

# Dataframe API - 개요
- 구조, 포맷 등 몇몇 특정 연산 등에 있어, Pandas의 Dataframe에 영향을 많이 받음
- 이름 있는 컬럼과 스키마를 가진 분산 인메모리 테이블처럼 동작
- Spark Dataframe은 하나의 표의 형태로 보임

# Dataframe API - 데이터 타입
- 기본 타입
    - Byte, Short, Integer, Long, Float, Double, String, Boolean, Decimal
- 정형화 타입
    - Binary, Timestamp, Date, Array, Map, Struct, StructField
- 실제 데이터를 위한 스키마를 정의할 때 어떻게 이런 타입들이 연계되는지를 아는 것이 중요

# Dataframe API - 스키마(Schema)
- 스파크에서의 스키마는 Dataframe을 위해 컬럼 이름과 연관된 데이터 타입을 정의한 것
- 외부 데이터 소스에서 구조화된 데이터를 읽어 들일 때 사용
- 읽을 때 스키마를 가져오는 방식과 달리, 미리 스키마를 정의하는 것은 여러 장점 존재
    - 스파크가 데이터 타입을 추측해야 되는 책임을 덜어줌
    - 스파크가 스키마를 확정하기 위해, 파일의 많은 부분을 읽어 들이려 별도의 Job을 만드는 것을 방지
    - 데이터가 스키마와 맞지 않는 경우, 조기에 문제 발견 가능

### DataFrame은 DSL과 SQL 쿼리 방식 모두를 지원
- DSL : sciDocs = data.filter(col("label") == 1)
- SQL : data_scaled = spark.sql("SELECT * FROM data WHERE label = 1")

# RDD와 DataFrame의 차이점
| 구분 | RDD | DataFrame |
| :--- | :--- | :--- |
| **데이터 표현 방식** | 값만 표현 가능, 스키마 표현 불가능 | 명확한 스키마(컬럼, 데이터 타입)을 가진 구조적 데이터 |
| **최적화 및 성능** | 최적화가 어려움, 직접적 연산 필요 | Catalyst Optimizer를 통한 자동 최적화 및 빠른 처리 가능 |
| **사용 편의성** | 낮음(저수준 API) | 높음(고수준 API, SQL 활용 가능) |

- DataFrame을 사용하면 데이터를 더욱 효율적이고 편리하게 처리 할 수 있으며, 데이터의 메타 정보를 활용하여 더 빠르고 최적화 된 분석을 수행 할 수 있다

# RDD를 사용하는 경우
1. 저수준의 Transformation과 Action을 직접 제어해야 할 때
2. 스트림 데이터(미디어나 텍스트 스트림)가 구조화되지 않은 경우
3. 특정 도메인 표현을 위해 함수형 프로그래밍이 필요할 때
4. 스키마 변환이 필요 없을 때 (예 : 열 기반 저장소를 사용하지 않는 경우)
5. DataFrame이나 Dataset에서 처리할 수 없는 성능 최적화가 필요할 때

# DataFrame를 사용하는 경우
1. 고수준의 추상화와 도메인 기반 API가 필요할 때
2. 고수준의 표현(filter, map, agg, avg, sum SQL, columnar access)등 복잡한 연산이 필요하거나 반구조적인 데이터에 대한 lambda 식이 필요할 때
3. 타입 안정성과 최적화를 위해 컴파일 시 타입 안전성을 보장하고,
4. Catalyst 최적화 및 Tungsten의 효율적인 코드 제너레이션이 필요할 때
5. Spark API의 일관성과 간결함을 원할 때

