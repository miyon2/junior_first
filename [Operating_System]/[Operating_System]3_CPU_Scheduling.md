## 1. Scheduling Metrics
#### <?>Turnaground Time?
Time(turnaround) = Time(completion) - Time(arrival)
프로그램의 실행시간은 끝난시각-시작시각이다. 또한 어떤 스케쥴러가 더 나은지의 판단은 매우 어렵기 때문에 다음 5가지 사실들을 가정하고 하나씩 지워가면서 어떤 스케쥴링이 어떤 상황에 더 좋을 수 있는지 알아본다.

## 2. 5가지 가정
1. 각 작업들은 동일한 작업시간을 가진다.
2. 모든 작업은 동시에 작업을 해야할 queue에 들어간다.
3. 일단 시작하면 끝날때 까지는 context switch 중에도 멈추지 않는다.
4. 모든 작업은 오직 CPU만을 사용한다. (i.e., no I.O)
5. 작업의 실행 시간은 알 수 있다.

## 3. 5가지 가정이 모두 적용될 경우
##### First Come, First Served(FCFS)

## 4. 4가지 가정이 적용될 경우
1. 모든 작업은 동시에 작업을 해야할 queue에 들어간다.
2. 일단 시작하면 끝날때 까지는 context switch 중에도 멈추지 않는다.
3. 모든 작업은 오직 CPU만을 사용한다. (i.e., no I.O)
4. 작업의 실행 시간은 알 수 있다.

##### case 1 : 가장 짧게 걸리는 프로세스를 먼저 실행할 경우
pdf 그림에 따르면 average turnaround time은 50이다.

##### case 2 : 가장 오래 걸리는 프로세스를 먼저 실행할 경우
pdf 그림에 따르면 average turnaground time은 110으로 이전 case보다 cost가 더 많이 든다. 이렇게 가장 오래걸리는 프로세스가 먼저 스케쥴링 될 경우 효율이 떨어지는데, 이 현상을 convoy effect 현상이라고 한다.

## 5. 3가지 가정이 적용될 경우
1. 일단 시작하면 끝날때 까지는 context switch 중에도 멈추지 않는다.
2. 모든 작업은 오직 CPU만을 사용한다. (i.e., no I.O)
3. 작업의 실행 시간은 알 수 있다.

pdf 그림에 따르면 실행시간이 가장 긴 A 프로세스가 제일 먼저 실행되고, 실행 도중에 B,C 프로세스가 큐에 들어왔지만, `일단 시작하면 끝날때 까지는 context switch 중에도 멈추지 않는다`는 가정때문에  context switch가 불가능하다.

## 6. 2가지 가정이 적용될 경우
1. 모든 작업은 오직 CPU만을 사용한다. (i.e., no I.O)
2. 작업의 실행 시간은 알 수 있다.

##### case 1 : Non-preemptive scheduler
Preemption은 CPU를 뺏어서 다른 프로세스에 넘겨주는 것을 말한다. 따라서 Non-preemptive는 실행도중 context switch가 일어나지 않았던 3가지 가정이 적용된 경우와 같다. 옛날에 쓰이던 방식이다.

##### case 2 : Preemptive scheduler(Preemptive Shortest Job First - PSJF)
이 경우에는 실행 도중에도 context swtich가 발생한다. 이 경우에는 실행할 남은 시간을 가지고 shortest를 정하게 된다. 때문에 두가지 scheduling metrics가 필요하다.
1. Time(turnaround) = Time(completion) - Time(arrival)
2. Time(response) = Time(firstrun) - Time(arrival)

## 7. 1가지 가정이 적용될 경우
1. 작업의 실행 시간은 알 수 있다.

## 8. 어떤 가정도 적용되지 않을 경우