## 1. Key Elements of a Protocol
1. Syntax - 데이터의 형식
2. Semantics - control과 오류관리를 위한 제어정보
3. Timing - 속도조절과 순서조정

통신 과정 자체가 너무나 크고 복잡하기 때문에 파트를 나누었다.

## 2. TCP/IP Protocol Architecture
TCP/IP 프로토콜 구조는 DARPA에서의 패킷교환망인 ARPANET의 연구개발로부터 유래했으며, 일반적으로 이 모두는 TCP/IP Suite로 불리며, TCP/IP Protocol Suite는 인터넷 표준으로 채택된 많은 프로토콜로 이루어진다.

## 3. TCP/IP Layers
TCP/IP Layer은 공식적인 모델은 아니지만 그러한 역할을 하고있다. 즉, 사실표준이며 다음과 같은 5개의 peer한 layer로 이루어져 있다.

#### 1. Physical Layer
물리계층은 데이터 전송장치와 전송 매체 또는 네트워크 사이의 물리적인 인터페이스를 다룬다. 이 계층은 전송 매체의 특성, 신호의 성질, 데이터율 및 관련 사항을 규정하는데에 관련된다.

#### 2. Network Access Layer
네트워크 접속 계층은 Destination와 그 네트워크 사이의 데이터 교환을 다룬다. 이때 위쪽 layer들의 link specifics(보내는 방식)는 기재하지 않는다. - 이부분이 헷갈린다. 다른 계층으로 분리함으로써 그 방식을 기재하지 않는 장점은 존재하지만 어떤 방식인지가 와닿지 않는다.

#### 3. Internet Layer
인터넷 계층은 ARPA Project로 등장했다. 이 계층은 여러 개의 네트워크를 통과하는 경로 배정기능을 제공하며, 이 프로토콜은 Destination 뿐만이 아니라 라우터에서도 작동한다. 때문에 여러 네트워크가 묶여 연결되어 있을 수 있고 이로인해서 특정 한 지역에서의 네트워크가 마비되더라도 연결된 모두가 마비되는것이 아니라 다른 지역에서는 네트워크가 살아있다.

#### 4. Transport Layer
이 계층에서는 도착한 Packet data를 reordering한다.

#### 5. Applicaion Layer
이 계층에서는 다양한 사용자에게의 사용을 지원하기 위해서 필요한 작업들을 다룬다. 각각 응용프로그램의 특성에 따라 하는 작업 수행이 달라진다.

## 4. Operation of TCP and IP
전체 통신 설비는 여러 개의 네트워크로 구성되며, 각 성분 네트워크를 보통 서브네트워크라고 한다. 이때 목적지 호스트가 다른 서브네트워크에 있는 경우 라우터에 데이터를 전송하는데, 이때 라우터는 Internet layer ~ physical layer의 총 3개의 층을 가진다.

> #### Addressing Requirement
> 통신을 위해서는 모든 개체는 반드시 유일한 주소를 가져야하며, 실제로 두 단계의 주소 지정이 필요하다. 호스트 내의 모든 프로세스는 호스트에서 유일한 주소를 가지고 있으며, 이 주소는 TCP가 올바른 프로세스로 데이터를 전달하는데 이용되며, 두번째 주소는 포트라고 한다.

