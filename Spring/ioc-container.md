# IoC 컨테이너

### IoC(Inversion of Control, 제어의 역전)란?

제어권을 개발자가 아닌 외부에서 갖는 것을 의미한다.

> **IoC 분류**
>
> - 의존성 검색(DL, Dependency Lookup)
>   저장소에 저장되어 있는 Bean에 접근하기 위해 컨테이너가 제공하는 API를 이용하여 빈을 Lookup 하는 것
> - 의존성 주입(DI, Dependency Injection)
>   각 클래스간의 의존관계를 컨테이너가 연결해주는 것

<br>

### IoC 컨테이너란?

스프링 빈을 관리하고 제공해주며, **ApplicationContext 인터페이스를 구현**한 클래스의 오브젝트다.

> **컨테이너란?**
>
> 보통 인스턴스의 생명주기를 관리하며, 생성된 인스턴스들에게 추가적인 기능을 제공하도록 하는 것이다.

<br>

### IoC 컨테이너 종류

- BeanFactory
- ApplicationContext

#### BeanFactory

- 스프링 컨테이너의 최상위 인터페이스
- 스프링 빈 관리(생성, 조회, 반환 등)
- getBean() 메서드 정의

#### ApplicationContext

- BeanFactory를 상속받아서 모든 기능을 제공
- 스프링의 각종 부가 서비스를 제공
- 여러 구현 클래스 존재

<br>

## 참고

- https://dog-developers.tistory.com/12
