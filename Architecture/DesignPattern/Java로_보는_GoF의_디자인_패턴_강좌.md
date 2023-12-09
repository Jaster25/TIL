# Java로 보는 GoF의 디자인 패턴 강좌

[유튜브 - Java로 보는 GoF의 디자인 패턴 강좌](https://www.youtube.com/playlist?list=PLe6NQuuFBu7FhPfxkjDd2cWnTy2y_w_jZ)

<br>

## 1강. 시작하기

### 소프트웨어 개발 과정

- 요구사항 분석
- **설계**
- 구현
- 테스트
  반복되는 위 과정에서 **설계**를 효율적으로 하기 위해 디자인 패턴을 적용

### 디자인 패턴(Design Pattern)

- 소프트웨어 설계 방법
- 반복되는 패턴처럼 자주 나타나는 클래스 간의 **관계를 맺는 방법(설계)**

### 올바른 관계를 맺어야 하는 이유

1. 클래스는 최소한의 단위 기능을 가짐
2. 큰 기능은 이러한 단위 기능을 갖는 클래스들 간의 관계를 통해 개발됨
3. 꼭 필요한 것들만으로 구성된 최적화된 소프트웨어 개발이 용이함
4. 문제 발생 시 최소한의 코드 수정으로 유지보수가 용이함
5. 기존 기능에 영향을 주지 않고 새로운 기능 추가가 용이함

### GoF의 디자인 패턴

- 가장 유용하며 대표적인 디자인 패턴
- 4명의 개발자(Gang of Four)가 체계적으로 정리해 놓은 설계 방법
- **총 23개의 패턴으로 구성되며 생성 패턴(5개), 구조 패턴(7개), 행위 패턴(11개)으로 분류**

|     **생성**     | **구조**  |        **행위**         |
| :--------------: | :-------: | :---------------------: |
|  Factory Method  |  Adapter  |       Interpreter       |
| Abstract Factory |  Bridge   |     Template Method     |
|     Builder      | Composite | Chain of Responsibility |
|    Prototype     | Decorator |         Command         |
|    Singleton     |  Facade   |        Iterator         |
|                  | Flyweight |        Mediator         |
|                  |   Proxy   |         Memento         |
|                  |           |        Observer         |
|                  |           |        Strategy         |
|                  |           |         Visitor         |

_외우려고만 하지 말고 클래스들 간의 최적의 관계를 효과적으로 설계할 수 있는 하나의 사례로 이해하자._

<br>

## 📚 References

- [유튜브 - Java로 보는 GoF의 디자인 패턴 강좌](https://www.youtube.com/playlist?list=PLe6NQuuFBu7FhPfxkjDd2cWnTy2y_w_jZ)
