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

### Thread 코드

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

### 실행 결과

```log
[  restartedMain]: Main Finished, Elapsed: 0
[io-8080-exec-10]: Initializing Spring DispatcherServlet 'dispatcherServlet'
[io-8080-exec-10]: Initializing Servlet 'dispatcherServlet'
[io-8080-exec-10]: Completed initialization in 1 ms
[       Thread-9]: 37 + 35 = 72, Elapsed: 174
[      Thread-10]: 97 + 52 = 149, Elapsed: 174
[      Thread-12]: 5 + 7 = 12, Elapsed: 174
[      Thread-11]: 92 + 6 = 98, Elapsed: 174
[       Thread-7]: 94 + 18 = 112, Elapsed: 174
[       Thread-8]: 36 + 44 = 80, Elapsed: 174
[      Thread-16]: 39 + 90 = 129, Elapsed: 174
[      Thread-15]: 52 + 62 = 114, Elapsed: 174
[      Thread-14]: 72 + 98 = 170, Elapsed: 174
[      Thread-13]: 63 + 38 = 101, Elapsed: 174
```

### 문제점

- 작업할 때마다 스레드가 생성된다.(스레드가 객체로 남아 메모리를 차지함)

<br>

## 2. ExecutorService

### Executor Service 구조

![executor](https://imgur.com/pn7T2oq.png)

### Executor Service 클래스 구조

![executor class](https://imgur.com/yd77rLS.png)

### ExecutorService 코드

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
        final ExecutorService executorService = Executors.newFixedThreadPool(10);
        // 유동적인 사이즈의 스레드풀 생성
        // final ExecutorService executorService = Executors.newCachedThreadPool();
        // 애플리케이션 특성에 맞게 선택하자

        for (int i = 0; i < 10; i++) {
            executorService.submit(
                    () -> service.sendRequest(random.nextInt(100), random.nextInt(100))
            );
        }

        log.info("Main Finished, Elapsed: {}", System.currentTimeMillis() - startTime);
        Thread.sleep(3000);
        executorService.shutdown();
        context.close();
    }
}
```

### 실행 결과

```log
[  restartedMain]: Main Finished, Elapsed: 1


[nio-8080-exec-4]: Initializing Spring DispatcherServlet 'dispatcherServlet'
[nio-8080-exec-4]: Initializing Servlet 'dispatcherServlet'
[nio-8080-exec-4]: Completed initialization in 0 ms
[ool-2-thread-10]: 2 + 82 = 84, Elapsed: 183
[pool-2-thread-1]: 4 + 52 = 56, Elapsed: 183
[pool-2-thread-4]: 25 + 79 = 104, Elapsed: 183
[pool-2-thread-7]: 33 + 71 = 104, Elapsed: 183
[pool-2-thread-6]: 48 + 24 = 72, Elapsed: 183
[pool-2-thread-3]: 68 + 93 = 161, Elapsed: 183
[pool-2-thread-2]: 39 + 24 = 63, Elapsed: 183
[pool-2-thread-5]: 14 + 15 = 29, Elapsed: 183
[pool-2-thread-8]: 54 + 81 = 135, Elapsed: 183
[pool-2-thread-9]: 92 + 86 = 178, Elapsed: 183
[pool-2-thread-8]: 55 + 18 = 73, Elapsed: 102
[pool-2-thread-3]: 47 + 19 = 66, Elapsed: 105
[pool-2-thread-1]: 60 + 91 = 151, Elapsed: 105
[pool-2-thread-7]: 28 + 62 = 90, Elapsed: 105
[pool-2-thread-5]: 12 + 77 = 89, Elapsed: 105
[pool-2-thread-4]: 94 + 8 = 102, Elapsed: 106
[pool-2-thread-6]: 35 + 54 = 89, Elapsed: 107
[pool-2-thread-2]: 10 + 8 = 18, Elapsed: 107
[ool-2-thread-10]: 37 + 90 = 127, Elapsed: 107
[pool-2-thread-9]: 35 + 87 = 122, Elapsed: 107
```

### 추가 요구사항

- 비동기 작업의 결과값을 받아오고 싶음

<br>

## 3. Future, CompletableFuture

### Future 코드

```java
@Slf4j
@SpringBootApplication(scanBasePackages = "com.js.studyasync.common")
public class FutureApplication {

    public static void main(String[] args) throws ExecutionException, InterruptedException {
        final var context = SpringApplication.run(FutureApplication.class, args);
        final PlusService service = context.getBean(PlusService.class);
        final var startTime = System.currentTimeMillis();
        final var random = new Random();

        final ExecutorService executorService = Executors.newFixedThreadPool(5);
        for (int i = 0; i < 10; i++) {
            final Future<Integer> future = executorService.submit(
                    () -> service.sendRequest(random.nextInt(100), random.nextInt(100))
            );

            // get()를 사용하는 순간 비동기의 의미가 사라진다.
            log.info(String.valueOf(future.get()));
        }

        log.info("Main Finished, Elapsed: {}", System.currentTimeMillis() - startTime);
        Thread.sleep(3000);
        executorService.shutdown();
        context.close();
    }
}
```

### 실행 결과

```log
[  restartedMain]: Started FutureApplication in 0.597 seconds (process running for 0.788)
[nio-8080-exec-1]: Initializing Spring DispatcherServlet 'dispatcherServlet'
[nio-8080-exec-1]: Initializing Servlet 'dispatcherServlet'
[nio-8080-exec-1]: Completed initialization in 0 ms
[pool-2-thread-1]: 95 + 84 = 179, Elapsed: 157
[  restartedMain]: 179
[pool-2-thread-2]: 70 + 28 = 98, Elapsed: 111
[  restartedMain]: 98
[pool-2-thread-3]: 31 + 35 = 66, Elapsed: 107
[  restartedMain]: 66
[pool-2-thread-4]: 11 + 80 = 91, Elapsed: 107
[  restartedMain]: 91
[pool-2-thread-5]: 73 + 82 = 155, Elapsed: 107
[  restartedMain]: 155
[pool-2-thread-1]: 54 + 49 = 103, Elapsed: 107
[  restartedMain]: 103
[pool-2-thread-2]: 0 + 65 = 65, Elapsed: 107
[  restartedMain]: 65
[pool-2-thread-3]: 54 + 69 = 123, Elapsed: 107
[  restartedMain]: 123
[pool-2-thread-4]: 63 + 31 = 94, Elapsed: 110
[  restartedMain]: 94
[pool-2-thread-5]: 0 + 51 = 51, Elapsed: 103
[  restartedMain]: 51


[  restartedMain]: Main Finished, Elapsed: 1128
```

### 문제점

- API 호출 결과를 Future.get() 메서드로 받아 오는 순간 비동기의 의미가 사라진다.
  - Log에서 Main finished의 경과 시간을 보자.

<br>

### 추가 요구사항

- API 호출 결과를 비동기로 조회하자.

### CompletableFuture 코드

```java
@Slf4j
@SpringBootApplication(scanBasePackages = "com.js.studyasync.common")
public class CompletableFutureApplication {

    public static void main(String[] args) throws InterruptedException {
        final var context = SpringApplication.run(CompletableFutureApplication.class, args);
        final PlusService service = context.getBean(PlusService.class);
        final var startTime = System.currentTimeMillis();
        final var random = new Random();

        final ExecutorService executorService = Executors.newFixedThreadPool(5);
        for (int i = 0; i < 10; i++) {
            CompletableFuture.supplyAsync(
                            () -> service.sendRequest(random.nextInt(100), random.nextInt(100))
                            , executorService)
                    .thenAccept(t -> log.info(String.valueOf(t)));
        }

        log.info("Main Finished, Elapsed: {}", System.currentTimeMillis() - startTime);
        Thread.sleep(3000);
        executorService.shutdown();
        context.close();
    }
}
```

### 실행 결과

```log
[  restartedMain]: Main Finished, Elapsed: 1


[nio-8080-exec-1]: Initializing Spring DispatcherServlet 'dispatcherServlet'
[nio-8080-exec-1]: Initializing Servlet 'dispatcherServlet'
[nio-8080-exec-1]: Completed initialization in 1 ms
[pool-2-thread-1]: 23 + 6 = 29, Elapsed: 170
[pool-2-thread-4]: 47 + 25 = 72, Elapsed: 170
[pool-2-thread-3]: 77 + 52 = 129, Elapsed: 170
[pool-2-thread-5]: 72 + 17 = 89, Elapsed: 170
[pool-2-thread-2]: 44 + 58 = 102, Elapsed: 170
[pool-2-thread-2]: 102
[pool-2-thread-3]: 129
[pool-2-thread-1]: 29
[pool-2-thread-4]: 72
[pool-2-thread-5]: 89
[pool-2-thread-1]: 83 + 99 = 182, Elapsed: 110
[pool-2-thread-2]: 42 + 57 = 99, Elapsed: 110
[pool-2-thread-1]: 182
[pool-2-thread-4]: 90 + 13 = 103, Elapsed: 110
[pool-2-thread-5]: 39 + 89 = 128, Elapsed: 110
[pool-2-thread-3]: 65 + 74 = 139, Elapsed: 110
[pool-2-thread-4]: 103
[pool-2-thread-3]: 139
[pool-2-thread-5]: 128
[pool-2-thread-2]: 99
```

### 결과

- API 호출 결과 조회도 비동기 처리가 된 것이 확인된다.

<br>

### 추가 요구사항

- 여러 개의 비동기 작업의 결과물을 가지고 다시 비동기 호출하자.

### 연쇄 API 호출 비동기 코드

```java
@Slf4j
@SpringBootApplication(scanBasePackages = "com.js.studyasync.common")
public class CompletableFutureApplication2 {

    public static void main(String[] args) throws InterruptedException {
        final var context = SpringApplication.run(CompletableFutureApplication2.class, args);
        final PlusService service = context.getBean(PlusService.class);
        final var startTime = System.currentTimeMillis();
        final var random = new Random();

        final ExecutorService executorService = Executors.newFixedThreadPool(5);
        final var completableFuture1 = CompletableFuture.supplyAsync(
                () -> service.sendRequest(random.nextInt(100), random.nextInt(100))
                , executorService
        );

        completableFuture1.thenCompose(
            t -> CompletableFuture.supplyAsync(
                            () -> service.sendRequest(t, random.nextInt(100))
                            , executorService
            )
        ).thenAccept(t -> log.info(String.valueOf(t)));

        log.info("Main Finished, Elapsed: {}", System.currentTimeMillis() - startTime);
        Thread.sleep(3000);
        executorService.shutdown();
        context.close();
    }
}
```

### 실행 결과

```log
[  restartedMain]: Main Finished, Elapsed: 0


[nio-8080-exec-1]: Initializing Spring DispatcherServlet 'dispatcherServlet'
[nio-8080-exec-1]: Initializing Servlet 'dispatcherServlet'
[nio-8080-exec-1]: Completed initialization in 1 ms
[pool-2-thread-1]: 62 + 72 = 134, Elapsed: 156
[pool-2-thread-2]: 134 + 94 = 228, Elapsed: 107
[pool-2-thread-2]: 228
```

<br>

### 추가 요구사항

- 여러 개의 비동기 작업의 결과물을 혼합하여 다시 비동기 호출하자.

### 코드

```java
@Slf4j
@SpringBootApplication(scanBasePackages = "com.js.studyasync.common")
public class CompletableFutureApplication3 {

    public static void main(String[] args) throws InterruptedException {
        final var context = SpringApplication.run(CompletableFutureApplication3.class, args);
        final PlusService service = context.getBean(PlusService.class);
        final var startTime = System.currentTimeMillis();
        final var random = new Random();

        final ExecutorService executorService = Executors.newFixedThreadPool(5);
        final var completableFuture1 = CompletableFuture.supplyAsync(
                () -> service.sendRequest(random.nextInt(100), random.nextInt(100))
                , executorService
        );

        final var completableFuture2 = CompletableFuture.supplyAsync(
                () -> service.sendRequest(random.nextInt(100), random.nextInt(100))
                , executorService
        );

        completableFuture1.thenCombine(
                        completableFuture2,
                        (t1, t2) -> service.sendRequest(t1, t2)
                        // service::sendRequest Lambda
                )
                .thenAccept(t -> log.info(String.valueOf(t)));

        log.info("Main Finished, Elapsed: {}", System.currentTimeMillis() - startTime);
        Thread.sleep(3000);
        executorService.shutdown();
        context.close();
    }
}
```

### 실행 결과

```log
[  restartedMain]: Main Finished, Elapsed: 1


[nio-8080-exec-1]: Initializing Spring DispatcherServlet 'dispatcherServlet'
[nio-8080-exec-1]: Initializing Servlet 'dispatcherServlet'
[nio-8080-exec-1]: Completed initialization in 0 ms
[pool-2-thread-1]: 14 + 96 = 110, Elapsed: 172
[pool-2-thread-2]: 36 + 78 = 114, Elapsed: 172
[pool-2-thread-1]: 110 + 114 = 224, Elapsed: 108
[pool-2-thread-1]: 224
```

<br>

## 4. Spring Async

- 스레드를 관리할 수 있다.
- 여러 개의 비동기 작업의 결과물을 가지고 다시 비동기 호출할 수 있다.
- 비동기 관리 코드와 비즈니스 코드를 분리한다.

### 코드

```java
@Slf4j
@SpringBootApplication(scanBasePackages = "com.js.studyasync.common")
public class SpringAsyncApplication {

    public static void main(String[] args) throws InterruptedException, ExecutionException {
        final var context = SpringApplication.run(SpringAsyncApplication.class, args);
        final PlusAsyncService service = context.getBean(PlusAsyncService.class);

        final var startTime = System.currentTimeMillis();
        final var random = new Random();

        for (int i = 0; i < 10; i++) {
            service.sendRequest(random.nextInt(100), random.nextInt(100));
        }

//        final var completableFuture1 = service.sendRequestCompletable(random.nextInt(100), random.nextInt(100));
//        final var completableFuture2 = service.sendRequestCompletable(random.nextInt(100), random.nextInt(100));
//
//        completableFuture1.thenCombine(completableFuture2,
//                                       (t1, t2) -> service.sendRequestCompletable(t1, t2))
//                          .thenCompose(c -> c)
//                          .thenAccept(t -> log.info(String.valueOf(t)));
        log.info("Main Finished, Elapsed: {}", System.currentTimeMillis() - startTime);
        context.close();
    }

    @Configuration
    @EnableAsync
    public static class AsyncConfiguration {

        @Bean(destroyMethod = "shutdown")
        public Executor asyncExecutor() {
            final var executor = new ThreadPoolTaskExecutorBuilder()
                    .corePoolSize(10)
                    .maxPoolSize(10)
                    .threadNamePrefix("CustomTP-")
                    .build();
            executor.initialize();

            return executor;
        }
    }

    @Service
    @RequiredArgsConstructor
    public static class PlusAsyncService {

        private final PlusService plusService;
        private final Executor asyncExecutor;

        // 해당 bean에 해당하는 Executor를 가져와 비동기로 실행한다.
        @Async("asyncExecutor")
        public void sendRequest(int param1, int param2) {
            plusService.sendRequest(param1, param2);
        }

        public CompletableFuture<Integer> sendRequestCompletable(int param1, int param2) {
            return CompletableFuture.supplyAsync(() -> plusService.sendRequest(param1, param2));
        }
    }
}
```

### 실행 결과

```log
[  restartedMain]: Main Finished, Elapsed: 2
[nio-8080-exec-8]: Initializing Spring DispatcherServlet 'dispatcherServlet'
[nio-8080-exec-8]: Initializing Servlet 'dispatcherServlet'
[nio-8080-exec-8]: Completed initialization in 0 ms
[    CustomTP-10]: 32 + 99 = 131, Elapsed: 189
[     CustomTP-5]: 89 + 96 = 185, Elapsed: 189
[     CustomTP-3]: 50 + 37 = 87, Elapsed: 190
[     CustomTP-7]: 60 + 62 = 122, Elapsed: 189
[     CustomTP-2]: 26 + 74 = 100, Elapsed: 190
[     CustomTP-9]: 34 + 53 = 87, Elapsed: 189
[     CustomTP-8]: 44 + 87 = 131, Elapsed: 189
[     CustomTP-6]: 71 + 68 = 139, Elapsed: 189
[     CustomTP-1]: 49 + 98 = 147, Elapsed: 190
[     CustomTP-4]: 33 + 66 = 99, Elapsed: 190
```

<br>

## 📚 References

- [유튜브 - 개발자 장고](https://www.youtube.com/watch?v=CchV1XuEkSA)
