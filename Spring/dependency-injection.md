# 의존성 주입(DI, Dependency Injection)

외부에서 두 객체의 관계를 결정해주는 디자인 패턴으로, 유연성은 확보하고 결합도는 낮추기 위해 사용되는 스프링의 핵심 기술 중 하나입니다.

#### 스프링 핵심 기술

- IoC/**DI**
- AOP
- PSA

<br>

## 의존성 주입 방법

1. **생성자 주입**
2. 수정자 주입(setter, 기타 메서드 주입)
3. 필드 주입

> **생성자 주입** 방법이 가장 권장됩니다.

## 생성자 주입(Constructor Injection)

```java
public class Test {

  private final MemberService memberService;

  // @Autowired: 생략 가능
  public MemberService(MemberService memberService) {
    thie.memberService = memberService;
  }
}
```

- 호출 시점에 단 1회 호출 보장
- _생성자가 1개만 있을 경우 `@Autowired` 생략 가능_

## 수정자 주입(setter, 기타 메서드 주입)

```java
public class Test {

  private MemberService memberService;

  @Autowired
  public void setMemberService(MemberService memberService) {
    thie.memberService = memberService;
  }
}
```

- 주입받는 객체가 변경될 가능성이 있는 경우 사용

- 해당 메서드가 **public**으로 열려있다.

  처음 이후엔 접근할 일이 없는데도 접근이 가능해 위험하다.

- `@Autowired` 주입 대상이 존재하지 않는 경우 **NullPointerException 발생**

## 필드 주입

```java
public class Test {

  @Autowired
  private MemberService memberService;
}
```

- 코드가 간결하지만 외부에서 접근이 불가능하다는 단점이 있다. 즉, 테스트가 어렵다.
- DI 프레임워크에 의존해서 좋지 않다.

<br>

## 생성자 주입을 사용해야 하는 이유

1. 객체의 불변성 확보

   의존 관계 변경이 필요한 상황은 드물기 때문에 실수 방지 차원에서 수정 가능성을 배제하여 불변성을 보장하는 것이 좋다.

2. 테스트 코드 작성 편의

   테스트가 프레임워크에 의존하는 것은 좋지 않다. 생성자 주입 방법 아닌 코드는 순수 자바 코드로 단위 테스트 하는 것이 어렵다.

3. final 키워드 작성 및 Lombok과의 결합

   - `final` 키워드 사용 가능
   - `@RequiredArgsConstrucor` 사용 가능

4. 순환 참조 에러 방지

   ```java
   new MemberService( new UserService(new MemberService(new UserService( …
   ```

<br>

## 참고

- https://mangkyu.tistory.com/125
- https://mangkyu.tistory.com/150
