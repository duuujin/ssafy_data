# DAG 내 Task 간 의존성(Dependency)이란?
- Airflow에서는 DAG 내에서 Task 간 실행 순서(의존성)를 정의해야 함
- Task 간 의존성을 설정하면 특정 Task가 완료된 후 다음 Task가 실행됨
- 연산자(>> 또는 <<)를 활용하여 Task 간 의존성을 설정

# Task 연결 원리
- DAG 내에서 Task 의존성(Dependency)을 설정하여 실행 순서를 설정함
- Task 간의 실행 관계를 명확하게 지정해야 DAG 올바르게 동작함
- Task 연결 방식은 순차 실행(Sequential Execution), 병렬 실행(Parallel Execution)으로 나뉨

| 종류 | 설명 |
| :--- | :--- |
| **Upstream Task** | 현재 Task 이전에 실행되는 Task |
| **Downstream Task** | 현재 Task 이후에 실행되는 Task |
| **Linear Dependency** | 순차적으로 Task를 실행 (Task A → Task B → Task C) |
| **Branching** | 특정 조건에 따라 Task 실행 흐름을 분기 |
| **Parallel Execution** | 여러 Task를 병렬로 실행 |

# Trigger Rule(트리거 규칙)
- Task가 실행되기 위한 조건을 설정하는 기능
- 기본적으로 모든 Upstream Task가 성공해야 실행됨(all_success)
- 특정 Task의 실행 결과에 따라 실행 조건을 다르게 설정할 수 있음

| 종류 | 설명 |
| :--- | :--- |
| **Upstream Task** | 현재 Task 이전에 실행되는 Task |
| **Downstream Task** | 현재 Task 이후에 실행되는 Task |
| **Linear Dependency** | 순차적으로 Task를 실행 (Task A → Task B → Task C) |
| **Branching** | 특정 조건에 따라 Task 실행 흐름을 분기 |
| **Parallel Execution** | 여러 Task를 병렬로 실행 |