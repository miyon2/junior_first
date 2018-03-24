## 1. Scheduling Metrics
* Turnaround time : 실행시간을 모르면 잘 만들기가 어렵다.
* Response time : RR의 경우 좋지만, turnaround time관점에서는 좋지 못하다.

## 2. Crux of the Problem
어떻게 하면 실행시간을 모르는 상태에서도 turnaround time, response time 둘 다 좋게 해결해 줄 수 있을까?

## 3. Multi-Level Feedback Queue(MLFQ)
각 Queue는 우선순위를 갖는다. Priority가 높은 job은 높은 Priority를 가진 Queue에 들어가지만 여러 Queue에 동시에 들어가지는 않는다.

##### Basic Rules
1. Priority가 더 높은 job이 먼저 CPU를 할당받는다.
2. Priority가 같을 경우 RR로 할당된다.

## 4. How the Scheduler Sets Priorities
* 고정된 Priority 값을 각 job에 부여한다.
* job들의 수행 작업들을 보고 Priority를 조정한다.
자주 I/O에 진입해 반복적으로 CPU를 양보하는 job의 우선순위는 높이고, CPU를 오래 잡고있는 job은 우선순위를 낮춘다. 또한 이전의 작업 수행 이력을 보고 미리 다음 작업을 예측해 우선순위를 조정하기도 한다.

## 5. How to Change Priority
#### 1. Workload
* I/O-bound jobs
* CPU-bound jobs

#### 2. Priority adjustment algorithm
* Rule 3 : job이 system에 arrive하면 가장 높은 Priority에 배치된다.
* Rule 4a : CPU를 계속해서 쓰면 priority는 낮아진다.
* Rule 4b : CPU를 time slice가 끝나기 전에 주면 priority는 높아진다.

## 6. 문제점들
1. Starvation : 시간이 긴 job들은 계속해서 할당이 밀린다.
2. Gaming the scheduler : time slice 끝나기 바로 직전에 I/O를 실행해서 치트를 쓸 수 있다.
3. Changing behavior : 모두 다 I/O-bound로 바꾸려 할것이다.


## 7. 해결책 - Priority Boost
주기적으로 모든 job의 Priority를 가장 높게 끌어 올려주면 문제가 해결된다.
* Rule 5 : 일정 시간이 지나면, 모든 job의 priority를 가장 높게 끌어 올려준다.
* time slice 간격 처럼 일정한 기준이 없다. 그때그때 마다 정도에 맞춰 설정해주면 된다.

## 8. Gaming the scheduler 해결책 - Better Accounting
Rule 4a와 Rule 4b를 Rule 4로 바꾼다.
* Rule 4: CPU 할당 포기 여부로 priority 조절하던 것을 CPU 할당시간의 합으로 바꾸면 된다.