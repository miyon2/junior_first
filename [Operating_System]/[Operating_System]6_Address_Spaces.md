## 1. Early Systems (Batched Systems)
가상 메모리가 전혀 제공되지 않았다. 또한 물리메모리가 섹션이 나누어져 있지 않아서 매우 비효율적이었다.

## 2. Multiprogramming and TimeSharing (ex - Palm OS)
* Multiprogramming
CPU의 유틸성을 높이고, OS를 이용해 멀티프로세스를 가능하게 했다.

* Time Sharing
전체 메모리를 효율적으로 사용하기 위해서 구역을 나눴고, switching 시에 level process가 존재했다.

* Protection Issue
프로세스들 끼리는 서로의 메모리에 접근할 수 없어야만 했다.

## 3. Address Spaces
가상적으로 주어진 주소를 가상 주소(virtual address) 또는 논리 주소(logical address) 라고 하며, 실제 메모리 상에서 유효한 주소를 물리 주소(physical address) 또는 실주소(real address)라고 한다. 가상 주소의 범위를 가상 주소 공간, 물리 주소의 범위를 물리 주소 공간이라고 한다.

프로세스의 데이터는 virtual memory에 저장되며, physical memory는 virtual memory의 시작주소만을 가지고있다. 때문에 코딩시에 virtual 환경만 신경써주면 되고, physical memory는 신경쓰지 않아도 된다.

> 가상메모리에서는 주소가 연속적이어야 하지만, 물리메모리에서도 연속적일 필요는 없다.

## 4. Virtual Memory
프로그램은 OS area의 바로 다음에 오는 주소(0)에 load되며, 32비트 혹은 64비트의 주소공간을 가진다. 이때 지켜져야 할 세가지 조건이 있다.

#### 1. Transparency
프로세스는 서로의 메모리가 공유될 필요가 없다. 

#### 2. Efficiency
* Time : 프로그램이 실행중 더 느려지면 안된다.
* Space : 가상화에 너무나 많은 메모리를 사용하면 안된다.
* 전체적으로, Sharing으로 CPU, Memory 모두 성능이 저하되면 안된다.

#### 3. Protection
프로세스들 끼리의 isolation이 보장되어야 하며, 이는 OS가 보장해주어야 한다. 또한 컨널모드와 유저모드 끼리의 isolation도 보장되어야 한다.

## 5. Type of Memory
#### 1. Stack(=automatic memory)
컴파일러에게 명확히 보여지고, 컴파일러가 알아서 할당해주고 해제해준다.(implicitly)
#### 2. Heap
programmer가 직접 할당/해제해주어야 한다.(explicitly)

## 6. Common Errors
##### 1. 동적할당을 해주지 않은경우(Segmentation Fault)
##### 2. 충분한 공간을 할당해주지 않은경우
이 경우 가장 많은 문제가 발생한다. 만약 현재 할당해준 변수 이외에 다른 변수가 할당되어 있지 않다면, 이후에 변수 할당시에 해당 변수에서의 데이터 손실이 일어나게 되고, 만약 변수가 할당되어 있었다고 해도 ovewrite되기 때문에 데이터손실이 발생한다.
##### 3. 할당된 공간을 초기화해주지 않은 경우
##### 4. 메모리 해제를 해주지 않은 경우
힙 크기가 점점 자라서 kill-restart해주지 않으면, 실행이 되지 않는다.(Memory leak)
##### 5. 작업이 끝나기 전에 해재해준 경우
포인터만 덩그라니 남는다.(Dangling pointer)
##### 6. free를 해줄 필요가 없는데 해준경우(crash)