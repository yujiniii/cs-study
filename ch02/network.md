# 네트워크

네트워크란 노드와 링크가 서로 연결되어 있거나, 연결되어 있으며 리소스를 공유하는 집합을 의미

- 노드(node) : 서버, 라우터, 스위치 등 네트워크 장치
- 링크(link) : 유선 또는 무선

### 좋은 네트워크란?

많은 처리량을 처리할 수 있으며, 지연 시간이 짧고, 장애 빈도가 적으며, 좋은 보안을 갖춘 네트워크

![image](/images/throughput.jpg)

- 처리량(throughput) : 링크를 통해 전달되는 단위 시간당 데이터 량 (bps, bits/sec)
  - 트래픽, 대역폭, 에러, 하드웨어 스펙 등에 영향을 받음
- 트래픽 : 사람들이 많이 접속할 때마다 커짐
- 대역폭 : 주어진 시간 동안 네트워크 연결을 통해 흐를 수 있는 최대 비트 수

### 네트워크 프로토콜

다른 장치들까리 데이터를 주고받기 위해 설정된 공통된 인터페이스  
IEEE, IETF 등의 표준화 기관에서 정함

> **중요한 아이들**  
> `IEEE802.3` : 유선 LAN 프로토콜, 이거 덕분에 만든 기업이 다르더라도 데이터 수신 가능  
> `HTTP` : 웹 접속시 사용, 약속된 인터페이스, 웹서비스로 노드끼리 데이터 송수신 가능

## 지연시간(latency)

요청이 처리되는 시간  
어떤 메시지가 두 장치 사이를 "왕복" 하는데 걸린 시간  
매체 타입(유무선), 패킷 크기, 라우터의 패킷 처리 시간에 영향을 받음

## 네트워크 토폴로지(network topology)

노드와 링크가 어떻게 배치되어 있는지에 대한 방식이자, 연결 형태를 의미
|유형|특징|
|:---:|---|
|트리(tree) 토폴로지<br> == <br>계층형 토폴로지|노드의 추가/삭제 간단 <br> 노드에 트래픽이 집중될 때 하위 노드에 영향을 줄 수 있음|
|버스 토폴로지<br>(bus) |보통 LAN(근거리 통신망)에 사용<br>설치 비용이 적고 신뢰성 우수, 노드 추가/삭제 간편<br>스푸핑이 가능한 문제|
|스타 토폴로지<br>(star)| 노드를 추가하거나 에러를 탐지하기 쉽, 패킷 충돌 발생 가능성 적음 <br> 장애가 발생한 노드 찾기 쉽 , 중앙노드만 아니면 장애 영향도 ㄱㅊ<br>중앙 노드에 장애가 발생하면 전체 사용 불가, 비용 고가|
|링형 토폴로지<br>(ring)|노드에서 노드로 이동<br>노드 수가 증가해도 네트워크 손실 적, 충돌 가능성 적, 고장시 쉽게 발견<br>네트워크 구성 변경 어렵, 회선에 장애-> 전체 와장창|
|메시(mesh) 토폴로지<br>==<br>망형 토폴로지|장애 발생해도 ㄱㅊ, 트래픽 분산처리 ㄱㄴ<br>노드 추가 어렵, 구축 및 운용 비쌈|

> **스푸핑**  
> `스위칭 기능`을 마비시키거나 속여서 특정 노드에 해당 패킷이 오도록 처리하는 것  
> 올바르게 수신부로 가야 할 패킷이 악의적인 노드에 전달되게 된다

> **스위칭 기능**  
> LAN 상에서 송신부의 패킷을 송신과 관련 없는 다른 호스트에 가지 않도록 한다.

### 병목(bottleneck) 현상

전체 시스템의 성능이나 용량이 하나의 구성요소로 인해 제한을 받는 현상  
서비스에서 이벤트를 열었을 때, 트래픽이 많이 생기고 그 트래픽을 잘 관리하지 못하면 병목현상이 생겨 사이트 접속이 불가해진다.

- **이를 방지하기 위해 네트워크 토폴로지를 올바르게 선택하는 것이 중요하다!**

## 네트워크 분류

> 아래로 갈 수록 혼잡해지고 속도 느려짐

- LAN (Local Area Network) : 근거리 통신망 / 같은 건물, 캠퍼스 내 등 / 빠르다  
  \\/
- MAN (Metropolitan Area Network) : 대도시 지역 네트워크 / 도시 / 평균  
  \\/
- WAN (Wide Area Network) : 광역 네트워크 / 국가 또는 대륙 / 전송 속도 낮음

## 네트워크 상태 알아보기

**일반적인 네트워크 장애 원인**

- 네트워크 대역폭
- 네트워크 토폴로지
- 서버 CPU, 메모리 사용량
- 비효율적인 네트워크 구성

### ping(Packet INernet Groper)

네트워크를 확인하려는 대상 노드를 향해 일정 크기의 패킷을 전송  
정보 : 노드에 패킷 수신 상태, 도달하기까지 시간, 해당 노드까지 잘 연결돼있는지  
TCP/IP - ICMP 프로토콜 (해당 프로토콜을 지원하지 않으면 사용할 수 없음)

```bash
ping [ip or domain] -n [패킷 개수]
```

### netstat

접속되어 있는 서비스들의 네트워크 상태 표시  
정보 : 네트워크 접속, 라우팅 테이블, 네트워크 프로토콜, 포트확인 등

```bash
netstat
```

### nslookup

DNS 관련 내용 확인  
정보 : 특정 도메인에 매핑된 IP 등

```bash
nslookup
..
> [domain name]
..
```

### tracert/traceroute

목적지 노드까지 네트워크 경로를 확인할 때 사용
정보 : 목적지 노드까지 구간들 중 어느 구간에서 응답이 느려지는 지, ftp로 대형 파일을 보내 테스팅, tcp dump 를 통해 패킷을 수집할 수 있

```bash
# Linux
traceroute [domain name]

# Windows
tracert [domain name]
```