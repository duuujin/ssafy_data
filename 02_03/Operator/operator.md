# 오퍼레이터(Operator)란?
- Airflow에서 Task를 실행하는 역할을 수행하는 객체
- DAG 내에서 개별 Task로 사용되며, 다양한 실행 방식을 제공
- Python 기반으로 확장 가능하며, 기본 제공(내장) 오퍼레이터와 사용자 정의 오퍼레이터가 있음

# 오퍼레이터(Operator) 종류
| 오퍼레이터 종류 | 설명 | 예제 |
| :--- | :--- | :--- |
| **Action Operators**<br>(기본 실행 오퍼레이터) | 특정 동작을 수행하는 오퍼레이터 | PythonOperator, BashOperator, EmailOperator |
| **Sensor Operators**<br>(센서 오퍼레이터) | 특정 이벤트를 감지할 때까지 대기 | FileSensor, HttpSensor, S3KeySensor |
| **Transfer Operators**<br>(데이터 전송 오퍼레이터) | 한 위치에서 다른 위치로 데이터를 이동 | S3ToGCSOperator, MySQLToGCSOperator |
| **Database Operators**<br>(데이터베이스 관련 오퍼레이터) | DB에서 SQL을 실행하는 오퍼레이터 | PostgresOperator, MySqlOperator, SnowflakeOperator |
| **Big Data & ML Operators**<br>(빅데이터 & 머신러닝 오퍼레이터) | Spark, Hive, Dataproc, ML 관련 오퍼레이터 | SparkSubmitOperator, DataflowOperator |
| **Docker & Kubernetes Operators** | 컨테이너 환경에서 실행 | DockerOperator, KubernetesPodOperator |
| **Empty Operators** | Task 흐름을 설정하는 데 사용 | DummyOperator (Empty Operator) |

# Action Operators
- Task는 특정 오퍼레이터를 기반으로 정의됨
    - BashOperator -> Bash 명령어 실행
    - PythonOperator -> Python 함수 실행
    - EmailOperator -> 이메일 전송
- 각 오퍼레이터를 조합하여 다양한 워크플로우를 구성할 수 있음

## Empty Operator
    - Task 실행 없이 DAG 구조를 설정하는데 사용 됨
    - DAG의 논리적 흐름을 구성하는 용도로 활용

## Branch Operator
    - Airflowㅇ서 DAG 실행 흐름을 조건에 따라 분기할 수 있도록 하는 오퍼레이터
    - 특정 조건을 평가하여 어떤 Task를 실행할지 동적으로 결정
    - 선택되지 않은 Task는 자동으로 Skipped 상태가 됨
    - DAG 실행을 최적화하고 불필요한 작업을 줄이는데 유용
    - BranchPythonOperator
        - Python 함수를 사용하여 실행 Task 결정
    - BranchDagRunOperator
        - 다른 DAG 실행을 분기 처리하는 오퍼레이터

# Airflow Decorators
- Python 함수 데코레이터로 태스크 정의
    - @task 데코레이터를 사용하여 Python 함수를 간단하게 Airflow 태스크로 변환
    - 코드의 가독성 향상 : 태스크 정의가 간결하고 직관적
    - DAG 정의 간소화 : DAG과 태스크 간의 관계를 쉽게 정의할 수 있음
- 주요 구성
    - @task : 함수 데코레이터, Python 함수가 Airflow 태스크로 변환
    - DAG 정의 : dag 매개변수로 태스크가 속할 DAG를 정의
    - 태스크 간 의존성 : 데코레이터를 사용하여 태스크 간 의존성 간단히 설정 가능

# Airflow Decorators 코드 비교
- 전통적인 방식
    - PythonOperator를 사용하여 각 태스크를 명시적으로 정의
    - XCom을 사용하여 데이터를 전달하고 의존성 설정

- Python 함수 데코레이터로 태스크 정의
    - @task : @task 데코레이터를 사용하여 Python 함수가 자동으로 Airflow 태스크로 변환
    - 데이터 전달 : 함수 간 데이터 전달이 자동으로 처리되므로 XCom을 수동으로 사용하지 않음
    - 의존성 설정 : 함수 간의 의존성은 변수를 통해 자연스럽게 설정