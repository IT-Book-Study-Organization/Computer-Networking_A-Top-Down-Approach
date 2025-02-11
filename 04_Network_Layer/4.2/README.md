## 4.2 What's Inside a Router? ##

### 라우터의 주요 구성 요소 ###

```
라우터는 패킷을 네트워크 내에서 올바른 방향으로 전달하는 역할을 하며,
이를 위해 다음과 같은 주요 하드웨어 및 소프트웨어 구성 요소를 포함함.
```

![image](https://github.com/user-attachments/assets/cecdf2a0-0556-489f-8ee1-dcae7e9837d5)

*라우터 구조도* 

- **입력 포트** (Input Port)
  - 물리 계층에서 들어오는 데이터를 처리하고 링크 계층 기능을 수행.
  - 가장 중요한 기능은 포워딩 테이블 조회로, 수신된 패킷의 목적지를 확인하고 어떤 출력 포트로 보낼지를 결정.
  - 제어 패킷(예시: 라우팅 프로토콜 정보가 담긴 패킷)은 라우팅 프로세서로 전달됨.
  - 대규모 ISP 라우터에서는 수백 개의 고속 포트를 가질 수 있음. (예시: Juniper MX2020은 800개의 100Gbps 이더넷 포트를 지원하며, 총 용량은 800Tbps에 달함.)
 
- **스위칭 패브릭** (Switching Fabric)
  - 라우터의 입력 포트와 출력 포트를 연결하는 내부 네트워크.
  - 패킷이 올바른 출력 포트로 전달되도록 함.
  - 성능이 높은 라우터는 크로스바 스위칭이나 병렬 스위칭 패브릭을 사용하여 여러 패킷을 동시에 처리.
 
- **출력 포트** (Output Port)
  - 스위칭 패브릭을 통해 전달된 패킷을 저장하고 출력 링크로 전송함.
  - 링크 계층 및 물리 계층 기능 수행.
  - 큐잉(Queuing) 메커니즘을 사용하여 혼잡을 관리함.
 
- **라우팅 프로세서** (Routing Processor)
  - 제어 계층(Control Plane)의 핵심 요소.
  - 라우팅 프로토콜 실행 및 라우팅 테이블 유지.
  - 전통적 라우터에서는 자체적으로 라우팅 테이블을 계산하지만, SDN 라우터에서는 원격 컨트롤러와 통신하여 포워딩 테이블을 다운로드함.
  - 네트워크 관리 기능도 담당.
 
---

### 패킷 전달 과정 ###

1. **입력 포트 처리** (Input Port Processing)
   - 패킷이 입력 포트에 도착하면 포워딩 테이블 조회를 통해 적절한 출력 포트를 결정.
   - SDN 환경에서는 원격 컨트롤러에서 받은 포워딩 테이블을 사용.
   - 고속 라우터에서는 포워딩 테이블의 복사본을 각 입력 포트에서 관리하여 중앙 처리 장치의 부하를 줄임.
  
2. **스위칭** (Switching)
   - 스위칭 패브릭을 통해 패킷을 결정된 출력 포트로 전달.
   - 주요 스위칭 방식:
     - 메모리를 이용한 스위칭: 초기 방식으로, CPU가 포워딩 결정을 수행.
     - 버스를 이용한 스위칭: 입력 포트에서 출력 포트로 패킷을 직접 전송하는 방식. 단, 동시에 하나의 패킷만 전송 가능.
     - 인터커넥션 네트워크 기반 스위칭: 다중 패킷을 동시에 처리할 수 있는 방식 사용.
    
3. **출력 포트 처리** (Output Port Processing)
  - 패킷을 저장한 후, 스케줄링을 통해 출력 링크로 전송.
  - 스위칭 패브릭의 속도와 패킷 처리 속도에 따라 큐잉(Queuing)이 발생할 수 있음.

---

### 큐잉과 패킷 스케줄링 ###

```
라우터에서 패킷이 지연되거나 손실되는 주요 원인은 큐잉(Queuing) 때문이다.
```

- 입력 포트 큐잉
  - 스위칭 패브릭이 여러 패킷을 동시에 처리하지 못할 경우 발생.
  - 헤드 오브 라인(HOL) 블로킹 현상이 생길 수 있음.
 
- 출력 포트 큐잉
  - 특정 출력 포트로 향하는 패킷이 몰릴 경우, 해당 포트에서 지연이 발생.
  - 패킷이 너무 많으면 버퍼 오버플로우로 인해 손실됨.
 
- 패킷 스케줄링 기법
  - FIFO (First In, First Out): 먼저 도착한 패킷이 먼저 전송됨.
  - 우선순위 큐잉 (Priority Queueing): 특정 패킷에 우선권을 부여하여 전송.
  - 라운드 로빈 (Round Robin): 여러 큐를 번갈아 가며 서비스.
  - 가중치 공정 큐잉 (Weighted Fair Queuing, WFQ): 각 클래스별로 비율을 정해 공정하게 처리.

---

### 라우터에서의 패킷 손실 ###

- 입력 포트에서의 패킷 손실
  - 큐가 가득 차면 새로운 패킷이 저장되지 못하고 버려짐.
  - 해결책: 스위칭 패브릭 속도 향상, 큐 크기 증가.
 
- 출력 포트에서의 패킷 손실
  - 동일한 출력 포트를 공유하는 패킷이 많을 때 발생.
  - 해결책: 고속 링크 사용, 패킷 스케줄링 최적화.

 ---

 ## 4.2.1 Input Port Processing and Destination-Based Forwarding

 ### 입력 포트 처리 개요 ###

입력 포트는 라우터의 중요한 구성 요소 중 하나로, 다음과 같은 주요 기능을 수행함:
  - **물리 계층 및 링크 계층 처리**: 개별 입력 링크에 대한 물리 계층과 링크 계층 처리를 수행.
  - **포워딩 테이블 조회**: 수신된 패킷의 목적지 주소를 기반으로 적절한 출력 포트를 결정.
  - **로컬 처리**: 각 입력 포트가 자체적으로 포워딩 결정을 내릴 수 있도록, 라우팅 프로세서로부터 전달받은 포워딩 테이블을 사용.

이러한 기능은 라우터의 성능을 결정하는 핵심 요소로, 중앙 라우팅 프로세서의 부담을 줄이는데 중요한 역할을 함.

---

### 포워딩 테이블과 패킷 전송 과정 ###

![image](https://github.com/user-attachments/assets/fd25d497-6889-41c4-9605-1c4e5d7ff2ba)

*Input port processing*

#### 포워딩 테이블의 역할 ####

```
라우터는 포워딩 테이블을 이용하여 수신된 패킷의 출력 포트를 결정함. 이 테이블은 두 가지 방식으로 유지될 수 있음.
포워딩 테이블은 일반적으로 라우팅 프로세서에서 각 입력포트로 복사되며, 이를 통해 개별 포트에서 독립적으로 패킷 처리 가능 
```

- 라우팅 프로세서에 의해 계산 및 업데이트: 전통적인 방식으로, 라우팅 프로토콜을 통해 더룬 러유토둙허 정보를 교환하며 포워딩 테이블을 갱신.
- SDN 컨트롤러부터 수신: 소프트웨어 정의 네트워크(SDN)에서는 원격 컨트롤러가 테이블을 계산하고 업데이트하여 라우터에 전달.

#### 패킷 처리 과정 ####

패킷이 입력 포트로 들어오면, 다음 단계를 거침:<br>

1. 물리 계층 및 링크 계층 처리 수행
2. 포워딩 테이블 조회: 패킷의 목적지 주소를 기반으로 적절한 출력 포트를 결정
3. 출력 포트로 전송: 스위칭 패브릭을 통해 결정된 출력 포트로 패킷을 전달.

*이러한 과정은 패킷 전달 속도를 결정하는 중요한 요소이며, 빠른 조회를 위해 TCAM(Ternary Content Addressable Memory) 같은 고속 메모리 기술이 사용될 수 있다.*

---

### 효율적인 포워딩 테이블 설계 ###

#### 목적지 주소기반 포워딩 ####
- 기본적인 포워딩 방식은 **목적지 주소(destination address) 기반 포워딩**으로, 각 패킷의 목적지 주소를 확인한 후 출력 포트를 결정하는 방식.
- 하지만, IPv4의 32비트 주소를 고려하면 모든 가능한 주소를 포워딩 테이블에 저장하는 것은 비효율적. 예를 들어, 2^32(약 40억) 개의 주소를 모두 저장하는 것은 비현실적이므로, 주소 범위를 기반으로 한 포워딩 방식이 필요.

#### 주소 범위 기반 포워딩 ####
라우터는 포워딩 테이블을 효율적으로 유지하기 위해 주소 범위(prefix-based forwarding) 를 사용.

---

### 입력 포트에서의 큐잉과 스위칭 패브릭 연결 ###

#### 입력 포트에서의 큐잉 ####
- 스위칭 패브릭이 동시에 여러 패킷을 처리하지 못하면 입력 포트에서 큐잉이 발생할 수 있음. 다음과 같은 상황에서 문제 발생이 가능함:
  - 여러 개의 패킷이 동시에 같은 출력 포트로 가려고 할 때.
  - 스위칭 패브릭이 순간적으로 높은 트래픽 부하를 처리하지 못할 때.<br>
 
*이 문제를 해결하기 위해 HOL(Head-of-line) 블로킹 방지 기법과 같은 큐잉 관리 기법이 사용됨.* 

#### 스위칭 팹릭과 패킷 전달 ####
- 패킷이 큐에서 처리된 후에는 스위칭 패브릭을 통해 출력 포트로 전달됨.
- 스위칭 패브릭의 성능이 입력 포트의 성능보다 낮으면 패킷이 지연되거나 손실될 수 있기 때문에, 고성능 크로스바 스위치 같은 기술이 사용됨.

---

## 4.2.2 Switching ##

```
스위칭 패브릭(Switching Fabric)은 라우터의 핵심 구성 요소로, 입력 포트에서 출력 포트로 패킷을 전달하는 역할을 한다.
패킷이 올바르게 전달되기 위해 다양한 스위칭 기법이 사용된다.
```

![image](https://github.com/user-attachments/assets/2cfc79e0-a514-4c3c-8372-17f65bdaa0b5)

*Three switching techniques*

---

### 메모리 기반 스위칭 ###

- 초기의 라우터는 CPU를 사용하여 포워딩을 수행하는 방식
  1. 입력 포트로 패킷이 도착하면 CPU가 인터럽트를 통해 이를 감지.
  2. CPU가 패킷의 목적지 주소를 읽고 포워딩 테이블을 조회하여 적절한 출력 포트를 결정.
  3. CPU가 패킷을 출력 포트의 버퍼로 복사하여 전달.
 
- 한계점
  1. 패킷 처리 속도가 CPU의 처리 속도에 의존.
  2. 한 번에 하나의 패킷만 처리 가능하여 성능이 제한됨.
  3. 현대 라우터에서는 잘 사용되지 않음.
 
---

### 버스 기반 스위칭 ###

- 이 방식에서는 입력 포트가 패킷을 공용 버스를 통해 직접 출력 포트로 전송함.
  1. 입력 포트에서 패킷이 도착하면 내부적으로 추가된 스위칭 라벨을 사용하여 출력 포트를 지정.
  2. 해당 라벨을 확인한 후, 출력 포트가 패킷을 수신하여 버퍼에 저장.
  3. 이후 물리 계층을 통해 출력 링크로 패킷을 전송.

- 한계점
  - 버스는 한 번에 하나의 패킷만 전송 가능.
  - 여러 개의 패킷이 동시에 전송될 경우 충돌 발생 가능.
  - 대역폭이 제한적이며, 고성능 라우터에서는 충분한 속도를 제공하기 어려움.
 
---

### 인터커넥션 네트워크 기반 스위칭 ###

- 고성능 라우터에서는 대역폭을 확장하고 다중 패킷 처리를 지원하기 위해 인터커넥션 네트워크를 사용.
- 대표적인 방식은 크로스바 스위칭(Crossbar Switching).
  1. N개의 입력 포트와 N개의 출력 포트가 서로 교차하는 방식.
  2. 각 교차점에서 스위칭 패브릭 컨트롤러가 특정 연결을 열어 패킷을 전달.
  3. 여러 개의 패킷이 동시에 다른 출력 포트로 전달될 수 있음.
 
- 장점
  1. 다중 패킷을 동시에 처리 가능 (비블로킹 구조)
  2. 스위칭 성능이 향상되며, 대형 네트워크 라우터에 적합
 
- 단점
  1. 동일한 출력 포트로 향하는 다중 패킷 충돌 가능.
  2. 복잡한 제어 로직과 높은 비용 필요.
 
---

### 스위칭 성능 향상 기법 ###

- 현대 라우터에서는 높은 트래픽을 처리하기 위해 여러 개의 스위칭 패브릭을 병렬로 운영하는 방식을 사용.
  1. 멀티 스테이지 스위칭 네트워크 (Multi-stage Switching Network)
     - 여러 단계의 스위칭 패브릭을 사용하여 성능을 극대화.
     - 트래픽이 분산되어 병목 현상 감소.
  2. 패킷 분할 및 병렬 전송 (Packet Splitting and Parallel Transmission)
     - 큰 패킷을 작은 조각(Chunks)으로 나누어 여러 스위칭 패브릭을 통해 전송.
     - 출력 포트에서 패킷을 다시 조합하여 원래의 형태로 복구.
    
---

## 4.2.3 Output Port Processing ##

![image](https://github.com/user-attachments/assets/b1354d0f-1784-476d-850b-bd17a1341f5c)

*Output port processing*

```
출력 포트 처리는 출력 포트의 메모리에 저장된 패킷을 가져와 출력 링크를 통해 전송하는 과정이다.
이 과정에는 패킷 선택(스케줄링), 패킷 디큐잉(전송을 위한 큐에서 패킷 제거), 링크 계층 및 물리 계층의 전송 기능 수행이 포함된다.
```

---

## 4.2.4 Where does Queuing Occur? ##

```
입력 및 출력 포트의 기능과 라우터 구성 방식을 고려하면, 입력 포트와 출력 포트 모두에서 패킷 큐(queue)가 형성될 수 있다.
큐잉(Queuing)의 발생 위치와 정도는 트래픽 부하, 스위칭 패브릭의 상대적인 속도, 그리고 라인 속도에 따라 달라진다.
큐가 커질수록 라우터의 메모리가 부족해질 수 있으며, 결국 새로운 패킷을 저장할 공간이 없을 때 패킷이 손실된다.
"네트워크 내에서 패킷이 손실된다" or "라우터에서 패킷이 삭제된다"라고 표현했던 상황은
바로 라우터 내부의 이러한 큐에서 패킷이 실제로 버려지고 손실되는 것을 의미한다.
```
