# 2.4 DNS - 인터넷 디렉토리 서비스
사람들이 여러 가지 방식으로 식별될 수 있는 것처럼, 인터넷 호스트도 가능. 호스트를 식별하는 한 가지 방법은 호스트 이름임 (http://www.google.com/)

호스트 이름은 가변 길이의 영숫자로 구성되기에 라우터가 처리하기 어려울 수 있음. 그렇기에 호스트는 IP주소라고 불림

---

### 2.4.1 DNS에서 제공하는 서비스
호스트를 식별하는 두 가지 방법: 호스트이름과 IP주소

**hostname(www.naver.com)을 IP주소(125.209.222.141)로 변환하는 디렉토리 서비스가 필요함. => 인터넷 도메인 네임 시스템(DNS)의 주요 작업**

☆ DNS의 두 가지 특징 ☆
1) 계층 구조로 된 분산 데이터베이스
2) 애플리케이션 계층 프로토콜

그외 기능:

**1 호스트 별칭 지정(Host Aliasing)**

 - 복잡한 호스트 이름을 가진 호스트에 하나 이상의 별칭 이름을 부여할 수 있음
 - 실제 네이버의 호스트 이름은 www.naver.com.nheos.com인데 별칭으로 www.naver.com사용하는 것임

   
2 메일 서버 별칭 지정(간단한 이메일 주소 사용)


**3 부하 분산(Load Distribution):** 복제된 웹 서버 간의 부하를 분산하는 데 사용(트래픽이 많은 사이트는 여러 서버에 복제되어 있으며, 각 서버는 다른 IP주소를 가짐)


   - IP주소 세트: 복제된 웹 서버는 하나의 호스트 별칭에 여러 IP주소가 연결되는데, DNS 데이터베이스는 이러한 세트를 포함함
   - DNS 쿼리 응답: 클라이언트가 IP주소 세트를 요청하면, DNS서버는 전체 IP주소의 순서를 회전시켜 응답으로 반환
   - 트래픽 분산: DNS 회전을 통해 트래픽이 복제된 서버 간에 분산됨

---
### 2.4.2 DNS 작동 개요
  1. 호스트 이름 요청: 호스트이름을 IP주소로 변환해야할 때, 애플리케이션은 gethostbyname() 함수 호출을 통해 변환 요청을 수행
  2. DNS 쿼리 전송: DNS클라이언트는 쿼리 메시지를 UDP 데이터그램(포트 번호 53)으로 전송함
  3. DNS 응답 수신: 지연 시간에 따라 DNS클라이언트는 매핑 정보를 포함한 응답을 받고 애플리케이션에 전달됨

##### 중앙 집중형 설계의 문제점
1. 단일 장애 지점(Single Point of Failure): 중앙 DNS 서버가 다운되면 인터넷 전체가 영향을 받음.
2. 트래픽 과부하(Traffic Volume): 모든 HTTP 요청에서 발생하는 수많은 DNS 쿼리를 단일 서버가 처리해야 하므로, 트래픽 과부하가 발생
3. 거리 문제(Distant Centralized Database): 중앙 서버가 특정 위치(예: 뉴욕)에 있다면, 호주와 같은 먼 지역에서 오는 쿼리는 지구 반대편으로 이동해야 함
  -> 이는 느리고 혼잡한 링크를 통해 전송될 수 있어 상당한 지연을 초래함
4. 유지 관리(Maintenance): 중앙 서버는 인터넷에 존재하는 모든 호스트의 기록을 유지해야하므로 새로운 호스트 추가 시 빈번히 업데이트해야함 

**★ 분산형 계층적 데이터베이스 ★**
DNS는 전 세계에 분산된 다수의 서버를 사용하며, 이는 계층적으로 조직되어있음. 

크게 세 가지 계층으로 나뉨: 
![image](https://github.com/user-attachments/assets/a5fc7e24-1320-4e09-ba1e-a0a70407f087)


 **1. 루트 DNS 서버 (Root DNS Servers)**
DNS 계층 구조의 최상위에 위치하며, TLD(최상위 도메인) 서버의 IP 주소를 제공, 요청이 들어오면 .com, .org, .net 등 특정 TLD 서버로 요청을 전달
 
 **2. 최상위 도메인(TLD) DNS 서버 (Top-Level Domain DNS Servers)**
각 TLD(예: .com, 국가 코드 .kr, .jp 등)에 대한 정보를 저장하며, 권한 있는 DNS 서버의 IP 주소를 제공
 
 **3. 권한 있는 DNS 서버 (Authoritative DNS Servers)**
특정 도메인(예: amazon.com)에 대한 최종 정보를 저장하며, 호스트 이름과 해당 IP 주소 간의 매핑 정보를 제공

**로컬 DNS 서버**이는 계층 구조에 속하지는 않지만 DNS 아키텍처의 중심에 있음

호스트가 ISP에 연결되면 ISP는 호스트에게 로컬 DNS서버의 IP주소를 제공함. 호스트가 DNS쿼리를 수행하면 쿼리는 프록시 역할을 하는 로컬 DNS서버로 전송됨

#### 그림 2.19 다양한 DNS서버의 상호작용 ★★★
![image](https://github.com/user-attachments/assets/a5b1e17a-30f0-4f09-9ea6-11bda62043a6)
> 이 과정에서 총 8개의 메시지(4개의 쿼리와 4개의 응답)이 교환됨
> 
>재귀(Recursive)와 반복(Iterative) 쿼리로 사용되었고. 이러한 과정은 분산형 계층적 구조 덕분에 이루어짐.
> 
>재귀 쿼리: 클라이언트(cse.nyu.edu)가 로컬 DNS 서버(dns.nyu.edu)에 요청을 보낼 때, 로컬 DNS 서버가 대신 매핑을 찾아주는 방식
>
>반복 쿼리: 로컬 DNS 서버가 루트, TLD, 중간, 권한 있는 DNS 서버와 직접 통신하며 각각의 응답을 받는다
> 이때 반복적인 요청 시, DNS캐싱을 통해 네트워크 트래픽과 지연 시간을 줄일 수 있음 

#### DNS 캐싱 
**DNS 쿼리 결과를 임시로 저장하여 성능을 향상시키고 네트워크 트래픽을 줄이는 데 기여**

DNS 캐싱의 작동 방식

 1. 캐싱 과정:
     - DNS 서버는 쿼리에 대한 응답을 받을 때, 해당 정보를 로컬 메모리에 저장
     - 동일한 호스트 이름에 대한 쿼리가 발생하면, 로컬 DNS 서버는 캐시된 정보를 사용하여 즉시 응답 가능
 2. TTL(Time-to-Live):
     - 호스트 이름-IP 주소 매핑은 영구적이지 않으며, TTL값에 따라 일정 시간(보통 2일 정도)이 지나면 삭제됨
     
#### DNS 재귀적 질의
![image](https://github.com/user-attachments/assets/88b756e6-e3f6-4a72-8c07-6cd0d73f1417)
루트 DNS서버에서 TLD DNS서버에 해당되는 서버들(.edu)의 IP주소들을 반환하고, TLD 서버가 또다시 재귀적으로 하위 서버인 권한있는 DNS서버(.umass.edu)에 쿼리를보냄. 

그러면 권한있는 DNS는 gaia.cs.umass.edu의 IP 주소를 반환하고, 결론적으로 로컬 DNS 서버는 루트, TLD, 권한 있는 DNS 서버들로부터 받은 정보를 캐싱하고,  IP 주소를 요청 호스트에 반환. 

---
### 2.4.3 DNS 기록 및 메시지
DNS서버는 **호스트 이름에서 IP주소로의 매핑을 제공하는 RR을 포함한 리소스 레코드(RRs)**을 저장

#### 리소스 레코드 구성 (Name, Value, Type, TTL)
 Name: 도메인 이름.
 Value: Name과 관련된 값(IP 주소, 호스트 이름 등).
 Type: 레코드 유형(A, NS, CNAME, MX 등).
 TTL(Time to Live): 캐시에 저장될 수 있는 시간(초 단위).

**리소스 레코드 유형(Type)**
 1. Type=A (주소 레코드): 호스트 이름과 IPv4 주소를 매핑
 2. Type=NS (네임 서버 레코드):  DNS 쿼리가 올바른 권한 있는 DNS 서버로 전달되도록 라우
 3. Type=CNAME (정식 이름 레코드): 별칭 호스트 이름을 정식 이름으로 매핑
 4. Type=MX (메일 교환기 레코드): 메일 서버를 위한 간단한 별칭 제공


#### DNS Messages
DNS 메시지에는 두가지 종류(쿼리와 응답 메시지)가 존재. 쿼리와 응답 메시지는 동일한 형식을 가짐
![image](https://github.com/user-attachments/assets/3c964e1d-afcc-41d2-8ea5-276e17862c6a)

**헤더 섹션**: 첫 12바이트, 여러 필드를 포함함
 첫 번째 필드는 쿼리를 식별하는 16비트. 
 플래그 필드에는 여러 플래그가 있음. 일단은 메시지가 쿼리(0)인지 응답(1)인지 나타내는 1비트쿼리/응답 플래그 외에 3개 플래그 더 있음. 

**메시지 본문**
- 질문 섹션(Questions): 쿼리에 대한 이름과 타입 필드를 포함하는 가변 길이 섹션
- 응답 섹션(Answers): 쿼리에 대한 응답으로 리소스 레코드(RR)를 포함하는 가변 길이 섹션
- 권한 섹션(Authority): 권한 있는 서버에 대한 레코드를 포함하는 가변 길이 섹션
- 추가 정보 섹션(Additional information): 추가적인 "도움이 되는" 정보를 포함하는 가변 길이 섹션

> DNS 쿼리를 직접 보내려면 nslookup 프로그램을 사용할 수 있음


#### DNS 데이터베이스에 레코드 삽입
도메인 네임 networkutopia.com을 등록 기관(DNS registrar)에 등록한다고 가정

등록기관은 도메인 이름의 고유성을 확인하고 DNS데이터베이스에 도메인 이름을 입력함
현재는 여러 등록기관이 경쟁하고 있으며, ICANN(Internet Corporation for Assigned Names and Numbers)이 이들을 인증해주고 있다.

>도메인 이름을 등록할 때는 **기본(Primary) 및 보조(Secondary) 권한 DNS 서버와 IP주소를 제공**해야한다
>
>기본 DNS서버: dns1.networkutopia.com / IP 주소: 212.2.212.1
>
>보조 DNS서버: dns2.networkutopia.com / IP 주소: 212.212.212.2

>등록기관은 각 권한 DNS서버에 대해 **TLD서버에 NS레코드와 A레코드를 입력**
>
>NS 레코드: (networkutopia.com, dns1.networkutopia.com, NS)
>
>A 레코드: (dns1.networkutopia.com, 212.212.212.1, A)

>**권한 DNS 서버 설정을 위해 A레코드와 MX레코드 입력**
>
>웹 서버를 위한 Type A 레코드 (www.networkutopia.com)
>
>메일 서버를 위한 Type MX 레코드 (mail.networkutopia.com)

---
#### DNS VULNERABILITIES
**2002년 DNS 대역폭 플러 공격**
 - ICMP 핑 메시지를 사용하여 13개의 DNS 루트 IP 주소를 공격하여 패킷의 홍수를 유도
 - 패킷 필터링과 DNS 캐싱 덕분에 최소한의 피해로 막음

**2016년 최상위 도메인 서비스 제공업체Dyn 공격**
 - IoT 기기로 구성된 Mirai 봇넷을 통한 대규모 DDoS 공격 실행
 - 약 10만 개의 프린터, IP 카메라, 게이트웨이, 베이비 모니터 등이 감염됨
 - Amazon, Twitter, Netflix, Github, Spotify 등 주요 서비스 중단



