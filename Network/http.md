# HTTP

- HyperText Transfer Protocol
- WWW(World Wide Web)에 내재된 프로토콜
- 인터넷에서 데이터(HTML과 같은 하이퍼미디어 문서)를 주고 받을 수 있는 프로토콜이다.

> **Protocol이란?**
>
> 데이터 통신을 원활하게 하기 위해 필요한 통신 규약을 의미한다.
>
> **Web이란?**
>
> 인터넷에 연결된 사용자들이 서로의 정보를 공유할 수 있는 공간을 의미한다.

<br>

## HTTP 특징

1. 클라이언트-서버 구조
2. 무상태 프로토콜
3. 비연결성
4. HTTP 메세지
5. 단순
6. 확장성

<br>

### 1. 클라이언트-서버 구조

클라이언트가 서버에 요청을 보내면 서버는 그에 대한 응답을 보내는 클라이언트-서버 구조로 이루어져 있다.

- **Request-Response 구조**
- 클라이언트는 서버에 요청을 보내고 응답을 대기
- 서버가 요청에 대한 결과를 만들어 응답

<br>

### 2. 무상태 프로토콜

HTTP에서 서버는 클라이언트의 상태를 보존하지 않는 무상태 프로토콜이다.

- 장점: 서버 확장성 높음
- 단점: 클라이언트가 추가 데이터 전송

<br>

### 3. 비연결성

HTTP에서는 요청을 주고 받을 때만 연결을 유지하고 응답을 주고나면 TCP/IP 연결을 끊는다.
이를 통해 최소한의 자원으로 서버 유지를 가능하게 한다.

<br>

### 4. HTTP 메시지

- HTTP 메시지는 클라이언트와 서버 간 데이터가 교환되는 방식이다.

- Request와 Response 방식이 있으며 ASCII로 인코딩된 텍스트 정보이다.

- 메시지는 세 부분으로 구성되어 있다.

  - Start Line
  - Header
  - (Blank Line): 헤더의 끝을 표현
  - Body

<br>

#### HTTP Request 메시지 구조

- Start Line
  - HTTP 메서드
  - Request target: HTTP Request가 전송되는 목표 주소
  - HTTP version
- Header
  HTTP Request에 대한 정보를 Key-Value 형태로 담고 있다.
  - **공통 Header**
  - Host: 요청하려는 서버 호스트 이름과 포트 번호
  - User-agent: 클라이언트 프로그램 정보
  - Referer: 바로 직전에 머물렀던 주소
  - Accept: 클라이언트가 처리 가능한 미디어 타입 나열
  - If-Modified-Since: 지정된 날짜 이후 수정 된 경우 요청된 리소스를 돌려준다.
  - Authorization: 인증 토큰
  - Origin: `POST` 요청 시 어느 주소에서 시작되었는지 나타내는 값
  - Cookie: Key-Value 형태의 쿠키 값
- Body
  전송하는 데이터를 담고 있다. `POST` 요청일 경우, HTML form 데이터가 포함되어 있다.

<br>

#### HTTP Response 메시지 구조

- Start Line
  - HTTP version
  - Status Code
  - Status Text: Response 상태를 간단히 설명
- Header
  HTTP Request에 대한 정보를 Key-Value 형태로 담고 있다.
  - **공통 Header**
  - Location: 301, 302 상태 코드일 때만 볼 수 있는 Header로 서버 응답 위치를 지정해준다.
  - Server: 웹 서버 종류
  - Age: max-age 시간내에서 얼마나 흘렀는지 알려주는 값
  - Referrer-policy: 서버 referrer 정책(origin, no-referrer, unsafe-url)
  - WWW-Authenticate: 사용자 인증이 필요한 자원을 요구할 시, 서버가 제공하는 인증 방식
  - Proxy-Authenticate: 요청한 서버가 프록시 서버인 경우 유저 인증을 위한 값
- Body
  전송하는 데이터를 담고 있다.

#### 공통 Header

- Date
- **Cache-Control**
- Transfer-Encoding: Body 내용 자체 압축 방식
- Content-Encoding: Body의 리소스 압축 방식
- Content-Type: Body의 미디어 타입(application/json, text/html)
- Content-Length: Body의 길이
- Content-Language
- **Connection**: 클라이언트와 서버의 연결 방식 설정

<br>

### 5. 단순

<br>

### 6. 확장성

<br>

## HTTP 흐름

1. TCP 연결
2. HTTP 메세지 전송
3. 서버의 응답 읽기
4. 연결을 닫거나 다른 요청들을 위해 재사용

<br>

## 참고

- https://developer.mozilla.org/ko/docs/Web/HTTP
- https://hanamon.kr/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-http-http%EB%9E%80-%ED%8A%B9%EC%A7%95-%EB%AC%B4%EC%83%81%ED%83%9C-%EB%B9%84%EC%97%B0%EA%B2%B0%EC%84%B1/
- https://ohcodingdiary.tistory.com/5
