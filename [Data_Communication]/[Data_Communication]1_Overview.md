## 1. A Communications Model
Sender와 Receiver가 존재하며 다음과 같은 프로세스로 진행된다.

`Source` > `transmitter` > `transmission system` > `Receiver` > `destination`

`Source`, `transmitter`는 Source System에 해당하며, `Receiver`, `destination`은 Destination System에 해당한다.

## 2. Communications Task
1. Transmission System Utilization : 비싸고 규모가 커서 많은 사람이 쓰면 좋다.(유틸성)
2. Interfacing
3. Signal Generation : 기계어로 변한 코드를 인간이 읽을 수 있는 정보로 바꾼다.
4. Synchronization : 같은 시간에 통신을 맞춘다.(투수와 포수관계)
5. Exchange Management : 프로토콜에 관한것
6. Error Detection : 적합한 미디어를 통신에 이용한다.
7. Flow Control : 미디어는 제한된 자원을 가지기 때문에 traffic을 잘 조절해야 한다.
8. Addressing : 주고 받을 주소
9. Routing : traffic 양을 계산해 경로를 설정한다.
10. Recovery : 오류발생으로 인한 시스템 다운을 방지한다.
11. Message Formatting : 메세지 규격
12. Security
13. Network Management

> Traffic Engineering : How to Maximize Utilization

## 3. Networking
전화는 목소리통신이지만, 국제전화는 사실상 Data Communication에 해당한다.

- phone - voice - switch - PSTN (일반전화)
- phone - data - router - PSON (국제전화)

## 4. Circuit Switching (= Switching technology 1, 회선교환)
##### Voice Communication
전화로 말하는 동시에 destination으로의 경로가 설정되어 할당된다.

> logical channer : 할당하는데에 돈이 많이 든다.
> physical link : logical channer에 비해서는 돈이 많이 들지 않는다.

## 5. Packet Switching (= Switching technology 2, WAN)
##### Data Communicaion
data에는 두가지 상태가 있다.(original, raw) original한 데이터는 raw한 데이터로 잘게 쪼개져 패킷화 되어 전송된다. 패킷들은 크게는 Source에서 Destination으로 이동하며 이 때 node 에서 node로 이동한다.

한 데이터는 destination으로의 여러 경로로 패킷화 되어 전송된다. 이때 이런 전송을 hop이라고 부르며, destination으로 각 데이터는 순서를 무시하고 무작위로 전송되어도 상관없다. 전송된 데이터를 receiver가 proper order로 재배치 하기 때문이다. 이 점이 circuit switch와 상반된다. circuit switch는 무조건 proper order로 destination까지 전송되어야 한다.

## 5. Frame Relay (작고 가벼울수록 더 빠르다!)
Frame Relay는 Packet Switching 보다는 빠르다. 하지만 에러 체킹을 위해 더 붙는 부가적인 data들이 Packet Switching보다 적기 때문에 에러율은 높다.
> transmission 과정에서 붙는 tail error checking packet이 많을수록 용량은 상승하고(overhead rise), 에러율은 하락한다.(reliable)

## 6. Asynchronous Transfer Mode(이하 ATM, Synchronizations)

이전에 National Data를 다루는 방법을 논의할 때 IP, ATM 두가지 방식이 논의되었다. internet에서는 packet switching을 사용했고, 다른 방식에서는 ATM을 사용했었다. 두가지 방법 모두 장단이 있었지만 대세에 따라서 IP방식을 채택하게 되었다.

비동기적 시스템도, 동기적 시스템도 모두 Synchronization을 하지만, 비동기적 시스템은 전송 이전에 전송한다는 신호 없이 전송하고, 전송하는 data에 이 신호를 같이 실어 보낸다.(Implicit) 반면에 동기적 시스템은 전송 이전에 전송한다는 신호를 보내고 데이터를 전송한다.(Explicit)

ATM 방식은 많은 양의 데이터를 한번에 보낼 수 있다는 장점이 있지만, end node만이 error를 control할 수 있기 때문에 에러율이 높다.

Packet Switching은 지연율이 Fluctuate한데 비해, ATM은 constant하다. 그리고 데이터의 지연율이 constant한것은 매우 중요한데, 경로끼리의 불균형이 생기고 특정 경로에서 지연될 경우 결국 전체적인 데이터 지연율에서는 손해를 보기 때문이다. (traffic condition with control) ATM은 virtual cirtuit으로 constant data rate를 가능하게 한다.

> Cell = packet 보다는 좀더 큰 data 단위이다.

## 7. LAN
이더넷 환경이다. 이더넷은 `Ether + Net`으로, 물질적 경로로 이어져 있음을 뜻한다.

## 8. MAN
LAN보다는 좀 더 범위가 큰 환경이다. 하지만 WAN보다는 작다.

## 9. The Internet
인터넷은 많은 노드로 이루어져 있다. 때문에 데이터 전송에 표준이 필요하게 되었고 이를 Protocol로 만들었다. 이러한 프로토콜 표준은 ITF/ITU에서 정의하지만 이미 대다수가 사용중인 표준에 의해 통합되는 경우가 더 많다.(사실통합) 때문에 대부분은 ARPANET의 표준인 TCP/IP 방식을 사용한다.