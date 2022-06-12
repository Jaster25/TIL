# 스프링 빈(Spring Bean)

**Spring IoC 컨테이너가 관리하는 자바 객체를 Bean이라고 합니다.**

기존 자바에서는 Class를 생성하고 `new`를 사용하여 직접 생성하고 사용했습니다.

스프링에서는 직접 생성한 객체가 아니라, **스프링에 의하여 관리당하는 자바 객체**를 사용합니다.

## IoC 컨테이너에 Bean 등록 방법

1. **Component Scanning**
2. **빈 설정 파일에 직접 빈 등록**

> **어노테이션(Annotation)이란?**
>
> 사전상으로는 주석의 의미지만, 자바에서는 소스 코드에 추가하여 사용할 수 있는 메타데이터의 일종입니다.

### **Component Scanning**

`@ComponentScan`과 `@Component`를 사용해서 빈을 등록하는 방법입니다.

`@ComponentScan` 어노테이션은 어느 지점부터 컴포넌트를 찾을지 정하는 역할을 하고,

`@Component` 어노테이션은 빈으로 등록할 클래스를 정하는 역할을 합니다.

```java
@Component
public class Hello {
    public Hello() {
       System.out.println("Hello");
    }
}
```

> 직접 작성한 클래스를 빈에 등록할 수 있다.

### 빈 설정 파일에 직접 빈 등록

`@Configuration`과 `@Bean`을 사용하여 스프링 빈을 직접 등록할 수 있습니다.

```java
@Configuration
public class HelloConfiguration {
    @Bean
    public void hello() {
       System.out.println("Hello");
    }
}
```

환경설정과 유연한 빈 등록이 필요할 상황에는 `@Configuration`과 `@Bean` 어노테이션을,

그 외의 일반적인 상황에서는 `@Component` 어노테이션을 사용해서 빈을 등록하자.

## 참고

- https://atoz-develop.tistory.com/entry/Spring-스프링-빈Bean의-개념과-생성-원리
- https://galid1.tistory.com/494
- https://melonicedlatte.com/2021/07/11/232800.html
