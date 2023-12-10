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

_외우려고만 하지 말고 클래스들 간 최적의 관계를 효과적으로 설계할 수 있는 하나의 사례로 이해하자._

<br>

## 2강. Iterator

> 반복자 패턴

Aggregator: 동일한 데이터의 집합

### Aggregator 종류

- Array
- Linked List
- Tree
- Graph
- Table(DBMS)
- ...

구성 데이터를 얻는 방법을 통일된 하나의 방법으로 가져오기 위한 패턴

### 클래스 다이어그램

클래스 다이어그램: 클래스 간의 관계를 나타내는 다이어그램으로 UML에서 정의한 다이어그램

![class-diagram](https://imgur.com/IUgcFhN.png)

### 실습 코드

```java
public interface Iterator {
    boolean next();
    Object current();
}
```

```java
public interface Aggregator {
    Iterator iterator();
}
```

```java
public class Item {
    private String name;
    private int cost;

    public Item(String name, int cost) {
        this.name = name;
        this.cost = cost;
    }

    @Override
    public String toString() {
        return "Item{" +
                "name='" + name + '\'' +
                ", cost=" + cost +
                '}';
    }
}
```

```java
public class Array implements Aggregator {
    private Item[] items;

    public Array(Item[] items) {
        this.items = items;
    }

    public Item getItem(int index) {
        return items[index];
    }

    public int getCount() {
        return items.length;
    }

    @Override
    public Iterator iterator() {
        return new ArrayIterator(this);
    }
}
```

```java
public class ArrayIterator implements Iterator {
    private Array array;
    private int index;

    public ArrayIterator(Array array) {
        this.array = array;
        this.index = -1;
    }

    @Override
    public boolean next() {
        index++;
        return index < array.getCount();
    }

    @Override
    public Object current() {
        return array.getItem(index);
    }
}
```

```java
public class MainEntry {
    public static void main(String[] args) {
        Item[] items = {
                new Item("CPU", 1000),
                new Item("Keyboard", 2000),
                new Item("Mouse", 3000),
                new Item("HDD", 50),
        };

        Array array = new Array(items);
        Iterator iterator = array.iterator();

        while (iterator.next()) {
            Item item = (Item) iterator.current();
            System.out.println(item);
        }
    }
}
```

```
> Task :MainEntry.main()
Item{name='CPU', cost=1000}
Item{name='Keyboard', cost=2000}
Item{name='Mouse', cost=3000}
Item{name='HDD', cost=50}
```

### Iterator 패턴 핵심

개발자는 다양한 데이터 구조를 파악하지 않아도 표준화된 API를 통해 구성 데이터를 참조할 수 있게 된다.

<br>

## 📚 References

- [유튜브 - Java로 보는 GoF의 디자인 패턴 강좌](https://www.youtube.com/playlist?list=PLe6NQuuFBu7FhPfxkjDd2cWnTy2y_w_jZ)
