## 6.5 Link Virtualization: A Network as a Link Layer

**링크**: 초기에는 링크를 물리적인 선으로 이해했지만, 이후 다양한 매체(라디오, 스위치 기반 인프라 등)를 통해 링크를 채널로 이해하게 됨

**MPLS(Multiprotocol Label Switching)**: MPLS는 패킷 교환 기반의 링크 계층 기술로 IP 장치를 연결

**프레임 릴레이/ATM**: 이 기술들은 IP 장치 연결에 사용되었으나, 현재는 덜 사용됨

## 6.5.1 Multiprotocol Label Switching (MPLS)

![image](https://github.com/user-attachments/assets/b6382993-a97b-45cd-afdc-c1441d76ad8a)

- **MPLS 헤더 위치** : MPLS 헤더는 링크 계층 헤더와 네트워크 계층 헤더(IP 헤더) 사이에 위치

- **MPLS 헤더 필드** :
  - **Label** : MPLS 라벨 필드로 라우터가 데이터를 포워딩하는 데 사용
  - **Exp** : 실험적 필드(3비트)
  - **S** : 스택 끝을 나타내는 비트로 여러 개의 MPLS 헤더가 있을 때 사용
  - **TTL** : Time-to-live 필드, 데이터의 생명 주기를 제한
 
- **MPLS 라우터** : MPLS를 지원하는 라우터는 "레이블 스위치 라우터"라고 하며, MPLS 헤더를 참조하여 데이터그램을 포워딩

- **MPLS 라벨 관리** : 라우터는 IP 주소에 대해 어떤 MPLS 라벨을 할당할지 결정해야 함

![image](https://github.com/user-attachments/assets/15c4a229-126a-4df6-9142-9ebf88ef3159)

- **라우터 구성** :
   - R1, R2, R3, R4는 MPLS 지원 라우터
   - R5, R6는 표준 IP 라우터
   - R1은 R2와 R3에 목적지 A로 향하는 라벨 6을 광고함
   - R3는 R4에 목적지 A와 D로 향하는 라벨 10, 12를 광고함
   - R2는 R4에 목적지 A로 향하는 라벨 8을 광고함

- **MPLS 경로** :
   - R4는 A로 향하는 두 개의 MPLS 경로를 가짐:
     - 인터페이스 0 (라벨 10)
     - 인터페이스 1 (라벨 8)

- **MPLS의 역할** :
   - MPLS는 IP 라우팅 없이 MPLS 라벨을 통해 데이터를 포워딩
   - MPLS는 스위칭 속도 향상보다는 트래픽 관리 능력에서 장점이 있음

- **트래픽 엔지니어링** :
   - MPLS는 여러 경로를 통해 트래픽을 분산시켜 네트워크 성능 최적화
   - 예: 동일 목적지에 대해 두 개의 다른 경로를 사용할 수 있음

- **MPLS의 활용** :
   - 빠른 경로 복구: 링크 장애 시, 미리 계산된 경로로 트래픽 우회 가능
   - VPN: ISP가 MPLS 네트워크를 사용하여 고객 네트워크를 연결하고 분리
