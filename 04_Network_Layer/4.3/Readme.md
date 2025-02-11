## 4.3 인터넷 프로토콜(IP): IPv4, 주소 지정, IPv6 등
오늘날의 인터넷의 네트워크 계층의 측면과 유명한 인터넷 프로토콜IP에 대해 배울 것~
오늘날 사용되는 IP 버전은 두 가지
- IPv4,  IPv6

### 4.3.1 IPv4 데이터그램 형식
데이터그램 - 네트워크 계층 패킷
![image](https://github.com/user-attachments/assets/fb015eac-6ea2-415a-9694-7a527820de3e)

**기본 필드**<br>
버전 번호(4비트): IP 프로토콜 버전을 지정<br>
헤더 길이(4비트): 데이터그램에서 페이로드가 시작되는 위치 결정<br>
서비스 유형(TOS): 다른 유형의 데이터그램을 구별<br>
데이터그램 길이(16비트): 전체 IP 데이터그램의 길이(최대 65,535바이트)<br>

**제어 필드**<br>
식별자, 플래그, 단편화 오프셋: IP 단편화 관련<br>
생존 시간(TTL): 데이터그램의 무한 순환 방지<br>
프로토콜: 데이터를 전달할 전송 계층 프로토콜 지정<br>
헤더 체크섬: 비트 오류 감지<br>

**주소 및 데이터**<br>
출발지와 목적지 IP 주소: 송신자와 수신자의 주소<br>
옵션: IP 헤더 확장 가능(선택적)<br>
데이터(페이로드): 실제 전송할 데이터<br>

---

### 4.3.2 IPv4 주소 지정
호스트와 라우터는 어떻게 인터넷에 연결되는가?
> 호스트 구조: 단일 링크 보유, 데이터그램 전송 시 이 링크를 사용함. 호스트와 링크 사이의 경계를 '인터페이스'라고 함<br>
> 라우터 구조: 두개 이상의 링크 필수, 각 링크마다 하나의 인터페이스를 보유함. 한 링크에서 받은 데이터그램을 다른 링크로 전달하는 역할<br>
> IP 주소 할당의 특징:  IP 주소는 호스트나 라우터 자체가 아닌 '인터페이스'에 할당됨. 모든 인터페이스는 데이터그램의 송수신을 위해 고유한 IP주소를 가져야함<br>  
>> *인터페이스는 호스트나 라우터가 네트워크와 통신하기 위한 연결점<br>  
>> **IP 주소는 기술적으로 그 인터페이스를 포함하는 호스트나 라우터가 아닌, 인터페이스와 연관된다**

1. IP의 기본 구조:
   - 32비트(4바이트) 길이
   - 최대 약 40억개(2^32)의 주소
   - 이를 8비트씩 4개의 부분(옥텟)으로 나눔
   - 점으로 구분된 십진수로 표기 (예: 193.32.216.9)
2. 표기법:
   - 각 숫자는 8비트를 10진수로 변환한 값
   - 10진수 -> 2진수
     
첫 번째 옥텟 193 = 11000001<br>
두 번째 옥텟 32 = 00100000<br>
세 번째 옥텟 216 = 11011000<br>
네 번째 옥텟 9 = 00001001<br>

> 이렇게 각 옥텟을 2진수로 변환하면 11000001 00100000 11011000 00001001
> 전 세계 인터넷의 모든 호스트와 라우터의 각 인터페이스는 전역적으로 고유한 IP 주소를 가져야함.(NAT제외)

![image](https://github.com/user-attachments/assets/350cb20e-fbc8-4dc7-bd61-a066ff451847)

왼쪽 24비트가 동일함. 여기 인터페이스는 라우터가 없는 네트워크로 연결됨 (?) <br>
네트워크는 이더넷 LAN 연결, 인터페이스는 이더넷 스위치로 상호 연결 가능(6장)<br>
지금은 일단 구름으로 표현하고 6~7장<br>

> 세 개의 호스트 인터페이스와 하나의 라우터 인터페이스를 연결하는 이 네트워크는 **서브넷**을 형성<br>
> #### *서브넷: 네트워크 영역을 분할하여 더 작은 크기의 네트워크 영역으로 쪼갠 네트워크)<br>

IP 주소 지정은 이 서브넷에 주소를 할당합니다: 223.1.1.0/24<br>
/24: 서브넷 마스크 (32비트 값의 왼쪽 24비트가 서브넷 주소를 정의함)<br>
> 따라서 **223.1.1.0/24 서브넷은 세 개의 호스트 인터페이스(223.1.1.1, 223.1.1.2, 223.1.1.3)와 하나의 라우터 인터페이스(223.1.1.4)로 구성**

![image](https://github.com/user-attachments/assets/b28bccb1-ce7e-4700-95b8-1d347f23ca29)

> 고립된 네트워크의 섬(호스트나 라우터)에 인터페이스들이 종단점을 형성. <br>
=> 고립된 네트워크 각각을 **서브넷**이라고 함.
<br><br><br>

#### Q. 이 그림에서 서브넷은 몇개?
![image](https://github.com/user-attachments/assets/6d65229b-3219-456c-b8c1-03a5240ccf53)
<br><br>
#### A. 브로드캐스트 서브넷 3개, 점대점 링크 서브넷 3개
<br>

### 인터넷 주소 할당 전략 - CIDR

#### a.b.c.d/x

x비트 - 네트워크 프리픽스. 가장 중요한 비트로 IP주소의 네트워크 부분을 구성함. 

> 외부의 라우터가 목적지 주소가 조직 내부인 데이터그램을 전달할 때는 주소의 선두 x비트만 고려하면 됨. 

나머지 32-x비트 - 동일한 네트워크 프리픽스(x)를 가진 조직 내의 장치들을 구별. 조직 내 라우터에서 패킷을 전달할 때 고려되는 비트 

CIDR이 채택되기 전에는 IP 주소의 네트워크 부분이 8, 16, 또는 24비트 길이로 제한(클래스풀 주소 지정 체계)<br>
8, 16, 24비트 서브넷 주소를 가진 서브넷들 -> 각각 클래스 A, B, C 네트워크<br>
클래스 C(/24) 서브넷은 최대 (2^8, 예외2)254개 => 작아<br>
클래스 B(/16) 서브넷은 최대 (2^16)65,534개 => 너무 큼<br>
> 2000개의 호스트를 위해 클래스 B를 사용한 조직:<br>
>  클래스 B 주소 공간의 빠른 고갈과 할당된 주소 공간의 낮은 활용도를 초래

브로드캐스트 주소는 255.255.255.255라는 특별한 IP주소. 같은 서브넷의 모든 호스트에게 메시지 전달함

#### IP 주소 블록 할당 체계
- 조직은 ISP로부터 주소 블록을 받음<br>
- ISP는 자신의 큰 주소 블록을 작은 블록으로 나눠서 분배<br>
예: ISP가 가진 200.23.16.0/20을 8개 조직에 /23 단위로 분할<br>
아래 그림을 보면 Organization 1이 Fly-By-Night-ISP에서 ISPs-R-Us로 이동함.
-> 조직이 ISP를 변경할 수 있음. 
![image](https://github.com/user-attachments/assets/863ecd73-da29-4cff-8dd4-883a224d6e06)

#### 전체 관리 체계:
ICANN이 전체 IP 주소 관리<br>
지역 인터넷 등록기관(ARIN, RIPE, APNIC, LACNIC 등)에 주소 할당<br>
각 지역 등록기관이 해당 지역의 주소 관리

> **ICANN → 지역 등록기관 → ISP → 개별 조직 순으로 주소 블록이 할당**
<br><br>
#### 호스트 주소 얻기: 동적 호스트 구성 프로토콜(DHCP)
조직이 주소 블록을 얻으면 조직 내의 호스트와 라우터 인터페이스에 개별 IP주소를 할당할 수 있음. 일반적으로 동적 호스트 구성 프로토콜(DHCP)을 사용하여 수행

1. DHCP의 기본 기능:<br>
- **IP 주소 자동 할당**<br>
- 추가 정보 제공 (서브넷 마스크, 기본 게이트웨이, DNS 서버 주소 등)
- 특정 호스트에게 동일한 IP 또는 임시 IP 할당 가능<br>
2. DHCP의 장점:<br>
- 네트워크 자동화 (플러그 앤 플레이 또는 제로콘프 프로토콜)
- 관리자의 수동 설정 불필요
- 이동이 잦은 환경(대학, 기업, 무선 LAN)에 적합<br>
3. DHCP 구조:<br>
- 클라이언트-서버 프로토콜
- 각 서브넷에 DHCP 서버 필요
- 서버가 없는 경우 DHCP 릴레이 에이전트(라우터) 사용

새로 도착한 호스트의 경우?
![image](https://github.com/user-attachments/assets/7d69ed5e-a0f4-4184-83f7-7663bb51715d)

4단계 과정을 거침
![image](https://github.com/user-attachments/assets/57997d06-1f7d-4f5a-96e1-f1b0715fc657)

DHCP 단점: 새로운 서브넷에 연결할 때마다 DHCP에서 새로운 IP 주소를 얻기 때문에, 모바일 노드가 서브넷 간에 이동할 때 원격 애플리케이션에 대한 TCP 연결을 유지할 수 없음. 
<br><br>

---

### 4.3.3 네트워크 주소 변환(NAT(Network Address Translation))

모든 IP 장치가 고유한 IP 주소 필요함. 가정이나 소규모 사무실의 여러 기기들에게 모두 공인 IP 할당은 비효율적임<br>
=> NAT이 필요

NAT의 작동 방식:<br>
 - 내부 네트워크는 사설 IP 주소 사용 (예: 10.0.0.0/24)
 - NAT 라우터는 외부와 통신할 때 하나의 공인 IP만 사용
 - 내부 네트워크의 세부사항을 외부로부터 숨김<br>

사설 주소의 특징:<br>
 - RFC 1918에서 정의된 사설 주소 범위 사용
 - 같은 사설 IP를 여러 네트워크에서 중복 사용 가능
 - 내부 네트워크에서만 의미있는 주소<br>

주소 할당:<br>
 - 라우터는 ISP의 DHCP에서 공인 IP 받음
 - 내부 기기들은 라우터의 DHCP를 통해 사설 IP 받음

#### Q. WAN에서 NAT 라우터에 도착하는 모든 데이터그램이 동일한 목적지 IP 주소를 가진다면, 라우터는 주어진 데이터그램을 어떤 내부 호스트로 전달해야 하는지 어떻게 알까요?

#### A. NAT 라우터에서 NAT 변환 테이블을 사용하고, 테이블 항목에 IP 주소뿐만 아니라 포트 번호도 포함하는 것

![image](https://github.com/user-attachments/assets/e1d6ef7a-ddea-411f-bd38-abc88d62701a)

+ NAT에 반대하는 사람들이 있음<br>

NAT의 문제점:<br>
 - 포트 번호가 본래 프로세스 주소 지정용인데 호스트 주소 지정에 사용
 - 홈 네트워크의 서버 운영이 어려움
 - P2P 통신에서 NAT 뒤에 있는 피어 연결의 어려움
   
구조적 논란:<br>
 - 라우터는 네트워크 계층(3계층) 장치여야 한다는 원칙 위반
 - 호스트 간 직접 통신을 방해한다는 비판
<br><br>
---
### IPv6

놀라운 속도로 인터넷에 연결되고 IP주소가 할당되면서 IPv4 주소 공간이 소진되기 시작했다는 인식 <br>
=> 큰 IP 주소 공간 필요성에 대응하기 위해 새롭게 개발됨. 

#### IPv6 의 주요 변경 사항
1. 주소 확장
    - 32비트 → 128비트로 확장
    - 새로운 애니캐스트 주소 타입 도입

2. 헤더 간소화
   - 40바이트 고정 길이 헤더
   - IPv4보다 단순화된 구조
   - 더 빠른 처리 가능
  
3. 플로우 레이블 도입
   - 특별한 처리가 필요한 패킷 구분
   - QoS나 실시간 서비스 지원
  
     
#### IPv6 데이터그램 형식
![image](https://github.com/user-attachments/assets/57cf1091-64c7-4b8f-a5db-8d144ae4784c)

1. 버전
   - IP 버전 번호를 식별
     
2. 트래픽 클래스
   - 특정 애플리케이션의 데이터그램에 우선 순위를 부여

3. 플로우 레이블
   - 20비트 필드는 데이터그램의 플로우를 식별
4. 페이로드 길이
   - 16비트. 데이터그램 헤더 다음에 오는 IPv6의 바이트 수를 나타냄
5. 홉 제한
   - 내용은 라우터에 의해 1씩 감소, 0에 도달하면 데이터그램 폐기
7. 출발지와 목적지 주소
8. 데이터
   - 데이터그램의 페이로드 부분.
   
  

#### IPv4와 IPv6 데이터그램 형식을 비교
1. 단편화/재조립 기능 제거:
   - IPv6에서는 중간 라우터에서의 단편화/재조립을 허용하지 않고 출발지와 목적지에서만 수행 가능
   - 라우터가 너무 큰 데이터그램을 받으면: 데이터그램을 폐기하고 에러메세지 보냄. 그럼 송신자가 더 작은 크기로 재전송
   - 이유: 라우터의 처리 속도 향상
2. 헤더 체크섬 제거:
   - 전송 계층(TCP, UDP)과 링크 계층(이더넷)이 이미 체크섬 수행하므로 네트워크 계층에서는 중복된 기능으로 판단
   - IPv4에서는 TTL 변경 때문에 매 라우터마다 체크섬 재계산 필요
   - 이유: IP 패킷 처리 속도 향상
3. 옵션 필드 변경:
   - 표준 IP 헤더에서 제거하고 대신 '다음 헤더'로 지정 가능
   - TCP나 UDP처럼 옵션도 다음 헤더가 될 수 있음
   - 결과: 고정된 40바이트 IP 헤더
   
#### IPv4에서 IPv6로의 전환
IPv4를 기반으로 하는 공용 인터넷을 어떻게 IPv6로 전환할 것인가?
이미 배포된 IPv4 지원 시스템은 IPv6 데이터그램을 처리할 수 없다는 것.

> => 플래그 데이 선언 <br>
: 모든 인터넷 기기를 끄고 IPv4에서 IPv6로 업그레이드하는 특정 시간과 날짜를 정하는 것<br>
==> 그러나, 한번에 모든 시스템을 전환하는 것(flag day)은 불가능<br>
===> **해결책: 터널링(Tunneling)** <br>
> **: IPv6 노드들이 IPv4 네트워크를 통해 통신하는 방법, IPv6 데이터그램을 IPv4 데이터그램 안에 캡슐화하고 중간 IPv4 라우터들은 일반적인 IPv4 패킷으로 처리**

![image](https://github.com/user-attachments/assets/a9c83018-7f42-487f-a78e-8915fe2971b0)














