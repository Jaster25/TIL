# 테스트 코드 적용하기(JUnit, TDD)
[유튜브 - 테스트 코드 적용하기 (JUnit, TDD) [ 스프링 부트 (Spring Boot) ]](https://www.youtube.com/watch?v=SFVWo0Z5Ppo) 강의 정리

<br>

## TTD에 대한 간단한 정리
- 테스트 주도 개발이라는 의미를 가짐
- 단순하게 표현하자면 테스트를 먼저 설계 및 구축 후 테스트를 통과할 수 있는 코드를 짜는 것
- 코드 작성 후 테스트를 진행하는 일반적인 방식과 다소 차이가 있음
- 애자일 개발 방식 중 하나
    - 코드 설계 시 원하는 단계적 목표에 대해 설정하여 진행하고자 하는 것에 대한 결정 방향의 갭을 줄이고자 함
    - 최초 목표에 맞춘 테스트를 구축하여 그에 맞게 코드를 설계하기 때문에 보다 적은 의견 충돌을 기대할 수 있음

<br>

## 테스트 코드 작성 목적
- 코드의 안정성을 높일 수 있음
- 기능을 추가하거나 변경하는 과정에서 발생할 수 있는 Side-Effect를 줄일 수 있음
- 해당 코드가 작성된 목적을 명확하게 표현할 수 있음
    - 코드에 불필요한 내용이 들어가는 것을 비교적 줄일 수 있음

<br>

## JUnit이란?
- Java 진영의 대표적인 Test Framework
- 단위 테스트(Unit Test)를 위한 도구를 제공
    > **단위 테스트**
    > 
    > - 코드의 특정 모듈이 의도된 대로 동작하는지 테스트하는 절차를 의미
    > - 모든 함수와 메서드에 대한 각각의 테스트 케이스를 작성하는 것
- 애노테이션 기반으로 테스트 지원
- 단정문(Assert)으로 테스트 케이스의 기댓값에 대해 수행 결과를 확인할 수 있음
- Spring Boot 2.2버전부터 JUnit 5 버전 사용
- JUnit 5는 크게 Jupiter, Platform, Vintage 모듈로 구성됨

<br>

## JUnit 모듈 설명
### JUnit Jupiter
- TestEngine API 구현체로 JUnit 5를 구현하고 있음
- 테스트의 실제 구현체는 별도 모듈 역할을 수행하는데, 그 모듈 중 하나가 Jupiter-Engine
- Jupiter API를 사용하여 작성한 테스트 코드를 발견하고 실행하는 역할을 수행
- 개발자가 테스트 코드를 작성할 때 사용됨 

### JUnit Platform
- 테스트를 실행하기 위한 뼈대
- Test를 발견하고 테스트 계획을 생성하는 TestEngine 인터페이스를 가지고 있음
- TestEngine을 통해 Test를 발견하고, 수행 및 결과를 보고함
- 각종 IDE 연동을 보조하는 역할 수행(콘솔 출력 등)
- Platform = TestEngine API + Console Launcher + JUnit 4 Based Runner + ...

### JUnit Vintage
- TestEngine API 구현체로 JUnit 3, 4를 구현하고 있음
- 기존 JUnit 3, 4 버전으로 작성된 테스트 코드를 실행할 때 사용됨
- Vintage-Engine 모듈을 포함하고 있음

<br>

## JUnit LifeCycle Annotation

JUnit 5는 아래와 같은 테스트 생명 주기를 갖고 있다.

| Annotation  | Description                                                       |
|-------------|-------------------------------------------------------------------|
| @Test       | 테스트용 메서드를 표현하는 애노테이션                             |
| @BeforeEach | 각 테스트 메서드가 시작되기 전에 실행되어야 하는 메서드를 표현    |
| @AfterEach  | 각 테스트 메서드가 시작된 후에 실행되어야 하는 메서드를 표현      |
| @BeforeAll  | 테스트 시작 전에 실행되어야 하는 메서드를 표현 (static 처리 필요) |
| @AfterAll   | 테스트 종류 후에 실행되어야 하는 메서드를 표현 (static 처리 필요) |

<br>

## JUnit Main Annotation

### @SpringBootTest
- 통합 테스트 용도로 사용됨
- `@SpringBootApplication`을 찾아가 하위의 모든 Bean을 스캔하여 로드함
- 그 후 Test용 Application Context를 만들어 Bean을 추가하고, MockBean을 찾아 교체함

### @ExtendWith
- JUnit 4에서 `@RunWith`로 사용되던 애노테이션이 `@ExtendWith`로 변경됨
- `@ExtendWith`는 메인으로 실행될 Class를 지정할 수 있음
- `@SpringBootTest`는 기본적으로 `@ExtendWith`가 추가되어 있음

### @WebMvcTest(Class명.class)
- ()에 작성된 클래스만 실제로 로드하여 테스트를 진행
- 매개변수를 지정해주지 않으면 `@Controller`, `@RestController`,`@RestControllerAdvice` 등 컨트롤러와 연관된 Bean이 모두 로드됨
- 스프링의 모든 Bean을 로드하는 `@SpringBootTest` 대신 컨트롤러 관련 코드만 테스트할 경우 사용

### @Autowired about MockBean
- Controller에 API를 테스트하는 용도인 MockMvc 객체를 주입 받음
- `perform()` 메서드를 활용하여 컨트롤러의 동작을 확인할 수 있음
    - `andExpect()`, `andDo()`, `andReturn()` 등의 메서드를 같이 활용함

### @MockBean
- 테스트할 클래스에서 주입 받고 있는 객체에 대해 가짜 객체를 생성해주는 애노테이션
- 해당 객체는 실제 행위를 하지 않음
- `given()` 메서드를 활용하여 가짜 객체의 동작에 대해 정의하여 사용할 수 있음

### @AutoConfiguration
- spring.test.mockmvc의 설정을 로드하면서 MockMvc의 의존성을 자동으로 주입
- MockMvc 클래스는 REST API 테스트를 할 수 있는 클래스

### @Import
- 필요한 클래스들을 Configuration으로 만들어 사용할 수 있음
- Configuration Component 클래스도 의존성 설정할 수 있음
- Import된 클래스는 주입으로 사용 가능

<br>

## 통합 테스트
- 통합 테스트는 여러 기능을 조합하여 전체 비즈니스 로직이 제대로 동작하는지 확인하는 것을 의미
- 통합 테스트의 경우, `@SpringBootApplication`을 사용하여 진행
    - `@SpringBootApplication`을 찾아가서 모든 Bean을 로드하게 됨
    - 이 방법을 대규모 프로젝트에서 사용할 경우, 테스트를 실행할 때마다 모든 빈을 스캔하고 로드하는 작업이 반복되어 매번 무거운 작업을 수행해야 함

<br>

## 단위 테스트
- 단위 테스트는 프로젝트에 필요한 모든 기능에 대한 테스트를 각각 진행하는 것을 의미
- 일반적으로 스프링 부트에서는 'org.springframework.boot:spring-boot-starter-test' 디펜던시만으로 의존성을 모두 가질 수 있음
### F.I.R.S.T 원칙
- Fast: 테스트 코드의 실행은 빠르게 진행되어야 함
- Independent: 독립적인 테스트가 가능해야 함
- Repeatable: 테스트는 매번 같은 결과를 만들어야 함
- Self-Validating: 테스트는 그 자체로 실행하여 결과를 확인할 수 있어야 함
- Timely: 단위 테스트는 비즈니스 코드가 완성되기 전에 구성하고 테스트가 가능해야 함
    - 코드가 완성되기 전부터 테스트가 따라와야 한다는 TDD의 원칙을 담고 있음

<br>

## 참고
- [유튜브 - 테스트 코드 적용하기 (JUnit, TDD) [ 스프링 부트 (Spring Boot) ]](https://www.youtube.com/watch?v=SFVWo0Z5Ppo)