# Analyzer(분석기)
- 문서를 색인하고 검색할 때 텍스트를 처리하는 방식
- 문서의 내용을 토큰으로 변환하여 색인 및 검색
    - Analyzer를 거친 단어들만 검색 가능
- 어떤 Analyzer 사용하고 실행 순서를 정하는 것이 중요
    - Analyzer의 설정 방식에 따라 검색 결과가 달라질 수 있음
- 너무 많은 분석을 하면 색인 성능 저하
    - Analyzer가 복잡할수록 색인 속도가 느려질 수 있음

# 색인과 검색에서의 Analyzer의 차이
- 색인 할 때 분석기와 검색할 때 분석기가 다를 수도 있음
- 일반적으로 색인과 검색 시 같은 tokenizer를 사용하는 것이 좋음
- 색인과 검색에 다른 분석기를 적용하는 경우
    - 검색어 필터링이 필요한 경우
        - 검색어에서 의미 없는 단어(불용어)를 제거해야 하는 경우
        - 특정 단어를 제외하고 검색해야 하는 경우
    - 동의어나 맞춤법 교정을 적용하는 경우
        - "car"를 검색하면 "automobile"도 검색되도록 설정
        - "color"와 "colour"를 동일하게 처리
    
# Analyzer 구성요소
- 분석기(Analyzer)는 검색 성능을 향상시키기 위해 문서를 토큰화(tokenization)하고, 텍스트를 변환하는 기능을 수행
- Character Filter -> Tokenizer -> Token Filter 순서로 진행

# 색인과 검색에서의 Analyzer
- Character Filters (문자 필터)
    - 원본 텍스트를 전처리하는 단계
    - 특정 문서나 패턴을 변환하거나 제거함
    - html_strip -> HTML 태그 제거
    - mapping -> 특정 문자열을 다른 문자열로 매핑
    - pattern_replace -> 정규식을 이용한 텍스트 변경

- Tokenizer(토크나이저)
    - Charater Filter를 거친 텍스트를 특정 규칙에 따라 토큰(단어 단위)으로 분리
    - 분석기 구성 시 한 개의 Tokenizer만 사용 가능
    - whitespace -> 공백 기준으로 단어 분리
    - standard -> 일반적인 텍스트 토큰화
    - ngram -> 부분 문자열(서브스트링) 단위로 분리

- Token Filters(토큰 필터)
    - Tokenizer를 통해 분리된 토큰을 추가, 수정, 삭제하는 필터
    - 여러 개를 배열로 사용 가능
    - lowercase -> 모든 단어를 소문자로 변환
    - stop -> 불용어(예:the , is)제거
    - synonym -> 동의어 처리 (예:car <-> autommobile )

# Analyzer 조합(Custom Analyzer)
- cusotm analyzer를 생성할 때 여러 가지 요소를 조합 가능
- tokenizer는 하나만 지정할 수 있으며, char_filter와 filter는 여러 개 적용 가능

# 한국어 Analyzer (Nori Analyzer)
- 한국어 처리를 위해 nori 분석기를 제공
- nori 분석기 특징
    - 형태소 기반 분석기로, 한구어의 복잡한 문장 구조를 효과적으로 분석
    - Elasticsearch 기본 패키지가 아니므로 설치 필요
- nori 분석기 구성요소
    - Tokenizer(토크나이저)
    - nori_tokenizer -> 형태소 분석을 수행하여 단어를 분리
- Token Filters(토큰 필터)
    - nori_part_of_speech -> 품사 기반 필터링(예:명사만 남기기)
    - nori_readingform -> 한자/외래어 등을 한글 발음으로 변환
    - nori_number -> 숫자를 표준화(예: "일" -> "1")

# 한국어 Tokenizer(Nori Tokenizer)
- 한국어 형태소 분석기
- 형태소 분석을 지원하지 않는 기본 분석기 사용시 복합명사를 적절히 분해할 수 없음
- 사용자 정의 사전(user_dictionary)지원
- 복합명사 처리 방식(decompound_mode) 선택 가능
- 기본적으로 구두점(discard_punctuation) 제거

# 동의어 처리
- 색인 시 동의어 처리와 검색 시 동의어 처리 두 가지 방식 가능
- 색인 시 동의어 처리
    - 색인 시점에서 동의어를 확장하여 저장하는 방식
- 검색 시 동의어 처리
    - 검색 시점에서 동의어를 확장하여 질의하는 방식


## 색인 시 동의어 처리
- 색인(indexing) 시점에서 동의어를 확장하여 저장하는 방식
    - 문서가 색인될 때 미리 동의어를 확장하여 저장하므로, 검색 시 추가적인 처리가 필요하지 않음
    - 검색 시점에서 별도의 동의어 매핑 작업이 필요하지 않음
    - 색인된 데이터의 크기가 증가할 수 있음
    - 새로운 동의어를 추가하려면 전체 데이터 재색인이 필요

## 검색 시 동의어 처리
- 검색(query) 시점에서 동의어를 확장하여 질의하는 방식
    - 색인된 문서를 변경할 필요가 없으며, 동의어를 추가하면 즉시 반영
    - 검색 시 추가적인 처리 비용이 발생하여 성능이 저하될 수 있음
    - 동의어를 잘못 등록하면 쿼리가 예상치 못한 단어를 포함해 검색 품질이 저하될 수 있음

# 동의어 사전 구성
- 단어 동등 관계(A,B)
    - 색인 할 때 A와 B를 동일한 의미로 저장
    - 검색 시 A를 입력해도, B를 입력해도 같은 문서가 검색
- 단어 치환 단계(A -> B)
    - 색인할 때 A를 B로 변환하여 저장
    - 검색할 때 A를 검색해도 검색되지 않음
    - 검색 시 A -> B로 변환 후 검색하면, 기존 A로 저장된 문서는 검색되지 않음


# 동의어 사전 불러오기
- 동의어 사전 저장 위치
    - Elasticsearch 노드의 config 폴더 하위에 동의어 사전을 생성
    - 일반적으로 config/dictionary 디렉토리에 synonyms.txt 같은 파일로 저장
- 동의어 사전 리로드 API
    - 색인이 아닌 검색 시 동의어를 적용할 경우, 사전 업데이트가 즉시 반영되지 않음
    - 새로운 동의어를 반영하려면 리로드 API를 실행해야 함
    - 리로드 API 실행 후 캐시를 비워야 변경 내용이 반영

# 불용어(Stopword)처리
- 불용어 사전
    - Elasticsearch 노드의 config/dictionary/ 폴더에 파일을 생성
    - 불용어로 지정된 단어들은 색인 및 검색에서 제외
    - 검색타임에도 사전을 리로드 가능
    - 내용을 한 줄에 입력해 txt 파일로 저장

# 불용어(Stopword) 필터 적용
- 분석기(search_stop_analyzer)
    - search_stop_filter 적용 -> stopwords.txt에 있는 단어 제거
- 불용어 필터(search_stop_filter)
    - Stopwrods_path 옵션을 사용해 불용어를 파일에서 로드
    - 색인/검색 시 특정 단어가 제거

# 사용자 사전 처리
- 단어형태를 강제로 지정해 복합명사 단일명사를 원하는 형태로 검색하기 위한 방식
    - decompound_mode : mixed : 복합어 처리를 절절하게 수행
    - discard_punctuation : false : 구두점 유지
    - user_dictionary : dictionary/userdic_ko.txt : 사용자 사전 적용
    - 향후 색인 및 검색 시 적용 가능

