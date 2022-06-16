# HTTP method

HTTP 메서드는 클라이언트가 서버에게 요청의 목적이나 종류를 알려주는 수단이다.

<br>

## HTTP 메서드 종류

- **GET**
- **POST**
- **PUT**
- **PATCH**
- **DELETE**
- OPTIONS
- HEAD
- TRACE
- CONNECT

<br>

## HTTP 메서드 속성

- 안전
  - `GET`, `HEAD`, `OPTIONS` (`TRACE`는 클라이언트 쪽에 공격을 시도하여 credentials을 훔칠 수 있기에)
  - 서버에 어떤 영향을 줄 수 없는 속성
  - 읽기 전용
- 멱등성
  - `GET`, `PUT`, `DELETE`, `TRACE`, `HEAD`, `OPTIONS`
  - 특정 메서드 요청을 여러 번 했을 경우, 한번 요청했을 때와 결과가 같은 속성
- 캐시 가능성
  - 향후 재사용을 위해 이에 대한 응답을 저장할 수 있음을 나타내는 속성
  - `GET`, `HEAD`, `POST`

<br>

---

### GET

- 특정 리소스를 가져올 때 사용한다.
- GET 요청은 데이터를 가져올 때만 사용해야 한다.
  - Payload를 담지 않는 것이 바람직하다.

> **Payload란?**
>
> HTTP 요청 시 전송되는 데이터를 의미한다.

### POST

- 신규 리소스 생성 혹은 프로세스 처리에 사용된다.
- 요청 본문의 유형은 Content-Type 헤더로 나타낸다.
- 서버로 데이터를 전송한다.

### PUT

- Payload를 사용해 **새로운 리소스를 생성하거나**, **대상 리소스를 나타내는 데이터를 대체**한다.

### PATCH

- 리소스의 부분적인 수정을 할 때 사용한다.
- 멱등성 X

### DELETE

- 특정 리소스를 제거할 때 사용한다.

### OPTIONS

- 특정 리소스와의 통신 옵션을 설명하기 위해 사용된다.
  - 서버나 특정 URI에 해당하는 리소스가 제공하는 기능 확인
  - 서버가 살아있는지 확인
  - 해당 리소스를 처리할 수 있는지 확인

### HEAD

- 특정 리소스를 GET 메서드로 요청했을 때 돌아올 Header를 요청합니다.
  - GET 메서드로 보내고 응답 헤더만 받는 느낌
- GET 요청에 전 사전 확인 용도(?)

### CONNECT

- 요청한 리소스에 대해 양방향 연결을 시작하는 메서드이다.
  - 터널을 열기 위해서 사용될 수 있다.

### TRACE

- 서버에 Loopback 메시지를 호출하기 위해 사용된다.
- 요청이 서버에 도달했을 때 어떻게 보이는지 알려준다.
- 주로 진단을 위해 사용된다.(디버깅)

<br>

## 기타

### POST vs PUT

POST

- 리소스 생성
- 멱등성 X: 요청 시 마다 새로운 리소스가 생성된다

PUT

- 리소스 생성(해당 URI에 맞는 리소스가 없을 시) or 리소스 수정
- 멱등성 O

### PUT vs PATCH

PUT

- 리소스의 모든 속성 수정
- 멱등성 O

PATCH

- 리소스의 부분 수정

### HEAD & OPTIONS

- Spring Web MVC가 기본적으로 지원해준다.

<br>

## HTTP 메서드 요약

![http-method-summary](https://i.imgur.com/xBrXy9i.png)

<br>

## 참고

- https://developer.mozilla.org/ko/docs/Web
- https://ko.wikipedia.org/wiki/HTTP
- https://feel5ny.github.io/2019/08/16/HTTP_003_02/
