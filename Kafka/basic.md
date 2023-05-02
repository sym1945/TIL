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
  - log.retension.byte: 최대 레코드 보존 크


## 3.브로커, 복제, ISR(In-Sync-Replication)

- 카프카 브로커: 카프카가 설치되어 있는 서버 단위
- 보통 3개 이상의 브로커로 구성하여 사용하는 것을 권장
- replication: 파티션의 복제를 뜻함
  - partition: 1, replication: 1 -> 원본 파티션 1개만 존재
  - partition: 1, replication: 2 -> 원본 파티션 1개, 복제본 파티션 1개 존재
  - partition: 1, replication: 3 -> 원본 파티션 1개, 복제본 파티션 2개 존재
- 브로커 개수에 따라 replication 개수 제한됨 (브로커 3대 : replication 최대 3개)
- 원본 파티션: Leader 파티션
- 복제본 파티션: Follower 파티
- Leader, Follower 파티션 합쳐서 ISR 이라 부름
- replication을 사용하는 이유
  - 파티션의 고가용성을 위해 (Leader 브로커가 죽으면 Follower 브로커가 Leader 로 변경)
- Producer는 Leader 파티션에 데이터 전송
- Producer의 Ack 옵션
  - 0: Leader 파티션에 데이터 전송. 응답값 받지 않음. (제대로 데이터 전송됐는지 보장할 수 없음. 속도는 빠르지만 데이터 유실 가능)
  - 1: Leader 파티션에 데이터 전송. 응답값 받음. (Follower 파티션에 복사됐는지는 알 수 없음. 복제 전에 Leader 죽으면 데이터 유실 가능)
  - All: Leader 파티션에 데이터 전송 완료, Follower 파티션에 복제 완료까지 확인. (데이터 유실 없지만 매우 느림)
