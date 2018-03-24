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
#### Work stealing