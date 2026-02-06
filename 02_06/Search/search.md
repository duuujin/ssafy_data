# URI 검색
- 간단한 검색을 수행할 때 URI 검색(URI Query String Search)을 사용
- "key=value" 형식으로 전달
- URL에 검색할 컬럼과 검색어를 지정 가능하며 검색 조건을 추가할 수 있음
- Request Body 검색 대비 단순하고 사용이 편리하지만 복잡한 쿼리를 수행할 수 없다는 한계

# Query DSL
- Elasticsearch에서 검색을 수행하기 위한 JSON 기반의 질의 언어
- JSON 형식 사용
    - HTTP 요청 시 본문의 JSON 문서를 활용하여 Elasticsearch에 검색 요청
- Query Context
    - 검색어와 문서 간의 유사도 점수(__score)를 기반으로 검색 스코어링을 통해 문서의 중요도를 평가
- Filter Context
    - 문서가 검색 조건에 해당하는지 여부만 판단 _score 값을 계산하지 않음
    - 캐싱이 가능하여 성능 최적화 가능

# Query DSL 쿼리 형식
- size : 반환할 문서 개수 (기본값 : 10)
- from : 검색 결과에서 몇 번째 문서부터 표시할지 설정(기본값 : 0)
- timeout : 검색 수행 시간 제한(기본값 없음, 필요 시 "30s" 등 설정 가능)
- _source : 검색 결과에서 포함할 필드 지정(기본값 : 전체 포함)
- query : 검색 조건이 들어가는 공간
- aggs : 통계 및 집계 데이터 설정 공간
- sort : 문서 정렬 기준 설정


# 검색 결과 정렬
- 기본 정렬(Default Sorting)
    - _score : 기본적으로 검색 쿼리가 실행되면 _score 값(유사도 점수)에 따라 검색 결과가 정렬
    - _score 값이 높은 문서일수록 쿼리와 더 높은 유사도를 가지므로 상위에 노출됨
    - _score는 IF-IDF 또는 BM25 등의 알고리즘을 기반으로 계산(변경 가능)

- 특정 필드를 기준으로 정렬
    - price(가격) 기준 정렬 : 최신 제품 또는 가격이 높은 순으로 정렬 가능
    - name(상품명) 기준 정렬 : 이름순 정렬 가능
    - created_at(등록일) 기준 정렬 : 최신순 또는 오래된순 정렬 가능

# 검색 결과 페이징
- 결과 보여주기
    - from : 페이지를 가져올 때의 시작점
    - size : 검색 결과를 가져올 양

# Filter Context
- Term level query
    - 텍스트 분석기를 사용하지 않고 정확한 값을 검색할 때 사용
    - 필터 되는 속도가 빠름
    - SQL에서 =와 같은 역할

- Terms level query
    - 특정 필드에 대해 SQL의 IN 조건처럼 여러 값을 검색
    - 다중 값을 조회 할 수 있음
    - 텍스트와 다른 타입의 정보를 동시에 이용할 수 있음

# Range Query
- 숫자, 가격, 크기, 날짜 등을 범위로 필터링 할 때 사용
- SQL의 BETWEEN 또는 >=, <=, >,< 조건과 유사한 기능


# Query Context
- Match query
    - Elasticsearch에서 가장 기본적인 전체 텍스트 검색(full-text search)방식
    - 분석기(Analyzer)를 적용하여 단어를 토큰화 및 변환한 후 검색
    - SQL로 표현하면 LIKE '%검색어%' 와 비슷한 기능이지만 훨씬 효율적

- Match query with operator
    - operator는 검색어가 여러 개일 때 AND 또는 OR 조건을 적용할지 결정
    - Smasung과 Ultra 둘 다 포함된 문서만 반환

- Match Phrase Query
    - 단어 순서를 유지한 채 검색을 수행
    - Samsung Neo QLED 8K 75-inch는 검색됨, Samsung Odyssey Neo G9은 제외

- Match Phrase Prefix Query
    - 단어 순서를 유지한 채 검색을 수행하되 자동완성 형태로 검색 가능
    - Samsung Neo 가 기억이 안나는 경우 N까지만 입력해도 활용하기 위한 방식

- Multi match Query
    - 여러 개의 필드를 동시에 검색할 때 사용하는 쿼리
    - match 쿼리와 다르게 여러 필드에서 일치하는 문서를 찾을 수 있음
    - type을 설정하여 검색 방식 조정 가능(best_fields, most_fields, cross_fields)

- Multi Match Query (best_fields)
    - 여러 필드에서 가장 높은 점수를 가진 필드만 반영
    - Samsung이 name 또는 description 중 하나라도 포함된 문설르 반환
    - score가 가장 높은 필드를 기준으로 결정

- Multi Match Query (most_fields)
    - 여러 필드에서 모든 일치 항목의 점수를 합산
    - samsung이 name과 description 양쪽에서 검색되면 점수가 더 높아짐
    - samsung이 여러 필드에서 발견될수록 검색 결과의 점수가 증가

- Multi Match Query (cross_fields)
    - 여러 필드의 텍스트를 조합한 검색
    - Samsung과 Ultra가 서로 다른 필드에 있어도 검색 가능

# Exist Query
- 필드가 존재하는 문서만 존재
- Elasticsearch는 필드가 존재하는 문서와 존재하지 않는 문서 공존 가능

# Boolean Query
- 여러 개의 조건을 조합하여 검색을 수행하는 쿼리 방식
- must, must_not, should, filter ㅔㄴ 가지 방식으로 조합 가능
- SQL의 AND, OR, NOT 연산과 유사함

| Boolean Query 유형 | 설명 | 예제 |
| :--- | :--- | :--- |
| must | 모든 조건을 만족하는 문서 검색 | brand = Samsung AND description = AI |
| must_not | 특정 조건을 포함하지 않는 문서 검색 | brand != Google |
| should | 하나라도 조건을 만족하면 검색 | brand = Samsung OR brand = Apple |
| filter | 빠른 검색(점수 미계산) | brand = Samsung AND description = AI |