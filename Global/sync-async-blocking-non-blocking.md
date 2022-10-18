# 동기 vs 비동기, 블로킹 vs 논블로킹

### 동기 vs 비동기

- 동기와 비동기는 프로세스의 **수행 순서** 보장에 대한 매커니즘
- **결과의 관점**

### 블로킹 vs 논블로킹

- 블로킹과 논블로킹은 처리되어야 하는 작업이 전체적인 **작업 흐름**을 막는 지에 대한 관점이다.
- **제어의 관점**

<br>

## 동기(Synchronous)

- 호출된 함수의 응답이 올 때까지 기다렸다가 다음 작업을 실행한다.
- 순서가 보장되며 안전하다.
- 해당 작업이 완료될 때까지 기다리는 시간 때문에 **느리다**.

![sync](https://i.imgur.com/bU21iLI.png)

<br>

## 비동기(Asynchronous)

- 호출된 함수의 응답을 기다리지 않고 다음 작업을 실행한다.
- 순서가 보장되지 않아 처리하기 까다롭다.
- 다른 작업이 완료될 때까지 기다리지 않아 **빠르다**.

![async](https://imgur.com/hAGCHFL.png)

> 작업 완료 여부에 신경쓰지 않는다.

<br>

## 블로킹(Blocking)

- 다른 함수를 호출하면 제어권을 넘겨준다.
- B 함수에게 제어권을 넘겨줘서 A 함수 실행은 잠시 멈춘다.

![Blocking](https://imgur.com/rSYHBTc.png)

<br>

## 논블로킹(Non-Blocking)

- 다른 함수를 호출해도 제어권은 그대로 자신이 갖는다.
- A 함수가 제어권을 계속 가지고 있어서 B 함수를 호출해도 계속 자기 코드를 실행한다.

![Non-Blocking](https://imgur.com/s6VDVc9.png)

<br>

## 📚 References

- 유튜브 - [[10분 테코톡] 🐰 멍토의 Blocking vs Non-Blocking, Sync vs Async](https://www.youtube.com/watch?v=oEIoqGd-Sns)
- 블로그 - [동기 & 비동기 / 블로킹 & 논블로킹 💯 완벽 이해하기](https://inpa.tistory.com/m/entry/%F0%9F%91%A9%E2%80%8D%F0%9F%92%BB-%EB%8F%99%EA%B8%B0%EB%B9%84%EB%8F%99%EA%B8%B0-%EB%B8%94%EB%A1%9C%ED%82%B9%EB%85%BC%EB%B8%94%EB%A1%9C%ED%82%B9-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC)
