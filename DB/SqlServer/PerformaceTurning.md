# SQL Performance Turning이란?
## Performance turning?
시스템 성능을 향상시키는 것.
일반적으로 컴퓨터 시스템에서 이러한 활동에 대한 동기를 성능 문제라고 하며, 실제로 발생하거나 예상할 수 있다.
대부분의 시스템은 증가된 부하에 어느 정도 반응하여 성능이 저하된다.

## Database turning?
데이터베이스 응용프로그램을 보다 빨리 실행할 수 있도록 하는 활동.
"보다 빨리"는 일반적으로 처리량이 더 높음을 의미하지만, 시간이 중요한 애플리케이션의 경우 응답 시간이 더 짧다는 걸 의미함.

## SQL Turning?
응용 프로그램이 발급할 SQL 문을 최대한 빨리 실행할 수 있도록 보장하는 프로세스.

## 요청 Data 응답이 느린 이유?
- 잘못된 애플리케이션 구성
  * 잘못된 설정, 응용 프로그램 업데이트 안 됨
- 부족한 물리적 하드웨어 리소스
  * 메모리, 디스크, CPU, 네트워크
- 쿼리 옵티마이저가 최상의 계획을 선택하지 않음
  * 옵티마이저를 강제하기 위한 힌트가 필요할 수 있음
- 잘못 작성된 쿼리
  * 커서를 사용하여 쿼리를 더 효율적으로 쓸 수 있음
- 적절한 인덱싱이 없음
  * 테이블의 잘못된 인덱싱, 사용되지 않는 인덱싱, 너무 많은 인덱싱
  
## 성능 조정 및 모니터링에 사용할 수 있는 툴과 어플리케이션?
- Performance monitor
- Activity monitor
- SQL Profiler
- Extended Events
- DMV (Database management views)
- DBCCs (Database console commands)
- Stored procedures (sp_who, sp_monitor)
- SQL Standard Report
- Database tuning advisor
- Execution plans
- Data collection
- Query store
- Other monitoring tools

# Auto-growth
## Auto-growth란?
- 저장 공간이 부족할 때 SQL 서버 엔진이 데이터베이스 파일의 크기를 확장하는 프로세스. 
- 각 데이터베이스에는 모델 데이터베이스의 기본 설정으로 설정된 자체 자동 증가 설정이 있음.

## 각 데이터베이스마다 Auto-growth를 설정하는 이유?
- 자동 증가 이벤트가 수행될 때마다 SQL Server가 데이터베이스 처리를 지연시킴. 이로 인해 성능이 저하될 수 있음.

## Auto-growth 목적과 설정하지 않았을 경우 발생하는 문제
- 자동 증가 이벤트가 발생할 때마다 서버는 이 공간을 확장하고 디스크에 공간을 배치해야 함. 그러나 데이터베이스에 할당된 공간이 디스크의 서로 인접하지 않을 수 있기 때문에 데이터베이스 파일을 조각화하여 성능이 저하됨.
- 새 데이터베이스를 정의할 때 자동 증가를 위한 초기 설정이 기본값으로 설정됨. 이러한 기본값은 모델 데이터베이스 파일의 자동 증가 설정을 사용하여 설정됨.

## 자동 증가 이벤트가 너무 많이 발생하지 않게 하려면?
- 데이터베이스 크기를 미리 조정하고 자동 증가 크기를 적절하게 조정한다.
