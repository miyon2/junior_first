## 1. Program vs Process
### Program
* instructions, static data가 모인 단일 파일

### Process
* 동적으로 돌아가고 있는 프로그램
* 현재 machine state의 흐름을 알 수 있다. (Memory, Registers, Open-Shutdown log)
* APIs(System Call과 유사)

## 2. fork() & wait()
### CPU 할당
* wait()이 걸려있을 경우엔 CPU의 할당이 이미 고정된 상태라 실행중이지 않은 프로세스에서 할당받을 수 없다.
* 반면 걸려있지 않은 프로세스간의 CPU 할당은 스케쥴러에 따라서 달라진다. 임의 지정할 수 없다.

### wait()
wait() 사용시 CPU의 분배가 다 끝난 뒤 프로세스를 실행하기 때문에 자식 프로세스가 끝나기 전엔 부모 프로세스는 CPU 할당을 받을 수 없다. 또한 wait()은 자식 프로세스중 어느 하나라도 끝나서 어떤 값을 반환하면 부모 프로세스를 실행한다.

## 3. exec()
exec()이 실행되는 순간 stack에 차있던 기존 instruction들을 무효화하기 위해 PC는 main의 맨 위로 간다. 그 이후 새로 덮어 쓸 함수의 instruction set을 가져와서 PC의 다음 메모리 주소에 올려준다.

> <?> 만약 Memory 크기 범위를 넘어설 정도로 코드량이 많으면?
> <!> 컴파일 단계에서 알아서 걸러주기 때문에 걱정하지 않아도 된다.

## 4. Virtualization of the CPU
Time Sharing을 통해서 CPU를 많이 사용하는 것처럼 보이게 할 수 있다. 유저는 동시에 많은 프로세스를 실행할 수 있으며 시스템은 Context switch(mechanism)와 Scheduling policy(policy)를 이용해 time sharing한다.

#### <?> Scheduling 시에 OS는 CPU를 잡아먹을까?
아니다. OS는 계속 돌아가게끔 설계되어 있고, System Call을 부를 때마다 OS의 스케쥴러가 실행되고, 부르지 않더라도 interrupter를 이용하여 timer로서 주기적으로 체크한다. 이때 Scheduler는 process의 context로서 존재할 수 있다.

## 5. Process State
* Running
CPU를 할당받아 돌고 있는 프로세스

* Ready
CPU를 할당받을 준비가 된 프로세스

* Blocked(Waiting)
I/O가 끝나지 않아서 더이상의 실행이 불가능한 상태로, OS에서 CPU를 뺏어 다른 Process로 넘겨준다. 때문에 아예 CPU 할당 대상에서 제외된다.

#### <?> 어떻게 알고 CPU를 다른 process에 넘겨줄까?
process 끼리의 상태는 공유하지 않고 공유할 수도 없지만, OS가 Ready Queue, Blocked Queue, Running Queue를 관리하기 때문에 Process를 상태 관리하다 CPU를 할당해준다.

#### <?> Blocked의 시점은?
User mode에서 System call을 부르면 kernel mode로 넘어가고, OS내의 I/O의 큐가 빈걸 확인하면, Scheduler가 실행되고, 다른 Process를 실행한다. 그 후 다른 Process에서 input data가 확인되어 해당 data가 OS로 넘어오면, OS는 이를 인지해 미리 실행할 큐에 담아두었던 process로 Blocked를 해제한다.

## 6. Data Structures
* PCB(Process Control Block) = Task Control Block = Process에 대한 Queue
* 리눅스는 `TASK_RUNNING`, `TASK_INTERRUPTIBLE` 이 두 변수로 Running/Ready, Blocked를 구분한다.
* `/include/linux/sched.h`의 `*stack`변수는 각 Stack의 Top을 의미한다.

#### Context Switch 시의 Stack을 주고 받는 과정
Context Switch 중에 레지스터들의 상태저장 Stack의 보존이 매우 중요한데, 각 Process 별로 Stack이 따로 존재한다.
1. System Call 호출로 kernel에 진입하면, (* stack)을 넘겨준다.
2. Context Switch 시에 kernel에 진입하면, (* stack)을 넘겨준다.
3. Process2를 실행하며 쌓인 Stack의 data를 POP하고 POP한 (* stack)을 다시 Process 1의 kernel에 넘겨준다. (함수 호출 및 반환 후 돌아가는 stack의 과정을 생각하면 쉽다.)
4. 마찬가지로 쌓였던 data를 POP하고 User Mode에 넘겨준다.