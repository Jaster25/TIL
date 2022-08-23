# JPA vs MyBatis

## JPA(Java Persistence API)

![jpa](https://imgur.com/9UtCLGH.png)

Java ORM 기술에 대한 API 표준 명세로, 구현된 클래스와 매핑을 해주기 위해 사용되는 프레임워크

> **ORM(Object-Relational Mapping)**
>
> 객체와 관계형 데이터베이스를 매핑한다는 뜻

구현체로는 Hibernate, EclipseLink, Data Nucleus가 있으며 Hibernate가 가장 대중적이다.

### 장점

- 생산성
- 유지보수
- 성능
  - 1차 캐시
  - 쓰기 지연
  - 변경 감지
  - 지연 로딩
- 컴파일 타임 시 오류 확인
- 가독성
  - 쿼리를 직접 작성할 필요가 없어 코드량이 줄어든다.

### 단점

- 고도화 될수록 성능 이슈가 발생할 수 있다.
  - JPA를 제대로 사용하려면 알아야할 것이 많아서 학습하는데 시간이 걸린다.
  - N+1, FetchType, Proxy, 연관관계 문제 등
- 복잡한 쿼리 처리
  - 수행할 작업이 복잡할 경우 기존 SQL 쿼리를 사용하는게 나을 수 있다.

<br>

## MyBatis

![mybatis](https://imgur.com/pWtAZQk.png)

자바의 관계형 데이터베이스 프로그래밍을 더 쉽게 도와주는 프레임워크로 **SQL Builder**, **SQL Mapper**의 한 종류

자바에는 관계형 데이터베이스 프로그래밍을 처리하기 위해 JDBC를 제공한다. MyBatis는 JDBC를 더 편하게 사용하기 위해 개발되었다.

> **JDBC(Java Database Connectivity)**
>
> 자바 프로그램이 데이터베이스와 연결되어 데이터를 주고 받을 수 있게 해주는 프로그래밍 인터페이스
>
> - DriverClass
> - Connection
> - PreparedStatement
> - ResultSet

특징

- 관심사 분리
- 오픈소스
- 다양한 프로그래밍 언어로 구현 가능

### 장점

- SQL 쿼리를 직접 작성하여 최적화된 쿼리를 구현할 수 있다.
- JDBC 코드를 걷어내어 가독성을 얻는다.

### 단점

- 반복된 쿼리 작업
- 데이터베이스에 종속된 쿼리 작업
  - 데이터베이스 변경 시 관련된 로직도 수정해야 한다.
- 런타임 시에 오류 확인

<br>

## 📚 Reference

- [JPA란 무엇인가, 그리고 장단점은 뭐가 있을까?](https://velog.io/@dev_zzame/JPA%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%EC%9E%A5%EB%8B%A8%EC%A0%90%EC%9D%80-%EB%AD%90%EA%B0%80-%EC%9E%88%EC%9D%84%EA%B9%8C)
- [[MyBatis] MyBatis란 무엇인가?](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=wwwkang8&logNo=220989381100)
