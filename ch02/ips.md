## 인터넷 프로토콜 스위트(internet protocol suite)

인터넷에서 컴퓨터들이 서로 정보를 주고받는데 쓰이는 프로토콜의 집합  
TCP/IP 4계층 모델 혹은 OSI 7계층모델로 설명

# TCP/IP 4계층

각 계층은 특정 계층이 변경되었을 때 다른 계층이 영향을 받지 않도록 설계  
`어`->`전`->`인`->`링` 요청값들이 캡슐화 과정을 거쳐 서버로 통신  
->`링`->`인`->`전`->`어` 통신으로 받은 요청이 비캡슐화 과정을 거쳐 확인

> **캡슐화**  
> 상위 계층의 헤더와 데이터를 하위 계층의 데이터 부분에 포함시키고 해당 계층의 헤더를 삽입하는 과정
> ![image](/images/capsule.png)

## 어플리케이션 계층

- 응용프로그램이 사용되는 프로토골 계층, 웹서비스, 이메일 등 서비스를 실질적으로 사람들에게 제공하는 계층
- protocol : FTP, HTTP, SSH, SMTP, DNS
- PDU : 메세지

## 전송 계층

- 전송계층은 송신자와 수신자를 연결하는 통신 서비스를 제공
- 연결 지햐 데이터 스트림 지원, 신뢰성, 흐름제어를 제공, 중계기 역할
- protocol : TCP, UDP, QUIC
- PDU: 세그먼트(TCP), 데이터그램(UDP)

## 인터넷 계층

- 장치로부터 받은 네트워크 패킷을 IP주소로 지정된 목적지로 전송하기 위해 사용되는 계층
- 패킷을 수신해야야 할 상대의 주소를 지정하여 데이터를 전달
- 상대방이 제대로 받았는지에 대해 보장하지 않는 비연결형적인 특징
- protocol : IP, ARP, ICMP
- PDU : 패킷

## 링크 계층

- 전선, 광섬유, 무선 등으로 실질적으로 데이터를 전달
- 장치간의 신호를 주고받는 규칙을 정하는 계층
- protocol : 유선랜(IEEE 802.3)
- PDU : 프레임(데이터링크), 비트(물리)

#OSI 7계층