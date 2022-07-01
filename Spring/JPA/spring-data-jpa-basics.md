# Spring Data JPA 기초

[유튜브 - Spring Data JPA 기초 by 최범균](https://www.youtube.com/playlist?list=PLwouWTPuIjUgRvSmAVlZekcyAo4zkf_TK) 강의 정리

<br>

## 1강. 시작하기

### Spring Data JPA 사용법

- spring-boot-starter-data-jpa 의존
- 스프링 부트 설정
- Repository 인터페이스 생성
- 규칙에 맞게 메서드 정의
- 필요한 곳에 해당 인터페이스 타입을 주입해서 사용

### Repository 인터페이스

- 스프링 데이터 JPA가 제공하는 특별한 타입으로
- 이 인터페이스를 상속한 인터페이스를 이용해 Bean 객체를 생성

<br>

## 2강. 레포지토리 메서드 작성 규칙

### 식별자로 엔티티 조회 메서드

- findById
  - T findById(ID id)
  - Optional\<T> findById(ID id)

### 엔티티 저장 메서드

- save
  - void save(T entity)
  - T save(T entity)

### save() 동작 방식

- 새 엔티티면 EntityManager#persist() 실행
- 새 엔티티가 아니면 EntityManager#merge() 실행
- 새 엔티티인지 판단하는 기준
  - Persistable을 구현한 엔티티는 isNew()로 판단
  - @Version속성이 있는경우 null로 확인
  - 식별자가 참조 타입이면 null로 확인
  - 식별자가 숫자 타입이면 0으로 확인

```java
@Transactional
@Override
public <S extends T> S save(S entity) {

    Assert.notNull(entity, "Entity must not be null.");

    if (entityInformation.isNew(entity)) {
        em.persist(entity);
        return entity;
    } else {
        return em.merge(entity);
    }
}

userRepository.save(new User( ... ));
```

### 특정 조건으로 찾기

- findBy프로퍼티
  - List\<User> findByName(String name)
- 조건 비교
  - List\<User> findByNameLike(String keyword)

<br>

## 3강. 정렬 페이징, @Query

### 정렬 1

- find 메서드에 OrderBy 붙임
  - OrderBy 뒤에 프로퍼티명 + Asc/Desc
  - 여러 프로퍼티 지정 가능
  ```java
  List<User> findByNameLikeOrderByNameDesc(String keyword);
  List<User> findByNameLiekOrderByNameAscEmailDesc(String keyword);
  ```

### 정렬 2

- Sort 타입 사용
  ```java
  List<User> findByNameLike(String keyword, Sort sort);

  Sort sort1 = Sort.by(Sort.Order.asc("name"));
  List<User> users1 = userRepository.findByNameLike("이름%", sort1);
  ```

### 페이징

- Pageable/PageRequest 사용
  ```java
  // page는 0부터 시작
  Pageable pageable = PageRequest.ofSize(10).withPage(1).withSort(sort3);
  List<User> user3 = userRepository.findByNameLike("이름%", pageable);
  ```

### 페이징 조회 결과 Page로 구하기

- Page 타입: 페이징 처리에 필요한 값을 함께 제공
  - 전체 페이지 개수, 전체 개수 등
- Pageable을 사용하는 메서드의 리턴 타입을 Page로 지정하면 됨
  ```java
  Page<User> findByEmailLike(String keyword, Pageable pageable);

  Pageable pageable = PageRequest.ofSize(10).withPage(1).withSort(sort3);
  Page<User> page = userRepository.findByNameLike("이름%", pageable);

  page.getTotalPages(); // 전체 페이지 개수
  page.getContent(); // 현재 페이지 결과 목록
  page.getSize(); // 페이지 크기
  page.getNumber() // 현재 페이지
  page.getNumberOfElement() // content의 개수
  ```

### @Query

- 메서드 명명 규칙이 아닌 JPQL을 직접 사용
  - 메서드 이름이 간결해짐
  ```java
  @Query("select u from User u where u.createDate > :since order by u.createDate desc")
  List<User> findRecentUsers(@Param("since") LocalDateTime since);
  ```

### 정리

- 메서드 명으로 정렬 지정할 순 있지만 가능하면 Sort 사용
- Pageable/PageRequest로 페이징 처리 가능
  - findTop/findFirst, findTopN/findFirstN
- @Query를 사용해서 JPQL 지정

<br>

## 4강. Specification을 이용한 검색 조건 지정

- Specification: 검색 조건을 생성하는 인터페이스

  - Criteria를 이용해서 검색 조건 생성

  ```java
  public interface Specification<T> extends Serializable {
      @Nullable
      Predicate toPredicate(Root<T> root, CriteriaQuery<?> query, CriteriaBuilder criteriaBuilder);
  }

  ```

- 레포지토리: Specification을 이용한 검색 조건 지정
  - List\<T> findAll(Specification<T> spec)
  ```java
  public interface UserRepository extends Repository<User, String> {
      List<User> findAll(Specification<User> spec);
  }
  ```

### Specification 구현/사용

```java
public class UserNameSpecification implements Specification<User> {
    private final String value;

    public UesrNameSpecification(String value) {
        this.value = value;
    }

    @Override
    public Predicate toPredicate(Root<User> root, CriteriaQuery<?> query, CriteriaBuilder cb) {
        return cb.like(root.get("name"), "%" + value + "%");
    }
}
```

```java
UserNameSpecification spec = new UserNameSpecification("이름");
List<User> users = userRepository.findAll(spce);
```

### 람다로 간결하게 구현

- Specification을 구현한 클래스를 매번 만들기보다는 람다식을 이용해서 생성

```java
public class UserSpecs {
    public static Specification<User> nameLike(String value) {
        return (root, query, cb) -> cb.like(root.get("name"), "%" + value + "%");
    }
}
```

```java
UserNameSpecification spec = UserSpecs.nameLike("이름");
List<User> users = userRepository.findAll(spce);
```

### 검색 조건 조합

- Specification의 or/and 메서드로 이용해서 조합

```java
Specification<User> nameSpec = UserSpecs.nameLike("이름1");
Specification<User> afterSpec = UserSpecs.createdAfter(LocalDateTime.now().minusHours(1));
Specification<User> compositeSpce = nameSpec.and(afterSpec);
List<User> user2 = userRepository.findAll(compositeSpec);
```

```java
Specification<User> spec3 = UserSpecs.nameLike(keyword)
                                .and(UserSpecs.createdAfter(dateTime));
List<User> user3 = userRepository.findAll(spec3);
```

> Spec 인터페이스에 and와 or 메서드가 디폴트 메서드로 구현되어 있다.

### 선택적으로 조합

```java
Specification<User> spec = Specification.where(null);

if (keyword != null && !keyword.trim().isEmpty()) {
    spec = spec.and(UserSpecs.nameLike(keyword));
}
if (dateTime != null) {
    spec = spec.and(UserSpecs.createdAfter(dateTime));
}
List<User> users = userRepository.findAll(spec);
```

### Specification + 페이징, 정렬

- List\<User> findAll(Specification\<T> spec, Sort s)
- Page\<User> findAll(Specification\<T> spec, Pageable p)
- List\<User> findAll(Specification\<T> spec, Pageable p)

### SpecBuilder

- if 절을 덜 쓰기 위한 SpecBuilder 구현

### 정리

- Specification 인터페이스를 이용한 검색 조건 생성
- and/or 메서드를 사용하여 조합 가능

<br>

## 5강. 기타

### count 메서드

- long count()
- long countByNameLike(String keyword)
- long count(Specification\<User> spec)

### @Query 네이티브 쿼리 애노테이션

- JPQL이 아닌 SQL 실행

```java
@Query(value = "select * from user u where u.create_date >= date_sub(now(), interval 1 day"), nativeQuery = true)
List<User> findRecentUsers();

@Query(value = "select max(create_date) from user", nativeQuery = true)
LocalDateTime selectLastCreateDate();
```

### 1개 결과 조회

- User findByName(String name)
- Optional\<User> findByName(String name)
- 리턴 타입이 List가 아니다.
- 존재하면 해당 값, 없으면 null 또는 비어있는 Optional
- **조회 결과 개수가 두 개 이상이면 예외 발생!**

### 인터페이스를 상속하면 편리하나...

- Repository 하위 인터페이스를 상속하면 관련 메서드 모두 포함

```java
public interface UserRepository extends JpaRepository<User, String> {
    // 메서드를 정의하지 않아도 CrudRepository와 JpaRepository에 있는
    // CRUD 등의 메서드를 제공
}
```

- Repository를 상속 받고 딱 필요한 메서드만 생성하는 방법을 선호
  1. 명령 모델의 단위 테스트
     - 단위 테스트에서 레포지토리의 대역은 가짜 대역 사용
       - 모의 객체를 사용하지 않음
       - 인터페이스 메서드가 적어야 만들기가 쉬움
     - JpaRepository는 가짜 대역을 구현하기에 메서드가 너무 많음(대략 20여개)
  2. 가능하면 필요한 메서드만
     - 명령 모델의 레포지토리는 필요한 메서드가 매우 적다.
       - findById 또는 findOne, save 정도가 필수
       - 이외의 도메인이 제공할 기능에 따라 2~3개 정도의 메서드만 추가 필요
     - JpaRepository를 상속하면 필요하지 않은 메서드를 오용할 가능성 존재
  3. 조회 모델은?
     - 조회 모델도 JpaRepository를 상속하지 않음
       - 조회 모델에 save()가 존재하면?
       - Repository를 상속하고 필요한 메서드를 그때그때 추가
     - 조회 모델 구현 ⭐
       - 다양한 스펙 조합이 필요하면 JPA
       - 단순 조회면 JdbcTemplate
       - 쿼리가 복잡한 조회면 MyBatis
  4. 정리
     - JpaRepository를 상속하면 단위 테스트가 어렵고 CQRS에 맞지 않는다.
     - Repository를 상속하고 필요한 메서드만 추가하자.

### 정리

- Spring Data JPA로 편한 레포지토리 구현 가능
- 하지만 모두 Spring Data JPA로 구현하지 말고, MyBatis, JdbcTemplate 등 적절히 사용하자.(CQRS) ⭐

<br>

## 📚 Reference

- [유튜브 - Spring Data JPA 기초 by 최범균](https://www.youtube.com/playlist?list=PLwouWTPuIjUgRvSmAVlZekcyAo4zkf_TK)
- [유튜브 - [QA] JpaRepository를 상속하지 않은 이유 by 최범균](https://www.youtube.com/watch?v=MMH_ht8pf8U)
