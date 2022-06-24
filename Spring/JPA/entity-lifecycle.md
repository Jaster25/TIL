# 엔티티의 생명주기(Entity LifeCycle)

## 엔티티에는 4가지 상태가 존재한다.

- **비영속**: 영속성 컨텍스트와 전혀 관계가 없는 상태
- **영속**: 영속성 컨텍스트에 저장된 상태
- **준영속**: 영속성 컨텍스트에 저장되었다가 **분리된 상태**
- **삭제**: 엔티티가 영속성 컨텍스트와 데이터베이스에서 삭제된 상태

![entity-lifecycle](https://imgur.com/4gr37nI.png)

<br>

## 비영속(new/transient)

순수한 객체 상태로 아직 영속성 컨텍스트나 데이터베이스와는 관련이 없다.

```java
Member member = new Member();
member.setId("member1");
member.setUsername("회원1");
```

<br>

## 영속(managed)

엔티티 매니저를 통해 **영속성 컨텍스트에 저장되었으며 영속성 컨텍스트에 의해 관리된다.**

```java
// 객체를 저장한 상태(영속)
em.persist(member);
```

또한 `em.find()`나 JPQL을 사용해서 **조회한 엔티티**도 영속성 컨텍스트가 관리하는 영속 상태다. ⭐

<br>

## 준영속(detached)

영속성 컨텍스트가 관리하던 영속 상태의 엔티티를 영속성 컨텍스트가 관리하지 않으면 준영속 상태가 된다.

### 준영속 상태로 만드는 방법

- 특정 엔티티에 `em.detach()` 호출한다.
- `em.close()`를 호출해서 영속성 컨텍스트를 닫는다.
- `em.clear()`를 호출해서 영속성 컨텍스트를 초기화한다.

```java
// 회원 엔티티를 영속성 컨텍스트에서 분리, 준영속 상태
em.detach(member);
```

<br>

## 삭제(removed)

엔티티를 영속성 컨텍스트와 데이터베이스에서 삭제한다.

```java
// 객체를 삭제한 상태
em.remove(member);
```

<br>

## 📚 Reference

- 자바 ORM 표준 JPA 프로그래밍
