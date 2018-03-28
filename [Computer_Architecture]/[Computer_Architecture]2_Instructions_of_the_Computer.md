## 1. Instruction Set(명령어 묶음)
* CISC(Complex Instruction Set Computer)
x86 Intel Architecture가 속한다. 실제로 동작할때는 RISC LIKEY하게 동작한다.

* RISc(Reduced Instruction Set Computer)
CISC에서 좀 더 발전된 형태로, 자칫하면 CISC 이전의 Instruction Set처럼 보일 수 있지만, 자주 안쓰는 instruction들은 빼고 소규모 Set을 만든 것으로, CISC보다 속도는 높아졌고, 뺀 instruction들은 소프트웨어로 구현되었다.(cycle을 좀 더 주는 식으로)

## 2. The MIPS Instruction Set
현대의 전형적인 Instruction set이다.

## 3. Arithmetic Operations
MIPS 산술 명령어는 반드시 한 종류의 연산만 지시하고 변수 세 개를 갖으며 변수 세 개중 두 개는 Source, 하나는 Destination이다.

#### <span style="color:red">원칙 1 : 간단하게 하기 위해서는 규칙화하는 것이 좋다.</span>

## 4. Register Operands
모든 연산들의 operand는 무조건 register안에 다 있어야 하며 결과 또한 register에 저장된다. MIPS 구조에서 레지스터의 크기는 32비트이고, 이게 한 덩어리로 처리되는 일이 매우 빈번하기 때문에 **word**라고 칭한다.
> **Reg-to-Reg 구조**라고 하며 **Load-to-Store 구조**라고도 한다.

대부분 32개의 레지스터를 가지고 있기 때문에 산술 명령어의 각 피연산자는 32개의 32비트 레지스터 중 하나여야 한다. 레지스터가 크면 클 수록 좋을것 같지만 사실상 그렇지 않다. 왜냐하면 레지스터가 많아질수록 전기 신호가 더 멀리까지 전달되어야 하므로 클럭 사이클 시간이 길어지고, 처리속도는 느려지기 때문이다. 때문에 원하는 clock period 안에 작업을 다 하지 못할 수도 있다.

#### <span style="color:red">원칙2 : 작은 것이 더 빠르다.</span>

**BUT!** 무조건 작다고 빠른 것은 아니다. 레지스터 갯수 한두개는 차이가 별로 나지 않는다. 때문에 적절한 타협점을 찾아야한다.

## 5. Memory Operands
* reg -> mem = load
* mem -> reg = restore
* dynamic data는 heap 영역에 저장된다.
* Array는 무조건 memory에서 reg로 가져와야 한다.
:  **CISC**를 사용하던때에만 해도 reg로 가져오지 않아도 memory에서도 연산이 가능했었다. 하지만 **RISC**방식으로 바뀌면서 연산도 reg로 가져와서 하게 되었다.

* data transger instruction(데이터 전송 명령어) : 메모리와 레지스터 사이에서 데이터를 이동하는 명령어

> **8bit를 기준으로 움직이는 이유는?**
> 워드 주소는 워드를 구성하는 4바이트 주소중, 하나를 사용한다. 때문에 워드끼리의 주소는 연속되었을 경우, 4씩 차이가 난다. 또한 워드의 시작 주소는 항상 4의 배수여야 하기 때문에(**alignment restriction**) 자연스러운 경계를 지켜 데이터가 정렬되기 위해서는 바이트 단위로 데이터가 움직이는게 좋다.
> 때문에 최소 data set은 **1byte**이다.

## 6. Byte Address
컴퓨터는 제일 최상위(bit end) 혹은 최하위(little end) 비트를 워드 주소로 사용한다. MIPS는 빅엔디안이다.

## 7. Registers vs Memory
* reg -> mem = load
* mem -> reg = restore
* 속도는 reg가 mem보다 더 빠르다.
* intel의 CISC ASSEMBLY들은 모두 RISC LIKEY하게 작동한다.
* 컴파일러는 자주 사용하는 변수들을 메모리에서 꺼내와 레지스터에 저장해둔다.
* 레지스터는 메모리보다 접근시간이 짧고 처리량도 많으므로, 레지스터에 저장된 데이터를 사용하면 효율적이다.

## 8. Imidiate Operations - 수치 피연산자
연산에서 상수를 이용하는 일이 많은데, 이는 MIPS 산술 연산의 절반이상에 해당한다. 때문에 자주 사용하는 상수들을 미리 instruction 안에 넣어둔다면, load하지 않아도 사용할 수 있기 때문에 더 효율적이다. 이러한 상수를 수치 피연산자라고 한다. 일반적으로는, 16bit만 사용하지만(대부분 상수가 2의 16제곱 이내의 범위에 있기 때문에) 좀 더 큰 수를 쓰고 싶으면 소프트웨어적으로 구현하면 된다.

#### <span style="color:red">원칙 3 : 자주 쓰는 경우를 빠르게 만들자</span>

> **The Constant Zero**
> 상수 0은 또 다른 역할을 하는데, 유용한 여러 변형을 제공한다. 예를들어 복사(move)연산은 피연산자 중 하나가 0인 add 명령어이다. 때문에 MIPS에서는 $zero를 값 0으로 묶어두었다.

## 9. Representing Instructions
