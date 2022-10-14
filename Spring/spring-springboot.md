# Spring vs Spring Boot

## Spring

스프링은 자바의 객체 지향 언어라는 특징을 잘 살려내게 도움을 주는 웹 **프레임워크**


### Spring 핵심 기술
- **IoC(Inversion of Control)**
  - 흐름 제어권을 개발자가 아닌 프레임워크가 갖는 것을 의미한다.
- **AOP(Aspect Oriented Programming)**
  - 공통적으로 사용되는 기능을 분리하여 재사용할 수 있게 관리하는 것을 의미한다.
  - ex) 로그 관리, 트랜잭션, 인증, ...
- **PSA(Portable Service Abstraction)**
    - 추상화 계층을 사용하여 어떤 기술을 내부에 숨기고 개발자에게 편의성을 제공해주는 것을 의미한다.
    - ex) 간단하게 `@Controller` 어노테이션을 호출하는 것만으로 컨트롤러 역할을 수행
    

<br>
 
## Spring Boot

스프링 프레임워크 기반 프로젝트를 간편하게 사용할 수 있도록 지원해주는 **라이브러리**

> 자주 사용되는 기본 설정들을 알아서 해결해줘서 복잡한 설정없이 쉽고 빠르게 스프링을 사용할 수 있다.


- 라이브러리 관리의 자동화

  - 스프링 부트의 Starter 라이브러리를 등록해서 라이브러리 의존성을 간단하게 관리한다.

- 라이브러리 버전 자동 관리

  - 현재 스프링 부트 버전에 맞는 모든 라이브러리들을 호환되는 버전으로 알아서 받아준다.

- 내장 Tomcat

  - 스프링 부트는 Tomcat을 내장하고 있어 `@SpringBootApplication`이 선언되어 있는 클래스의 main 메서드를 실행하는 것만으로 서버를 구동시킬 수 있다.

- 독립적으로 실행 가능한 JAR
  
  - 웹 프로젝트라면 WAR 파일로 패키징 해야 하지만 스프링 부트는 내장 Tomcat을 지원하기 때문에 JAR 파일로 패키징 해서 웹 애플리케이션을 실행시킬 수 있다.

<br>

## 📚 Reference
- https://bamdule.tistory.com/158
- https://sabarada.tistory.com/127
