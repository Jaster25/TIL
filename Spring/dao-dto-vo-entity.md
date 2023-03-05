# DAO & DTO & VO & Entity

## DAO(Data Access Object)

- 데이터베이스를 사용해 데이터를 직접 다루는 기능을 전담하도록 만든 객체이다.
- 데이터 영속성의 추상화다.

### DAO와 Repository의 차이점

- DAO는 데이터베이스와 가깝고, Repository는 도메인과 가깝다.
- Repository에서는 하나 이상의 DAO를 사용해도 되지만, DAO는 Repository를 사용하면 안된다.
- 도메인의 규모가 작을수록 둘의 차이가 적어진다.

<br>

## DTO(Data Transfer Object)

- 계층 간 데이터 전달 목적의 객체를 의미한다.
- API 요청에 대해 관련된 엔티티를 그대로 보내지 않고, 필요한 정보만 응답한다.
    - 중요 데이터 노출 위험 방지
    - 응답 값을 줄이면서 성능 최적화
- 로직을 갖고 있지 않은 순수한 데이터 객체이며, 오직 Getter, (Setter)만을 갖는다.

<br>

## VO(Value Object)

- 값 그 자체를 표현하는 객체이다.
- DTO와 개념은 같은데 **Read Only**라는 속성을 갖는다.
- 모든 속성 값이 같으면 같은 객체를 의미한다. 즉, **equals()와 hashcode()의 오버라이딩 필수이다.**

<br>

## Entity

- 실제 데이터베이스 테이블과 1대1 매핑되는 클래스이다.
- 최소한의 로직을 갖도록 한다.
- View 계층과 분리되어야 한다.

<br>

## 참고

- https://velog.io/@agugu95/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%8C%A8%ED%84%B4%EA%B3%BC-DAO-DTO-Repository
- https://isaac56.github.io/etc/2021/08/29/difference_DAO_Repository/
