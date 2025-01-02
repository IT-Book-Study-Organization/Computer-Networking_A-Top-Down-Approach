# 1.7 컴퓨터 네트워킹과 인터넷의 역사

### 1.7.1 패킷 교환의 발전: 1961-1972

   
1960 - 전화망이 세계 주요 통신 네트워크였다. 회선 교환 방식으로 송/수신자 간의 정보를 전송하였다.
1960 초 - 컴퓨터의 중요성이 증가하며 시분할(timeshared) 컴퓨터가 등장했다. 
> 이때, 사용자들이 생성하는 트래픽은 **bursty**일 가능성이 높았다. **이는 활동의 간헐적인 간격, 데이터 전송에서의 변동성을 나타낸다.**   
-> 이를 해결하기 위해 **패킷 교환 기술**이 개발되기 시작한다.
   
1. 1961 - 패킷 교환 기술이 논문으로 최초 발표 [Kleinrock 1961]
   - MIT의 Leonard Kleinrock는 큐잉 이론을 사용하여 bursty traffic 에 대한 패킷 교환 기술의 효율성을 입증한다.
2. 1960 중반 - Paul Baran과 Donald Davies가 독립적으로 패킷 교환 개념을 발명
   - Baran은 군사 네트워크에서의 안전한 음성 전송을 위해 패킷 교환을 연구했다.
   - Davies는 대화형 시분할 시스템의 데이터 통신 개선을 위해 이 개념을 제안했다.
3. 1969 10월 - 현대 인터넷의 직접적인 선조
   - ARPAnet의 첫 노드가 UCLA와 Stanford Research Institute (SRI) 사이에 연결되었다.
   - 1972년 10월, Robert Kahn이 워싱턴 D.C.의 힐튼 호텔에서 ARPAnet의 첫 공개 시연을 진행했다.
  
       
---   


### 1.7.2 독점 네트워크 및 인터넷워킹: 1972-1980
초기 ARPAnet은 단일 폐쇄형 네트워크로 시작되었다. 다른 ARPAnet IMP에 물리적으로 연결되어야만 통신이 가능했으나, 점차 여러 독립적인 네트워크들이 등장하며 네트워킹의 범위가 확장되었다. 
1. ALOHAnet
   - 1970년 하와이 대학교에서 개발된 무선 패킷 교환 네트워크
   - 하와이 섬들 간 대학을 연결하는 마이크로파 네트워크로 작동 [Abramson 1970]
2. DARPA의 패킷 위성 및 라디오 네트워크 [RFC 829] [Kahn 1978]
3. Telenet
   - ARPAnet 기술을 기반으로 BBN(Bolt Beranek and Newman)이 설계
4. Cyclades
   - 프랑스에서 Louis Pouzin이 선도한 패킷 교환 네트워크 [Think 2012]
   - 인터넷 프로토콜 IP 개념에 중요한 영향을 미침
5. Tymnet 및 GE 정보 서비스 네트워크
   - 시분할 시스템 기반의 네트워크로, 1960~1970년대 등장 [Schwartz 1977]
6. IBM의 SNA(System Network Architecture)
   - ARPAnet과 유사한 시기에 병행하여 발전 [Schwartz 1977]   
     
네트워크 수가 증가하면서 네트워크를 연결하기 위한 포괄적인 아키텍처를 개발하게 되었다.    
Vinton Cerf와 Robert Kahn은 미국 국방고등연구계획국(DARPA)의 후원을 받아 네트워크를 상호 연결하는 작업을 수행하며 'internetting'이라는 용어를 만들었다.           


**TCP/IP의 초기 개발** 
- Cerf와 Kahn은 1974년 IEEE 논문 "A Protocol for Packet Network Intercommunication"에서 TCP(Transmission Control Protocol)를 처음으로 제안했다.
- 초기 TCP는 데이터의 신뢰성있는 순차적 전달과 패킷 전달 기능 둘다 포함했다. 
> 그러나, 실험을 통해 패킷화된 음성 애플리케이션에는 **비신뢰성 및 흐름 제어가 없는 전송 서비스**가 중요하다고 인식되었음   
   **-> 이에 따라 IP가 분리되고 UDP(User Datagram Protocol)가 개발되고 TCP, UDP, IP라는 세 가지 주요 인터넷 프로토콜이 자리 잡음.**


_DARPA 연구 외에도 진행중인 많은 중요한 네트워크가 있었다._ 


바로 하와이의 원격 사이트가 서로 통신할 수 있도록 하는 패킷 기반 무선 네트워크인 ALOHAnet. ALOHA프로토콜은 지리적으로 분산된 사용자들이 라디오 주파수를 공유할 수 있게 해준 **최초의 다중 접속 프로토콜**이다.   
   Melcalfe와 Boggs가 유선 기반의 공유 방송 네트워크를 위한 **Ethernet 프로토콜**[Metcalfe 1976]을 개발했다. 이더넷 프로토콜은 여러대의 PC, 프린터를 연결해야 할 필요성에서 비롯되었다.  
   
   > **이러한 초기 연구는 특히 무선 통신과 LAN의 발전에 중요한 기여를 했다.   ALOHAnet과 Ethernet은 오늘날 우리가 사용하는 다양한 컴퓨터 네트워크 기술의 기초가 되었다.**


---   


### 1.7.3 네트워크의 확산: 1980-1990   
1970년대 말 ARPANET에 연결된 호스트는 약 200개였으나, 1980년대 말에는 공공 인터넷에 연결된 호스트 수가 10만 개에 이르렀다. 이 성장은 대학 간 네트워크를 연결하려는 노력에서 비롯되었다.   
<주요 네트워크와 발전>   
1. BITNET (Because It’s Time Network): 북동부의 여러 대학에 이메일과 파일 전송을 제공
2. CSNET (Computer Science Network): ARPAnet에 접근할 수 없는 대학 연구자들을 연결하기 위해 설립
3. NSFNET (National Science Foundation Network): NSF가 후원하는 슈퍼컴퓨팅 센터에 접근하도록 설립

           
1983년 1월 1일, 공식적으로 TCP/IP가 ARPAnet의 새로운 표준 호스트 프로토콜로 채택됨. 이전에 사용하던 NCP를 대체한 것으로, "flag day"로 알려져 있음. 모든 호스트는 이 날까지 TCP/IP로 전환해야 했다.   
> Van Jacobson은 혼잡 제어 알고리즘을 TCP에 통합하여 네트워크 혼잡 문제를 해결하고 성능을 개선      
> 1983년 **도메인 이름 시스템(DNS)** 이 도입되어 인간이 읽을 수 있는 **도메인 이름**(예: gaia.cs.umass.edu)을 **IP주소**로 매핑하는 체계가 마련되었다.               


1980년대 초, 프랑스는 미니텔(Minitel) 프로젝트를 시작했다.
  - 목표: 데이터 네트워킹을 모든 가정으로 확산시키는 것
  - 공공 패킷 교환 네트워크(X.25 프로토콜 기반), 미니텔 서버, 저속 모뎀이 내장된 저렴한 터미널로 구성됨
  - 프랑스 정부가 가정에 무료로 제공하면서 큰 성공
  - 20,000개 이상의 다양한 서비스가 제공됨   

    
---    

   
### 1.7.4 인터넷 폭발: 1990년대
**주요 사건**   
1. ARPANET의 종료: 인터넷의 선구자인 ARPANET이 공식적으로 해체
2. NSFNET의 상업적 이용 허용: 1991년 NSFNET은 상업적 목적의 사용 제한을 해제
3. NSFNET의 해체: 1995년 NSFNET이 해체되었고, 인터넷 백본 트래픽은 상업 인터넷 서비스 제공업체(ISP)들에 의해 처리되기 시작

       
> **월드 와이드 웹의 등장**   
웹은 전 세계 수백만 명의 가정과 기업에 인터넷을 보급
   - 검색 엔진: Google, Bing
   - 인터넷 상거래: Amazon, eBay
   - 소셜 네트워크: Facebook
     
**-> 이러한 발전은 인터넷의 폭발적인 성장과 상업화를 이끌었으며, 현대 디지털 시대의 기반을 마련**   
1989~1991 Tim Berners-Lee는 월드 와이드 웹을 발명   웹의 주요 구성 요소는 다음과 같다.
   - HTML (하이퍼텍스트 마크업 언어)
   - HTTP (하이퍼텍스트 전송 프로토콜)
   - 웹 서버
   - 웹 브라우저

         
1993년 말까지 약 200개의 웹서버가 운영되고 있었고, 그래픽 사용자 인터페이스(GUI)를 갖춘 웹 브라우저를 개발하기 시작. Marc Andreessen은 Jim Clark와 함께 **Netscape Communications Corporation을 공동 설립함**    
1995년까지 대학생들은 Netscape 브라우저를 사용하고 있었고, 기업들은 웹 서버를 운영하고 전자상거래를 시작함    
이때, 1996년 Microsoft가 브라우저 시장에 진출하면서 **브라우저 전쟁이 시작되었고, Microsoft가 승리함.**    


1990년대 후반은 인터넷과 서비스에 대한 엄청난 성장과 혁신이 있었음    
인터넷은 수백 개의 인기 있는 애플리케이션을 지원하고 있었음     
   1. 이메일(첨부 파일 포함)
   2. 웹(브라우징과 인터넷 상거래)
   3. 인스턴트 메시징(연락처 목록 포함)
   4. MP3 파일의 P2P 공유(선구자는 Napster)         


수백 개의 인터넷 스타트업들이 기업공개(IPO)(기업이 주식을 상장하는 방법 중 하나)를 하고 주식 시장에서 거래됨    
그러나, 2000년에 인터넷 주식이 폭락하면서 많은 스타트업들이 문을 닫고, 닷컴 버블이 터지면서 NASDAQ지수가 77%가량 대폭 하락함    
       
> **-> 그럼에도 Microsoft, Cisco, Yahoo, eBay, Google, Amazon 등은 버블 붕괴 이후에도 살아남아 인터넷 산업을 주도하게 됨**


---    

    
### 1.7.5 새로운 천년    
21세기 첫 20년 동안 인터넷에 연결된 스마트폰만큼 사회를 변화시킨 기술은 없었다.   
하지만 다음과 같은 발전은 주목 받을만한 가치가 있다        
1. 고속 무선 인터넷 접속의 빠른 보급(케이블 모뎀과 DSL, 5G 고속 무선 접속):
   - 사용자 제작 동영상 배포 (예: YouTube)
   - 영화 및 TV 프로그램의 주문형 스트리밍 (예: Netflix)
   - 다자간 화상 회의 (예: Skype, Facetime, Google Hangouts)
2. 고속 무선 인터넷 접속의 보편화로 인해:
   - 이동 중에도 지속적인 연결이 가능해짐
   - Yelp, Tinder, Waze 등 위치 기반 애플리케이션이 등장
   - 스마트폰, 태블릿 등 휴대용 컴퓨터의 급속한 보급
3. 온라인 소셜 네트워크의 발전:
   - Facebook, Instagram, Twitter, WeChat등이 거대한 인적 네트워크 형성
   - API를 통해 모바일 결제, 분산 게임 등 새로운 네트워크 애플리케이션의 플랫폼 역할
4. Google, Microsoft 등 온라인 서비스 제공업체의 변화:
   - 전 세계에 분산된 데이터 센터를 연결하는 자체 프라이빗 네트워크 구축
   - 하위 계층 ISP와 직접 연동하여 가능한 한 인터넷 우회
   - 검색 결과와 이메일 접근을 거의 즉각적으로 제공
5. 클라우드 컴퓨팅의 확산:
   - 많은 인터넷 상거래 기업들이 Amazon EC2, Microsoft Azure, Alibaba Cloud 등에서 애플리케이션 운영        
   - 기업과 대학들도 이메일, 웹 호스팅 등의 인터넷 애플리케이션을 클라우드로 이전
   - 클라우드 기업들은 확장 가능한 컴퓨팅 및 스토리지 환경과 함께 고성능 프라이빗 네트워크 접근도 제공




