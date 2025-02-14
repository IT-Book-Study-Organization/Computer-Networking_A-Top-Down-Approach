## 5.6 ICMP: The Internet Control Message Protocol

---

### 개요
```
인터넷 제어 메시지 프로토콜(ICMP)은 호스트와 라우터 간 네트워크 계층 정보를 교환하는 데 사용된다. 주로 오류 보고에 활용되며, 예를 들어 "대상 네트워크에 도달할 수 없음(Destination network unreachable)"과 같은 오류 메시지가 ICMP를 통해 전달된다.

IP 라우터가 특정 목적지로의 경로를 찾을 수 없을 때, ICMP 메시지를 생성하여 송신자에게 오류를 알린다. ICMP는 구조적으로 IP 위에 위치하며, ICMP 메시지는 IP 데이터그램 내에서 운반된다.
```

---

### ICMP 메시지의 구조
ICMP 메시지는 **유형(Type)** 과 **코드(Code)** 필드를 포함하며, ICMP 메시지가 처음 생성되도록 한 IP 데이터그램의 헤더 및 최초 8바이트를 포함함. 이를 통해 송신자는 어떤 데이터그램이 오류를 발생시켰는지를 확인할 수 있음.

![image](https://github.com/user-attachments/assets/5a76ac39-28f1-4de0-907d-c9754c60fa51)
*ICMP 메시지 종류*

- 주요 ICMP 메시지 유형
  - **Ping (에코 요청 및 응답)**
    - 'ping' 프로그램은 ICMP 유형 8 코드 0 메시지를 보내고, 대상 호스트는 ICMP 유형 0 코드 0 응답을 반환함.
  
  - **소스 퀀치(Source Quench)**
    - 네트워크 혼잡을 완화하기 위해 사용되었으나, 현재는 거의 사용되지 않음. (TCP의 혼잡 제어 기능이 대체함)

  - **Traceroute**
    - 'Traceroute' 프로그램은 네트워크 경로를 추적하는 데 ICMP를 활용.
    - UDP 패킷을 전송하며, TTL(Time-To-Live) 값을 1부터 증가시키면서 보냄.
    - TTL이 0이 되면 중간 라우터가 ICMP 유형 11 코드 0 ("TTL 초과") 메시지를 반환하여 네트워크 경로를 확인할 수 있음.

---

### ICMPv6 (IPv6 버전)
IPv6에서 기존 ICMP 메시지 유형과 코드를 재조직하였으며, IPv6의 새로운 기능을 지원하기 위해 추가적인 유형과 코드가 포함됨.
ICMPv6는 IPv6에서 필수적인 요소로 작용하며, 네트워크 계층에서 중요한 역할을 수행합니다.

- ICMPv6 추가 기능
  - **Packet Too Big** 유형 추가 (MTU 문제 대응)
  - **인식할 수 없는 IPv6 옵션** 오류 코드 추가
