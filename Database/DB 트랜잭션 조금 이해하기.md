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

<br>

## 📚 References
- [유튜브 - 프로그래밍 초식 : DB 트랜잭션 조금 이해하기 01 by 최범균](https://www.youtube.com/watch?v=urpF7jwVNWs&list=PLwouWTPuIjUg0dmHoxgqNXyx3Acy7BNCz&index=5)
