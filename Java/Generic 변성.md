# Generic 변성

[유튜브 - 프로그래밍 초식 - Generic 변성 by 최범균](https://www.youtube.com/watch?v=PtM44sO-A6g) 강의 정리

<br>

## Generic

```java
List<String> codes = new ArrayList<>();
codes.add("1");
codes.add(1);   // 컴파일 에러
```

- 클래스 내부에서 사용할 데이터를 외부에서 지정하는 기법
- 컴파일 시점에 타입 체크
- JDK 1.5 도입

<br>

## Generic과 하위 타입

#### 만약 Cage<Tiger>가 Cage<Animal>의 하위 타입이라면?

```java
public class Tiger extends Animal {}
public class Lion extends Animal {}

Cage<Tiger> ct = new Cage<Tiger>();
Cage<Animal> ca = ct; // 1. 이 작업이 된다면

ca.push(new Lion()); // 2. Lion도 Animal이므로 가능하다.

List<Tiger> tigers = ct.getAll(); // 3. Lion 리스트 반환
```

<br>

## 무변성(Invariant)

- A가 B의 상위 타입일 때 GenericType<A>가 GenericType<B>의 상위 타입이 아니면 변성 없음

```java
Animal animal = new Tiger(); // OK! Animal은 Tiger의 사우이 타입
Cage<Animal> ca = new Cage<Tiger>(); // 에러! Cage<Animal>이 Cage<Tiger>의 상위 타입 아님
```

<br>

## 무변성일 때 문제

#### 육식동물(Carnivore) 우리에 고기 먹이를 주는 사육사

```java
public class Animal {}
public class Carnivore extends Animal {}
public class Tiger extends Animal {}
public class Lion extends Animal {}
```

```java
public class Zookeeper {
    public void giveMeat(Cage<Carnivore> cage, Meat m) {
        ...
    }
}
```

```java
Zookeeper zk = new Zookeeper();
Cage<Tiger> ct = new Cage<>();
zk.giveMeat(ct, m); // 에러! Cage<Carnivore>가 Cage<Tiger>의 상위 타입이 아님
```

<br>

## ⭐ 공변(Covariant)

**A가 B의 상위 타입이고 T<A>가 T<B>의 상위 타입이면 공변**

- `extends` 사용해서 공변 처리 가능

```java
public class Zookeeper {
    public void giveMeat(Cage<? extends Carnivore> cage, Meat meat) {
        List<? extends Carnivore> cs = cage.getAll();
        ...
    }
}
```

```java
Zookeeper zk = new Zookeeper();

// Cage<? extends Carnivore> 타입에 Cage<Tiger> 할당 가능
Cage<Tiger> ct = new Cage<>();
zk.giveMeat(ct, someMeat);

// Cage<? extends Carnivore> 타입에 Cage<Lion> 할당 가능
Cage<Lion> cl = new Cage<>();
zk.giveMeat(cl, anyMeat);
```

<br>

## 공변에서 값 사용

공변에서 Generic 타입을 사용하는 메서드에 값 전달 안 됨

```java
Cage<Tiger> ct = new Cage<Tiger>();

Cage<? extends Carnivore> cage = ct;
cage.push(new Tiger()); // 에러!

// cage의 실제 타입이 Cage<Tiger>인지 알 수 없음
```

```java
public class Cage<T> {
    public void push(T animal) {
        ...
    }
}
```

<br>

## ⭐ 반공변(contravariant)

**A가 B의 상위 타입이고 T<A>가 T<B>의 하위 타입이면 반공변**

- `super` 사용해서 반공변 처리 가능

```java
Cage<Tiger> ct = new Cage<>();
// Cage<? super Tiger> 타입에 Cage<Tiger> 할당 가능
Cage<? super Tiger> ctt = ct;
ctt.push(new Tiger()); // OK, ctt는 최소 Cage<Tiger>나 그 상위 타입

Cage<Carnivore> ct2 = new Cage<>();
// Cage<? super Tiger> 타입에 Cage<Carnivore> 할당 가능
Cage<? super Tiger> ctt2 = ct2;
ctt2.push(new Tiger()); // OK, ctt2는 최소 Cage<Tiger>나 그 상위 타입
```

<br>

## PECS

> Producer-Extends, Consumer-Super
>
> Effective Java

#### 값을 제공하면 `extends`

```java
public void giveMeat(Cage<? extends Carnivore> cage) {
    // 값을 제공하면 extends
    List<Carnivores> cs = cage.getAll();
    ...
}
```

#### 값을 사용하면 `super`

```java
Cage<? super Tiger> ctt = ct;
// 값을 사용하면 super
ctt.push(new Tiger());
```

<br>

## 📚 References

- [유튜브 - 프로그래밍 초식 - Generic 변성 by 최범균](https://www.youtube.com/watch?v=PtM44sO-A6g)
