## 1. Direct Execution
개념적으로는 OS가 응용프로그램의 Storage에서 CPU로 direct하게 checking 없이 로딩해주는 것을 말한다. DOS가 여기에 해당한다.

|  | OS | Program |
|:--------:|:--------|:--------|
| Step | Create entry for process list<br>Allocate memory for program<br>Load program into memory<br>Set up stack with argc/argv<br>Clear registers<br>Execute <span style="color:red">call</span> main()<br><br><br>Free memory of process<br>Remove from process list | <br><br><br><br>Run main()<br>Execute <span style="color:red">return</span> from main()|

이러한 과정에서는 크게 두가지 이슈가 발생한다.
##### 1. main()에서 함부로 사용하면 안되는 instruction에의 접근이 제한되지 않는다.
##### 2. return 지점의 조절이 불가능하다. (because without checking)

## 2. Problem #1: Restricted Operations (= privileged operations)
Program에서 해당되는 오퍼레이션들을 사용해야 할 경우에는 어떻게 하지?
##### => Processor Mode를 나누어 해결하자!

## 3. Processor Modes
* User mode
사용할 수 있는 오퍼레이션이 제한된다. 또한 프로세서가 exception을 발생하면 그 결과로 resticted operation이 발생한다.

* Kernel mode
OS는 커널모드에서 작동하며, 이 모드에서는 알아서 잘 작동한다.

> Direct Execution Protocol에서는 OS에서 trap 대신 call을 사용했기 때문에, main() 실행을 커널 내부에서 진행하게 된다. 때문에 restricted operation으로의 접근이 제한되지 않아서 문제가 발생하므로 생긴게 Processor Mode이다.

## 4. System Calls
##### 1. 모드의 전환
 제한된 오퍼레이션의 사용을 위해서는 유저모드에서 커널모드로 진입해야만 한다. 이때 사용하는 방법이 두가지가 있다 .첫째, 유저모드에서 <span style="color:red">**trap instruction**</span>을 실행해, system call을 호출함으로써 커널모드로 진입한다. 둘째, 주기적으로 발생하는 <span style="color:red">**time interrupt**</span>로 커널모드로 넘어간다.

##### 2. Trap Table
 처음 OS가 부팅될 때, OS는 Trap Table을 생성한다. 정확히는 trap handler를 형성한다. 이후 유저모드에서 trap을 실행하면(user mode에서 system call을 사용하겠다는 신호를 kernel mode에 날리면), kernel mode로 넘어간다. kernel mode에서는 trap table에서 해당하는 system call index를 읽어들여 실행한 후 <span style="color:red">**return-from-trap instruction**</span>을 이용하여 다시 user mode로 넘어간다.

##### <?> trap instruction
* 커널로 넘어가게 해주는 역할
* privilege mode로 실행시켜준다.

##### <?> return-from-trap instruction
* 유저모드로 넘어가게 해준다.
* privilege mode에서 돌아오도록 해준다.

## 5. Limited Direct Execution Protocol(using TRAP)
OS - Hardware - Program의 상호작용 테이블은 pdf를 참고하도록 하자

##### <?> 왜 OS에서는 프로그램을 위한 메모리를 할당하고 그 안의 Stack을 채워줄까?
커널모드에서 유저모드로 돌아오기 위해서는 return-from-trap instruction을 사용하게 되는데, 이 instruction은 해당 모드의 stack 요소중 가장 위의 두 요소를 각각 PC, u-stack으로 인지하고 가져온다. 그런데 부팅하고 처음 유저모드로 넘어가면, 프로그램이 아직 실행되지 않은 상태이기 때문에, 커널모드의 stack도 비어있는 상태이다. 때문에, 미리 넣어주어 오류를 방지하기 위해서 OS에서는 부팅시에 trap handler를 생성하고, 커널모드의 stack을 채워준다. 그리고 return-from-trap instruction을 이용해서 유저모드로 넘어간다.

## 6. Problem #2: Switching Between Processes
프로세스가 CPU에서 돌고 있다는 것은 OS가 돌고 있지 않다는 것인데 그렇다면, OS는 아무것도 할 수 없는걸까?

## 7. Cooperative Approach
Process가 자발적으로 넘겨주는 방법이다.
1. 너무 오래 도는 Process는 주기적으로 CPU 할당을 포기하도록 되어있다. 이때, system call을 호출함으로써, OS에 CPU를 넘겨준다.
2. 응용프로그램들은 예외를 발생시키면 control을 OS에 넘겨준다. (0으로 나누기 또는 segmentation fault같은)

## 8. Non-Cooperative Approach
OS가 강제적으로 control하는 방법이다.
1. Time interrupt를 발생시킨다.
timer divice가 주기적으로 interrupt를 발생시킨다. inturrupt가 발생하면, 현재 작동중이던 프로세스는 잠시 멈추고 interrupt handler가 돌아간다.

> **interrupt handler**는 프로세스와는 다른 개념이다. 완전히 독립된 프로세스 처럼 만들어졌지만 하드웨어처럼 항상 작동하고, 프로세스처럼 돌아가야 하며, 프로세스는 아니어야 한다. 이와 같은 특징 때문에 프로세스에 기생해서 작동한다. 이때 기생하는 프로세스는 current process의 kernel stack이다.

## 9. Limited Direct Execution Protocol(using TIME INTERRUPT)
OS - Hardware - Program의 상호작용 테이블은 pdf를 참고하도록 하자

<span style="color:red">**모호하게 느껴지는 점** : timer interrupt를 timer deviece에서 담당하면, trap과는 다르게 time interrupt는 hardware에서 담당하는건가 + 현재 돌아가고 있는 process를 가리키는 어떤 요소?에 대해서도 잘 모르겠다. 이것도 하드웨어가 담당하는 것 같긴한데 그럼 여기서 OS와 하드웨어의 역할에 대해서도 모호하게 느껴진다.</span>

##### <?>k-ctxt(???)는 어떤 stack 요소?
다른 Process로 넘어가서 다시 돌아왔을때, 함수 호출-return처럼 이전의 정보가 필요하다. 이 때 돌아와서 사용할 정보이다. 이 정보는 Stack 이외의 자료구조에도 저장이 가능하지만, linux에서는 stack에 저장한다.

##### <!>switch() routine에서 주의할 점
이 부분의 코드는 매우 복잡하고, high-level-language로 작성하게 되면 compiler에 따라서 다른 결과를 실행할 수도 있지만 대부분은 오류를 일으킨다. 때문에 어셈블리어로 짜여져 있고, switch() routine 중에 k-stack에 어떤 요소든 저장하면, context switch destination process에의 k-stack에 저장되지 않고 그 이전의 k-stack에 저장되므로 주의해야 한다. Protocol 공부를 하며 switch() routine시의 저장 시기에 대해서도 주의깊게 알아놓으면 좋을 것 같다.

## 10. Trap과 Interrupt의 차이점
Trap은 user mode - kernel mode의 상호작용을 위한 것이며 한 프로세스 내에서의 상호작용이고, Interrupt는 context switch를 통해서 process들 간의 상호작용을 위한 것이다.