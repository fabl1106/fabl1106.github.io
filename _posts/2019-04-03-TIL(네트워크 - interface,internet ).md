---
layout: post
title:  "2019_04_03_TIL(네트워크 - interface,internet)"
date:   2019-04-03 01:06:23 +0700
categories: [computer]
---

### 2019.04.03 TIL

(TIL은 스스로 이해한 것을 바탕으로 정리한 것으로 오류가 있을 수 있습니다)

\# 질문에 답하기

* 우리가 유튜브를 볼 때 일어나는 일들에 대해 최대한 상세하게 이야기 해보기    

# 네트워크의 시작
* 네트워크란 : 노드(데이터를 저장하는 공간)들이 서로를 공유할 수 있게 하는 통신망

1.  컴퓨터 네트워크의 등장
    1. 중앙 집중형: 하나의 노드가 파괴되면 한번에 파괴될 수 있음
    2. 분산형 : 중앙 집중형의 문제 해결

2. 패킷 통신 방식의 발달
    1. 데이터를 패킷 형태로 나누어서 전송 : 목적지에서 다시 패킹 작업을 해야함
    2. 네트워크 트래픽을 여러개의 컴퓨터가 공유할 수 있으므로 장점

* 분산 네트워크 + 패킷 통신 방식 ==> 인터넷의 기반
* 각각의 네트워크가 연결되며 인터넷의 구조를 가지게 되었다.
* 인터넷은 네트워크의 네트워크이다.

### 거대한 인터넷을 연결하고 데이터를 주고 받기 위한 규약이 필요해지게 되었다.

**분산 네트워크 + 패킷 통신 방식**

1. 네트워크 구조를 유지하기 위한 프로토콜 : IP(internet Protocol)
    1. 네트워크에 참여한 노드를 유일하게 확인해준다.
    2. 전체 네트워크에 식별 가능한 IP주소를 부여
    3. 노드와 노드 사이에서 경로 설정을 위한 라우터가 있음

2. 통신을 보장하기 위한 프로토콜 : TCP(transmission control protocol)
    1. 데이터는 여러 패킷조각으로 나누어 이동
    2. 패킷은 순서대로 오지 않을 수 있고, 훼손될 수 있다.
        1. TCP는 패킷을 재 조립하고 재 전송을 요청하는 등 흐름을 관리한다.

# OSI 7계층과 TCP/IP
* 인터넷 응용 개발을 위해 네트워크를 7계층으로 나눔
* OSI 모형(Open Systems Interconnection Reference Model)은 국제표준화기구(ISO)에서 개발한 모델로, 컴퓨터 네트워크 프로토콜 디자인과 통신을 계층으로 나누어 설명한 것이다. 일반적으로OSI 7 계층 모형이라고 한다.
* OSI 7계층을 통합하여 TCP/IP 4계층으로 통합

![TCP:IP](https://user-images.githubusercontent.com/46436843/55473720-4cf1e280-564a-11e9-94b5-26b7fa125582.jpg)

## OSI 7계층에서 꼭 알아야 하는 것들

* 응용 계층(Application Layer) : FTP, HTTP, SSH
* 표현 계층(Presentation Layer)
* 세션 계층(Session Layer)
* 전송 계층(Transport Layer) : TCP, UDP
* 네트워크 계층( Network Layer) : IP
* 데이터 링크 계층(Data Link Layer) : Ethernet, NIC
* 물리 계층(Physical Layer)

# Network interface(TCP/IP) : MAC Address를 다룬다.

* Network interface는 컴퓨터에서 internet(IP Address)까지 가는 것을 다룬다.(LAN)
* 물리 계층(physical Layer)
* 데이터 링크 계층(data link Layer)

## Physical Layer

* 컴퓨터 2대를 연결하는 것
* 서로의 컴퓨터에 LAN 케이블을 연결하여 스타크래프트 접속
* 눈에 보이는 것을 이야기한다.
*  랜케이블
* IPTIME
    * 라우터 기능 : 
    * 스위칭 기능 : twist가 되도록 해준다. 2개의 랜을 연결하면 알아서 스위치 해준다.
    * 리피터 기능 : 거리가 멀어지면 노이즈가 생기고 신호가 약해지므로 signal 증폭

## Data link Layer

* NIC(network interface card) : 일반적으로 랜 카드를 말하며, 네트워크 어댑터이다.
* MAC(media access control) 
    * NIC의 하드웨어 주소
    * 이 세상의 모든 NIC은 모두 독자적인 MAC주소를 가진다.
* 이더넷 프로토콜(ethernet protocol) - (LAN영역을 책임진다.)
    * 물리 계층(랜 케이블)과 네트워크 계층(IPTime)간의 LAN 통신을 책임진다.
    * Destination MAC Address : 6byte, 패킷 수신 NIC
    * Source MAC Address : 6byte, 패킷 송신 NIC
    * MTU(maximum trasmission unit) : 1500byte
        * 각각 또 헤드가 붙어야 되기 때문에 application에서 붙이는 데이터는 최대 1350바이트 정도 까지 가능하다.

# Internet(TCP/IP) : IP address를 다룬다.

* host에서 요청한 ip를 IPTIME(라우터)로 보내고 정보를 다시 라우터로 회신해 왔을 때
* 돌아온 데이터를 다시 라우터에서 host로 보낼 때 mac 주소가 필요하다.
* 네트워크 계층(Network Layer)

## 네트워크 계층 

* SK : ISP(internet service provider)
    * 라우터에 공인 ip주소를 할당해준다.
* internet은 IP address를 다루고 Network interfac는 맥 address를 다룬다.

### ARP (address resolution protocol) : 주소 결정 프로토콜

* 브로드캐스트로 어떤 IP를 사용하는 호스트의 MAC 주소를 알아낸다.
* 브로드캐스트(모두에서 보낸다.)
    * host1과 host2는 IPTIME으로부터 사설 ip주소를 부여 받는다.
    * host1이 host2로 데이터를 보내고자 할 때 사설 ip주소 밖에 모른다.
    * 따라서 먼저 패킷을 모두에게 쏜다. (패킷에는 보내는 사람의 mac address와 사설 ip주소 그리고 받는 사람의 mac address(00 00 00 00 - 모든 사람)와 사설 ip주소가 담겨있다.
    * 만약에 받는 사람의 ip주소가 아니면 패킷을 버린다.(host3)
    * host2는 받는 사람의 사설 ip가 동일하므로 이것에 대한 response로 mac address를 회신해준다. 
    * 그럼 그것을 host1이 다시 받아서 다시 데이터와 함께 전송해준다.
    * 매번 브로드캐스트 하는 과정을 매우 비효율적이므로 한번 IP주소가 mac address가 등록된 것은 ARP cache table로 담아놓는다.  
    * ARP cache table reset -> ping을 활용해 같은 라우터에 연결된 모든 컴퓨터의 맥주소를 받을 수 있다.

### IP 프로토콜

* Source address : 내 ip주소
* Destination address : 내가 data를 요청한 곳의(예 : youtube) ip주소
* TTL(time to live)
    * 만약에 서버가 없어졌다고 하면 언제 패킷을 버릴 것이다.
    * 패킷은 지속적으로 돌아다닐 수 있다. 라우터를 하나 지날 때마다 TTL을 하나씩 줄임
    * TTL이 0이 되는 순간 도착 했는 것과 상관없이 무조건 버린다.
* Protocol : 몇개를 TCP. 몇개를 UDP로 할 것인가?

###  IPV4 (IP version 4 : 4byte로 ip주소 표현)

* 기본 틀 : 1byte - 1byte - 1byte - 1byte ( 0 ~ 255, 0 ~ 255, 0 ~ 255, 0 ~ 255)

<img width="886" alt="IP class" src="https://user-images.githubusercontent.com/46436843/55488715-89820600-566b-11e9-920e-73b0e18f55ed.png">

1. 네트워크 주소 : 라우터 주소
2. Host 주소 : lan에 소속되어 있는 어떤 특정 호스트를 찾는 주소(컴퓨터)

class A : 네트워크는 2 ** 8 승 만큼 host는 2 ** 24 까지 부여 가능 ( 초창기)

* 네트워크 주소가 0으로 시작 . 뒤에는 01111111 까지 가능해서 127까지 

class B : 네트워크는 2 ** 16 승 만큼 host는 2 ** 16 까지 부여 가능 ( 네트워크가 늘어나고 있음)

* 네트워크 주소가 10으로 시작 
* 10 / 00 0000 부터 10 /11 1111 까지 (128 ~ 191까지)

class C: 네트워크는 2 ** 24 승 만큼 host는 2 ** 8 까지 부여 가능 ( 라우터의 수가 굉장히 늘어남)

* 네트워크 주소가 110으로 시작
* 110/0 0000 부터 110/1 1111 까지 ( 192 ~ 223)까지
* host는 255가 아니라 2명을 뺴서 253개 까지 배당가능 (ping(broadcast 255)와 나 자신(0))

### 서브넷 마스크

ipv4의 공인 주소가 고갈되어 감으로서 그 속도를 늦추고 host 부분을 쪼개어 낭비되는 공인주소가 없도록 하기 위한 방법

예를 들어 class C에서 host가 253명까지 가능한데 부서가 여러개로 나누어져 있다면 각 부서마다 공인 주소를 부여해줘야하는 문제가 발생할 수 있다. 이러면 공인 주소의 낭비문제가 발생한다. 따라서 host 부분을 쪼개어 최대 4개의 부서에 까지 배정할 수 있도록 함으로서 이러한 낭비를 줄이도록 한다.

class C : 

* host 가 253명까지 가능하나 규모가 어느정도 커지면 부서별로 따로 만들어줘야 한다. 따로 부서별로 공인 IP를 하면 되지만 비싸고 낭비이다. (회사내 총 인원 240명 / 각 부서별 60명 )
* class C에 속하는 공인 IP를 하나만 쓰되 IP의 Host를 다시 쪼갠다.
* host 8bit를 더 쪼갠다
* ㅡ ㅡ / ㅡ ㅡ ㅡ ㅡ ㅡ ㅡ (subnet 2개 와 host 6개로 쪼갠다.)
* subnet은 00 / 01/ 10/ 11 (4개 구성 가능하다)
* 각 나눈 subnet마다 2 ** 6만큼 host 배정가능
* host 입장에서는 subnet이 몇개 나누어졌는지 알아야 한다.(서버 담당자)
* subnet mast를 통해 어디까지가 호스트 주소인자 알 수 있다.
* ifconfig 
    * en0 inet : 201.175.122.74
    * subnetmask: 0xffffffc0

서브넷 마스크 해석 방법

<img width="851" alt="서브넷 계산" src="https://user-images.githubusercontent.com/46436843/55479599-68181e80-5659-11e9-9e63-d36907570f5f.png">

* 201.175.122.74 (ip 주소)
* 1100 1001 / 1010 1111 / 0111 1010 / 0100 1010
* subnetmask 0xffffffc0
* 255.255.255.192           (만약에 서브넷 마스크가 255.255.255.000 이면 무조건 안 나눈 것)
* 1111 1111 / 1111 1111/ 1111 1111/ 1100  0000
* 1100 1001/  1010 1111/ 0111 1010/ 0100 0000   —> and로 계산 
* **위를 통해 나온 IP주소를 서브넷 네트워크주소이며 이것을 바탕으로 2 \** 6 명의 호스트에게 하나씩 할당한다.**
* 201.175.122.74/26 (서브넷마스크의 1의 갯수 — 24개면 나누지 않은 것이다)


### IP

지속적으로 ipv4의 공인 주소가 고갈되어 가면서 해결책으로 ipv6(6byte로 표현)가 나오게 되었다.    
하지만 일단 ipv4를 완벽히 대체하기 까지 시간이 필요했고 그 해결책으로 공인과 사설 ip주소를 할당하게 되었다.

1. Public ip (공인 ip 주소)
2. Private up (사설 ip 주소)

<img width="853" alt="사설 IP" src="https://user-images.githubusercontent.com/46436843/55484733-fe514200-5663-11e9-92bc-2bcc6fac1f16.png">

위에 그림에서 보는 것 처럼 현재는 모두 class C이므로 192.168.0.0 부터 192.168.255.255 까지는 즉 2 ** 8 개 까지는 따로 할당하지 않고 사설 IP로 쓸 수 있도록 해놓았다.

공인 ip를 sk에서 사놓고 하나씩 배정해주는 것이다.    
최종적으로 ip를 관리하는 단체가 있다. 나 공인 ip 너에게 팔건데 위와 같은 부분은 팔지 않을거야.    

IP는 유일해야 한다. 근데 왜 장소를 바껴도 일치하냐?라는 질문에 답변을 하면   
사설 IP를 할당받아 사용하기 때문이다.

sk에서 커피숍에 IP time을 팔고나면 NIC가 2개 들어있는데 NIC1개에 ISP를 준다. 공인 ip주소이다.     
NIC 1개는 private ip address를 묶어 둔다.    

따라서 라우터는 LAN인터페이스 하나(사설 ip)와 WAN 인터페이스(공인 ip) 1개 이렇게 2개가 묶여 있다.

<img width="731" alt="router" src="https://user-images.githubusercontent.com/46436843/55487516-3444f500-5669-11e9-8701-cbf0d75dd933.png">

카페에 사람들이 방문하면 라우터는 호스트들에게 192.168.0.3 / 192.168.0.4 / 이렇게 나누어서 준다.    
이것은 private ip라고 한다(공인 ip는 전 세계에서 유일하다). 
사설 ip는 iptime이 알아서 할당해준다. (위의 대역대 중에서 마음대로 할당해준다)      

private network 상에 private ip가 있다.
private ip는 DHCP(Dynamic Host configuration protocol) 가 알아서 제공해준다.

커피숍에 앉아서 유튜브를 하면 나는 유튜브에 바로 접속할 수 있지만 유튜브는 불가능하다.    
왜냐하면 나에게는 사설IP가 없기 때문이다.   
사설 ip를 받은 애는 반드시 유튜브에서 보낸 data가 iptime을 거쳐서 와야한다.
실제로 만약에 host가 공인 IP가 있고 그 공인 ip를 알고 있으면 미국에서 바로 내 컴퓨터로 데이터를 보낼 수 있다.  

우리 컴퓨터에는 개개인의 ip주소가 없고 mac주소만 있다. 
ip주소는 우리가 속해 있는 라우터에 따라 매번 바뀐다.

랜 안에 속해 있는 사람끼리는 ARP를 통해서 맥주소를 알 수 있지만 유튜브에서 나를 특정해서 보낼 수 없다.    
밖에 있는 사람들이 우리를 특정하기 위해서는 NAT를 통해야 한다.

#### 내가 지금 속해있는 라우터의 주소 확인 방법

"traceroute amazon.com" - 우리가 방문하는 라우터를 추적하는 코드        
이렇게 해서 제일 처음 나오는 ip주소가 내가 속해 있는 라우터의 공인 ip 주소이다.       
whois 112.188.32.213 이것을 통해서 이 ip주소의 주인이 누군지 알 수 있다.


WAN mac주소와 (공인 ip에 묶여있는 맥주소)
LAN mac주소가 따로 있다. (private ip에 묶여 있는 맥주소)
이게 라우터가 2개 있다는 말이다.

내가 속해 있는 iptime이 또 거쳐서 나가는게 기본 게이트웨이 이다.

[TCP/IP 주소 지정 및 서브넷 구성 기본 사항의 이해](https://support.microsoft.com/ko-kr/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics)


## NAT (Network Address translation : 네트워크 주소를 바꾼다)

<img width="924" alt="NAT" src="https://user-images.githubusercontent.com/46436843/55487907-f8f6f600-5669-11e9-83d2-cd7e122b9342.png">

나 사설 ip긴 한데 내 ip주소는 저거고 도착지는 google이야    
나와 라우터는 LAN으로 묶여 있다. 사설 주소는 원래 못 나가는게 원칙이라 라우터가 바꿔준다.     
사설 ip와 공인 ip를 맵핑하고(변환테이블) 구글에 쏜다.    
이때 공인 ip는 어디든지 알 수 있으니깐 라우터의 공인 ip로 쏜다.(구글에서 접근 가능)     
구글에서 다시 돌아온 데이터는 변환 테이블을 보고 라우터가 ARP를 활용하여 나의 맥주소로 바꾼후 나에게 보내준다.

— 정말 옛날 시스템이다. 라우터가 2개의 공인 ip가 있으면 맵핑을 할 때 2명밖에 불가능하다.

## 해결책 NAPT (Network Address Port Translation)
* 라우터의 NIC가 딱 2개고 하나는 LAN 하나는 WAN에 물려있다.

<img width="901" alt="napt" src="https://user-images.githubusercontent.com/46436843/55488053-45423600-566a-11e9-8e60-2d690a84afb9.png">

라우터가 받은 다음 맵핑을 하되 포트까지 기록을 해놓는다.    
기본적으로 맵핑하는 공인 ip와 사설 ip를 동일한 포트번호로 쓰지만   
192.168.0.2 : 6000번과 192.168.0.4 : 6000번이 동시에 요청하면 포트번호를 한개는 6060으로 바꾸어서 저장한다.

이것을 통해서 우리가 같이 유튜브 영상을 볼 수 있고     
대신 밖에 있는 사람은 우리에게 직접적으로 무언가를 요청할 수 없다.

나는 유튜브의 공인 주소를 알고 있으므로 라우터를 거치지 않고 바로 data를 request할 수 있다.    
근데 유튜브는 나의 공인 주소를 알 수 없으므로 특정해서 데이터를 요청할 수 없다.

## 이때 해결책이 port forwarding이다.

라우터야 내 사설 ip가 192.168.0.2:6060인데 너 혹시     
217.111.222.333:1010을 나한테만 좀 걸어줄 수 있어?   

**1:1로 바인딩해놓으면 다른 사람들은 이 포트를 못 쓰므로 유튜브가 위로 요청하면 나에게 요청하는것과 100% 일치한다.**
