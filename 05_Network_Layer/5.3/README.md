# 5.3 인터넷에서의 AS 내 라우팅: OSPF (Intra-AS Routing in the Internet: OSPF)

```
인터넷은 자율 시스템(Autonomous System, AS)으로 구성되어 있으며, 각 AS 내부에서 실행되는 라우팅 프로토콜을 AS 내 라우팅 프로토콜(Intra-AS Routing Protocol) 또는 IGP(Interior Gateway Protocol)라고 한다. 
이 중 OSPF(Open Shortest Path First)는 가장 널리 사용되는 AS 내 라우팅 프로토콜 중 하나이다.
```

---

## OSPF 개요

```
OSPF는 링크 상태 라우팅(Link-State Routing) 프로토콜이며, AS 내에서 최단 경로 우선(Shortest Path First, SPF) 알고리즘을 사용하여 라우팅을 수행한다.
이는 네트워크의 토폴로지를 완전히 파악하고 최단 경로를 계산하는 방식으로 동작한다.
```

*주요 내용*
- **다익스트라(Dijkstra) 알고리즘**을 사용하여 최단 경로를 계산.
- 모든 OSPF 라우터는 **링크 상태 광고(LSA, Link-State Advertisement)** 를 통해 네트워크 정보를 공유.
- LSA 정보를 수집하여 **링크 상태 데이터베이스(LSDB, Link-State Database)** 를 구축.
- LSDB를 기반으로 SPF 알고리즘을 실행하여 **포워딩 테이블**을 생성.

---

## OSPF의 주요 특징

1. **계층적 라우팅(Hierarchical Routing)**
   - OSPF는 **영역(Area)** 개념을 도입하여 네트워크를 계층적으로 관리.
   - 모든 OSPF 영역은 **백본 영역(Area 0)** 과 연결되어야 함.
   - **ABR(Area Border Router)** 는 여러 영역을 연결하고, **ASBR(Autonomous System Border Router)** 는 다른 AS와 연결됨.

2. **라우팅 갱신 시 LSA 사용**
   - OSPF는 **라우팅 업데이트를 정기적으로 전송하지 않음** (BGP와 대비됨).
   - 네트워크 변경 사항이 발생할 때만 LSA를 전송하여 트래픽을 줄임.

3. **다양한 비용(metric) 설정**
   - OSPF는 링크의 **대역폭(Bandwidth)** 을 기반으로 경로 비용을 계산.
   - 기본적으로 **링크 속도가 높을수록 낮은 비용**을 가짐.

4. **라우팅 인증 지원**
   - OSPF는 **MD5, SHA 등 보안 인증**을 제공하여, 라우팅 정보가 변조되는 것을 방지.

---

## OSPF 패킷 유형

1. **Hello 패킷**: 인접 라우터 탐색 및 관계 설정
2. **DBD (Database Description)**: LSDB 정보 요약 제공
3. **LSR (Link-State Request)**: 특정 링크 상태 정보를 요청
4. **LSU (Link-State Update)**: 네트워크 변경 사항 업데이트
5. **LSAck (Link-State Acknowledgment)**: LSA 패킷 수신 확인

---

## OSPF 네트워크 구조

OSPF 네트워크는 **포인트 투 포인트(Point-to-Point)** 및 **브로드캐스트(Broadcast)** 환경에서 다르게 동작함.

- **포인트 투 포인트(P2P) 네트워크**: 직접 연결된 두 라우터가 Hello 패킷을 교환하여 이웃을 형성.
- **브로드캐스트 네트워크**: OSPF는 DR(Designated Router)과 BDR(Backup Designated Router)을 선택하여 트래픽을 줄임.

---

## OSPF vs. 다른 라우팅 프로토콜 비교

| 프로토콜 | 유형 | 알고리즘 | 사용 사례 |
|---------|---------|----------|----------|
| **OSPF** | 링크 상태 | 다익스트라(SPF) | 기업 및 ISP 내부 네트워크 |
| **RIP** | 거리 벡터 | 벨만-포드 | 소규모 네트워크 |
| **EIGRP** | 하이브리드 | DUAL | 시스코 전용 네트워크 |

