# Forward Index vs Inverted index
- Forward Index(정방향 인덱스)
    - 문서 중심으로 인덱스를 구축
    - 각 문서가 포함되는 단어 목록을 저장
    - 인덱스 구축이 단순하지만, 검색 속도가 느림(모든 문서를 순회)
    - 특정 문서의 내용을 확인할 때 유용

- Inverted Index(역방향 인덱스)
    - 단어 중심으로 인덱스를 구축
    - 각 단어가 포함된 문서 목록을 저장
    - Elasticsearch에서 기본적으로 사용하는 방식
    - 검색 시 특정 단어를 빠르게 찾을 수 있음


## Inverted Index(역방향 인덱스)에서 forward Index 활용
    - fileddata
        - 특정 텍스트 필드에 대한 정렬, 집계(aggregation)시 사용
        - 메모리 사용량이 증가하는 단점
    - doc_values
        - 메모리 사용량을 줄이기 위해 디스크 기반 컬럼 저장 방식 사용
        - 기본적으로 비 텍스트 필드에서 활성화 됨
        - keyword 필드 활용 가능

# 인덱스 엘리아스
- 인덱스 명을 대신하는 가상의 이름을 부여 할 수 있음
- 여러 개의 인덱스를 하나의 인덱스처럼 연결하여 사용 가능
- 신규 인덱스에 데이터를 색인하고, 엘리아스를 이용해 다운타임 없이 인덱스를 교체 가능
- _alias API를 사용하여 설정
- aliases로 선언한 products_alias를 통해 조회해도 products 인덱스의 데이터를 다룰 수 있음
- products_alias로 검색할 때 products와 products_v2 인덱스의 데이터를 모두 가져오게 됨

