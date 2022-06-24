# RestTemplate vs FeignClient

Monolithic Service와는 달리 Micro Service에서는 물리적으로 분산된 **서비스 간의 통신**이 필수적이다.

RestTemplate과 FeignClient는 마이크로 서비스 간의 통신에 사용하는 대표적인 라이브러리다.

<br>

## 마이크로 서비스 간의 통신 방법

- 동기(Synchronous): Request, Response 방식
  - RestTemplate
  - FeignClient
- 비동기(Asynchronous): Subscribe 방식
  - Kafka
  - AMQP

<br>

## RestTemplate

- 스프링 3.0부터 제공하는 HTTP 통신 템플릿으로 spring-boot-starter-web에 포함되어 있다.
  ```java
  dependencies {
      implementation "org.springframework.boot:spring-boot-starter-web"
  }
  ```
- RESTful 형식에 맞추어져 있다.
- Blocking I/O 기반 동기 방식
- Header와 Content-Type을 설정하여 외부 API를 호출한다.

<br>

## FeignClient

- Spring Cloud Netflix에서 제공하는 라이브러리
  ```java
  dependencies {
      implementation "org.springframework.cloud:spring-cloud-starter-openfeign"
  }
  ```
- **인터페이스**와 **애노테이션**을 사용하여 코드가 깔끔하다.

<br>

## FeignClient가 더 좋은 이유

- 예외 처리
  - RestTemplate: `try-catch`문 사용
  - FeignClient: `ErrorDecoder`로 에러 핸들링을 깔끔하게 처리한다.
- Spring Cloud 친화적
- 인터페이스와 애노테이션만으로 구현
- URL 관리
  - Eureka Client ID를 직접 사용하여 하드코딩을 피할 수 있다.
- 유닛 테스트 편리
- 로그 관리

<br>

## 참고

- [https://stackoverflow.com/questions/46884362/what-are-the-advantages-and-disadvantages-of-using-feign-over-resttemplate](https://stackoverflow.com/questions/46884362/what-are-the-advantages-and-disadvantages-of-using-feign-over-resttemplate)
- [https://www.linkedin.com/pulse/resttemplate-feignclient-its-variety-sameh-muhammed?trk=articles_directory](https://www.linkedin.com/pulse/resttemplate-feignclient-its-variety-sameh-muhammed?trk=articles_directory)
- [https://m.blog.naver.com/qjawnswkd/222326922628](https://m.blog.naver.com/qjawnswkd/222326922628)
