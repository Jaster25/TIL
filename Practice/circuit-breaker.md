# 서킷 브레이커 실습

몇 달 전, 외부 서비스에 장애가 발생하면서 우리 서비스까지 연쇄적으로 영향을 받아 장애가 발생한 적이 있었다. 그때 팀원들이 서킷 브레이커에 대해 얘기를 나눴었는데 나는 가볍게 개념만 익히고 넘어갔었다.

서킷 브레이커(Circuit Breaker)는 전기 회로 차단기처럼 **외부 서비스 호출 실패율이 일정 기준 이상 올라가면 호출을 강제로 차단하여 더 이상의 장애 전파를 막는 패턴**이다. 이를 통해 장애가 외부에서 내부로 확산되는 것을 방지하고, 시스템을 안정적으로 유지할 수 있다.

그동안은 개념만 대충 알고 있었는데, 이번 기회에 실습을 통해 동작 원리와 로그 변화를 직접 확인해 봤다.



## 실습 코드

#### 코드 구조
```bash
circuit-breaker
├─ src/main/java/com/jaster25/circuitbreaker
│  ├─ api
│  │  ├─ dto
│  │  │  └─ OrderDto.java
│  │  └─ OrderController.java
│  ├─ config
│  │  └─ Resilience4jConfig.java
│  ├─ external
│  │  └─ ExternalController.java
│  ├─ service
│  │  └─ OrderService.java
│  └─ CircuitBreakerApplication.java
└─ src/main/resources
   └─ application.yml
```


#### application.yml
```java
server:
  port: 8080

resilience4j:
  circuitbreaker:
    instances:
      orderService:
        slidingWindowType: COUNT_BASED
        slidingWindowSize: 10
        minimumNumberOfCalls: 10
        failureRateThreshold: 50
        waitDurationInOpenState: 5s
        permittedNumberOfCallsInHalfOpenState: 3

```
- slidingWindowType: 서킷브레이커가 실패율을 계산하는 기준
    - COUNT_BASED: 최근 호출 횟수를 기반
    - TIME_BASED: 최근 시간 기반
- slidingWindowSize: 실패율 계산에 사용할 최근 호출 기록 개수
- failureRateThreshold: 실패율 임계값(%)
- waitDurationInOpenState: 서킷브레이커가 OPEN 상태로 유지되는 시간
- permittedNumberOfCallsInHalfOpenState: HALF_OPEN 상태에서 허용되는 호출 횟수


#### OrderController.java
```java
package com.jaster25.circuitbreaker.controller;

import com.jaster25.circuitbreaker.dto.OrderDto;
import io.github.resilience4j.circuitbreaker.annotation.CircuitBreaker;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

@RestController
public class OrderController {

    private final RestTemplate restTemplate = new RestTemplate();

    @GetMapping("/order")
    @CircuitBreaker(name = "orderService", fallbackMethod = "fallbackOrder")
    public OrderDto placeOrder() {
        String response = restTemplate.getForObject("http://localhost:8080/external", String.class);
        return new OrderDto("Order completed: " + response);
    }

    public OrderDto fallbackOrder(Throwable t) {
        return new OrderDto("Order failed - fallback executed");
    }
}
```


#### OrderService.java
```java
package com.jaster25.circuitbreaker.service;

import com.jaster25.circuitbreaker.api.dto.OrderDto;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.cloud.client.circuitbreaker.CircuitBreaker;
import org.springframework.cloud.client.circuitbreaker.CircuitBreakerFactory;
import org.springframework.stereotype.Service;
import org.springframework.web.client.RestTemplate;

@Service
public class OrderService {

    private static final Logger log = LoggerFactory.getLogger(OrderService.class);

    private final CircuitBreakerFactory<?, ?> cbFactory;
    private final RestTemplate restTemplate = new RestTemplate();

    public OrderService(CircuitBreakerFactory<?, ?> cbFactory) {
        this.cbFactory = cbFactory;
    }

    public OrderDto order() {
        CircuitBreaker cb = cbFactory.create("orderService");
        return cb.run(
                () -> {
                    String res = restTemplate.getForObject("http://localhost:8080/external/process", String.class);
                    return new OrderDto(true, "External response: " + res);
                },
                ex -> {
                    log.error("Fallback called due to: {}", ex.toString());
                    return new OrderDto(false, "Fallback: external service unavailable");
                }
        );
    }
}
```

#### ExternalController.java
```java
package com.jaster25.circuitbreaker.controller;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.Random;

@RestController
public class ExternalController {

    private static final Logger log = LoggerFactory.getLogger(ExternalController.class);
    private final Random random = new Random();

    @GetMapping("/external")
    public String externalApi() {
        if (random.nextDouble() < 0.7) { // 70% 확률로 실패
            log.error("External API failure");
            throw new RuntimeException("External API failed");
        }
        log.info("External API success");
        return "External API success";
    }
}
```

#### Resilience4jConfig.java
```java
package com.jaster25.circuitbreaker.config;

import io.github.resilience4j.circuitbreaker.CircuitBreaker;
import io.github.resilience4j.core.registry.EntryAddedEvent;
import io.github.resilience4j.core.registry.RegistryEventConsumer;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class Resilience4jConfig {

    private static final Logger log = LoggerFactory.getLogger(Resilience4jConfig.class);

    @Bean
    public RegistryEventConsumer<CircuitBreaker> cbRegistryEventConsumer() {
        return new RegistryEventConsumer<>() {
            @Override
            public void onEntryAddedEvent(EntryAddedEvent<CircuitBreaker> event) {
                var cb = event.getAddedEntry();
                var name = cb.getName();
                cb.getEventPublisher()
                        .onStateTransition(e -> log.warn("[CB:{}] state transition: {} -> {} (event: {})",
                                name, e.getStateTransition().getFromState(),
                                e.getStateTransition().getToState(), e.getEventType()))
                        .onError(e -> log.warn("[CB:{}] call failed (recorded as failure): {}",
                                name, e.getThrowable().toString()))
                        .onCallNotPermitted(e -> log.warn("[CB:{}] call not permitted (OPEN)", name))
                        .onSuccess(e -> log.info("[CB:{}] call success", name));
            }
            @Override public void onEntryRemovedEvent(io.github.resilience4j.core.registry.EntryRemovedEvent<CircuitBreaker> event) { }
            @Override public void onEntryReplacedEvent(io.github.resilience4j.core.registry.EntryReplacedEvent<CircuitBreaker> event) { }
        };
    }
}
```
- 서킷브레이커 상태 추적을 위한 설정


#### OrderDto.java
```java
package com.jaster25.circuitbreaker.api.dto;

import java.time.Instant;

public class OrderDto {
    private boolean success;
    private String message;
    private Instant timestamp;

    public OrderDto() {}
    public OrderDto(boolean success, String message) {
        this.success = success;
        this.message = message;
        this.timestamp = Instant.now();
    }

    public boolean isSuccess() { return success; }
    public void setSuccess(boolean success) { this.success = success; }

    public String getMessage() { return message; }
    public void setMessage(String message) { this.message = message; }

    public Instant getTimestamp() { return timestamp; }
    public void setTimestamp(Instant timestamp) { this.timestamp = timestamp; }
}
```


## 실습 결과

#### API 호출 샘플
```bash
curl -X GET http://localhost:8080/orders
```

#### API 응답 - 정상
```json
{
    "success": true,
    "message": "External response: 외부 API 성공",
    "timestamp": "2025-08-10T13:20:10.682852Z"
}
```
#### API 응답 - 실패
```json
{
    "success": false,
    "message": "Fallback: external service unavailable",
    "timestamp": "2025-08-10T13:48:32.713907Z"
}
```

#### 서킷브레이커 관련 로그

![circuit-breaker-log](https://imgur.com/Kz3OgEf.png)
