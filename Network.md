# Network

## 1. OSI 7계층
> * 국제표준화기구(ISO)에서 개발한 모델로, 컴퓨터 네트워크 프로토콜 디자인과 통신을 계층으로 나누어 설명한 것이다.
> * 각 계층은 하위 계층의 기능만을 이용하고, 상위 계층에게 기능을 제공한다.
> * 일반적으로 하위 계층들은 하드웨어로, 상위 계층들은 소프트웨어로 구현된다.

![osi7](https://github.com/WeareSoft/tech-interview/raw/master/contents/images/osi-7-layer.png "osi 7계층")

### _1) 물리 계층 (Physical layer)_
* 전기적, 기계적, 기능적인 특성을 이용해 데이터를 전송한다.
* 데이터는 0과 1의 비트열, 즉 on/off의 전기적 신호 상태로 이루어져 있다.
* 에러가 있는지는 관여하지 않고 단지 데이터를 전달하기만 한다.  
▶ 전송 단위 : Bit

### _2) 데이터 링크 계층 (Data link layer)_
* 물리 계층에서 송수신되는 정보의 오류와 흐름을 관리하여 안전한 정보의 전달을 수행할 수 있도록 도와주는 역할을 한다.
* __포인트 투 포인트(Point to Point)__ 간 신뢰성있는 전송을 보장한다.
* 주소 값은 물리적으로 할당 받는데, 이는 네트워크 카드가 만들어질 때부터 맥 주소(MAC address)가 정해져 있다는 뜻이다.
* 데이터 링크 계층의 가장 잘 알려진 예는 이더넷이다.  
▶ 전송 단위 : Frame

### _3) 네트워크 계층 (Network layer)_
* 라우팅 : 목적지까지 가장 안전하고 빠르게 데이터를 보낼 수 있는 최적의 경로를 설정한다.
* 흐름 제어, 세그멘테이션(segmentation/desegmentation), 오류 제어, 인터네트워킹(Internetworking) 등을 수행한다.
* 논리적인 주소 구조(IP), 곧 네트워크 관리자가 직접 주소를 할당하는 구조를 가지며, 계층적(hierarchical)이다.  
▶ 전송 단위 : Packet

### _4) 전송 계층 (Transport layer)_
* 양 끝단(End to end)의 사용자들이 신뢰성있는 데이터를 주고 받을 수 있도록 해 주어, 상위 계층들이 데이터 전달의 유효성이나 효율성을 생각하지 않도록 해준다.
* 시퀀스 넘버 기반의 오류 제어 방식을 사용한다.
* 대표적인 프로토콜 : TCP, UDP  
▶ 전송 단위는 : Segment

### _5) 세션 계층 (Session layer)_
* 양 끝단의 응용 프로세스가 통신을 관리하기 위한 방법을 제공한다.
* 이 계층은 TCP/IP 세션을 만들고 없애는 책임을 진다.

### _6) 표현 계층 (Presentation layer)_
* 데이터를 어떻게 표현할 지 정하는 역할을 하는 계층으로 일종의 확장자이다.
* 세가지의 기능을 갖고 있다
  * 송신자에서 온 데이터를 해석하기 위한 응용계층 데이터 부호화, 변화
  * 수신자에서 데이터의 압축을 풀 수 있는 방식으로 된 데이터 압축
  * 데이터의 암호화와 복호화
* MIME 인코딩이나 암호화 등의 동작이 이 계층에서 이루어진다.

### _7) 응용 계층(Application layer)_
* 응용 프로세스와 직접 관계하여 일반적인 응용 서비스를 수행한다.
* 일반적인 응용 서비스는 관련된 응용 프로세스들 사이의 전환을 제공한다.


## 2. TCP & UDP
### _1) TCP (Transmission Control Protocol)_
> : 인터넷상에서 데이터를 메시지의 형태로 보내기 위해 IP와 함께 사용하는 프로토콜
* 일반적으로 TCP와 IP를 함께 사용하는데, IP가 데이터의 배달을 처리한다면 TCP는 패킷을 추적하고 관리한다.
* 연결형 서비스를 지원하는 프로토콜로 인터넷 환경에서 기본으로 사용한다.

![tcp](https://t1.daumcdn.net/cfile/tistory/991BEB3359FEB5712F "TCP")

__[TCP 특징]__
* 연결형 서비스로 가상회선 방식을 제공한다.
  * 발신지와 수신지 간의 논리적 경로를 배정한다.
* 3-way handshaking과정을 통해 연결을 설정하고 4-way handshaking을 통해 해제한다.
* 흐름 제어 및 혼잡 제어를 수행한다.
* 높은 신뢰성을 보장한다.
* UDP보다 속도가 느리다.
* 전이중(Full-Duplex), 점대점(Point to Point) 방식
* 패킷에 대한 응답을 해야하기 때문에 성능이 낮다.
* Streaming 서비스에 불리하다.

#### _1-1) 3-way handshaking & 4-way handshaking_
* 3-way handshaking
: TCP 통신을 이용하여 데이터를 전송하기 위해 네트워크 연결을 설정(Connection Establish) 하는 과정

![3way](https://github.com/WeareSoft/tech-interview/raw/master/contents/images/3-way-handshaking.png "3-way handshaking")

* 4-way handshaking
: TCP의 연결을 해제(Connection Termination) 하는 과정

![4way](https://github.com/WeareSoft/tech-interview/raw/master/contents/images/4-way-handshaking.png "4-way handshaking")


### _2) UDP (User Datagram Protocol)_
> : 데이터를 데이터그램 단위로 처리하는 프로토콜
* 데이터그램 : 독립적인 관계를 지니는 패킷
* 비연결형 프로토콜
* 각각의 패킷은 다른 경로로 전송되고, 서로 독립적인 관계를 가진다.

![udp](https://t1.daumcdn.net/cfile/tistory/9969973359FEB59309 "UDP")

__[UDP 특징]__
* 비연결형 서비스로 데이터그램 방식을 제공한다.
* 정보를 주고 받을 때 보내거나 받는다는 신호절차를 거치지 않는다.
* UDP헤더의 CheckSum 필드를 통해 최소한의 오류만 검출한다.
* 신뢰성이 낮다
* TCP보다 속도가 빠르다.
* 1:1, 1:N, N:M 등으로 연결될 수 있다.
* 흐름제어가 없다.

### _3) TCP VS UDP_
![tcp/udp](https://t1.daumcdn.net/cfile/tistory/990C0F3359FDD3F80C "tcp VS udp")

## 3. 통신 방식
### _1) Http (HyperText Transfer Protocol)_
> : 클라이언트의 요청(request)이 있을 때만 서버가 응답(Response)하여 해당 정보를 전송하고 곧바로 연결을 종료하는 방식

![http](https://t1.daumcdn.net/cfile/tistory/99926F335C6939EA38 "http")
#### [Http 통신의 특징]
* TCP와 UDP를 사용하며, 80번 포트를 사용한다.
* 단방향적 통신
  * Server가 Client로 요청을 보낼 수 없다.
* 실시간 연결이 아닌, 필요한 경우에만 서버로 접근하는 콘텐츠 위주의 데이터를 사용할 때 용이하다.
* 비연결(Connectionless) : 클라이언트가 요청을 서버에 보내고 서버가 적절한 응답을클라이언트에 보내면 바로 연결이 끊긴다.
* 무상태(Stateless) : 연결을 끊는 순간 클라이언트와 서버의 통신은 끝나며 상태 정보를 유지하지 않는다.
* 어플리케이션 개발에 주로 사용된다.

### _1-1) Https (HyperText Transfer Protocol over Secure Socket Layer)_
> : 웹 통신 프로토콜인 HTTP의 보안이 강화된 버전의 프로토콜
#### [Https 통신의 특징]
* HTTPS의 기본 TCP/IP 포트로 443번 포트를 사용한다.
* 웹 상에서 정보를 암호화하는 SSL이나 TLS 프로토콜을 통해 세션 데이터를 암호화한다.
  * TLS(Transport Layer Security) 프로토콜은 SSL(Secure Socket Layer) 프로토콜에서 발전한 것이다.
  * 두 프로토콜의 주요 목표는 기밀성(사생활 보호), 데이터 무결성, ID 및 디지털 인증서를 사용한 인증을 제공하는 것이다.
#### [Https가 필요한 이유]
* 클라이언트인 웹브라우저가 서버에 HTTP를 통해 웹 페이지나 이미지 정보를 요청하면 서버는 이 요청에 응답하여 요구하는 정보를 제공하게 된다.
* 웹 페이지(HTML)는 텍스트이고, HTTP를 통해 이런 텍스트 정보를 교환하는 것이다.
* 이때 주고받는 텍스트 정보에 주민등록번호나 비밀번호와 같이 민감한 정보가 포함된 상태에서 네트워크 상에서 중간에 제3자가 정보를 가로챈다면 보안상 큰 문제가 발생한다.
* 즉, 중간에서 정보를 볼 수 없도록 주고받는 정보를 암호화하는 방법인 HTTPS를 사용하는 것이다.

### _2) Socket_
> :  Server와 Client가 특정 Port를 통해 실시간으로 양방향 통신을 하는 방식
![socket](https://t1.daumcdn.net/cfile/tistory/9939C6385C6939FD26 "socket")
#### [Socket 통신의 특징]
* Server와 Client가 계속 연결을 유지하는 양방향 통신이다.
* Server와 Client가 실시간으로 데이터를 주고받는 상황이 필요한 경우에 사용된다.
* 실시간 동영상 Streaming이나 온라인 게임 등과 같은 경우에 자주 사용된다.

## 4. REST


---
## _* 참고_
1. <https://github.com/WeareSoft/tech-interview/blob/master/contents/network.md>
1. <https://reakwon.tistory.com/59>
1. <https://mangkyu.tistory.com/15?category=762469>
1. <https://mangkyu.tistory.com/48?category=762469>
