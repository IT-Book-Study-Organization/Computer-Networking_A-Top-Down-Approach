# 5.4 ISP 간 라우팅: BGP 요약

- AS(Autonomous System) 내부에서는 **OSPF 같은 내부 라우팅 프로토콜**이 경로를 결정하지만, 여러 AS를 거쳐야 하는 경우 **인터-AS 라우팅 프로토콜**이 필요함.  
- 인터넷에서는 **모든 AS가 동일한 인터-AS 라우팅 프로토콜, 즉 BGP(Border Gateway Protocol)를 실행**해야 함.  
- **BGP는 수천 개의 ISP를 연결하는 인터넷의 핵심 프로토콜**로, 거리 벡터(distance-vector) 라우팅과 유사하지만 **비집중적(decentralized)이고 비동기적(asynchronous)인 방식**으로 작동함.  
- BGP는 복잡하지만, 인터넷 구조를 이해하는 데 필수적인 프로토콜임.

## 5.4.1 BGP의 역할

인터넷에서 AS(Autonomous System) 간 라우팅을 담당하는 프로토콜은 BGP(Border Gateway Protocol)이다. 내부 라우팅은 OSPF 같은 프로토콜이 처리하지만, AS 외부 목적지는 BGP가 경로를 결정한다. BGP는 CIDR(Classless Inter-Domain Routing) 방식으로 서브넷 단위로 라우팅(즉, 개별 IP 주소가 아니라 프리픽스([Prefix](https://jdcyber.tistory.com/51)) 단위로 경로를 설정하고 관리)하며, 두 가지 핵심 역할을 수행한다.

1. **프리픽스 도달 가능성 정보 획득**
   - 특정 네트워크(프리픽스)의 존재를 다른 AS에 알리고 학습하게 함.
2. **최적 경로 결정**
   - 네트워크 정책과 도달 가능성 정보를 기반으로 최적의 경로 선택.

BGP가 없다면, AS 간 연결이 불가능하여 인터넷이 정상적으로 동작할 수 없다.

## 5.4.2 BGP 경로 정보 광고

### BGP 경로 광고(Advertising) 개념

BGP는 AS 간의 프리픽스 도달 가능성 정보를 공유하여, 인터넷의 모든 라우터가 특정 네트워크를 인식할 수 있도록 한다.

![Screenshot 2025-02-05 at 2 47 45 AM](https://github.com/user-attachments/assets/39748087-4207-43f4-a00c-ffd65216ea0f)

#### 경로 광고 예시
1. AS3가 AS2에 **"AS3 x"** 메시지를 보냄.
2. AS2가 AS1에 **"AS2 AS3 x"** 메시지를 전달.
3. AS1은 프리픽스 x로 가는 경로를 학습.

이를 통해 AS1과 AS2는 프리픽스 x의 존재뿐만 아니라 **x로 가는 경로(AS 경로 정보)**까지 학습할 수 있다.

### BGP 메시지 전달 방식

BGP는 반영구적인 TCP 연결(포트 179)을 유지하며, **eBGP(External BGP)와 iBGP(Internal BGP)**를 통해 정보를 전달한다.

![Screenshot 2025-02-05 at 2 47 54 AM](https://github.com/user-attachments/assets/c00475ad-3845-49bd-bfbf-5fae29143648)

- **eBGP**: AS 간 BGP 연결 (예: AS1의 라우터 1c ↔ AS2의 라우터 2a)
- **iBGP**: AS 내부 BGP 연결 (예: AS2 내부의 모든 라우터 간 연결)

#### BGP 경로 광고 과정
1. AS3 → AS2 (eBGP 메시지): "AS3 x"
2. AS2 내부 (iBGP 메시지): "AS3 x" 확산
3. AS2 → AS1 (eBGP 메시지): "AS2 AS3 x"
4. AS1 내부 (iBGP 메시지): "AS2 AS3 x" 확산

이 과정을 통해 모든 라우터가 프리픽스 x와 최적 경로를 학습한다.

## 5.4.3 최적 경로 결정

### BGP의 핵심 속성

BGP는 프리픽스를 광고할 때 여러 속성을 포함하여 경로 정보를 제공한다.

1. **AS-PATH**: 지나온 AS 목록을 포함하여 루프를 방지함. (이 번호들의 갯수가 작을 경우 짧은 경로로 판단함, 이 번호들 중 자신의 AS 번호가 있으면 해당 정보를 무시함)
2. **NEXT-HOP**: AS-PATH에서 첫 번째 AS의 라우터 IP 주소를 지정. (BGP 정보를 전송하는 라우터의 IP 주소로써, 목적지까지 가는 경로에서 반드시 자신을 거쳐야만 한다고 알리는 Next Hop 라우터의 주소를 말함)

### 핫 포테이토 라우팅 (Hot Potato Routing)

- AS 내부 비용을 최소화하는 경로를 선택하는 방식.
- 예: AS1의 1b 라우터가 프리픽스 x까지 가는 두 개의 경로를 학습할 경우,
  - "AS2 AS3 x" (2 홉)
  - "AS3 x" (3 홉)
  - 홉 수가 적은 "AS2 AS3 x"를 선택.

![Screenshot 2025-02-05 at 2 48 01 AM](https://github.com/user-attachments/assets/b6da896a-3b94-4ea4-bbcc-a97f32e8c6b1)

### BGP의 경로 선택 알고리즘

1. **로컬 프리퍼런스(Local Preference)**: 네트워크 정책에 따라 우선 순위가 높은 경로 선택.
2. **AS-PATH 길이 확인**: AS를 가장 적게 거치는 경로를 선호.
3. **핫 포테이토 라우팅 적용**: AS-PATH 길이가 동일하면, 인트라-AS 비용이 가장 낮은 경로 선택.
4. **BGP 식별자(BGP Identifier) 비교**: 모든 조건이 동일하면 라우터 ID 기준으로 경로 결정.
