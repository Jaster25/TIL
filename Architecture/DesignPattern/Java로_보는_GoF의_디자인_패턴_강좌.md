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

### 실습 클래스 다이어그램

![class-diagram](https://imgur.com/IUgcFhN.png)

클래스 다이어그램: 클래스 간의 관계를 나타내는 다이어그램으로 UML에서 정의한 다이어그램

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

## 3강. Strategy

> 전략 패턴

어떤 하나의 기능을 구성하는 특정 부분을 실행 중에 다른 것으로 효과적으로 변경할 수 있는 패턴
(필요할 때 전략을 바꿀 수 있는)

### 실습 클래스 다이어그램

![strategy](https://imgur.com/RaG06UW.png)

### 실습 코드

```java
public class SumPrinter {
    void print(SumStrategy strategy, int n) {
        System.out.printf("The sum of 1 - %d: ", n);
        System.out.println(strategy.get(n));
    }
}
```

```java
public interface SumStrategy {
    int get(int n);
}
```

```java
public class SimpleSumStrategy implements SumStrategy {
    @Override
    public int get(int n) {
        int sum = n;
        for (long i = 1; i < n; i++) {
            sum += i;
        }
        return sum;
    }
}
```

```java
public class GaussSumStrategy implements SumStrategy {
    @Override
    public int get(int n) {
        return (n + 1) * n / 2;
    }
}
```

```java
public class MainEntry {
    public static void main(String[] args) {
        SumPrinter cal = new SumPrinter();

        cal.print(new SimpleSumStrategy(), 10);
        cal.print(new GaussSumStrategy(), 10);
    }
}
```

```
> Task :MainEntry.main()
The sum of 1 - 10: 55
The sum of 1 - 10: 55
```

### Strategy 패턴 핵심

하나의 기능에 대해서 서로 다른 방식의 구현을 실행 중에 변경할 수 있는 패턴

<br>

## 4강. Template

어떤 기능에 대해서 실행되어야 할 각 단계는 정해져있으나, 각각의 세부 구현은 상황에 맞게 다르게 구현할 수 있도록 하는 패턴

### 실습 클래스 다이어그램

![template](https://imgur.com/t4y8Lfu.png)

### 실습 코드

```java
import java.util.ArrayList;

public class Article {
    private String title;
    private ArrayList<String> content;
    private String footer;

    public Article(String title, ArrayList<String> content, String footer) {
        this.title = title;
        this.content = content;
        this.footer = footer;
    }

    public String getTitle() {
        return title;
    }

    public ArrayList<String> getContent() {
        return content;
    }

    public String getFooter() {
        return footer;
    }
}
```

```java
public abstract class DisplayArticleTemplate {
    protected Article article;

    public DisplayArticleTemplate(Article article) {
        this.article = article;
    }

    public final void display() {
        title();
        content();
        footer();
    }

    protected abstract void title();
    protected abstract void content();
    protected abstract void footer();
}
```

```java
import java.util.ArrayList;

public class SimpleDisplayArticle extends DisplayArticleTemplate {
    public SimpleDisplayArticle(Article article) {
        super(article);
    }

    @Override
    protected void title() {
        System.out.println(article.getTitle());
    }

    @Override
    protected void content() {
        ArrayList<String> content = article.getContent();
        for (String s : content) {
            System.out.println(s);
        }
    }

    @Override
    protected void footer() {
        System.out.println(article.getFooter());
    }
}
```

```java
import java.util.ArrayList;

public class CaptionDisplayArticle extends DisplayArticleTemplate {
    public CaptionDisplayArticle(Article article) {
        super(article);
    }

    @Override
    protected void title() {
        System.out.println("TITLE: " + article.getTitle());
    }

    @Override
    protected void content() {
        System.out.println("CONTENT");
        ArrayList<String> content = article.getContent();
        for (String s : content) {
            System.out.println("    " + s);
        }
    }

    @Override
    protected void footer() {
        System.out.println("FOOTER: " + article.getFooter());
    }
}
```

```java
public class MainEntry {
    public static void main(String[] args) {
        String title = " 디자인 패턴";
        ArrayList<String> content = new ArrayList<>();
        content.add("1장. 디자인 패턴이란");
        content.add("2장. 디자인 패턴 종류");
        String footer = "2023.12.12";

        Article article = new Article(title, content, footer);

        System.out.println("[CASE-1]");
        DisplayArticleTemplate template1 = new SimpleDisplayArticle(article);
        template1.display();

        System.out.println("[CASE-2]");
        DisplayArticleTemplate template2 = new CaptionDisplayArticle(article);
        template2.display();
    }
}
```

```
> Task :MainEntry.main()
[CASE-1]
 디자인 패턴
1장. 디자인 패턴이란
2장. 디자인 패턴 종류
2023.12.12
[CASE-2]
TITLE:  디자인 패턴
CONTENT
    1장. 디자인 패턴이란
    2장. 디자인 패턴 종류
FOOTER: 2023.12.12
```

<br>

## 5강. Adapter

Adapter: 원하는 형태로 변환해 주는 장치

### 실습 클래스 다이어그램

![Adapter](https://imgur.com/6awVUai.png)

> Tiger 클래스는 코드를 변경할 수 없어 그대로 사용해야 하는 상황

### 실습 코드

```java
public abstract class Animal {
    protected String name;

    public Animal(String name) {
        this.name = name;
    }

    public abstract void sound();
}
```

```java
public class Dog extends Animal {
    public Dog(String name) {
        super(name);
    }

    @Override
    public void sound() {
        System.out.println(name + " Barking");
    }
}
```

```java
public class Cat extends Animal {
    public Cat(String name) {
        super(name);
    }

    @Override
    public void sound() {
        System.out.println(name + " Meow");
    }
}
```

```java
public class Tiger {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    void roar() {
        System.out.println("Growl");
    }
}

```

```java
public class TigerAdapter extends Animal {
    private Tiger tiger;

    public TigerAdapter(String name) {
        super(name);

        tiger = new Tiger();
        tiger.setName("name");
    }

    @Override
    public void sound() {
        System.out.print(tiger.getName() + " ");
        tiger.roar();
    }
}
```

```java
import java.util.ArrayList;
import java.util.List;

public class User {

    public static void main(String[] args) {
        List<Animal> animals = new ArrayList<>();
        animals.add(new Dog("댕댕이"));
        animals.add(new Cat("냥이1"));
        animals.add(new Cat("냥이2"));

        // animals.add(new Tiger("사나운냥이"));
        animals.add(new TigerAdapter("사나운냥이"));

        animals.forEach(Animal::sound);
    }
}

```

```
> Task :User.main()
댕댕이 Barking
냥이1 Meow
냥이2 Meow
name Growl
```

변경할 수 없는 클래스를 원하는 형태의 인터페이스나 클래스로 사용하고자 할 때 Adapter 클래스를 도입하여 사용할 수 있다.

<br>

## 6강. Bridge

기능 계층과 구현 계층의 분리로 시스템의 확장성과 유지보수성을 높이는 패턴
![BridgePattern](https://imgur.com/flx8KfV.png)

### 실습 클래스 다이어그램

![bridge-class](https://imgur.com/z59fUpz.png)

### 실습 코드

```java
public class Draft {
    private String title;
    private String author;
    private String[] contents;

    public String getTitle() {
        return title;
    }

    public String getAuthor() {
        return author;
    }

    public String[] getContents() {
        return contents;
    }

    public Draft(String title, String author, String[] contents) {
        this.title = title;
        this.author = author;
        this.contents = contents;
    }

    public void print(Display display) {
        display.title(this);
        display.author(this);
        display.contents(this);
    }
}
```

```java
public interface Display {
    void title(Draft draft);
    void author(Draft draft);
    void contents(Draft draft);
}
```

```java
public class SimpleDisplay implements Display {

    @Override
    public void title(Draft draft) {
        System.out.println(draft.getTitle());
    }

    @Override
    public void author(Draft draft) {
        System.out.println(draft.getAuthor());
    }

    @Override
    public void contents(Draft draft) {
        for (String content : draft.getContents()) {
            System.out.println(content);
        }
    }
}
```

```java
public class CaptionDisplay implements Display {
    @Override
    public void title(Draft draft) {
        System.out.println("제목: " + draft.getTitle());
    }

    @Override
    public void author(Draft draft) {
        System.out.println("저자: " + draft.getAuthor());
    }

    @Override
    public void contents(Draft draft) {
        System.out.println("내용: ");
        for (String content : draft.getContents()) {
            System.out.println("    " + content);
        }
    }
}
```

```java
public class Publication extends Draft {
    private String publisher;
    private int cost;

    public Publication(String title, String author, String[] contents, String publisher, int cost) {
        super(title, author, contents);
        this.publisher = publisher;
        this.cost = cost;
    }

    private void printPublicationInfo() {
        System.out.println("#" + publisher + " $" + cost);
    }

    @Override
    public void print(Display display) {
        super.print(display);
        printPublicationInfo();
    }
}
```

```java
public class MainEntry {
    public static void main(String[] args) {
        String title = "복원된 지구";
        String author = "김형준";
        String[] contents = {
                "플라스틱 사용의 감소와",
                "바다 생물 어획량 감소로 인하여",
                "지구는 복원되었다."
        };

        Draft draft = new Draft(title, author, contents);
        Display display1 = new SimpleDisplay();
        draft.print(display1);

        Display display2 = new CaptionDisplay();
        draft.print(display2);


        String publisher = "북악출판";
        int cost = 200;

        Publication publication = new Publication(title, author, contents, publisher, cost);
        publication.print(display1);
    }
}
```

### 핵심

새로운 기능에 대해서 기존 클래스 변경 없이 추가할 수 있게 해주는 패턴

<br>

## 7강. Singleton

하나의 클래스 타입에서 오직 하나의 객체만이 생성되도록 보장하는 패턴

### 실습 클래스 다이어그램

![SingletonPattern](https://imgur.com/nspI0v1.png)

### 실습 코드

```java
public class King {

    // 생성자는 private으로 선언하여 나 자신 이외에는 아무도 생성 못하도록 한다.
    private King() {}

    private static King self = null;

    // synchronized: 멀티스레드에서도 해당 메서드를 호출할 때 문제가 없도록 동기화하기 위함
    public synchronized static King getInstance() {
        if (self == null) {
            self = new King();
        }
        return self;
    }

    public void say() {
        System.out.println("I am only one.");
    }
}
```

```java
public class MainEntry {

    public static void main(String[] args) {
        // 외부에서는 생성자를 생성할 수 없다.
        // King king = new King();

        King king = King.getInstance();

        king.say();

        King king2 = King.getInstance();
        if (king == king2) {
            System.out.println("same object");
        } else {
            System.out.println("different object");
        }
    }
}
```

```
> Task :MainEntry.main()
I am only one.
same object
```

<br>

## 8강. Flyweight

### Flyweight Pattern의 목적

- 어떤 객체를 사용하기 위해 매번 생성하지 않고 한번만 생성하고 다시 필요해질 때는 이전에 생성된 객체를 재사용할 수 있다.
- 객체를 생성할 때 많은 자원이 소모될 경우 플라이웨이트 패턴을 적용하여 훨씬 적은 자원만으로 필요한 객체를 재사용할 수 있다.

### 실습 클래스 다이어그램

![flyweight](https://imgur.com/pJRKxgO.png)

### 실습 코드

```java
public class Digit {
    private final List<String> data = new ArrayList<>();

    public Digit(String filename) {
        FileReader fr = null;
        BufferedReader br = null;
        try {
            fr = new FileReader(filename);
            br = new BufferedReader(fr);

            for (int i = 0; i < 8; i++) {
                data.add(br.readLine());
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (fr != null) fr.close();
                if (br != null) br.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    public void print(int x, int y) {
        for (int i = 0; i < 8; i++) {
            String line = data.get(i);
            System.out.printf("%c[%d;%df", 0x1B, y + i, x);
            System.out.print(line);
        }
    }
}
```

```java
public class DigitFactory {

    private Digit[] pool = new Digit[] {
      null, null, null
    };

    public Digit getDigit(int n) {
        if (pool[n] != null) {
            return pool[n];
        } else {
            String fileName = String.format("./%d.txt", n);
            Digit digit = new Digit(fileName);
            pool[n] = digit;
            return digit;
        }
    }
}
```

```java
public class Number {

    private final List<Digit> digitList = new ArrayList<>();

    public Number(int number) {
        String strNumber = Integer.toString(number);
        int len = strNumber.length();

        DigitFactory digitFactory = new DigitFactory();
        for (int i = 0; i < len; i++) {
            int n = Character.getNumericValue(strNumber.charAt(i));
            Digit digit = digitFactory.getDigit(n);
            digitList.add((digit));
        }
    }

    public void print(int x, int y) {
        int cntDigits = digitList.size();
        for (int i = 0; i < cntDigits; i++) {
            Digit digit = digitList.get(i);
            digit.print(x + (i * 8), y);
        }
    }
}
```

```java
public class MainEntry {

    public static void main(String[] args) {
        Number number = new Number(102);
        number.print(5, 5);
    }
}
```

### ❓ Singleton 패턴과의 차이점

- Flyweight 패턴은 불변, Singleton 패턴은 가변적
- Singleton 패턴은 오직 하나의 인스턴스를 허용, Flyweight 패턴은 각각의 고유한 상태를 갖는 여러 인스턴스를 허용

<br>

## 📚 References

- [유튜브 - Java로 보는 GoF의 디자인 패턴 강좌](https://www.youtube.com/playlist?list=PLe6NQuuFBu7FhPfxkjDd2cWnTy2y_w_jZ)
