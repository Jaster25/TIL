# 강의 - DB 트랜잭션 조금 이해하기

[유튜브 - 프로그래밍 초식 : DB 트랜잭션 조금 이해하기 1~2 by 최범균](https://www.youtube.com/watch?v=urpF7jwVNWs&list=PLwouWTPuIjUg0dmHoxgqNXyx3Acy7BNCz&index=5) 강의 정리

## 프로그래밍 초식 : DB 트랜잭션 조금 이해하기 01

### 트랜잭션

여러 읽기/쓰기를 논리적으로 하나로 묶음

- 트랜잭션 시작 - 여러 쿼리 실행 - 커밋 또는 롤백
- 모두 반영(커밋) 또는 모두 반영하지 않거나(롤백)

<br>

### 트랜잭션 범위는 커넥션 기준

<br>

### 트랜잭션 전파

- 여러 메서드 호출이 한 트랜잭션에 묶이도록 하기 위해 필요
  - ex) 스프링 프레임워크의 트랜잭션 처리 -> `@Transactional`
    - 메서드 간에 커넥션 객체를 전달하지 않아도 한 트랜잭션으로 묶어서 실행

<br>

### 트랜잭션과 외부 연동

외부 연동이 섞여 있으면 롤백 처리에 주의!
![rollback-call-external-api](https://imgur.com/AKtaFFy.png)

- 외부 시스템 연동 후 문제 발생 시 원복
  - Batch, API, 직접 취소 요청, ...

<br>

### 글로벌 트랜잭션

- 2PC(two-phase commit)
- 두 개 이상 자원(DB, 메시징큐 등)을 한 트랜잭션으로 처리
  - 각 자원이 2PC를 지원해야 함
  - 글로벌 트랜잭션 관리자 필요
  - 두 개 이상 자원에 대한 트랜잭션 처리가 쉬워짐
- (거의) **사용하지 않음**
  - 성능이 떨어짐, 서비스/마이크로서비스 -> 사용할 수 없는 구조
- 다중 자원에 대한 데이터 처리가 필요하면 다른 수단 고려
  - ex) 이벤트/비동기 메시징

<br>

### 정리

- 원자성(Atomicity) - **A**CID
  - all or nothing
- 트랜잭션 범위가 중요하다
  - 문제가 발생했을 때 롤백해야 하는 범위

<br>

## 프로그래밍 초식 : DB 트랜잭션 조금 이해하기 02

### 같은 데이터에 동시 접근

- 동시성은 초심자가 놓치기 쉬운 문제가 발생
  - ex) 당직 담당자를 최소 1명 유지해야 함
    ![같은데이어에동시접근](https://imgur.com/Sd8Cf8N.png)

<br>

### 경쟁 상태(Race Condition)

- 여러 클라이언트가 같은 데이터에 접근할 때 문제 발생
- 트랜잭션 격리(Isolation)
  - 트랜잭션을 서로 격리해서 다른 트랜잭션이 영향을 주지 못하게 함
- 가장 쉬운 방법은 트랜잭션을 순서대로 실행
  - 장점: 동시 접근 문제가 아예 없음
  - 단점: 한 번에 한 개 트랜잭션만 처리하므로 성능(처리량) 저하
- 다양한 격리 수준 지원
  - Read Uncommitted
  - Read Committed
  - Repeatable Read
  - Serializable

<br>

### 동시성 관련 다양한 문제들

1. 커밋되지 않은 데이터 읽기/덮어쓰기
2. 읽는 동안 데이터 변경 1
3. 변경 유실
4. 읽는 동안 데이터 변경 2

<br>

### 1. 커밋되지 않은 데이터 읽기, 덮어쓰기

- Dirty read
  ![dirty read](https://imgur.com/c6rkkb8.png)
- Dirty write
  ![dirty write](https://imgur.com/KVW9tJM.png)

#### 해결 방안: Read Committed

- 커밋된 데이터만 읽기
  - 커밋된 값과 트랜잭션 진행 중인 값을 따로 보관
- 커밋된 데이터만 덮어쓰기
  - 행 단위 잠금 사용
    - 같은 데이터를 수정한 트랜잭션이 끝날 때까지 대기

<br>

### 2. 읽는 동안 데이터 변경 1

- read skew
- 읽는 시점에 따라 데이터가 바뀜
  ![읽는 동안 데이터 변경1](https://imgur.com/Aj1zoz7.png)

#### 해결 방안: Repeatable Read

- 트랜잭션 동안 같은 데이터를 읽게 함 - ex) MVCC(Multi-Version Concurrency Control) - 읽는 시점에 특정 버전에 해당하는 데이터만 읽음
  ![repeatable-read](https://imgur.com/PMV1L2z.png)

<br>

### 3. 변경 유실

- Lost Update
- 같은 데이터를 쓸 때 발생 - ex) 조회수 증가, 위키 페이지 수정
  ![변경 유실](https://imgur.com/S2TFu3a.png)

#### 해결 방안

- 원자적 연산 사용
- 명시적인 잠금
- CAS(Compare And Set)

#### 원자적(atomic) 연산

- DB가 지원하는 원자적 연산 사용
  - 동시 수정 요청에 대해 DB가 순차 처리

```sql
update article
set readcnt = readcnt +1
where id = 1;
```

#### 명시적 잠금

- 조회할 때 수정할 행을 미리 잠금

```sql
select ...
for update
```

![명시적 잠금](https://imgur.com/jEiONwk.png)

#### CAS(Compare And Set)

- 수정할 때 값이 같은지 비교
  ![CAS](https://imgur.com/2Yh4oZc.png)

<br>

### 4. 읽는 동안 데이터 변경 2

- 한 트랜잭션의 결과가 다른 트랜잭션의 쿼리 결과에 영향 - 같은 데이터를 쓰지 않지만 실제로는 경쟁 상태
  ![읽는동안데이터변경2](https://imgur.com/Tq6a874.png)

#### 해결 방안: Serializable

- 인덱스 잠금이나 조건 기반 잠금 등 사용
  ![Serializable](https://imgur.com/aLmxbqY.png)

<br>

### 정리

- 동시성은 초보자가 놓치기 쉬운 문제
  - 동시성 문제와 격리 수준을 이해하면 문제 발생을 줄일 수 있음
- 잠금 시간은 최소화
  - 잠금 시간이 길어지면 성능(처리량) 저하
- 동시성 문제를 다룰 때는 다음을 아면 좋음
  - 사용하는 DB의 기본 격리 레벨
  - DB의 격리 레벨 동작 방식(DB마다 동작 방식이 다를 수 있음)

<br>

## 📚 References

- [유튜브 - 프로그래밍 초식 : DB 트랜잭션 조금 이해하기 01 by 최범균](https://www.youtube.com/watch?v=urpF7jwVNWs&list=PLwouWTPuIjUg0dmHoxgqNXyx3Acy7BNCz&index=5)
- [유튜브 - 프로그래밍 초식 : DB 트랜잭션 조금 이해하기 02 by 최범균](https://www.youtube.com/watch?v=poyjLx-LOEU&list=PLwouWTPuIjUg0dmHoxgqNXyx3Acy7BNCz&index=7)
