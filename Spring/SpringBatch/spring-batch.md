# 스프링 배치

기존 배치를 스프링 배치로 전환하는 작업이 필요하여 설계부터 개발, 배포까지 전체적인 프로세스를 경험해봤다. 개발 과정에서 새로 배운 것과 아직 해결하지 못한 것이 있어 기록해본다.

## 스프링 배치 구조

![Spring-batch-arch](https://imgur.com/lA3qsYE.png)

#### JobRepository

배치를 수행하는데 필요한 모든 정보를 갖고있는 저장소

#### JobLauncher

Job을 실행시키는 역할을 담당한다.

Job과 JobParameters를 매개변수로 받고 배치를 수행한 후 JobExecution을 반환한다.

#### ExecutionContext

스프링 배치에서 지원하는 key/value 형식의 공유 객체로 Job을 수행하며 Step 간에 데이터 공유가 필요할 때 사용한다.

Job과 Step에 대해서 각각의 ExecutionContext를 가진다.

Job과 Step에 대한 상태도 저장하고 있다.

#### Job

배치 처리 과정을 하나의 단위로 만들어놓은 최상위 개념

한 개 이상의 Step으로 구성되어야 한다.

#### Step

배치 작업의 독립적인 단위로 아래 두 가지 방식으로 구현된다.(같이 사용할 수 없다.)

- Tasklet
- Chunk

Tasklet 방식은 처리할 데이터가 많지 않고 구현할 로직이 단순할 때 권장된다.

Chunk 방식은 데이터들을 덩어리(chunk) 단위로 처리하며 ItemReader, ItemProcessor(optional), ItemWriter 이렇게 세가지 단계로 구성된다. 그렇기에 대규모 데이터를 처리하거나 로직이 복잡한 경우에 권장된다.

**Step - Tasklet 구현 방식**

```java
@Bean
public Step taskletStep() {
    return stepBuilderFactory.get("Step name")
            .tasklet((contribution, chunkContext) -> {

                // 로직

                return RepeatStatus.FINISHED;
            })
            .build();
}
```

**Step - Chunk 구현 방식**

```java
@Bean
public Step chunkStep() {
    return stepBuilderFactory.get("Step name")
            .<Object, Object>chunk(CHUNK_SIZE)
            .reader(itemReader())
            .processor(itemProcessor())
            .writer(itemWriter())
            .build();
}
```

<br>

### ❓ Chunk 기반 구현 방식에서 ExecutionContext을 다루는 법

Job 시작 시(`@Value` 등을 사용해) 초기 값을 가져오는 것 뿐만이 아니라, Tasklet 방식에서 chunkContext를 다루듯 편하게 저장하고 가져오는 방식을 아직 못 찾았다.

**Tasklet 방식에서 ExecutionContext을 다루는 코드 샘플**

```java
@Bean
@JobScope
public Step step() {
    return stepBuilderFactory.get("Step name")
            .tasklet((contribution, chunkContext) -> {
                // 데이터 공유
                chunkContext.getStepContext().getStepExecution().getJobExecution()
                        .getExecutionContext().put("key", "value");

                return RepeatStatus.FINISHED;
            })
            .build();
}
```

> 아래 링크에서 ItemWriter 인터페이스를 직접 구현하는 방식을 봤지만, 현재 프로젝트 구조에는 알맞지 않아서 고려해보진 않았다.
>
> 🔗 https://stackoverflow.com/questions/14949985/how-to-get-jobparameter-and-jobexecutioncontext-in-the-itemwriter

<br>

## 📚 References

- https://www.baeldung.com/spring-batch-tasklet-chunk
- https://gngsn.tistory.com/187
- https://zzang9ha.tistory.com
- https://velog.io/@cho876
