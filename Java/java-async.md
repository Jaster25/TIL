# Java 비동기 구현하기

자바에서 비동기 처리하는 방식을 정리한다.

<br>

## 요구사항

기다려야하는 작업(API 호출)을 동시에 여러번 호출한다.

### API 호출 샘플 코드

```java
@Slf4j
@RestController
public class PlusController {

    @GetMapping("/plus")
    public int plus(@RequestParam int param1, @RequestParam int param2) throws InterruptedException {
        Thread.sleep(100);
        return param1 + param2;
    }
}
```

```java
@Slf4j
@Service
public class PlusService {

    private final RestTemplate restTemplate = new RestTemplate();
    private static final String URL_FORMAT = "http://localhost:8080/plus?param1=%d&param2=%d";

    public int sendRequest(int param1, int param2) {
        final var startTime = System.currentTimeMillis();

        final var result = restTemplate.getForObject(String.format(URL_FORMAT, param1, param2), int.class);
        log.info("{} + {} = {}, Elapsed: {}", param1, param2, result, System.currentTimeMillis() - startTime);
        return result;
    }
}
```

<br>

## 1. Thread

```java
@Slf4j
@SpringBootApplication(scanBasePackages = "com.js.studyasync.common")
public class ThreadApplication {

    public static void main(String[] args) throws InterruptedException{
        final var context = SpringApplication.run(ThreadApplication.class, args);
        final PlusService service = context.getBean(PlusService.class);
        final var startTime = System.currentTimeMillis();
        final var random = new Random();

        for (int i = 0; i < 10; i++) {
            new Thread(
                    () -> service.sendRequest(random.nextInt(100), random.nextInt(100))
            ).start();
        }

        log.info("Main Finished, Elapsed: {}", System.currentTimeMillis() - startTime);
        Thread.sleep(3000);
        context.close();
    }
}
```

### 문제점

- 작업할 때마다 스레드가 생성된다.(스레드가 객체로 남아 메모리를 차지함)

<br>

## 2. ExecutorService

### Executor Service 구조
![executor](https://imgur.com/pn7T2oq.png)

### Executor Service 클래스 구조
![executor class](https://imgur.com/yd77rLS.png)

```java
@Slf4j
@SpringBootApplication(scanBasePackages = "com.js.studyasync.common")
public class ExecutorServiceApplication {

    public static void main(String[] args) throws InterruptedException {
        final var context = SpringApplication.run(ExecutorServiceApplication.class, args);
        final PlusService service = context.getBean(PlusService.class);
        final var startTime = System.currentTimeMillis();
        final var random = new Random();

        // 고정된 사이즈의 스레드풀 생성
        // final ExecutorService executorService = Executors.newFixedThreadPool(10);
        // 유동적인 사이즈의 스레드풀 생성
        final ExecutorService executorService = Executors.newCachedThreadPool();
        // 애플리케이션 특성에 맞게 선택하자

        for (int i = 0; i < 10; i++) {
            executorService.submit(
                    () -> service.sendRequest(random.nextInt(100), random.nextInt(100))
            );
        }

        log.info("Main Finished, Elapsed: {}", System.currentTimeMillis() - startTime);
        Thread.sleep(3000);
        executorService.shutdown();
    }
}
```

### 추가 요구사항
- 비동기 작업의 결과값을 받아오고 싶음

<br>

## 2. Future, CompletableFuture


<br>

## 📚 References

- [유튜브 - 개발자 장고](https://www.youtube.com/watch?v=CchV1XuEkSA)
