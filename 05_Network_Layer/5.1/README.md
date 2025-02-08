# 5.1 서론 (Introduction)

## 개요

```
이 장에서는 네트워크 제어 평면(Control Plane)의 개념을 설정하고, 데이터 평면(Data Plane)과의 관계를 설명한다. 
네트워크 계층에서 포워딩 테이블(Forwarding Table) 과 플로우 테이블(Flow Table) 은 패킷이 어떻게 전달되는지를 결정하는 핵심 요소이다.
```

네트워크 제어 평면의 설계 방식에는 두 가지 주요 접근 방식이 있음.

---

## 1. 각 라우터별 제어 (Per-router Control)
- 라우팅 알고리즘이 **각 라우터에서 독립적으로 실행**됨.
- 라우터는 **포워딩 기능**과 **라우팅 기능**을 모두 포함함.
- 라우터들은 **서로 통신하면서 포워딩 테이블을 계산**함.
- **OSPF**(Open Shortest Path First) 및 **BGP**(Border Gateway Protocol)와 같은 프로토콜이 이 방식을 따름.
- 인터넷에서 오랫동안 사용된 전통적인 방식.

**관련 내용**: OSPF (5.3), BGP (5.4)

---

## 2. 논리적으로 중앙 집중화된 제어 (Logically Centralized Control)
- **중앙 컨트롤러가 네트워크 전체를 제어**함.
- 포워딩 테이블을 **중앙에서 계산하고 배포**하는 방식.
- **SDN(Software-Defined Networking)** 같은 기술이 이 방식을 사용함.
- 일반적인 **매치(match)-플러스-액션(match-plus-action) 추상화**를 통해 다양한 네트워크 기능(예: 방화벽, NAT, 로드 밸런싱 등)을 수행할 수 있음.
- 각 라우터의 **로컬 제어 에이전트(Control Agent, CA)** 가 중앙 컨트롤러와 통신하며 포워딩 테이블을 적용함.

**관련 내용**: SDN 및 OpenFlow (5.5)
