# JPA에서 엔티티를 수정하는 방법

JPA에는 수정과 관련된 메서드가 따로 존재하지 않는다.

영속 상태 엔티티의 변경 사항은 데이터베이스에 자동으로 반영되며, 이 기능을 **변경 감지(Dirty Checking)** 이라 한다. ⭐

> 영속 상태가 아니라면 변경 사항이 적용되지 않는다!

<br>

### Dirty Checking 과정

엔티티를 영속성 컨텍스트에 보관 시 **스냅샷(최초 상태)** 을 복사해서 저장하고, 트랜잭션이 끝나는 시점에 스냅샷과 엔티티를 비교해 다른점이 있다면 UPDATE Query를 데이터베이스로 전달한다.

![dirty-checking](https://imgur.com/0kiMWID.png)

<br>

## 수정하려는 엔티티가 현재 영속 상태가 아닌 경우(=준영속 상태인 경우)

1. 레포지토리에서 `find()` 메서드 등을 사용해 영속성 컨텍스트에 보관한 후 변경 감지가 가능하게 한다.
2. `save()`, `merge()` 메서드를 사용해서 변경을 직접 적용한다.

   ### SimpleJpaRepository의 `save()` 메서드

   ```java
   @Transactional
   public <S extends T> S save(S entity) {
       if (this.entityInformation.isNew(entity)) {
           this.em.persist(entity);
           return entity;
       } else {
           return this.em.merge(entity);
       }
   }
   ```

   새로운 엔티티이면 `persist()` 메서드를, 아니면 `merge()` 메서드를 호출한다.

    ### `persist()` 메서드
    -  엔티티를 영속성 컨텍스트에 저장한다.
    
    ### `merge()` 메서드
     - 준영속 상태의 엔티티를 영속 상태로 변경한다.
     - 엔티티의 변경 사항을 영속성 컨텍스트나 데이터베이스에 업데이트한다.

<br>

## 📚 Reference

- 자바 ORM 표준 JPA 프로그래밍
- https://jojoldu.tistory.com/415
