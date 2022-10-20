# JWT(JSON Web Token)

JWT란 **토큰 기반의 인증 시스템**에서 주로 사용되는 기술로, JSON 형식을 이용하여 **사용자에 대한 속성을 Claim 기반으로 저장**하는 방식이다.

JWT는 Header, Payload, Signature 3개의 영역으로 이루어지며 모두 Base64로 인코딩 되어 표현된다. 각 영역을 구분하기 위해 영역 사이에 `.` 구분자가 사용된다.

![jwt](https://imgur.com/Tc0VDQM.png)

> **Encoding**
>
> - 정보의 형태나 형식을 표준화, **보안**, 처리 속도 향상, 저장 공간 절약 등을 위해 다른 형식으로 처리하는 것

> **Base64 Encoding**
>
> - Binary Data를 텍스트로 변경하는 인코딩
> - 암호화된 문자열이 아니고 같은 문자열에 대해 항상 같은 인코딩 문자열을 반환함
> - 데이터의 크기가 약 33% 증가하는데 왜 사용하는가?
>   - 통신 과정에서 바이너리 데이터의 손실을 막기 위해 사용한다.
>   - 아스키 코드는 시스템 간 데이터를 전달하기에 안전하지 않다.

<br>

## JWT 구성 요소

### Header

토큰 타입과 토큰 생성에 사용된 알고리즘이 담겨있다.

```JSON
{
  "alg": "HS256",
  "typ": "JWT"
}
```

<br>

### Payload

토큰에 사용된 Claim 정보들이 Key-Value 형태로 담겨있다.

> **Claim**
>
> 정보의 조각(JSON 형식)

3가지로 나누어진다.

- 등록된 클레임(Registered Claim)
  - 토큰 정보를 표현하기 위해 이미 정해진 종류의 데이터들
- 공개 클레임(Public Claim)
  - 공개 정보를 위한 사용자 정의 클레임
  - 충돌 방지를 위해 URI 형식 사용
- 비공개 클레임(Private Claim)
  - 비공개 정보를 위한 사용자 정의 클레임
  - 클라이언트와 서버 사이에 임의로 지정된 정보를 저장

```JSON
{
  "sub": "1234567890",
  "name": "Jaster25",
  "userId": "220ASDJ2N3D2DASDO",
  "exp": "123229000",
  "iat": 1516239022
}
```

<br>

### Signature

서명은 토큰을 인코딩하거나 유효성 검증을 할 때 사용하는 고유한 암호화 코드이다.

위에서 만든 Header와 Payload의 값을 각각 Base64로 인코딩하고, Secret Key를 이용해 Header에서 정의한 알고리즘으로 해싱을 하고, 이 값을 다시 Base64로 인코딩하여 생성한다.

```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  SECRET_KEY
)
```

<br>

## JWT 장단점
### 장점
- 확장성
- 데이터 변조가 어렵다.
- 인증을 위한 별도의 저장 공간이 필요없다.
### 단점
- 토큰의 길이가 짧지 않고, 인증 과정이 필요해서 서버에 부담이 될 수 있다.
- 발급된 토큰이 탈취되면 곤란해진다.

<br>

## JWT 저장 위치
보안과 관련된 JWT의 저장 위치는 매우 중요하다.

### 보안 위협 종류
- XSS(Cross Site Scripting)
    > CSS는 기존에 쓰이는 약자가 존재해서 XSS라 명칭
    - 웹 서비스에 악의적인 JavaScript 같은 스크립트 코드를 삽입하는 공격
- CSRF(Cross Site Request Forgery)
    - 정상적인 요청을 가로채고 위조하여 서버에 변조된 Request를 보내는 공격


### Cookie에 저장
- XSS 공격에 LocalStorage 보다는 안전
    - `HttpOnly` 설정으로 JavaScript 접근을 막을 수 있다.
- CSRF 공격에 취약
    - HTTP Request에 항상 담겨 보내지기 때문에 경로를 알면 위험해진다.

### LocalStorage에 저장
- CSRF 공격에 안전
    - HTTP 헤더에 담겨 보내지기 때문에 XSS를 뚫지 않는 이상 위조하기 어렵다.
- XSS 공격에 취약
    - LocalStorage에 접근하는 스크립트 코드를 삽입하면 위헙해진다.


<br>

## 📚 References
- https://jwt.io/
- https://velopert.com/2389
- [https://velog.io/@0307kwon/JWT는-어디에-저장해야할까-localStorage-vs-cookie](https://velog.io/@0307kwon/JWT%EB%8A%94-%EC%96%B4%EB%94%94%EC%97%90-%EC%A0%80%EC%9E%A5%ED%95%B4%EC%95%BC%ED%95%A0%EA%B9%8C-localStorage-vs-cookie)