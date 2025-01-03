# Chapter 2
##### Application Layer

---

## 2.2 The Web and HTTP

### 서론

- 1990년대 초까지 인터넷은 주로 연구자, 학자, 대학생들이 원격 호스트에 로그인하거나 파일을 전송하고, 뉴스나 이메일을 주고받는 용도로 사용되었다.
- 이러한 응용 프로그램은 매우 유용했지만, 인터넷은 학계와 연구 커뮤니티 외에는 거의 알려지지 않았다.
- 1990년대 초, 월드 와이드 웹(World Wide Web)이 등장하면서 인터넷은 대중의 관심을 끌게 되었다.
- 웹은 사람들이 작업 환경 안팎에서 상호작용하는 방식을 극적으로 변화시켰으며, 인터넷을 단순한 데이터 네트워크에서 사실상 유일한 데이터 네트워크로 끌어올렸다.
- 웹의 가장 큰 매력은 사용자에게 원하는 정보를 즉시 제공한다는 점이다. 이는 전통적인 라디오나 TV 방송과는 달리, 사용자가 원하는 시간에 콘텐츠를 받을 수 있음을 의미한다.
- 웹은 또한 정보를 쉽게 공유할 수 있게 만들어, 누구나 저렴한 비용으로 콘텐츠를 발행할 수 있게 해준다.
- 하이퍼링크, 검색 엔진, 사진, 비디오 등 다양한 기능들이 정보를 탐색하고 사용자 경험을 향상시킨다.
- 웹과 그 프로토콜은 YouTube, Gmail, Instagram, Google Maps와 같은 웹 기반 애플리케이션의 플랫폼 역할을 한다.

---

## 2.2.1 Overview of HTTP

- **HTTP** : 웹의 애플리케이션 계층 프로토콜, 클라이언트와 서버가 메시지를 교환하는 방식을 정의
- 객체 : HTML 파일, JPEG 이미지, 자바스크립트 파일, CCS 스타일 시트 파일 또는 비디오 클립과 같은 파일, 단일 URL로 주소 지정
- URL 구성요소 : 객체를 수용하는 서버의 호스트 이름, 객체의 경로 이름
  
  EX) http://www.someschool.edu/someDepartment/picture.gif

  호스트 이름 :  www.someSchool.edu

  경로이름 : someDepartment/picture.gif

- HTTP 웹 서버 : URL로 주소 지정 가능, Apache, Microsoft Internet Information Server 등 다양한 웹 서버가 존재함

- 클라이언트와 서버 간의 상호 작용
  
  ![image](https://github.com/user-attachments/assets/904ae999-e08f-4816-b18e-0689f9a1d855)
        
        1. PC (Internet Explorer 사용): 사용자가 PC에서 Internet Explorer 브라우저를 통해 웹 서버에 HTTP 요청을 보냄
        
        2. 서버 (Apache Web 서버): 웹 서버는 받은 HTTP 요청을 처리하고, 클라이언트에게 HTTP 응답을 보냄
        
        3. Android 스마트폰 (Google Chrome 사용): 동일한 서버에 Android 스마트폰에서 Google Chrome을 통해 HTTP 요청을 보냄
        
        4. 서버 (Apache Web 서버): 서버는 두 번째 HTTP 요청을 처리하고, 또 다른 HTTP 응답을 보내 클라이언트에게 정보를 전달함

- HTTP는 TCP를 전송 프로토콜로 사용하며, 클라이언트와 서버는 소켓 인터페이스를 통해 데이터를 주고받음

- 클라이언트는 HTTP 요청 메시지를 보내고, 서버는 이를 처리하여 HTTP 응답 메시지를 반환함

- TCP는 신뢰할 수 있는 데이터 전송을 보장하여 HTTP 요청과 응답이 온전하게 전달되도록 함

- HTTP는 데이터 손실이나 재정렬을 처리할 필요 없이, TCP에 의존해 안정적인 전송을 함

- **비저장(stateless) 프로토콜** : 클라이언트의 상태 정보를 저장하지 않으며, 클라이언트가 같은 객체를 여러 번 요청해도 서버는 매번 객체를 새로 전송하는 것

- HTTP의 원래 버전은 HTTP/1.0이며, 2020년 현재 대부분의 HTTP 트랜잭션은 HTTP/1.1을 사용함, 최근에는 HTTP/2도 지원되는 경우가 많음 

---

## 2.2.2  Non-Persistent and Persistent Connections

- 클라이언트-서버 상호작용이 TCP를 통해 이루어질 때, 각 요청/응답을 별도의 TCP 연결로 처리할지 아니면 동일한 TCP 연결을 사용할지 결정해야 함

- 요청/응답을 각각 별도의 연결로 보내는 방식은 **비영속적 연결(non-persistent connection)**, 동일한 연결로 보내는 방식은 **영속적 연결(persistent connection)**

- HTTP는 기본적으로 영속적 연결을 사용하지만, 클라이언트와 서버는 비영속적 연결을 설정할 수도 있음

### HTTP with Non-Persistent Connections

BASE URL : http://www.someschool.edu/someDepartment/home.index

        1. 클라이언트가 서버와 TCP 연결 시작 : HTTP 클라이언트는 기본 포트 80을 사용해 서버와 TCP 연결을 시작하고 클라이언트와 서버 각각에 소켓이 연결
        
        2. HTTP 요청 메시지 전송 : 클라이언트는 소켓을 통해 HTTP 요청 메시지를 서버에 보냄. 요청 메시지에는 /someDepartment/home.index 경로가 포함
        
        3. 서버가 요청 처리 및 응답 전송 : 서버는 요청을 받고, 해당 객체(/someDepartment/home.index)를 메모리나 디스크에서 찾은 후 HTTP 응답 메시지로 클라이언트에게 전송
        
        4. 서버가 TCP 연결 종료 : 서버는 TCP에게 연결을 종료하라고 지시 후 클라이언트가 응답을 온전히 받았는지 확인한 후에 연결을 실제로 종료

        5. 클라이언트가 응답 수신 및 TCP 연결 종료 : 클라이언트는 응답 메시지를 받고, 이를 통해 HTML 파일을 확인하며, 파일 내의 10개의 JPEG 객체에 대한 참조를 발견

        6. 참조된 JPEG 객체 처리 반복 : 위의 4단계를 각 JPEG 객체에 대해 반복하여 처리

- 비영속적 연결 : HTTP/1.0은 비영속적 연결을 사용하며, 각 TCP 연결은 하나의 요청과 응답을 처리하고, 이후 연결이 종료

- **왕복 시간(RTT)** : 클라이언트에서 서버로 작은 패킷이 왕복하는 데 걸리는 시간(패킷 전파 지연, 라우터 큐잉 지연, 처리 지연)

- 왕복 시간 계산 방법 : 웹 페이지 요청 시, TCP 연결을 시작하는 데 1회 RTT가 소요, 요청 후 서버에서 HTML 파일을 전송하는 데 또 다른 RTT가 소요.

- **Three-Way Handshake** : 클라이언트가 서버에 연결 요청을 보내고, 서버가 이를 확인 후 응답하며, 클라이언트가 다시 이를 확인하는 과정

![image](https://github.com/user-attachments/assets/7124b262-6978-4443-b427-e950b02d2980)

- 해당 Three-Way Handshake 그림에서는 총 2 RTT가 소요.

### HTTP with Persistent Connections

- 비영속적 연결의 단점: 각 객체마다 새로운 연결을 설정하고 유지해야 하며, 이로 인해 클라이언트와 서버에서 TCP 버퍼와 변수를 할당해야 하므로 서버에 부담을 줄 수 있음

- 영속적 연결 : HTTP/1.1에서는 서버가 응답을 보낸 후 TCP 연결을 유지하며, 같은 클라이언트와 서버 간의 후속 요청과 응답은 동일한 연결을 통해 전송

- **Pipelining** : 여러 웹 페이지 요청이 동일한 연결을 통해 후속적으로 이루어질 수 있고, 요청에 대한 응답을 기다리지 않고 동시에 처리된다는 뜻

- 비영속적 연결과의 성능 비교는 후속장에서 진행됨

## 2.2.3  HTTP Message Format

### HTTP Request Message

 **HTTP Request Message** : 첫 번째 줄은 요청 라인(request line), 이후 라인은 헤더 라인(header line)
 
 HTTP Request Message 구성 예시 : 

    GET /somedir/page.html HTTP/1.1
    Host: www.someschool.edu
    Connection: close
    User-agent: Mozilla/5.0
    Accept-language: fr

- **GET /somedir/page.html HTTP/1.1**: 클라이언트가 서버에서 /somedir/page.html 객체를 요청하며, HTTP/1.1 버전을 사용한다는 요청
- **Host: www.someschool.edu** : 요청된 객체가 있는 서버의 호스트를 지정.
- **Connection : close** : 서버에 요청 후 연결을 닫도록 지시, 즉 영속적 연결을 사용하지 않음.
- **User-agent : Mozilla/5.0** : 요청을 보낸 브라우저 유형을 지정, 여기서는 Firefox 브라우저를 사용
- **Accept-language : fr** : 프랑스어 버전의 객체를 선호한다고 서버에 알려주며, 해당 버전이 없으면 기본 버전을 요청

- **HTTP Request Message 기본 포맷** :
  
    ![image](https://github.com/user-attachments/assets/982aecf8-6d58-429f-8c15-4171751f53c0)

1. **요청 라인(Request Line)** : 첫 번째 줄에는 method(메서드), URL(요청된 자원의 URL), Version(HTTP 버전)이 포함. 예를 들어, GET /somedir/page.html HTTP/1.1과 같은 형식.

2. **헤더 라인(Header Lines)** : 요청 메시지에 대한 추가 정보가 포함된 여러 줄로 이루어져 있으며, 각 줄은 header field name: value 형식. 이 라인은 여러 개일 수 있으며, 각 필드는 캐리지 리턴과 라인 피드로 구분.

       캐리지 리턴(CR): \r로 표현되며, 텍스트 커서를 현재 줄의 맨 앞으로 이동
    
       라인 피드(LF): \n으로 표현되며, 텍스트 커서를 다음 줄로 이동
   
3. **빈 라인(Blank Line)** : 헤더 라인 뒤에는 빈 라인이 포함되며, 이는 헤더와 본문을 구분하는 역할을 수행.

4. **본문(Entity Body)** : 본문은 요청 메시지의 마지막 부분에 위치하며, 사용자가 입력한 데이터가 포함될 수 있음. GET 요청에서는 본문이 비어 있지만, POST 요청에서는 사용자가 제출한 데이터가 포함.

HTTP 요청 메시지에서 사용되는 메서드:

- **GET** : 서버에서 웹 페이지를 요청할 때 사용
- **POST** : 사용자가 폼에 입력한 데이터를 서버에 전송할 때 사용
- **HEAD** : GET과 유사하지만, 서버는 요청된 객체를 포함하지 않고 HTTP 메시지만 응답
- **PUT** : 웹 서버에 파일을 업로드할 때 사용
- **DELETE** : 서버에서 객체를 삭제할 때 사용

### HTTP Response Message

**HTTP Response Message** : 상태 라인(Status Line), 헤더 라인(Header Lines), 본문(Entity Body) 으로 구성됨
 
 HTTP Response Message 구성 예시 : 

    HTTP/1.1 200 OK
    
    Connection: close
    Date: Tue, 18 Aug 2015 15:44:04 GMT
    Server: Apache/2.2.3 (CentOS)
    Last-Modified: Tue, 18 Aug 2015 15:11:03 GMT
    Content-Length: 6821
    Content-Type: text/html
    
    (data data data data data ...)

- **HTTP/1.1 200 OK** : 서버가 HTTP/1.1을 사용하며 요청이 정상적으로 처리되었음을 나타냄
- **Connection: close** : 서버가 응답 후 TCP 연결을 닫겠다고 클라이언트에게 전
- **Date: Tue, 18 Aug 2015 15:44:04 GMT** : 응답 메시지가 서버에서 생성되고 전송된 시간
- **Server: Apache/2.2.3 (CentOS)** : 서버 소프트웨어 정보, Apache 서버가 사용
- **Last-Modified: Tue, 18 Aug 2015 15:11:03 GMT** : 요청된 객체가 마지막으로 수정된 시간
- **Content-Length: 6821** : 전송되는 객체의 크기를 바이트 단위로 표시
- **Content-Type: text/html** : 응답 본문에 포함된 객체가 HTML 텍스트임을 나타
- **(data data data data data ...)** : 요청된 객체의 실제 내용, HTML 파일 내용이 사용됨

- **HTTP Response Message 기본 포맷** :

    ![image](https://github.com/user-attachments/assets/417733e3-619f-4b12-b112-d55e4e2c2084)

1. **상태 라인(Status Line) :** : 첫 번째 줄에는 version(HTTP 프로토콜 버전), status code(요청 처리 결과), phrase(상태 코드와 관련된 설명 문구)이 포함.

      상태 코드 종류:

    - **200 OK** : 요청이 성공적으로 처리되어 정보가 응답에 포함
    - **301 Moved Permanently** : 요청한 객체가 영구적으로 이동되었으며, 새로운 URL이 Location 헤더에 포함
    - **400 Bad Request** : 요청이 서버에서 이해되지 않음
    - **404 Not Found** : 요청한 문서가 서버에 없음
    - **505 HTTP Version Not Supported** : 요청한 HTTP 프로토콜 버전이 서버에서 지원되지 않음

2. **헤더 라인(Header Lines)** : 각 헤더는 header field name: value 형식으로 제공, 클라이언트가 요청한 객체에 대한 추가 정보를 제공

3. **빈 라인(Blank Line)** : 헤더 라인 뒤에는 빈 라인이 포함되며, 이는 헤더와 본문을 구분하는 역할을 수행.

4. **본문(Entity Body)** : 본문은 요청 메시지의 마지막 부분에 위치하며, 사용자가 입력한 데이터가 포함될 수 있음. GET 요청에서는 본문이 비어 있지만, POST 요청에서는 사용자가 제출한 데이터가 포함.

- 실제 HTTP 응답 메시지 :
  
        telnet gaia.cs.umass.edu 80
        GET /kurose_ross/interactive/index.php HTTP/1.1
        Host: gaia.cs.umass.edu

  - telnet을 사용하여 웹 서버에 접속한 후, HTTP 요청 메시지를 보내면 응답 메시지를 받을 수 있음
  - /kurose_ross/interactive/index.php HTTP/1.1을 입력하면 HTML 파일을 받을 수 있음. 만약 객체 자체를 받지 않고 메시지만 보고 싶다면 GET 대신 HEAD를 사용

**브라우저는 브라우저 유형, 버전, 사용자 설정, 캐시된 객체의 유효성 여부에 따라 요청 메시지에 헤더 라인을 생성함**

## 2.2.4  User-Server Interaction: Cookies

**COOKIE** :  웹 브라우저에 저장되는 작은 데이터 파일, 주로 사용자의 웹사이트 방문 기록, 설정, 로그인 상태 등을 유지하는 데 사용됨

![image](https://github.com/user-attachments/assets/29492a7c-7827-499c-b599-e781cda713f4)

1. **클라이언트에서 HTTP 요청** : 클라이언트가 웹사이트를 처음 방문하면 Amazon 서버에 HTTP 요청 메시지를 보냄. 이 요청에는 GET /와 같은 일반적인 HTTP 요청 메시지가 포함됨

2. **서버에서 Set-cookie 헤더 포함 응답** : Amazon 서버는 클라이언트에게 Set-cookie: 1678라는 헤더를 포함한 HTTP 응답을 보냄. 이 헤더는 클라이언트가 자신의 쿠키 파일에 저장할 고유한 식별 번호를 제공함

3. **쿠키 저장** : 클라이언트의 브라우저는 Set-cookie 헤더를 읽고, 1678 식별 번호를 쿠키 파일에 저장함. 쿠키 파일에는 Amazon과 eBay와 같은 다른 사이트들의 쿠키도 저장됨

4. **후속 요청 시 쿠키 사용** : 클라이언트가 Amazon 사이트를 다시 방문하면 클라이언트의 브라우저는 이전에 저장된 쿠키에서 식별 번호 1678을 추출 후 Cookie: 1678 헤더를 포함한 HTTP 요청 메시지를 Amazon 서버에 보냄

5. **서버에서 쿠키 기반 작업 처리** : Amazon 서버는 이 식별 번호를 사용하여 클라이언트를 추적하고 해당 사용자와 관련된 데이터베이스 항목에 접근하여 맞춤형 작업을 수행함. 예를 들어, 사용자의 쇼핑 카트나 이전 방문 기록을 불러옴

- 쿠키는 사용자를 식별하는 데 사용됨.

- 첫 방문 시 사용자 식별 정보를 제공하고, 이후 방문 시 쿠키 헤더를 통해 사용자 식별.
  
- 예시: 웹 기반 이메일 서비스에서 쿠키를 사용해 로그인 후 사용자 식별 및 세션 유지.
 
- 쿠키는 개인정보 침해 우려가 있으며, 웹사이트가 사용자 데이터를 추적하고 제3자에게 판매할 수 있음.

## 2.2.5  Web Caching

**웹 캐시(프록시 서버)** :  원본 웹 서버를 대신하여 HTTP 요청을 처리하는 네트워크 엔티티

  - 웹 캐시는 자체 디스크 저장소를 가지고 있으며, 최근에 요청된 객체들의 복사본을 저장
  - 사용자의 브라우저는 모든 HTTP 요청을 먼저 웹 캐시로 전달하도록 설정할 수 있음
  - 웹 캐시 설치 : ISP나 기관에서 설치
  - 웹 캐시의 장점 : 응답 시간 단축, 트래픽 감소

  ![image](https://github.com/user-attachments/assets/3cdcf1c0-fe52-4a90-b65d-21e52055fcb8)

1. **브라우저와 웹 캐시 연결** : 브라우저는 웹 캐시와 TCP 연결을 설정하고, 객체에 대한 HTTP 요청을 웹 캐시로 보냄.

2. **웹 캐시에서 객체 확인** : 웹 캐시는 요청된 객체가 자신의 저장소에 있는지 확인함. 만약 있다면, 객체를 HTTP 응답 메시지로 클라이언트 브라우저에 반환함.

3. **웹 캐시에 객체 없음** : 웹 캐시가 객체를 가지고 있지 않으면, 원본 서버(www.someschool.edu)와 새로운 TCP 연결을 열고, 해당 객체를 요청함. 원본 서버는 객체를 HTTP 응답 메시지로 웹 캐시에 보냄.

4. **웹 캐시에서 객체 저장 및 전달** : 웹 캐시는 객체를 자신의 저장소에 저장하고, 그 후 클라이언트 브라우저에 해당 객체를 HTTP 응답 메시지로 전달함. 이때 기존의 TCP 연결을 사용함.

  ![image](https://github.com/user-attachments/assets/eabdfdd4-ae04-483b-b52f-e0159439fbc1)

그림 구성 : 
- **원본 서버(Origin servers)** : 공용 인터넷에 연결되어 있으며, 전 세계 여러 곳에 분산되어 있음
- **공용 인터넷** : 15 Mbps의 액세스 링크로 연결되어 있음
- **네트워크** : 100 Mbps의 고속 LAN으로 구성되어 있으며, 기관 내의 여러 클라이언트들이 연결되어 있음

1. **LAN에서의 트래픽 밀도** :
   - 트래픽 밀도는 초당 요청 수와 요청당 데이터 크기를 대역폭으로 나눈 값으로 계산
   - **계산식** :
     ```
     트래픽 밀도 = (초당 요청 수 × 요청당 데이터 크기) / 대역폭
     ```
   - **값 대입** :
     ```
     트래픽 밀도 = (15 requests/sec × 1 Mbits/request) / 100 Mbps = 0.15
     ```
   - **결과** : 트래픽 밀도 0.15는 LAN에서의 지연을 거의 무시할 수 있는 수준

2. **액세스 링크에서의 트래픽 밀도**:
   - 액세스 링크는 15 Mbps의 대역폭을 가지고 있으며, 여기에 같은 방식으로 트래픽 밀도를 계산
   - **계산식** :
     ```
     트래픽 밀도 = (초당 요청 수 × 요청당 데이터 크기) / 대역폭
     ```
   - **값 대입** :
     ```
     트래픽 밀도 = (15 requests/sec × 1 Mbits/request) / 15 Mbps = 1
     ```
   - **결과** : 트래픽 밀도 1은 액세스 링크에서의 트래픽 밀도가 최대치에 달하는 것으로, 이 경우 지연이 **무한히 커질 수 있으며** 몇 분 이상의 응답 시간이 발생할 수 있음

**결론** :
- **LAN에서의 트래픽 밀도**는 0.15로, 대부분의 지연을 무시할 수 있지만, **액세스 링크에서의 트래픽 밀도**가 1에 도달하면 지연이 매우 커져, **응답 시간이 수 분 이상** 걸릴 수 있음 => 해결책이 필요

**해결책** :
1. 해결책 1 : **액세스 링크 업그레이드**
   - 액세스 링크의 대역폭을 15 Mbps에서 100 Mbps로 업그레이드하면, 액세스 링크의 트래픽 밀도가 0.15로 줄어들어 두 라우터 간의 지연 시간이 거의 없어짐.
   - 이 경우, 전체 응답 시간은 대략 2초가 되며, 이는 인터넷 지연에 해당됨.
   - 하지만 이 해결책은 **비용이 많이 듬**
  
2. 해결책 2 : **웹 캐시 설치**
   ![image](https://github.com/user-attachments/assets/082da37a-82b0-48d6-a8bb-3dbec5b08e1d)

   - 액세스 링크를 업그레이드하지 않고 **웹 캐시**를 설치하여 해결 가능
   - 웹 캐시는 **요청의 40%를 즉시 처리**하고, 나머지 60%는 원본 서버를 통해 처리
   - 웹 캐시가 설치되면, 액세스 링크를 통과하는 요청이 60%로 줄어들어 트래픽 밀도가 1.0에서 0.6으로 감소
   - **트래픽 밀도 0.6**은 **수십 밀리초의 지연**으로, 이는 인터넷 지연에 비해 거의 무시할 수 있음
   - **계산식** :
     ```
     평균 딜레이=(캐시에서 처리된 요청 비율×캐시 처리 시간)+(원본 서버에서 처리된 요청 비율×서버 처리 시간)
     ```
   - **값 대입** :
     ```
     평균 딜레이 = 0.4 × (0.01 초) + 0.6 × (2.01 초) = 1.2 초
     ```
- **콘텐츠 배급 네트워크(CDN)** : 전 세계에 분산된 캐시 서버를 통해 트래픽을 로컬화하고, 전체 인터넷 트래픽을 감소

### The Conditional GET

**The Conditional GET** : 웹 캐시가 객체가 최신 상태인지 확인하는 HTTP 메커니즘
    - 캐시된 객체가 오래되어 업데이트가 필요한지 확인하는 과정
    - 불필요한 데이터 전송을 줄이고 네트워크 성능을 향상
