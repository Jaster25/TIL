# HTTP status code

HTTP 상태 코드는 특정 HTTP 요청이 성공적으로 완료되었는지 알려준다.

응답은 총 5개의 그룹으로 나눠진다.

- 1XX Information
- 2XX Success
- 3XX Redirection
- 4XX Client Error
- 5XX Server Error

<br>

## 1XX Information

서버가 클라이언트의 요청을 인지했다는 것을 의미한다.

### 100: Continue

- 클라이언트가 보낸 요청을 서버가 확인했다는 것을 의미한다.

<br>

## 2XX Success

클라이언트의 요청을 성공적으로 처리했을 때 사용한다.

### 200: OK

- 대표적인 성공 코드

### 201: Created

- 요청이 성공적으로 처리되어 **새로운 자원을 생성**했을 때 사용한다.

### 202: Accepted

- 요청이 성공적으로 접수되었지만, 아직 완료를 못했음을 의미한다.

### 204: No Content

- 요청은 성공적으로 처리되었지만 **응답할 데이터는 없을 때** 사용한다.

<br>

## 3XX Redirection

클라이언트는 요청을 완료하기 위해 추가 작업을 해줘야 알리기 위해 사용한다.

### 300: Multiple Choices

- 요청에 대해서 하나 이상의 응답이 가능하다.

### 301: Moved Permanently

- 요청 자원이 응답 헤더의 Location으로 **영구적**으로 옮겨졌을 때 사용한다.

### 302: Found

- 요청 자원이 응답 헤더의 Location으로 **일시적**으로 옮겨졌을 때 사용한다.

### 304: Not Modified

- 요청 후 수정 사항이 없을 때 캐시 목적으로 사용한다.

<br>

## 4XX Client Error

클라이언트가 유효하지 않은 요청을 할 때 사용한다.

### 400: Bad Request

### 401: Unauthorized

> Authentication 관련

- 클라이언트의 인증이 필요한 경우 사용된다.
- **인증** X

### 403: Forbidden

> Authorization 관련

- 클라이언트에게 권한이 없을 경우 사용한다.
- 인증 O, **인가** X

### 404: Not Found

- 클라이언트가 요청한 자원을 찾을 수 없을 때 사용한다.

### 409: Conflict

- 요청 자원과 대상 자원이 충돌할 때 사용한다.
- 주로 `PUT`요청 시 발생한다.

<br>

## 5XX Server Error

서버 상의 문제로 클라이언트의 요청을 제대로 수행하지 못했을 때 사용한다.

### 500: Internal Server Error

### 503: Service Unavailable

- 서버 과부하, 점검 등의 이유로 일시적으로 접근이 불가능할 때 사용한다.

<br>

## 참고

- https://developer.mozilla.org/ko/docs/Web/HTTP/Status
- https://velog.io/@geeneve/%EC%9E%90%EC%A3%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-HTTP-%EC%83%81%ED%83%9C-%EC%BD%94%EB%93%9C
