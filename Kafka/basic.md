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
