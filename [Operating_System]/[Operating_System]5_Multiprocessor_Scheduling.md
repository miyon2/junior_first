## 1. Multiprocessor Architecture
각 CPU는 한개의 Cache를 가진다. 이때 각각 Cache는 L(n) Cache로 표현될 수 있다. 가장 마지막 Cache인 LL Cache는 모든 캐시의 내용을 공유한다. 

하지만 만약 이때 각 캐시에서 가지고 있던 값의 변화가 일어나는 시점이 다를경우, 원본과 사본사이의 차이가 생기는데 이 차이는 하드웨어에서 처리해준다. 그렇지만 cost는 여전히 발생하므로 최대한 차이가 발생하지 않도록 해주는 것이 좋다.

## 2. Single-Queue Scheduling
###### >> Single-Queue multiprocessor scheduling (SQMS)
하나의 Queue를 가지고 Scheduling 해줄 때의 상황이다. 한 time slice가 끝나면 실행했던 process들을 모두 queue에 삽입한다. 다시 CPU를 할당할 때 CPU의 수만큼(n개 만큼) Queue에서 빼낸다.

> ##### Synchronization
> 각각 CPU의 상황을 동기화 해주어야 하는 이슈가 있다.
> ##### Cache Affinity
> 캐시 내의 데이터의 저장, 삭제 이슈가 있다.

## 3. Multi-Queue Scheduling
이전에는 시스템 통틀어 queue가 하나였다면, Multi-Queue Scheduling에서는 CPU 하나당 여러개의 큐가 생성된다. 각 CPU에 실행할 수 있는 프로세스가 정해지며, 그 프로세스들이 각 CPU의 Queue에 들어가는 식으로 작업이 수행된다.

이 경우 이론상으로는 Synchronization과 Cache Affinity의 이슈는 해결될 수 있다. 하지만 실질적으로는 각 CPU에 Process를 분배해줄 때 여전히 작업시간이 고르게 분배되지 않을 가능성이 높다. 때문에 imbalnce한 process queue를 조정해서, balance하게 해주기 위해 Migration하면 또다시 Cache Affinity가 발생한다.

#### Migration
각 CPU에 할당되어 있던 process를 남는 CPU에 옮겨준다. 하지만 cost역시 존재하므로, time slice와 같이 overhead를 적절하게 조절해 주는것이 중요하다.

#### Work stealing
작업을 다 마친 CPU는 그렇지 못한 CPU의 프로세스를 가져와 실행한다. 이때 프로세스의 갯수는 한개 이상이 될 수 있다.

## 4. Linux CPU Schedulers
리눅스에는 3가지 스케쥴러가 내장되어 있으며, 코어 1개마다 각각 스케쥴러 3개 모두를 가지고 있다. 때문에 스케쥴러끼리 동기화가 필요하지 않지만, migration은 가능한다.(imbalance)
##### 1. Completely Fair Schduler(CFS)
레드블랙 트리로 이루어져 있으며, time complexity는 `log(n)`이다. O(1) 스케줄러와 성능적으로 큰 차이는 없지만, vruntime을 이용한 fariness가 중요하기 때문에 트리로 이루어져 있다.

이때 fair는 프로그래밍 적으로 fair한 것이므로, 일반적으로 우리가 생각하는 fair와는 거리가 많이 멀 수 있다.

##### 2. Real-Time Schedulers
FIFO또는 RR로 이루어져 있으며 보통은 같은 priority를 어떻게 해결할지를 두고 결정한다. 우선순위를 두고 스케줄링하며, multi-level feedback Queue와 거의 유사하다.

하지만 starvation을 혼자 해결하지 않기 때문에 root계정으로 직접 다뤄줘야 한다.

##### 3. Deadline Scheduler
root 유저만 조작할 수 있으며, 시간 텀을 두고 스케쥴링하는 스케쥴러다.

##### 4. 작동 우선 순위
Deadline > Real-time > Completely

## 5. Completely Fair Scheduler(CFS)
#### <?> vruntime?
CFS에서의 가중치를 의미한다. 일반적으로 CPU를 자주 쓰지 않으면 값이 작아지므로 I/O bound가 제일 작은 값을 가질 가능성이 높다. 때문에 shortest의 장점도 가지고 있으며, turnaround time/response time측면에서도 좋은 성능을 보인다.

#### <?> nice?
CPU를 얼마나 잘 넘겨줄지를 정해줄 수 있는 지표다. 일반적으로 nice 값이 높으면(대체로 양수), CPU를 욕심내지 않기 때문에 자주 실행되기가 힘들다. 반면 nice 값이 낮으면(대체로 음수), vruntime 값은 작기 때문에 자주 실행되기 쉽다.

## 6. Priority
##### <?> prio = nice + 120
- CFS의 prio 범위 : 100-139
- Real-Time의 prio범위 : 0-99

##### renice command
default가 0이지만, root유저가 renice 해줄 수 있다. 일반 유저의 경우에는 nice를 더 큰 수로 변경해주는 것밖에 하지못하고(not [-1]~[-20]) root유저만 음/양수를 줄 수 있다.