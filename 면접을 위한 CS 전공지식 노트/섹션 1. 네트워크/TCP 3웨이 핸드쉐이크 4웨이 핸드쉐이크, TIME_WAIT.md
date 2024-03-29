# TCP : 3웨이 핸드쉐이크 4웨이 핸드쉐이크, TIME_WAIT

SYN : SYNchronization의 약자, 연결 요청 플래그

ACK : ACKnowlegement의 약자, 응답 플래그

ISN : Initial Sequence Numbers의 약자, 초기 네트워크 연결을 할 때 할당된 32비트 고유 시퀀스 번호이다.

- 승인번호는 ISN + 1

TCP는 신뢰성을 확보할 때 ‘3-웨이 핸드셰이크'라는 작업을 진행한다(연결성립과정).

<img width="327" alt="스크린샷_2022-07-11_오후_10 49 54" src="https://user-images.githubusercontent.com/88222461/182016038-9b50216f-ea04-402f-96d1-90847cd1b0f5.png">

1. SYN단계 : 클라이언트는 서버에 클라이언트의 ISN을 담아 SYN을 보낸다. ISN은 새로운 TCP연결의 첫 번째 패킷에 할당된 임의의 시퀀스 번호를 말하며(예: 12010) 장치마다 다를 수 있다.
2. SYN + ACK 단계 : 서버는 클라이언트의 SYN을 수신하고 서버의 ISN을 보내며 승인번호로 클라이언트의 ISN + 1을 보낸다.
3. ACK 단계 : 클라이언트는 서버의 ISN + 1한 값인 승인번호를 담아 ACK를 서버에 보낸다. 이렇게 3-웨이 핸드셰이크 과정 이후 신뢰성이 구축되고 데이터 전송을 시작한다. TCP는 이 과정이 있기때문에 신뢰성이 있는 계층이라고 하며 UDP는 이 과정이 없기 때문에 신뢰성이 없는 계층이라고 한다.

3웨이 핸드쉐이크가 established 과정이었다면,
4웨이 핸드쉐이크는 연결 해제 과정이다.

<img width="412" alt="스크린샷_2022-07-11_오후_11 02 27" src="https://user-images.githubusercontent.com/88222461/182016043-b67acceb-4443-4151-9c8b-cf3845e9a732.png">


1. 클라이언트가 연결을 닫으려고 할 때 FIN으로 설정된 세그먼트를 보낸다. 그리고 클라이언트는 FIN_WAIT_1 상태로 들어가고 서버의 응답을 기다린다.
2. 서버는 클라이언트로 ACK라는 승인 세그먼트를 보낸다. 그리고 CLOSE_WAIT 상태에 들어간다. 클아이언트가 세그먼트를 받으면 FIN_WAIT_2 상태에 들어간다.
3. 서버는 ACK를 보내고 일정 시간 이후에 클라이언트에 FIN이라는 세그먼트를 보낸다.
4. 클라이언트는 TIME_WAIT 상태가 되고 다시 서버로 ACK를 보내서 서버는 CLOSED상태가 된다. 이후 클라이언트는 어느정도의 시간을 대기한 후 연결이 닫히고 클라이언트와 서버의 모든 자원의 연결이 해제된다.

바로 연결을 닫지 왜 TIME_WAIT로 일정 시간뒤에 닫을까?

첫번째는 지연 패킷이 발생할 경우를 대비하기 위해

패킷이 뒤늦게 도달하고 이를 처리하지 못한다면 데이터 무결성 문제가 발생한다.

- TIME_WAIT : 소켓이 바로 소멸되지 않고 일정 시간 유지되는 상태를 말하며 지연 패킷 등의 문제점을 해결하는데 쓰인다. CentOS6, 우분투에는 60초, 윈도우는 4분
- 데이터 무결성(data integrity) : 데이터의 정확성과 일관성을 유지하고 보증하는 것
