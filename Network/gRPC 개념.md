# gRPC 개념

[유튜브 - gRPC 개념 by 개발자 장고](https://www.youtube.com/watch?v=r1eJjMeFnnw) 강의 정리

<br>

## gRPC 도입 배경

- HTTP를 사용하며 데이터를 주고 받을 때
  - key 값을 굳이 계속 명시할 필요가 있을까?
  - key를 생략해서 데이터를 줄이고 효율적인 통신을 할 수 있지 않을까?

<br>

## gRPC란?

> **Google Remote Procedure Call**

- 다른 프로세스와 통신할 때 인터페이스를 사용해서 통신하는 방법
- 클라이언트와 서버에 존재하는 각각의 **Stub**이 serialize와 deserialize 역할을 수행

<br>

## gRPC 특징

- Protocol Buffer: serialization 하여 데이터를 전달
  - 데이터 구조를 .ptoto 파일에 전달
  - _REST는 Serialization 없이 JSON, XML 등을 사용(용량이 큼)_
- HTTP 2 사용: Multiplexing, Header Compression 등을 사용할 수 있음
  > Multiplexing: HTTP connection 유지 가능
  - _REST는 HTTP 1.1를 사용함_

<br>

## gRPC HTTP Request & Response

- HTTP2, content-type: application/grpc
- Request message - binary format(protobuf)에 의해 Serialize
- Response message - binary format(protobuf)에 의해 Serialize

<br>

## ⭐ gRPC vs REST

- Body의 용량이 적을 때는 REST가 더 효율적임
  - HTTP2 혹은 Stub - Serialize/Deserialize의 영향
- Body에 용량이 커질수록 gRPC가 효율적
  - Stub - Serialize/Deserialize의 영향

<br>

## 결론

- 주고받는 데이터의 사이즈가 클 때는 gRPC가 좋은 선택이 될 수도 있다.
- gRPC를 통해 HTTP2의 Multiplexing 같은 기능도 활용할 수 있다.

<br>

## 📚 References

- [유튜브 - gRPC 개념 by 개발자 장고](https://www.youtube.com/watch?v=r1eJjMeFnnw)
