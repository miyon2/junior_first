## 1. 개요
 컴퓨터의 성능은 대부분 transistor와 함께 발전해왔다. 퍼포먼스도 자연스레 같이 향상되었지만, 열처리가 힘들어졌다. 때문에 multi-core 방식이 등작하였고, 직렬 프로세싱에서 병렬 프로세싱으로 자연스럽게 전환되었다. 하지만 멀티코어 방식도 정말 효율적인 방식은 아니었던게, 기존 직렬로 프로그래밍 되어있던 코드들을 활용하기 위해서는 병렬 프로그래밍을 해주어야했다. 그렇지 않으면 병렬 프로세싱의 장점을 가져올 수가 없었다. 이후에 voltage를 더이상 줄일 수 없게 되자 CPU가 발전하게 되었다.

## 2. Performance
performance 측정시에 다음과 같은 요소들이 고려된다.

* 어떤 요소가 performance에 영향을 주는지
* 어떤 instruction set이 영향을 주는지
* through put(쓰루풋, 주어진 시간동안 얼마나 많은 일을 할 수 있는가)은 어떠한지
* response time(작업시간)은 어떠한지

보통 성능 개선을 위해서는 response time과 through put중 어느것을 개선할지와 얼마나 개선됐는지 파악하는게 중요하다.

## 3. Relative performance
성능은 시간과 반비례한다. 보통은 작업수행 시간이 길 수록 성능이 낮고, 작업수행 시간이 짧을 수록 성능이 좋다.

## 4. Measuring Execution Time
두가지의 미묘한 차이를 잘 아는게 중요하다.

* Elapsed Time(= wall clock time, response time)
task time을 말한다. 이 안에 processing, I/O, OS overhead, idle time 모두 들어간다. 즉 OS에 involoved 되는 모든 것들을 포함한다.

* CPU Time
특정 작업을 수행하기 위해 CPU가 소비한 실제 시간을 의미한다. processing, I/O, OS overhead, idle time등은 들어가지 않으며, 실제 CPU가 돌아간 시간이다.

> **CPU Clocking**
> 클럭의 시간 간격을 클럭사이클이라고 한다. 클럭 주기는 한 클럭 사이클에 걸리는 시간이나 클럭 속도로 표시한다.(클럭 속도는 클럭 주기의 역수이다.)

## 5. CPU Time
가장 기본적인 척도인 클럭 사이클 수와 클럭 사이클 시간으로 CPU 시간을 표시하면 다음과 같다.

`CPU Time = (CPU Clock Cycles X Clock Cycle Time)` 혹은 `CPU Clock Cycles / clock Rate`이다.

때문에 성능을 높이기 위해서는 Clock Rate(클럭속도)를 높이거나 CPU Clock Cycle를 낮추어야 한다.

작은 아키텍쳐의 경우, instruction 수가 적어서 Clock Cycle 수는 올라가고,, Clock Time은 낮아진다.

## 6. Instruction Count and CPI
instruction당 cycle이 몇개인지 알면 총 clock수를 알 수 있다는 아이디어에서 나왔다.

`CPU Time = Instruction Count X CPI X Clock Cycle Time`이며, `(Instruction Count X CPI)/Clock Rate`와도 같다.

최근의 컴퓨터는 한 사이클에 명령어를 여러개 처리가 가능하며, 어떤 ISA를 쓰는지도 instruction count에 영향을 미친다.

각각 식을 잘 엮어 스스로 유도해보는게 중요할 것 같다. 예시와 엮어서 몇가지 연습문제를 풀어보자

## 7. SPEC CPU Benchmark
성능을 몇가지 지표로 간단히 표현하기 위해서 만든 표준형태이다. `SPEC Power Benchmark`는 모바일 때문에 생긴 성능지표이다.

`CINT2006 for Opteron X4 2356`을 보면 `SPECpower_ssj2008 for X4`과 차이난다. `CINT2006 for Opteron X4 2356`의 경우 intel에서의 수행시간이 명령어 갯수, CPI와 클럭 사이클 시간을 기준으로 해서 차이가 난 것이다.

## 8. Amdahl's Law
개선 정도는 어떤 테크닉을 적용시에 성능이 개선되는 부분과 개선되지 않는 부분 모두 존재한다. 이때 개선 후의 프로그램 실행시간은 암달의 법칙으로 알 수 있다.

`Time(improved) = [Time(affected)/improvement factor] + Time(unaffected)`이며, 우변의 첫번째 피연산자는 특정한 부분에서 발전되는 비율을 뜻한다.