# SparkSubmitOperator 개요 및 역할
- Apache Airflow에서 Spark 애플리케이션을 실행하기 위한 전용 연산자
- Spark 클러스터(YARN, Kubernetes 등)에 .py , .jar, .scala 파일 등을 제출(submit)
- 복잡한 Spark 작업을 Airflow DAG 내에 통합하여 자동화된 데이터 파이프라인 구성 가능
- DAG 태스크로서 Spark 작업 실행을 스케줄링 및 추적 간으함
- SparkSubmit 명령어를 Python 코드로 대체하여 운영 효율성 확보

# SparkSubmitOperator 주요 파라미터
- application : 실행할 Spark 애플리케이션 경로(.py, .jar 등)
- conf : Spark 설정("spark.executor.memory":"2g")
- executor_memory,dirver_memory: 리소스 지정
- application_args : 애플리케이션에 전달할 인자 목록
- conn_id : Spark 클러스터 연결 정보(spark_default)
- name:  spark 작업 이름

