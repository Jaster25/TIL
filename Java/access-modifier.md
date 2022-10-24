# 접근 제어자(Access Modifier)

접근제어자는 한 클래스 내부의 변수나 메서드의 **사용 범위**를 결정하는 것으로,

**내부 정보를 외부로부터 숨겨서 결합도를 낮추기 위해 사용한다.**

<br>

## 접근 제어자 종류

> public > protected > default(package-private) > private

정보 은닉과 캡슐화를 위해 **모든 클래스와 멤버의 접근성을 가능한 한 좁혀야 한다.**

| 접근 제어자 | 같은 클래스 | 같은 패키지 | 자손 클래스 | 전체 |
| ----------- | ----------- | ----------- | ----------- | ---- |
| public      | O           | O           | O           | O    |
| protected   | O           | O           | O           | X    |
| default     | O           | O           | X           | X    |
| private     | O           | X           | X           | X    |

### Public

모든 곳에서 접근 가능

### Protected

해당 패키지와 해당 클래스를 상속 받은 경우 접근 가능

### Default(package-private)

해당 패키지 내에서만 접근 가능

### Private

해당 클래스에서만 접근 가능

<br>

## 관련 키워드

### 결합도(Coupling)

어떤 모듈이 다른 모듈에 의존하는 것을 의미한다.

_결합도는 낮을수록, 응집도(Cohesion)는 높을수록 좋다._

<br>

### 캡슐화(Encapsulation)

OOP의 특징 중 하나로, 필요하지 않은 클래스 내부 구현 내용을 외부로부터 숨겨서 위험을 최소화하는 것

<br>

### 정보 은닉(Information hiding)

외부에서 접근하지 못하도록 클래스의 세부 정보를 숨기는 것

<br>

### 캡슐화와 정보 은닉 차이점

- 캡슐화는 관련된 변수와 메서드를 묶어주는 것
- 정보 은닉은 캡슐 속에 있는 변수와 메서드를 숨기는 것

<br>

## 📚 References

- 유튜브 - [[개발자 면접 질문] 자바 - 접근제어자](https://www.youtube.com/watch?v=mEsCTQ1pB-Q)
- 블로그 - [캡슐화와 정보 은닉](https://frontierdev.tistory.com/93)
