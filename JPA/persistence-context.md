# 영속성 컨텍스트(Persistence Context)

### 엔티티 매니저 팩토리(EntityManagerFactory)

- JPA 구현체가 **엔티티 매니저 팩토리**와 **데이터베이스 커넥션 풀**을 생성하며, 이때 비용이 커서 **싱글턴**으로 설계됐다.
- **클라이언트의 요청이 올 때마다 엔티티 매니저를 생성**한다.
- **스레드 안전**

<br>

### 엔티티 매니저(EntityManager)

- 엔티티와 관련된 모든 일을 처리한다.
- 생성 비용이 거의 안 든다.
- **데이터베이스 연결이 필요한 시점에 커넥션 풀로부터 커넥션을 하나 얻는다.**
- **트랜잭션 단위로 생성되며** 수행된 후에는 반드시 닫고 데이터베이스 커넥션을 반환한다.
- **스레드 안전 X**

<br>

### 엔티티 트랜잭션(EntityTransaction)

데이터를 변경하는 작업은 반드시 트랜잭션 안에서 이루어져야 한다.

```java
EntityTransaction tx = entityManager.getTransaction();

tx.begin(); // 트랜잭션 시작
tx.commit(); // 트랜잭션 수행
tx.rollback(); // 문제 발생 시 롤백
```

<br>

### 영속성 컨텍스트(Persistence Context)

- **엔티티를 영구 저장하는 환경**을 의미한다.
- 애플리케이션과 데이터베이스 사이에서 객체를 보관하는 **가상의 데이터베이스** 역할을 한다.
- **엔티티 매니저가 엔티티를 다룰 때 엔티티 컨텍스트를 사용한다.**
- 엔티티를 식별자 값(`@Id`를 사용하여 테이블의 기본 키와 매핑한 값)으로 구분한다.
  **따라서 영속 상태가 되기 위해서는 반드시 식별자가 존재해야 한다.**

<br>

### 영속성 컨텍스트 장점

- 1차 캐시

  - 내부에 1차 캐시를 가지고 있으며 영속 상태의 엔티티들은 모두 이곳에 저장된다.
  - **Map<@Id, 인스턴스> 형태**

- 동일성 보장

  - 영속 엔티티 간의 동일성 보장

    ```java
    Member member1 = em.find(Member.class, 1L);
    Member member2 = em.find(Member.class, 1L);

    member1 == member2 // true
    ```

  - 영속 엔티티와 비영속 혹은 준영속 엔티티 비교

    ```java
    Member member1 = em.find(Member.class, 1L);
    em.clear();
    Member member2 = em.find(Member.class, 1L);

    member1 == member2 // false
    ```

- 트랜잭션을 지원하는 쓰기 지연
  - 필요한 쿼리들을 **쓰기 지연 저장소**에 저장하고, **트랜잭션이 커밋되는 순간** 데이터베이스에 보낸다.
- 변경 감지(Dirty Checking)
  - 엔티티를 수정할 때는 엔티티를 조회해서 데이터만 변경하면 JPA가 데이터베이스에 반영해 준다.
- 지연 로딩
  - 연관 관계 매핑되어 있는 엔티티 조회 시 프록시(가짜 객체)를 반환하고, 진짜 필요한 경우에 데이터베이스를 조회하는 기능

<br>

## 참고

- 자바 ORM 표준 JPA 프로그래밍
- https://ttl-blog.tistory.com/108
