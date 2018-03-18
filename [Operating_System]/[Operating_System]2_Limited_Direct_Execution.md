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

## 4. System Calls
##### 1. 모드의 전환
 제한된 오퍼레이션의 사용을 위해서는 유저모드에서 커널모드로 진입해야만 한다. 이때 사용하는 방법이 두가지가 있다 .첫째, 유저모드에서 <span style="color:red">**trap instruction**</span>을 실행해, system call을 호출함으로써 커널모드로 진입한다. 둘째, 주기적으로 발생하는 <span style="color:red">**time interrupt**</span>로 커널모드로 넘어간다.

##### 2. Trap Table
 처음 OS가 부팅될 때, OS는 Trap Table을 생성한다. 정확히는 trap handler를 형성한다. 이후 유저모드에서 trap을 실행하면(user mode에서 system call을 사용하겠다는 신호를 kernel mode에 날리면), kernel mode로 넘어간다. kernel mode에서는 trap table에서 해당하는 system call index를 읽어들여 실행한 후 <span style="color:red">**return-from-trap instruction**</span>을 이용하여 다시 user mode로 넘어간다.

##### <?> trap instruction
* 커널로 넘어가게 해주는 역할
* privilege level을 올려준다.

##### <?> return-from-trap instruction
* 유저모드로 넘어가게 해준다.
* privilege level을 다시 낮춰준다.

## 5. Limited Direct Execution Protocol

| OS | Hardware | Program |
|:--------|:--------|:--------|
| Initialize trap Table<br> | <br>Remember address of systemcall handler |  |
| Create entry for process list(PCB)<br>Allocate memory for program<br>Load program into memory<br>Set up user stack with argc/argv<br>Fill kernel stack with regs/PC<br>Return-from-trap| <br><br><br><br><br><br>Restore regs from kernel stack<br>Move to user mode<br>Jump to main | <br><br><br><br><br><br><br><br><br>Run main()<br>...<br>Call system call<br>Trap into OS |
|  |  |  |