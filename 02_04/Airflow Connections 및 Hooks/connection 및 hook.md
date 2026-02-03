# Airflow Connection & Hook
- Connection
    - Airflow가 외부 시스템(DB, API, 클라우드 서비스)과 연결할 수 있도록 설정
    - Web UI 또는 환경 변수를 통해 관리 가능
- Hook
    - Connection을 사용하여 실제 데이터를 전송하거나, 외부 시스템과 상호작용하는 역할
    - Operator에서 Hook를 활용하여 작업 수행

| 연결 대상 | Connection Type | 사용 Hook |
| :--- | :--- | :--- |
| MySQL | MySQL | MySqlHook |
| PostgreSQL | Postgres | PostgresHook |
| REST API | HTTP | HttpHook |
| AWS S3 | AWS | S3Hook |