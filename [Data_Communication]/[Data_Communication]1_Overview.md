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

## 4. Circuit Switching (=회선교환)
전화로 말하는 동시에 destination으로의 경로가 설정되어 할당된다.

> logical channer : 할당하는데에 돈이 많이 든다.
> physical link : logical channer에 비해서는 돈이 많이 들지 않는다.