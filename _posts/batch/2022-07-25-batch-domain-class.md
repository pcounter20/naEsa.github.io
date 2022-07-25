---
layout: post
title: batch 도메인 클래스 이해
subtitle: batch 도메인 클래스 이해 및 배치 실행 흐름
categories: spring batch
tags: [spring batch]
---

#### spring batch 도메인 이해

> Spring Batch 초기화의 간략한 이해

---
![img01](https://github.com/pcounter20/naEsa.github.io/blob/main/_posts/image/batch/classDomain/img_01.jpeg?raw=true)

---

Job 은 interface로 이를 구현한 AbstractJob 추상클래스가 존재한다.  또한 실제 JobLauncher 이 매개변수로 받아 실행하는 job 은 추상 클래스 AbstractJob 을 상속 받아 구현한, Simple Job 과 FlowJob 을 실행한다.

---
![img02](https://github.com/pcounter20/naEsa.github.io/blob/main/_posts/image/batch/classDomain/img_02.jpeg?raw=true)

---

#### Job Instance / Parameters

위 그림에서 Job은 사용자(개발자) 가 지정한 Job 에 관한 명세서이며, 실제
 고유한 식별 가능 객체는 JobInstance 가 있다. 쉽게 말하자면, 예로 하루에 한번 정해진 시간에 실행되는 배치 Job 은 우리가 작성한 Job 에 따라 동작하며 이 JobInstance 가 매일 생성되는 것으로 이해하면 된다.

 그림에서 보다시피 jobLauncher가 job을 run 할 때 매개변수로 job과 parameters 를 받는데 여기서 job은 위에서 언급한 AbstractJob 을 상속받은 SimpleJob 과 FlowJob 중에 하나이며, parameters 는 JobInstance 를 생성할 때 받는 매개변수로 이해하면 된다.

---
![img03](https://github.com/pcounter20/naEsa.github.io/blob/main/_posts/image/batch/classDomain/img_03.jpeg?raw=true)

---

위의 그림에서 JobRepository는 현재 실행한 job,parameters 에 대해 같은 jobInstance 가 존재하는지 확인한다. 그래서 jobParamters 까지 같은 job 이 이미 있으면 기존 job Instance 를 반환하고 에러를 발생시킨다.

 없다면 새로운 jobInstance 를 생성한다. 위와 같이 currentTime 을 파라미터로 넘기면 왠만하면 jobInstance 가 같은 객체가 생성되기 어렵지 않을까.

 #### JobExecution

 > 만약 매일 1회 실행하도록 되어있는 Job이 실패했다면?  같은 파라미터라 재실행이 안되면?

 ---
![img04](https://github.com/pcounter20/naEsa.github.io/blob/main/_posts/image/batch/classDomain/img_04.jpeg?raw=true)

---

먼저 JobExecution 에 대해서 짧게 이해하자면, JobInstance 가 한번 실행되면 생성되는 객체로 실행중 발생한 정보를 가지고 있다. 속성들 중에서 유심히 봐야할 속성인 실행 상태 결과이다. JobExecution 은 Failed, Completed 등의 상태 정보를 갖는다. 

위 그림에서 유심히 봐야할 3가지가 있다.

1. new JobExecution 생성 : jobKey 와 jobName을 가지고 해당 jobInstance 가 존재하는지 확인 후 JobInstance 를 새로 생성하는 부분에서 그 실행 정보인 JobExecution 생성한다. **성공 흐름에서 jobExecution 객체는 completed 속성**을 갖는다.

2. 기존 객체를 반환하는 flow 에서 Status 를 검사 : 만약 해당 생성되어있는 JobInstance의 **jobExecution 객체 상태 정보가 Failed 로 끝났으면, 해당 Job을 다시 실행**할 수 있다. 즉, 실패한 job을 실행할 수 있는 조건이다. flow 에서 보면 **new JonInstance는 하지 않는다**. 오직 jobExecution 만 새로 생성하는데, 이걸 보면 **jobInstance 와 execution 의 관계가 1:N 임을 알 수 있다**.

3. Status? flow Completed : 만약 Status 가 **Completed** 이면, 해당 JobInstance 는 **이미 생성**되어 있으며, 완전히 문제없이 끝났기 때문에, **새로 생성할 수 없게된다.** 그러면 호출단까지 위의 예외가 올라오며 종료된다.

#### Step / StepExecution

> 하나의 Job은 하나 이상의 Step으로 구성되어 있다.

---
![img05](https://github.com/pcounter20/naEsa.github.io/blob/main/_posts/image/batch/classDomain/img_05.jpeg?raw=true)

---

Job 은 하나 이상의 Step으로 이루어져 있으며, 해당 Job은 지정한 순서에 따라 Step 을 순차적으로 실행한다. 해당 Step은 내부에 실제적 Biz Logic인 Tasklet을 가지고 있으며, Tasklet 은 interface 로 사용자가 재정의 해서 사용할 수 있다.

---
![img06](https://github.com/pcounter20/naEsa.github.io/blob/main/_posts/image/batch/classDomain/img_06.jpeg?raw=true)

---

> Job에서 하나의 Step이 실패한다면?

예상대로 Job 이 실패로 돌아간다.  트랜잭션에서 논리트랜잭션 전부가 커밋되어야 물리 트랜잭션이 성공하는것처럼.  그림에서 보듯 stepB가 실패했다면, **Job 은 최종적으로 실패**로 되며, JobExecution 과 Step'B'의 Execution 상태정보는 Failed 로 기록된다. 그럼 실행되지 않은 StepC는? **Execution 객체가 생성되지 않는다.** 즉, StepExecution은 실행된 Step에만 한해 생성된다.

그럼 위 상황에서 **재실행**을 한다면 어떻게 될까?  JobExecution 의 상태 정보가 Failed 으로 Execution이 하나 재 생성된다. StepA의 Execution은 생성될까? 아니다. SpringBatch는 성공한 Step은 Skip해서 StepB 부터 재시도 하고, Execution을 생성한다. 즉, 총 4개의 StepExecution이 생성된다.

#### StepContribution / Context

tasklet 의 실행에 따라 변경 사항을 버퍼링 한 후 StepExecution 의 상태를 업데이트하는 객체로 StepContribution이 있다. 해당 객체는 Commit 직전에 apply 함으로서 변경사항을 적용한다. 해당 객체는 read,write,filter count,skip,ExitStatus,stepExecution 등의 속성을 가지고있다. 해당 사항은 후에 자세히 정리한다.

ExecutionContext. Context는 문맥이다. 이 이전에 Step 과 Job의 속성 공유 범위에 대해 이해할 필요가 있다.
* Job: Job 은 각 JobExecution 에 저장되며, **Job끼리 공유가 안되며, Job내의 Step간 공유가 가능**하다.
* Step: Step 은 StepExecution 에 저장되며, **Step간 공유가 불가능**하다.

즉, 위 그림에서 stepA의 정보를 stepB가 참조해서 사용할 수 있다는 이야기이다. stepE -> jobE -> executionContext 에서 stepA가 저장한 데이터를 get할 수 있다. (executionContext는 map)

#### JobRepositoy / JobLauncher

JobRepositoy는 배치 작업중 정보를 저장한다. Job 수행 시간, 종료 시간, 실행 횟수, 결과 등 모든 메타데이터를 저장하며, 여타 Repo와 같이 JobLauncher, Job, Step 등의 CRUD 기능을 처리한다.

사실 **@EnableBatchProcessing을 달아주면, 자동으로 JobRepositoy 가 빈으로 생성된다.** BatchConfigurer interface 를 구현하거나, BasicBatchConfigurer 를 상속받아 JobRepository를 커스터마이징 할 수 있다.

* JDBC 방식 - JobRepositoryFactoryBean: 내부적으로 Aop기술을 통해 트랜잭션 처리를 해주며, isolation level 은 최고수준 설정이 default. 해당 factory를 통해 jobRepo 를 생성하면, .setXX를 통해 많은 설정을 할 수 있다.

* inMemory: h2 DB와 같이 test에서 특별히 db에 저장하지 않고 imbedded방식으로 하고 싶을때 사용가능하다.

JobLauncher은 맨 처음부터 나온 객체로 Job을 실행시키는 역할을 한다.
Job과 parameters 를 인자로 받아 실행하며 client에게 최종적으로 **JobExecution을 반환**한다.

스프링부트에서는 JobLauncherApplicationRunner가 자동으로 JobLauncher를 실행시킨다. Job의 실행에는 **동기적 실행**과 **비동기적 실행**이 있다.

---
![img07](https://github.com/pcounter20/naEsa.github.io/blob/main/_posts/image/batch/classDomain/img_07.jpeg?raw=true)

---

동기적 실행 방식은 과정이 종료되고 나서, ExitStatus가 finished 혹은, failed 로 정해지고 나서, JobExecution을 반환하는 반면, 비동기적 실행방식은 실행 후 JobExecution을 즉시 반환하면서 flow를 진행한다. **Http 요청, 사용자의 Controller**에 의해 응답할 경우 **비동기 실행**이 빠르니 주로 사용되고, **스케쥴러**나 정해진 시간에 정해진 횟수만 정확히 실행할 경우 **동기적 실행**이 주로 사용된다.

**해당 포스팅은 아래의 강의를 정리했습니다**

[정수원 강사님의 스프링 배치](https://www.inflearn.com/course/스프링-배치/dashboard)