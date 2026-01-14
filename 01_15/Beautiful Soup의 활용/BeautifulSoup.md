# BeautifulSoup
- BeautifulSoup란
    - HTML 및 XML 문서를 구문 분석하기 위한 Python 패키지로, 데이터를 쉽게 추출할 수 있도록 도와줌
    - 웹 브라우저가 하는 일과 비슷하게, HTML 소스를 트리 형태로 해석한 뒤 접근 할 수 있음
- Task, 학식 정보 가져오기
    - 구글에 '서울대학교 식단'을 검색하면, 생활협동조합 식단 페이지에 접속
    - 이름 get으로 요청한 뒤, 응답 텍스트를 BeautifulSoup의 인자로 넣고 실행

# BeautifulSoup 활용
- 겉보기엔 별 변화가 없지만, 현재 bs 객체에는 이미 HTML의 정보가 BeautifulSoup를 통해 해석되어 있다.
- 학생회관식당의 점심 메뉴가 HTML 코드 상에서 어디에 위치하는지 알기 위해, 요소 검사를 실행
- DOM을 잘 확인하는 것이 가장 중요
- 개발자도구로 코드를 보면, 메뉴 전체는 `<table class="menu-table">`
- 요일/식당 별 메뉴는 `<tbody>` 안의 여러 `<tr>`로 구성되어 있음
- 각 `<tr>`이 식당 1곳을 의미