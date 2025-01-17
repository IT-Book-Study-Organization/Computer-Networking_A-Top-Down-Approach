## 3.7 TCP Congestion Control

### 개요

- TCP의 신뢰할 수 있는 전송 서비스와 혼잡 제어 메커니즘
- 클래식 TCP는 네트워크 혼잡에 대한 명시적인 피드백 없이 종단 간 혼잡 제어를 사용하 RFC 2581과 RFC 5681에 표준화되어 있음
- 섹션 7.3.1은 클래식 TCP를 깊이 있게 다룸
- 섹션 7.3.2에서는 네트워크 계층에서 제공하는 명시적 혼잡 지시자나 클래식 TCP와 차이가 있는 최신 TCP 버전들을 소개함

### 3.7.1 Classic TCP Congestion Control

- TCP는 네트워크 혼잡에 따라 송신자가 전송 속도를 제한하는 방식을 사용함

- 송신자는 전송 경로에 혼잡이 있는지 어떻게 인지하는지에 대한 의문이 있음

- 송신자는 인지한 혼잡에 따라 전송 속도를 어떻게 조절해야 하는지에 대한 알고리즘이 필요함

- TCP 송신자는 cwnd(혼잡 윈도우)라는 변수를 사용하여 전송 속도를 제한함. 송신자가 전송하는 미확인 데이터의 양은 `LastByteSent – LastByteAcked` 값으로 표현되고 `min{cwnd, rwnd}` 로 제한됨

  - `LastByteSent`: 송신자가 마지막으로 전송한 바이트의 번호
  - `LastByteAcked`: 수신자가 마지막으로 확인한 바이트의 번호
  - `cwnd`: 혼잡 윈도우 크기
  - `rwnd`: 수신 윈도우 크기

```
LastByteSent - LastByteAcked ≤ min(cwnd, rwnd)
```

- 송신자는 이 식을 통해 데이터 전송을 제한, 혼잡 윈도우(cwnd)가 송신 속도를 결정하는 주요 요소임

```
송신자의 전송 속도 = cwnd / RTT (바이트/초)
```

- 송신자는 전송 경로에서 혼잡을 감지하기 위해 손실 이벤트를 사용함, 손실 이벤트는 타임아웃 발생 또는 세 개의 중복 ACK 수신으로 정의됨

- 네트워크 상의 라우터 버퍼가 오버플로우되면 패킷이 손실되고 이로 인해 손실 이벤트가 발생함. 이를 혼잡의 징후로 인식함

- 송신자는 혼잡을 인지한 후 전송 속도를 조절하여 네트워크의 혼잡 상황을 완화함

**혼잡이 없는 경우** : 
- 네트워크가 혼잡하지 않은 경우 + 손실 이벤트가 발생하지 않으면 이전에 전송된 세그먼트에 대한 확인 응답이 송신자에게 도달하게 됨
- TCP 송신자는 이 응답을 네트워크가 정상적으로 동작하고 있다는 신호로 받아들이고 이를 기반으로 혼잡 윈도우 크기를 증가시켜 전송 속도를 높임

**응답에 따른 속도 증가** 
- 확인 응답이 느리게 도달하면 혼잡 윈도우 크기가 천천히 증가함
- 응답이 빠르게 도달하면 윈도우 크기가 더 빠르게 증가함
- TCP는 이와 같은 방식으로 전송 속도를 증가시키며, 이를 self-clocking이라고 함

**송신 속도 결정** : 
- 전송 속도를 결정하는 요소는 네트워크를 과도하게 혼잡시키지 않으면서 네트워크의 대역폭을 충분히 활용하는 것
- 너무 빠르게 보내면 네트워크가 혼잡해지고 너무 느리게 보내면 대역폭을 충분히 사용하지 못함

**송신자 간 협력** : 네트워크 혼잡 상태에 대한 전역적인 정보를 사용하지 않고 각 송신자가 로컬 정보만으로 전송 속도를 조절하는 분산 접근 방식을 사용함

**혼잡 제어 알고리즘** : TCP의 혼잡 제어는 세 가지 주요 단계로 이루어짐:

  - **슬로우 스타트(Slow Start)** : 초기에는 혼잡 발생 시에 혼잡윈도우(cwnd)를 최소값(ex. 1MSS)부터 전송률을 낮게(천천히) 시작, 이후 cwnd를 빠르게 증가시켜 송신 속도를 높임

  - **혼잡 회피(Congestion Avoidance)** : 슬로우 스타트 후, 혼잡 윈도우 크기를 천천히 증가시킴

  - **빠른 복구(Fast Recovery)** : 손실된 세그먼트 복구 후 전송 속도를 빠르게 회복시킴 (추천되지만 필수는 아님)

---

### Slow Start

![image](https://github.com/user-attachments/assets/76260e5e-eb6a-4ab2-9156-f19c2c40a884)

- 처음에 혼잡 윈도우(cwnd)가 1 MSS(최대 세그먼트 크기)로 설정됨

- 매 라운드 트립 타임(RTT)마다 cwnd가 두 배씩 증가함. 예를 들어, 첫 번째 RTT에는 cwnd가 1 MSS로 시작하고, 두 번째 RTT에서는 2 MSS, 세 번째 RTT에서는 4 MSS로 증가함

- 슬로우 스타트 종료 조건:

  - **손실 이벤트 발생 시** : 타임아웃이 발생하면 TCP 송신자는 cwnd를 1로 초기화하고, 슬로우 스타트를 다시 시작함. **ssthresh(슬로우 스타트 임계값)** 는 cwnd/2로 설정됨. ssthresh는 혼잡이 발생했을 때의 cwnd 값의 절반으로 설정됨.
  
  - **ssthresh와의 비교** : cwnd 값이 ssthresh에 도달하거나 이를 초과하면 슬로우 스타트는 종료되고 TCP는 **혼잡 회피(Congestion Avoidance)** 모드로 전환됨. 혼잡 회피 모드에서는 cwnd를 더 신중하게 증가시킴
    
  - **세 개의 중복 ACK 감지 시** : 세 개의 중복된 ACK가 감지되면 **빠른 재전송(Fast Retransmit)** 을 수행하고, **빠른 복구(Fast Recovery)** 상태로 전환됨
    
- 슬로우 스타트의 목표 : 슬로우 스타트는 네트워크 대역폭을 빠르게 찾기 위한 방식

- 점진적으로 cwnd를 증가시키는 방식이 빠르게 전송 속도를 증가시키지만 **슬로우 스타트**라는 이름은 초기 상태에서 느리게 시작된다는 의미에서 유래한 것임

---

### Congestion Avoidance

![image](https://github.com/user-attachments/assets/f4cd5658-11b5-4a73-875e-5cdd17ecba2e)

**TCP 혼잡 제어 상태 기계**

1. **슬로우 스타트(Slow Start)**
    - 초기 상태에서 cwnd(혼잡 윈도우)는 1 MSS로 설정
    - 각 새로운 ACK를 받을 때마다 cwnd가 1 MSS씩 증가.
    - 이 상태에서는 cwnd가 ssthresh(슬로우 스타트 임계값)에 도달하면 혼잡 회피(Congestion Avoidance) 상태로 전환
    - 손실 이벤트가 발생하면 cwnd는 1 MSS로 리셋되고 ssthresh는 cwnd의 절반 값으로 설정되며 슬로우 스타트가 다시 시작

2. **혼잡 회피(Congestion Avoidance)**
    - cwnd를 1 MSS씩 점진적으로 증가시킴. cwnd가 ssthresh에 도달하거나 이를 초과하면 더 이상 지수적으로 증가하지 않고 선형적으로 증가함
    - 타임아웃 발생 시 cwnd는 다시 1 MSS로 설정되고 ssthresh는 cwnd의 절반 값으로 업데이트됨
    - 이 상태에서 세 개의 중복 ACK가 수신되면 cwnd는 절반으로 줄어들고 ssthresh는 cwnd의 절반 값으로 설정됨. 그리고 빠른 복구(Fast Recovery) 상태로 전환됨

3. **빠른 복구(Fast Recovery)**
    - 세 개의 중복 ACK가 감지되면 cwnd가 절반으로 줄어들고 ssthresh는 cwnd의 절반 값으로 설정됨. 이 후 **빠른 재전송(Fast Retransmit)** 을 수행하고, 빠른 복구 상태로 전환하여 세그먼트 손실을 빠르게 복구함
    - cwnd는 계속해서 증가할 수 있고 손실된 세그먼트가 재전송되면 송신 속도 조정이 더 빨리 이루어짐

4. **상태 전환 설명:**
    - 슬로우 스타트 → 혼잡 회피 : cwnd가 ssthresh에 도달하거나 초과하면 상태가 전환
    - 혼잡 회피 → 슬로우 스타트 : 타임아웃 발생 시 cwnd가 1 MSS로 리셋되고 다시 슬로우 스타트로 돌아감
    - 혼잡 회피 → 빠른 복구 : 세 개의 중복 ACK가 감지되면 빠른 복구 상태로 진입
    - 빠른 복구 → 혼잡 회피 : 빠른 복구가 완료되면 다시 혼잡 회피로 돌아감

---

### Fast Recovery

**Fast Recovery(빠른 복구)** : TCP Reno에서 도입된 혼잡 제어 메커니즘으로, 타임아웃이나 세 개의 중복 ACK가 발생했을 때 **혼잡 윈도우(cwnd)** 를 효율적으로 처리하여 성능을 개선하는 기법

**Fast Recovery 동작 원리** :

  - **세 개의 중복 ACK 수신** : TCP Reno에서 세 개의 중복 ACK가 수신되면, 이는 네트워크에서 일부 세그먼트가 손실되었음을 나타냅니다. 이때, TCP Reno는 **Fast Recovery**를 활성화하여, **혼잡 윈도우(cwnd)** 를 1 MSS로 급격히 줄이지 않고, 손실된 세그먼트의 재전송을 시작
  
  - **혼잡 윈도우 증가** : Fast Recovery 상태에서, **혼잡 윈도우** 는 각 중복 ACK마다 1 MSS씩 증가(혼잡 윈도우를 서서히 증가시켜 혼잡을 피하면서도 네트워크 대역폭을 효율적으로 활용하려는 목적)
  
  - **손실된 세그먼트의 ACK 수신** : 손실된 세그먼트에 대한 ACK가 수신되면, TCP Reno는 **혼잡 윈도우** 를 ssthresh의 값으로 리셋하고 혼잡 회피 상태로 전환
    
  - **타임아웃 발생 시 처리** : TCP Reno에서 타임아웃이 발생하면 Fast Recovery는 **슬로우 스타트(Slow Start)** 로 돌아가며 cwnd는 1 MSS로 리셋되고 ssthresh는 손실된 세그먼트의 cwnd 값의 절반으로 설정
  
**TCP Reno vs TCP Tahoe**

  - TCP Tahoe는 세 개의 중복 ACK나 타임아웃 발생 시, cwnd를 1 MSS로 리셋하고 슬로우 스타트로 전환하여 혼잡 윈도우를 다시 초기화함

  - TCP Reno는 세 개의 중복 ACK 발생 시 Fast Recovery를 통해 혼잡 윈도우를 감소시키고 재전송을 통해 더 빠르게 복구하며 혼잡 회피 상태로 전환됨

![image](https://github.com/user-attachments/assets/d161fb77-e42d-40fb-8b6e-11346061f0db)

- **초기 설정** : ssthresh는 8 MSS로 설정

- **슬로우 스타트** : 혼잡 윈도우는 지수적으로 증가하며 4번째 라운드에서 ssthresh에 도달. 이후 선형적으로 증가

- **세 개의 중복 ACK 발생** : 혼잡 윈도우가 12 MSS에 도달한 후 ssthresh는 6 MSS로 설정

  - TCP Reno는 cwnd를 9 MSS로 설정하고 선형적으로 증가

  - TCP Tahoe는 cwnd를 1 MSS로 리셋하고 다시 슬로우 스타트로 돌아가며 지수적으로 증가
 
---

### TCP Congestion Control: Retrospective

**AIMD** (Additive Increase, Multiplicative Decrease) 형식의 혼잡 제어 : **선형 증가(Additive Increase)** 와 **배수 감소(Multiplicative Decrease)** 를 결합하여 네트워크 혼잡을 피하면서 효율적으로 대역폭을 활용하는 방식

AIMD 동작 방식 : 

**선형 증가 (Additive Increase)** : cwnd는 1 MSS씩 매 RTT마다 선형적으로 증가합니다.

**배수 감소 (Multiplicative Decrease)** : 세 개의 중복 ACK 이벤트가 발생하면, cwnd는 절반으로 감소합니다.

- AIMD 방식은 **"톱니 모양"** 의 패턴을 생성
  
![image](https://github.com/user-attachments/assets/7e345448-00bf-4cc1-83ac-e13cbf955f6c)

- cwnd는 선형적으로 증가하다가 세 개의 중복 ACK가 발생하면 급격히 감소하고, 그 후 다시 선형적으로 증가를 반복
