# Chapter 3
## Transport Layer
### 3.1 Introduction and Transport-Layer Services
- 전송 계층 프로토콜은 서로 다른 호스트에서 실행 중인 애플리케이션 프로세스 간의 논리적 통신을 제공함. 애플리케이션 프로세스는 전송 계층이 제공하는 논리적 통신을 사용하여 서로 메시지를 전송하며, 이러한 메시지를 전송하는데 사용하는 물리적 인프라의 세부 사항에 대해 걱정할 필요가 없음.
- 애플리케이션 계층 메세지를 전송 계층 패킷으로 바꾸면 전송 계층 세그먼트라고 이름이 바뀜
- 어플리케이션 메시지를 더 작은 조각으로 나누고 각 조각에 전송 계층 헤더를 추가하여 전송 계층 세그먼트를 생성하는 방식으로 이루어짐.
- TCP UDP가 있음

#### 3.1.1 전송 계층과 네트워크 계층의 관계
- 전송 계층은 프로토콜 스택에서 네트워크 계층 바로 위에 위치함
- 전송 계층 프로토콜은 서로 다른 호스트에서 실행 중인 프로세스 간의 논리적 통신을 제공하는데, 네트워크 계층 프로토콜은 호스트간의 논리적 통신을 제공함

#### 3.1.2 인터넷에서의 전송 계층 개요
- TCP의 전송 계층 패킷은 세그먼트라고 부르고 UDP의 패킷은 데이터그램이라고 부를 때도 있음 하지만 두 패킷 모두 세그먼트라고 해도됨
- 네트워크 계층은 IP으로 Best effort 전달 서비스임.
- IP가 통신하는 호스트 간 세그먼트를 전달하기 위해 '최선을 다한다' 라는 의미인데, 보장은 안한다.
- 세그먼트의 전달, 순서 전달, 세그먼트의 데이터 무결성 보장 않한다.
- 모든 호스트는 최소한 하나의 네트워크 계층 주소, 즉 IP 주소를 가지고 있음.
- UDP는 프로세스간 데이터 전달과 오류 검사를 하는데 이게 유일함 그 외에는 아무것도 안함 best-effort라서 IP와 마찬가지고 신회할 수 가 없다
- 반면 TCP는 신뢰할 수 있는 서비스를 제공하고 순서하로 전달할 뿐만 아니라 혼잡제어도 제공함
