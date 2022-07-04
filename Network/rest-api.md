# REST API(Representational State Transfer API)

HTTP를 잘 활용하기 위한 하나의 표준화된 아키텍처 스타일로, 어떤 자원에 대해 어떤 작업을 하는지 알기 쉽게 표현하는 방식이다.

> **API(Application Programming Interface)**
>
> 데이터와 기능의 집합을 제공하여 컴퓨터 프로그램 간의 상호작용을 도와주는 매개체

> **RESTful이란?**
>
> REST의 원리를 잘 따르는 서비스를 의미한다.

<br>

## 특징

- Server-Client 구조
- Stateless
- Cacheable
- Layered System
- Uniform Interface

<br>

## 구성 요소

1. Resource(자원): HTTP URI
2. Verb(행위): HTTP Method
3. Retpresentation of Resource(내용): HTTP Message Payload

<br>

## REST API 네이밍 컨벤션

1. 자원을 나타낼 때는 명사를 사용하자.
   - Document
     - 개체 인스턴스 또는 데이터베이스 레코드와 유사한 개념
     - 이 집합들 중 객체를 구분할 수 있는 값(id 등)
   - Collection
     - 리소스들의 집합
     - 복수형으로 사용
   - Store
     - 클라이언트가 관리하는 자원의 집합
     - 복수형으로 사용
   - **Controller**
     - 예외적으로 HTTP method만으로 행위를 표현하기 어려울 때 사용
     - 동사형으로 사용
2. 계층 관계를 나타낼 때는 `/`를 사용하자.
   ```
   device-management/managed-devices
   device-management/managed-devices/{id}
   ```
3. URI 마지막에 `/`를 사용하지 말자.
   ```
   device-management/managed-devices    O
   device-management/managed-devices/   X
   ```
4. 하이픈(`-`)을 사용하자.
   ```
   device-management/managed-devices
   ```
5. 밑줄(`_`)를 사용하지 말자.
6. 소문자를 사용하자.
7. 파일 확장자를 적지 말자.
8. URI에 CRUD 메서드명을 사용하지 말자.
9. **쿼리 스트링**을 사용하여 URI 컬렉션 필터링 하자.
   ```
   device-management/managed-devices?region=USA&brand=XYZ
   ```

<br>

## REST API 성숙도 모델(by Rechardson)

4단계로 나눠져 있으며 각 단계를 만족할 수록 REST API에 가까워진다고 한다.

![rest-api-level](https://imgur.com/CY9k04L.png)

### Level 0: 1 URI, 1 HTTP method

- 웹 매커니즘을 사용하지 않고 단순히 원격 통신을 위한 전송 시스템으로 사용

### Level 1: N URI, 2 HTTP method

- URI 설계 시 리소스 개념 사용

### Level 2: N URI, 4 HTTP method

- HTTP Method 사용
- HTTP Status Code 사용

### Level 3: Hypermedia As The Engine of Application State(HATEOAS)

> 여기서부터 진정한 REST API라고 한다.

- API의 응답에 클라이언트의 다음 단계에 대한 API URI를 함께 제공

<br>

## 📚 Reference

- https://restfulapi.net/resource-naming/
- https://jaehoney.tistory.com/176
