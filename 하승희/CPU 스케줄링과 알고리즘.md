# CPU 스케줄링과 알고리즘

## CPU 스케줄링

- 현재 레디 큐(Ready Queue)에 들어와 있는 CPU를 얻고자 하는 프로세스 중에서 어떤 프로세스에게 CPU를 줄 것인지 결정하는 메커니즘
- 여러 종류의 job(=process)이 섞여 있기 때문에 CPU 스케줄링이 필요하다.
    - I/O bound process
        - CPU를 잡고 계산하는 시간보다 I/O에 많은 시간이 필요한 job
    - CPU bound process
        - 계산 위주의 job

## CPU Scheduler & Dispatcher

- **CPU Scheduler**
    - Ready 상태의 프로세스 중에서 이번에 CPU를 줄 프로세스를 고른다.
- **Dispatcher**
    - CPU의 제어권을 CPU scheduler에 의해 선택된 프로세스에게 넘긴다.
    - 이 과정을 context switch(문맥 교환)라고 한다.
- CPU 스케줄링의 2가지 방식
    - `non-preemptive`(비선점형) 방식 : 자진 반납할 때까지 강제로 CPU를 빼앗지 않는 방법
    - `preemptive`(선점형) 방식 : 강제로 CPU를 빼앗는 방식

## Scheduling Criteria(CPU 스케줄링을 위한 성능 척도)

### 시스템 입장에서의 성능 척도

- **CPU utilization(이용률)**
    - 전체 시간 중에서 CPU가 일하는 시간의 비율
- **Throughput(처리량)**
    - 단위 시간당 완료되는 작업 수

### 프로그램 입장에서의 성능 척도

- **Turnaround time(소요시간, 반환시간)**
    - 프로세스가 완료된 시간에서 프로세스가 도착한 시간을 뺀 시간
    - 즉, 어떤 프로세스가 완료될 때까지 걸린 시간
- **Waiting time(대기 시간)**
    - 레디 큐(Ready Queue)에서 기다린 시간의 합
- **Response time(응답 시간)**
    - CPU를 사용하러 들어와서 처음 CPU를 사용하기까지 걸리는 시간. 즉, 프로세스가 준비 상태에서 최초로 CPU를 얻을 때까지 걸리는 시간
    - 처음 실행되는 시간에서 프로세스가 도착한 시간을 뺀 시간

⇒ CPU는 굉장히 비싼 자원이므로 이용률과 처리량은 높을수록 좋고, 시간은 짧을수록 좋다!

## Scheduling Algorithms

### 1. FCFS(First-Come First-Served)

- 처음 들어온 프로세스가 처음으로 실행된다.
    - ex) 은행 창구의 번호표순, 화장실 대기줄
- 비선점형(non-preemptive) 스케줄링
    - 들어온 작업이 무엇이든 먼저 들어온 작업이 있다면, 그 작업이 수행된 뒤에 새로 들어온 작업이 수행된다.
    - 따라서 효율적이지 않다.

> ⚠️ convoy effect 문제


- 비선점 스케줄링(nonpreemptive scheduling)에서 작업량이 많아 시간이 오래 걸리는 프로세스가 CPU를 일단 차지해 버리면 다른 모든 프로세스들은 그 프로세스가 CPU를 내놓을 때까지 기다려야 한다.
- 실행시간이 긴 프로세스가 앞에 있으면, 뒤에 있는 프로세스들의 Turnaround time이 길어진다.

### 2. SJF(Shortest-Job-First)

- CPU burst time이 가장 짧은 프로세스를 제일 먼저 스케줄
- 각 프로세스의 다음번 CPU burst time을 가지고 스케줄링에 활용한다.
- 두 가지 방식
    - **Non-preemptive**
        - 일단 CPU를 잡으면 이번 CPU burst가 완료될 때까지 CPU를 선점(preemption) 당하지 않는다
    - **Preemptive**
        - 현재 수행중인 프로세스의 남은 burst time보다 더 짧은 CPU burst time을 가지는 새로운 프로세스가 도착하면 CPU를 빼앗긴다.
        - 이 방법을 Shortest-Remaining-Time-FIrst(SRTF)라고도 부른다.
- 평균 대기 시간을 최소화하는 알고리즘(preemptive 방식일 때)

> ⚠️ Starvation(기아 현상) 문제

- 실행 시간이 긴 프로세스가 자원을 계속 할당받지 못하는 문제
- 실행 시간이 긴 프로세스가 대기하는 상황에서, 실행 시간이 짧은 프로세스가 Ready queue에 계속 들어오면, 실행 시간이 긴 프로세스는 계속 자원을 할당받지 못하는 문제가 생긴다.
- SJF는 극단적으로 CPU 사용시간이 짧은 job을 선호한다. 따라서 CPU 사용시간이 긴 프로세스는 영원히 서비스를 못 받을 수도 있다.

> ⚠️ CPU 사용 시간을 미리 알 수 없는 문제

- 다음번 CPU burst time을 어떻게 알 수 있는가?
    - 프로그램은 input data를 받아 실행되기도 하고, if 분기문처럼 branch가 일어나기도 하는 등 매번 CPU burst time을 미리 파악할 수 없다.
    - 과거의 CPU burst time을 이용해서 예측(estimate)만 가능하다.
    - 예측 시간이 제일 적은 프로세스에게 CPU를 할당하는 SJF

### 3. SRTF(Shortest-Remaining-Time-First)

- 선점형(Preemptive) 스케줄링
    - 현재 수행중인 프로세스의 남은 burst time보다 더 짧은 CPU burst time을 가지는 새로운 프로세스가 도착하면 CPU를 빼앗긴다.

> ⚠️ Starvation(기아 현상) 문제

- 실행 시간이 긴 프로세스가 자원을 계속 할당받지 못하는 문제
- 실행 시간이 긴 프로세스가 대기하는 상황에서, 실행 시간이 짧은 프로세스가 Ready queue에 계속 들어오면, 실행 시간이 긴 프로세스는 계속 자원을 할당받지 못하는 문제가 생긴다.

### 4. Priority scheduling

- 우선순위가 제일 높은 프로세스에게 CPU를 할당하는 스케줄링
- 두 가지 방식
    - **Non-preemptive**
        - 일단 CPU를 잡으면, 더 높은 우선순위의 CPU가 도착하더라도 이번 CPU burst가 완료될 때까지 CPU를 선점(preemption) 당하지 않는다.
    - **Preemptive**
        - 현재 수행중인 프로세스보다 우선순위가 더 높은 프로세스가 도착하면 CPU를 빼앗긴다.
- SJF는 일종의 Priority scheduling이다
    - SJF에서 우선순위는 예측되는 CPU 사용시간이다.
    - `priority = predicted next CPU burst time`

> ⚠️ Starvation(기아 현상) 문제

- 우선순위가 낮은 프로세스가 지나치게 오래 기다려서 영원히 CPU를 얻지 못할 수도 있다.
- 해결법: **Aging(노화)**
    - Ready queue에 있는 프로세스에 나이를 부여하는 방법
    - 특정 프로세스가 얼마나 오래
    - 아무리 우선순위가 낮은 프로세스라도, 오래 기다리면 우선순위를 높여주는 것

### 5. Round Robin(RR)

- 각 프로세스는 동일한 크기의 **할당 시간(time quantum)**을 가진다
    - 일반적으로 10-100 milliseconds
- 할당 시간이 지나면 프로세스는 선점(preempted)당하고 ready queue의 제일 뒤에 가서 다시 줄을 선다.
- Ready queue에 `n개의 프로세스`가 있고 `할당 시간이 q time unit`인 경우, 각 프로세스는 최대 q time unit 단위로 CPU 시간의 1/n을 얻는다.

  ⇒ 즉, 어떤 프로세스도 (n-1)q time unit 이상 기다리지 않는다.

  ⇒ n개 중에서 자기 자신을 빼고(n-1), (n-1)개가 q만큼의 시간을 쓰고 나면 적어도 내 차례가 한 번은 돌아온다.

- Performance
    - q large(할당 시간을 아주 크게 잡았을 때) ⇒ FCFS
    - q small(할당 시간을 아주 적게 잡았을 때) ⇒ context switch 오버헤드가 커진다.
    - 따라서 적당한 규모의 time quantum을 주는 것이 바람직하다.
- 일반적으로 SJF보다 평균 turnaround time이 길지만, response time은 더 짧다.

---

### Reference

- [반효경 교수님 운영체제 강의](http://www.kocw.net/home/cview.do?cid=3646706b4347ef09)