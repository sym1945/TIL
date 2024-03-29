## 1. 아파치 카프카 개요 및 설명

- 고가용성 Message Queue
- Source Application과 Target Application의 커플링을 줄여줌
- Producer: 메시지 생산자. Topic이라는 Queue에 메시지 전달.
- Consumer: 메시지 소비자. Topic에서 메시지 꺼내 씀.


## 2. 토픽이란?

- 데이터가 들어갈 수 있는 공간
- 토픽은 여러개 생성 가능
- 하나의 토픽은 여러개의 파티션으로 구성 가능
- 실제 데이터는 파티션에 누적됨
- 파티션에 데이터 넣을 때
  - 키가 NULL 이면 Round robin으로 할당
  - 키가 있으면 Hash 값을 구하고 특정 파티션에 할당
- 파티션 늘리는 이유
  - Consumer 개수를 늘려 데이터 처리 분산 가능
- 파티션 늘릴 시 주의 사항
  - 늘릴 수는 있으나 줄일 수는 없음
- 파티션 레코드 언제 삭제?
  - log.retension.ms: 최대 레코드 보관 시간
  - log.retension.byte: 최대 레코드 보존 크기


## 3.브로커, 복제, ISR(In-Sync-Replication)

- 카프카 브로커: 카프카가 설치되어 있는 서버 단위
- 보통 3개 이상의 브로커로 구성하여 사용하는 것을 권장
- replication: 파티션의 복제를 뜻함
  - partition: 1, replication: 1 -> 원본 파티션 1개만 존재
  - partition: 1, replication: 2 -> 원본 파티션 1개, 복제본 파티션 1개 존재
  - partition: 1, replication: 3 -> 원본 파티션 1개, 복제본 파티션 2개 존재
- 브로커 개수에 따라 replication 개수 제한됨 (브로커 3대 : replication 최대 3개)
- 원본 파티션: Leader 파티션
- 복제본 파티션: Follower 파티션
- Leader, Follower 파티션 합쳐서 ISR 이라 부름
- replication을 사용하는 이유
  - 파티션의 고가용성을 위해 (Leader 브로커가 죽으면 Follower 브로커가 Leader 로 변경)
- Producer는 Leader 파티션에 데이터 전송
- Producer의 Ack 옵션
  - 0: Leader 파티션에 데이터 전송. 응답값 받지 않음. (제대로 데이터 전송됐는지 보장할 수 없음. 속도는 빠르지만 데이터 유실 가능)
  - 1: Leader 파티션에 데이터 전송. 응답값 받음. (Follower 파티션에 복사됐는지는 알 수 없음. 복제 전에 Leader 죽으면 데이터 유실 가능)
  - All: Leader 파티션에 데이터 전송 완료, Follower 파티션에 복제 완료까지 확인. (데이터 유실 없지만 매우 느림)


## 4.파티셔너(Partitioner) 란?

- Producer는 파티셔너를 통해 파티션으로 데이터를 보냄
- 파티셔너가 어떤 파티션에 데이터를 넣을지 결정함
- Producer의 기본 파티셔너 설정은 `UniformStickyPartitioner`
  - 메시지 키(hash)를 기준으로 파티션 결정
  - 메시지 키가 없는 경우 Round Robin 으로 파티션에 들어감
    - Producer 에서 배치로 모을 수 있는 최대한의 레코드를 모아서 파티션으로 보냄
- 파티셔너 인터페이스를 통해 커스텀 파티셔너 작성 가능


## 5.컨슈머 랙(Consumer Lag) 이란?

- Producer의 데이터 생산 속도가 Consumer의 데이터 소비 속도보다 빠른 경우 발생하는 Offset 차이 (Producer Offset ~ Consumer Offset)
- 파티션 마다 컨슈머 랙이 존재할 수 있음
- records-lag-max : 컨슈머 랙 중 가장 큰 값


## 6.컨슈머 랙 모니터링 애플리케이션, 카프카 버로우(Burrow)

- 컨슈머 랙을 모니터링 할 수 있는 독립된 어플리케이션
- 카프카 버로우 특징
  - 멀티 카프카 클러스터 지원
  - Sliding window를 통한 Consumer의 status 확인 ('ERROR', 'WARNING', 'OK')
  - HTTP api 제공


## 7.카프카, 레빗엠큐, 레디스 큐의 차이점

- 메시징 플랫폼
  - 메시지 브로커
    - 처리된 메시지는 즉시(짧은 시간 내에) 삭제
    - 이벤트 브로커 역할 수행 불가
  - 이벤트 브로커: 
    - 메시지(이벤트)는 하나만 보관하고 인덱스로 관리
    - 업무상 필요한 시간동안 메시지(이벤트) 보존
    - 메시지 브로커 역할 수행 가능
- 이벤트 브로커의 이점
  - 딱 한번 일어난 이벤트를 브로커에 저장함으로써 단일 진실 공급원으로 사용 가능
  - 장애가 일어난 지점부터 재처리 가능
  - 많은 양의 스트림 실시간 데이터를 효과적으로 처리 가능
- 메시지 브로커 서비스
  - 레빗엠큐
  - 레디스
- 이벤트 브로커 서비스
  - 카프카
  - 키네시스


## 8.카프카 Producer

- Topic에 해당하는 메시지를 생성
- 특정 Topic으로 데이터를 publish
- 처리 실패/재시도
- Key를 사용하여 파티션에 데이터 넣는 중 토픽에 새로운 파티션이 추가되면 Key와 파티션의 매칭이 깨짐 (키 ↔ 파티션 일관성 보장되지 않음)


## 9.카프카 Consumer

- Topic의 Partition으로 부터 데이터 polling
- Partition offset 위치 기록(commit)
- Cousumer group을 통해 병렬처리
