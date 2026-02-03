# XCom이란?
- Cross-Communication의 약자로, Airflow에서 Task 간 데이터를 주고 받기 위한 기능
- 각 Task가 독립적으로 실행되기 때문에, Task 간 데이터 공유를 위해 XCom을 활용
- DAG Run 내에서만 존재하며, 다른 DAG Run과는 공유되지 않음
- DataFrame과 같은 대용량 데이터는 지원하지 않으며, 주로 문자열, 숫자 등 작은 크기의 데이터를 공유함
- PythonOperator를 사용할 경우, 해당 함수의 return 값이 자동으로 XCom에 등록됨

# Xcom을 이용한 데이터 전달 원리
- XCom 데이터 저장(xcom_push)
    - Task 실행 중 데이터를 저장할 때 사용
    - task_instance.xcom_push(key,value)를 사용하여 특정 키로 값 저장
    - Key-Value 형식으로 저장됨
    - 해당 DAG의 실행 내에서만 사용 가능
- XCom 데이터 조회(xcom_pull)
    - Task 실행 시 이전 Task에서 저장한 데이터를 가져올 때 사용
    - task_instance.xcom_pull(task_ids, key)를 사용하여 특정 Task의 데이터를 가져옴
    - Task 실행 시, 특정 task_id에서 저장한 데이터를 가져옴

# Xcom 사용 방법
- PythonOperator Return 값을 이용한 Xcom
- Push-pull을 이용한 Xcom
- Jinja template을 이용한 Xcom
- @task 데코레이터 사용 시 반환값으로 자동 XCom 저장

# 전역 공유 변수(Variable)란?
- Airflow에서 여러 DAG 및 Task 간에 데이터를 공유하기 위한 변수
- 모든 DAG가 공유할 수 있음
    - 협업 환경에서 표준화된 DAG를 만들기 위해 사용되며, 상수로 지정해서 사용할 변수를 세팅
- Variable에 등록한 key, value는 메타 데이터베이스에 저장
- 변수 값은 Airflow UI, CLI, API를 통해 관리 가능

# 전역 공유 변수(Variable) 사용하기
- Variable 라이브러리의 get 함수를 사용하여 값 사용
- var.value에 꺼내고 싶은 key 값을 입력

# 전역 공유 변수(Variable) vs XCom
- XCom은 Task 간 데이터 전달용, Variable은 DAG 실행 간 설정값 저장용으로 사용됨
- DAG 실행 단위로 데이터를 유지하고 싶으면 XCom, 여러 DAG에서 공통으로 사용할 데이터는 Variable 사용

| 비교 항목 | XCom | Variable |
| :--- | :--- | :--- |
| **데이터 유지 기간** | DAG 실행 단위로 유지됨 | Airflow 전체에서 지속적으로 유지됨 |
| **데이터 저장 위치** | Airflow 메타데이터 DB | Airflow 메타데이터 DB |
| **사용 목적** | Task 간 데이터 전달 | DAG 실행 간 설정값 저장 |
| **데이터 호출 방식** | `xcom_push()`, `xcom_pull()` 사용 | `Variable.get()`, `Variable.set()` 사용 |
| **저장 가능한 데이터 유형** | JSON 직렬화 가능한 작은 데이터<br>(문자열, 숫자, 리스트, 딕셔너리) | 문자열 및 JSON 직렬화 가능한 데이터 |
| **범위** | DAG 실행 내에서만 사용 가능 | 모든 DAG에서 전역적으로 사용 가능 |
| **자동 저장 여부** | `PythonOperator`의 return 값이 자동 저장됨 | 자동 저장되지 않음, 명시적으로 설정 필요 |
| **보안 고려 사항** | Task 간 민감한 데이터 전달 시 사용하지 않음 | API 키, 비밀번호 등은 Connection을 활용하는 것이 더 안전함 |