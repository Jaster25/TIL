# SQL vs NoSQL
> 관계형 데이터베이스 vs 비관계형 데이터베이스


## SQL(Structured Query Language)
- 관계형 데이터베이스
- 데이터베이스에서 데이터를 저장, 조회, 수정, 삭제하는데 사용되는 질의(query) 언어
- 데이터가 자주 변경되거나 명확한 스키마가 있는 경우 사용 ⭐ 

> DBMS(DataBase Management System)
> 
> - 데이터베이스 관리 시스템
> - 데이터에 접근하여 사용할 수 있도록 도와주는 소프트웨어

### SQL 종류
- 데이터 정의 언어(DDL, Data Definition Language)
    - 테이블, 인덱스 등의 개체를 만들고 관리하는데 사용되는 명령어
    - `CREATE`, `ALTER`, `DROP`
- 데이터 처리 언어(DML, Data Manipulation Language)
    - `INSERT`, `UPDATE`, `DELETE`, `SELECT`
- 데이터 제어 언어(DCL, Data Control Language)
    - 데이터 핸들링 권한 설정, 데이터 무결성 처리 등 수행
    - `GRANT`: 데이터베이스 개체(테이블, 인덱스 등)에 대한 사용 권한 설정
    - `BEGIN`: 트랜잭션 시작
    - `COMMIT`: 트랜잭션 내의 실행 결과 적용
    - `ROLLBACK`: 트랜잭션 실행 취소


### 관계형 데이터베이스 특징
- 고정된 행과 열로 구성된 테이블에 데이터를 저장한다.
- 데이터는 정해진 데이터 스키마에 따라 테이블에 저장된다.
- 데이터는 관계를 통해 여러 테이블에 분산된다.
- ACID
    - 트랜잭션이 안전하게 수행된다는 것을 보장하기 위한 성질
    - Atomicity(원자성)
        - 트랜잭션의 연산은 모든 연산이 완벽히 수행되어야 하며, 한 연산이라도 실패하면 트랜잭션은 실패해야 한다.
    - Consistency(일관성)
        - 트랜잭션은 유효한 상태로만 변경될 수 있다.
    - Isolation(고립성)
        - 트랜잭션은 동시에 실행될 경우 다른 트랜잭션에 의해 영향을 받지 않고 독립적으로 실행되어야 한다.
    - Durability(내구성)
        - 트랜잭션이 커밋된 이후에는 시스템 오류가 발생하더라도 커밋된 상태로 유지되는 것을 보장해야 한다.

> **용어**
> 
> - 테이블 = 관계
> - 행(Column) = Field = 속성(Attribute)
> - 열(Row) = Record = Tuple


<br>

## NoSQL(Non-SQL, Not Only SQL, Non-Relational Operational Database SQL)
- 비관계형 데이터베이스
- 관계형 데이터베이스가 아닌 다른 형태로 데이터를 저장하는 기술
- 데이터를 자주 변경하지 않고 정확한 데이터 구조를 알 수 없는 경우 사용 ⭐

### 특징
- 스키마, 테이블 X
- Join 개념이 없는 대신, 데이터를 복제하여 중복되게 넣어준다.
- **유연한 스키마**를 제공하여 보다 빠르고 반복적인 개발을 가능하게 해준다.
- 언제든지 데이터 수정, 새로운 "필드" 추가 가능
- **수직 & 수평 확장**


### 유형
- Document
    - 데이터를 테이블이 아닌 문서처럼 저장
    - JSON, XML과 같은 Collection 데이터 모델 구조
    - 각각의 문서는 하나의 속성에 대한 데이터를 가지고 있고, 컬렉션이라 하는 그룹으로 묶어 관리
    - 예시
        - **MongoDB**
        - CoughDB
- Key-Value
    - Key와 Value의 쌍으로 데이터가 저장되는 가장 단순한 형태
    - 예시
        - Redis
        - Dynamo
- Wide-Column
    - 데이터베이스의 열(column)에 대한 데이터를 집중적으로 관리
    - 하나의 행에 많은 열을 포함할 수 있어 유연성이 높다.
    - 대용량 데이터 분석에 주로 사용
    - 예시
        - Cassandra
        - HBase
- Graph
    - 자료구조 그래프와 비슷한 형식으로 데이터 간의 관계를 구성
    - 예시
        - Neo4J
        - InfiniteGraph


<br>

## 📚 Reference
- https://github.com/ksundong/backend-interview-question
- https://github.com/yoonje/developer-interview-questions-and-answers/blob/master/Database/README.md
- https://hanamon.kr/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-sql-vs-nosql/
- https://siyoon210.tistory.com/130
- https://www.fun-coding.org/mysql_basic1.html