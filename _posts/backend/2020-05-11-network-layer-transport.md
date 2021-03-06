---
title: "[Backend][RoadMap] Chap 0. Internet(Transport layer)"
excerpt: "github.com/kamranahmedse/developer-roadmap"
date: 2020-05-11
categories:
  - Backend
tags:
  - Backend 
  - roadmap

toc : true
toc_label: "=== Contents ==="
toc_icon: "heart"  # corresponding Font Awesome icon name (without fa prefix)
toc_sticky: true
classes: wide
---

본 포스팅은 [kamranahmedse의 개발자 로드맵](https://github.com/kamranahmedse/developer-roadmap)을 따라서 진행됩니다.  
---

Application Layer은 네트워크 애플리케이션과 애플리케이션 계층 프로토콜이 있는 곳이라고 설명했습니다. HTTP가 바로 애플리케이션 계층에서 동작하는 프로토콜입니다.**(만약 애플리케이션 계층 프로토콜로 RTMP를 사용하는 경우 목적지 URL이 rtmp://로 시작합니다. 즉, HTTP는 사용되지 않습니다.)** 트랜스포트 계층은 클라이언트와 서버 사이에 애플리케이션 계층 메시지를 전달하는 서비스를 제공합니다. 그런데 HTTP와 같은 애플리케이션 메시지가 어떻게 TCP 소켓에서 전송되는지 알지 못해서 뒤의 내용이 깊게 이해되지 않습니다. 그래서 트랜스포트 계층에 대한 이론을 짚고 넘어가겠습니다.  

# 트랜스포트 계층 서비스 및 개요

트랜스포트 계층 프로토콜은 서로 다른 호스트에서 동작하는 애플리케이션 프로세스 간의 `논리적인 통신logical communications`을 제공합니다. 논리적 통신은 애플리케이션의 관점에서 보면 프로세스들이 동작하는 호스트들이 직접 연결된 것처럼 보인다는 것을 의미합니다. 애플리케이션 프로세스는 메시지 운반에 사용되는 물리적인 하위 구조의 세부 사항에 상관없이 서로 메시지를송신하기 위해서 트랜스포트 계층에서 제공하는 논리적인 통신을 사용합니다.  

트랜스포트 계층 프로토콜은 네트워크 라우터가 아닌 종단 시스템에서 구현됩니다. 송신 측의 트랜스포트 계층은 송신 애플리케이션 프로세스로부터 수신한 메시지를 (송신측에도 트랜스포트 계층이 있음을 주목), 트랜스포트 계층 `세그먼트segment`인 트랜스포트 계층 패킷으로 변환합니다. 이러한 변환은 애플리케이션 메시지를 트랜스포트 계층 세그먼트로 만들기 위해서 작은 조각을 분할하고, 각각의 조각에 트랜스포트 계층 헤더를 추가함으로써 수행됩니다. 그런 후에 트랜스포트 계층은 송신 종단 시스템에 있는 네트워크 계층으로 세그먼트를 전달하고, 여기서 세그먼트가 네트워크 계층 패킷(datagram)안에 캡슐화되어 목적지로 전달됩니다. 네트워크 라우터는 오로지 datagram의 네트워크 계층 필드에 대해 동작한다는 것에 유념합니다. (즉, 라우터는 데이터 그램과 함꼐 캡슐화된 트랜스포트 계층 세그먼트의 필드를 검사하지 않습니다.) 수식 측에서, 네트워크 계층은 데이터그램으로부터 트랜스포트 계층 세그먼트를 추출하고, 트랜스포트 계층으로 세그먼트를 보냅니다. 이후 트랜스포트 계층은 수신 애플리케이션에서 세그먼트 내부의 데이터를 이용할 수 있도록 수쉰된 세그먼트를 처리합니다.  

## 트랜스포트 계층과 네트워크 계층 사이의 관계

트랜스포트 계층 프로토콜은 서로 다른 호스트에서 동작하는 `프로세스`들 사이의 논리적인 통신을 제공합니다. 네트워크 계층 프로토콜은 ` 호스트`들 사이의 논리적인 통신을 제공합니다.  

TCP/IP 네트워크는 애플리케이션 계층에게 두 가지 트랜스포트 계층 프로토콜을 제공합니다. 하나는 UDP로 비신뢰적이고 비연결형인 서비스이고, 나머지 하나는 TCP로 신뢰적인 연결지향적인 서비스입니다. IP 서비스 모델은 호스트들 간에 논리적인 통신을 제공하는 최선형 전달 서비스(best-effort delivery service)입니다. 최대한 노력하지만 어떠한 보장도 하지 않는 것을 의미합니다. 특히 IP는 세그먼트의 전달을 보장하지 않고, 세그먼트가 순서대로 전달되는 것을 보장하지 않습니다.  또한 IP는 세그먼트 내부 데이터의 무결성을 보장하지 않습니다. 이러한 이유 때문에 IP는 `비신뢰적인 서비스`라고 부릅니다. 트랜스포트 계층을 설명할 때는 각 호스트가 적어도 하나의 IP 주소를 가지고 있다고 가정합니다.  

UDP와 TCP의 가장 기본적인 기능은 '종단 시스템 사이의 IP 전달 서비스'를 '종단 시스템에서 동작하는 두 프로세스 간의 전달 서비스'로 확장하는 것입니다. 즉, `호스트 대 호스트` 전달을 `프로세스 대 프로세스` 전달로 확장하는 것이고 이를 `트랜스포트 다중화 transport multiplexing`와 `역다중화demultiplexing`이라고 부릅니다. UDP와 TCP는 헤더에 오류 검출 필드를 포함해서 무결성 검사를 제공합니다. 지금까지 설명한 `프로세스 대 프로세스 데이터 전달`과 `오류 검출`은 UDP가 제공하는 유일한 두 가지 서비스입니다.  

TCP는 UDP의 기능에 더해 `신뢰적인 데이터 전달reliable data transfer`를 제공합니다. 흐름제어, 순서번호, 확인응답, 타이머를 사용함으로써 TCP는 송신하는 프로세스로부터 수신하는 프로세스에게 데이터가 순서대로 정확하게 전달되도록 확실하게 합니다. 특히, TCP는 `혼잡제어congestion control`을 사용합니다. 혼잡제어는 인터넷에 대한 통상적인 서비스같이 애플리케이션이 야시 시켜서 제공되는 특정 서비스가 아니라, 전체를 위한 일반 서비스입니다. 예를 들어 한 TCP 연결이 과도한 양의 트래픽으로 모든 통신하는 호스트들 사이의 스위치와 링크를 폭주되게 하는 것을 방지합니다.  

## 다중화와 역다중화

`호스트 대 호스트` 전달을 `프로세스 대 프로세스` 전달로 확장하는 과정을 살펴봅니다. 목적지 호스트에서이 트랜스포트 계층은 바로 아래의 네트워크 계층으로부터 세그먼트를 수신합니다. 트랜스포트 계층은 호스트에서 동작하는 해당 애플리케이션 프로세스에게 이 세그먼트의 데이터를 전달하는 의무를 가집니다. 예를 들어서 제 컴퓨터에서 하나의 FTP 세션과 2개의 텔넷 세션을 실행하면서 웹 페이지를 다운로드 하고 있다고 가정하면, 저는 동작중인 4개의 네트워크 애플리케이션 프로세스를 갖습니다.(텔넷 2개, FTP 1개, HTTP 1개) 제 컴퓨터의 트랜스포트 계층이 하위 네트워크 계층으로부터 데이터를 수신할 때, 트랜스포트 계층은 이들 4개의 프로세스 중 하나에게 수신한 데이터를 전달할 필요가 있습니다.  

네트워크 애플리케이션의 한 부분으로서 프로세스는 소켓을 가집니다. 이를 통해서 네트워크에서 프로세스로의 데이터를 전달하며, 프로세스로부터 네트워크로 데이터를 전달하는 출입구 역할을 합니다. 따라서 수식 측 호스트의 트랜스포트 계층은 실제로 데이터를 직접 프로세스로 전달하지 않습니다. 대신에 중간 매개자인 소켓(애플리케이션 계층과 트랜스포트 계층 사이의)에게 전달합니다. 어떤 주어진 ㅅ ㅣ간에 수신 측 호스트에 하나 이상의 소켓이 있을 수 있으므로, 각각의 소켓은 어떤 하나의 유잃나 식별자를 가집니다. 이 식별자의 포멧은 UDP인지 TCP인지에 따라서 달라집니다.  

적절한 소켓을 찾는 방법은 다음과 같습니다. 각각의 트랜스포트 계층 세그먼트는 소켓을 찾기 위해 세그먼트에 필드 집합을 가지고 있습니다. 수신 측의 트랜스포트 계층은 수신 소켓을 식별하기 위해서 이들 필드를 검사합니다. 그리고 해당 세그먼트를 적절한 소켓으로 보냅니다. 트랜스포트 계층 세그먼트의 데이터를 올바른 소켓으로 전달하는 작업을 `역다중화demultiplexing`이라고 합니다. 출발지 호스트에서 소켓으로부터 데이터를 모으고 이에 대한 세그먼트를 생성하기 위해서 각 데이터에 헤더 정보로 캡슐화하고, 그 세그먼터들을 네트워크 계층으로 전달하는 작업을 `다중화multiplexing`이라고 합니다.  

이제 다중화와 역다중화의 개념을 이했으므로 실제로 호스트에서 어떻게 수행되는지 살펴보겠습니다. 트랜스포트 계층 다중화에는 다음 두 가지 요구 사항을 가지고 있음을 알 수 있습니다. 첫째, 소켓은 유일한 식별자를 갖습니다. 둘째, 각 세그먼트는 세그먼트가 전달될 적절한 소켓을 가리키는 특별한 필드를 가지고 있습니다. 이 특별한 필드라는 것이 `출발지 포트 번호 필드`와 `목적지 포트 번호 필드`입니다. 

### 비연결형 에서의 다중화와 역다중화

```python
clientSocket = socket(AF_INET, SOCK_DGRAM)
```

위 코드는 UDP 소켓을 생성합니다. UDP 소켓이 생셩될 때 트랜스포트 계층은 포트 번호를 소켓에게 자동으로 할당합니다. 특히 트랜스포트 계층은 현재 호스트에서 UDP 포트로 사용하지 않는 1024~65535 사이의 포트 변호를 할당합니다. 반면에 우리는 소켓을 생성한 뒤에, 특정 포트 번호(여기서는 19157)를 UDP 소켓에 할당하기 위해서 아래와 같이 합니다. 

```python
clientSocket.bind(('', 19157))
```

만약 코드를 작성하는 애플리케이션 개발자가 잘 알려진 프로토콜의 서버 측을 구현하고 있다며, 개발자는 상응하는 잘 알려진 포트 번호를 할당해야 합니다. 일반적으로 애플리케이션 서버 측이 특정 포트 번호를 할당하는 것에 반하여, 애플리케이션 클라이언트 측은 트랜스포트 계층이 포트 번호를 자동으로 할당해서 위와 같은 bind를 진행하지 않아도 됩니다.  

호스트의 트랜스포트 계츠은 애플리케이션 데이터, 출발지 포트 번호, 목적지 포트 번호, 그리고 나중에  설명할 2개의 값을 묶어서 트랜스포트 계층 세그먼트를 생성합니다. 그리고 네트워크 계층으로 전달합니다. 네트워크 계층은 세그먼트를 IP datagram으로 캡슐화하고 최선형 전달 서비스로 수신 호스트로 전달합니다. 수신 호스트는 datagram을 받아서 segment를 검사해서 목적지 포트 번호를 찾고 적절한 소켓에 전달합니다.  

### 연결지향형 다중화와 역다중화

TCP 역다중화를 수행하기 위해서는 TCP 소켓과 TCP 연결 설정을 살펴봐야합니다. UDP 소켓은 목적지 IP 주소와 목적지 포트 번호로 구성되고, TCP 소켓은 출발지 IP주소, 출발지 포트 번호, 목적지 IP 주소, 목적지 포트 번호 4개로 구성됩니다. 그래서 네트워크로부터 호스트에 TCP 세그먼트가 도착하면, 호스트는 해당되는 소켓으로 세그먼트를 역다중화하기 위해서 4개 값을 모두 사용합니다.  

서버가 받는 세그먼트는 클라이언트에 의해서 선택된 출발지 포트 번호를 포함합니다. 서버는 클라이언트로 부터의 연결을 받는 포트를 하나 생성하고, 이 포트 번호가 목적지인 연결 요청 세그먼트를 수신하면 연결을 기다리고 있는 서버 프로세스를 찾습니다. 그리고 서버는 새로운 소켓을 생성합니다. 이 때 새롭게 생성된 소켓은 세그먼트에 포함된 4개의 값에 주목합니다. 만약 다음에 도착하는 세그먼트의 4개의 값이 새롭게 생성된 소켓과 일치한다면 세그먼트는 이 소켓으로 역다중화됩니다. 이를 적절한 TCP 연결과 함꼐 클라이언트와 서버가 데이터를 주고 받는 것을 의미합니다.  

### 웹 서버와 TCP

아파치 웹 서버가 포트 번호 80 상에서 동작하는 호스트라고 가정하겠습니다. 클라이언트가 이 서버로 세그먼트로 보내면, 모든 세그먼트는 목적지 포트 번호 80을 가지고 있습니다. 특히 초기 연결 설정 세그먼트들과 HTTP 요청 메시지를 전달하는 모든 세그먼트들은 목적지 포트 번호 80을 가지고, 서버는 클라리언트가 보낸 세그먼트들을 출발지 IP 주소와 출발지 포트 번호를 통해서 구별합니다.  

그러나 연결 소켓과 프로세스 사이에 항상 일대일 대응이 이루어지는 것은 아닙니다. 많은 고성능 웹 서버는 하나의 프로세스만 사용하고, 각각의 새로운 클라리언트 연결을 위해 새로운 연결 소켓과 함께 새로운 쓰레드를 생성합니다. 이런 웹 서버에서는 하나의 동일한 프로세스에 첨부된 많은 연결 소켓들이 동시에 존재할 수도 있습니다. 

# 신뢰성 있는 데이터 전송의 원리

[그림 3.8]

위 그림은 신뢰적인 데이터 전송 연구에 대한 프레임워크를 보여줍ㄴ디ㅏ. 상위 계층 개체에세 제공되는 서비스 추상화는 데이터가 전송될 수 있는 신뢰적인 채널의 서비스 추상화입니다. 신뢰적인 채널에선느 전송된 데이터가 손상되거나 손실되지 않습니다. 그리고 모든 데이터는 전송된 순서대로 전달됩니다. TCP는 비신뢰적인 종단간의 네트워크 계층 IP의 바로 위에 구현된 신뢰적인 데이터 전송 프로토콜입니다. 신뢰적으로 통신하는 두 종단점 바로 아래에 있는 계층은 물리적 데이터 전송 링크일 수도 있고 전 세계 인터넷 네트워크일 수도 있지만 이 절에서는 하위 계층을 간단한 비신뢰적인 점대점 채널로 가정하고 설명하겠습니다.  

여기서는 신뢰적인 채널을 구성하기 위해서 송신자와 수신자 각각의 측면에서 점진적으로 설명해보겠습니다. 예를 들어서 하위 채널에서 비트가 손상되거나 전체 패킷을 손실하는 경우, 어떤 프로토콜 메커니즘이 필요한지 고려해보겠습니다.  

일단 원리는 생략합니다. (책 191 ~ 212 페이지)

# 연결지향형 트랜스포트 TCP

## TCP 연결

TCP는 애플리케이션 프로세스가 데이터를 다른 프로세스에게 보내기 전에, 두 프로세스가 서로 `핸드셰이크`를 먼저 해야하므로 `연결지향형`입니다. 즉, 데이터 전송을 보장하는 파라미터들을 각자 설정하기 위한 사전 세그먼트들을 보내야 합니다. TCP 연결 설정의 일부로서 연결의 양단은 TCP 연결과 연관된 많은 TCP 상태 변수를 초기화합니다.(뒤에서 설명)  

TCP 프로토콜은 오직 종단 시스템에서만 동작하고 중간의 네트워크 요소에서는 동작하지 않으므로, 중간의 네트워크 요소들은 TCP 연결 상태를 유지하지 않습니다. 사실, 중간 라우터들은 TCP 연결을 전혀 감지하지 못합니다. 이들은 연결은 보지 못하고 데이터그램만 볼 뿐입니다. 두 호스트의 프로세스 사이에 TCP 연결이 있다면, 애플리케이션 계층 데이터는 A에서 B로 흐르는 동시에, B에서 A로 흐를 수 있습니다. 또한 TCP 연결은 항상 단일 송신자와 단일 수신자 사이의 점대점입니다. 멀티캐스팅으로서의 TCP는 불가능합니다.  

TCP 연결을 초기화하는 프로세스를 클라이언트 프로세스 , 다른 프로세스를 서버 프로세스라고 합니다. 클라이언트 애플리케이션 프로세스는 서버의 프로세스와 연결을 설정하기 원한다고 TCP 클라이언트에게 먼저 알립니다.  

```python
clientSocket.connect((serverName, serverPort))
```

serverName으로 구분된 서버에서 serverPort는 프로세스를 구별합니다. 지금은 클라이언트가 먼저 특별한 TCP 세그먼트를 보낸다는 것만 알면 됩니다. 서버는 두 번째로 특별한 TCP 세그먼트로 응답합니다. 마지막으로 클라이언트가 세 번째 특별한 세그먼트로 다시 응답합니다. 처음 2개의 세그먼트에는 애플리케이션 계층 데이터가 없습니다. 세 번째 세그먼트는 애플리케이션 계층 데이터를 포함할 수 있습니다. 이 연결 설정 절차를 흔히 `three-way handshake`라고 부릅니다.  

일단 TCP 연결이 설정되면, 두 애플리케이션 프로세스는 서로 데이터를 보낼 수 있습니다. 클라이언트가 보내고 싶은 데이터를 소켓으로 보내면 데이터는 이제 클라이언트에서 동작하는 TCP에게 맡겨집니다. TCP는 세 방향 핸드셰이크 동안 준비가 된 `송신 버퍼`로 데이터를 보냅니다. TCP는 자신이 편한대로 세그먼트의 데이터를 전송해얗나다고 기술하면서, 언제 버퍼된 데이터를 전송해야하는지 명시하지 않습니다.  

TCP가 TCP 헤더와 클라이언트 데이터를 TCP 세그먼트로 만들고, 네트워크 계층을 통해서 전달되고(IP 데이터 그램 안에 각각 캡슐화됨), TCP가 상대에게서 세그먼트를 수신했을 때 세그먼트의 데이터는 TCP 연결의 수신 버퍼에 위치합니다. 애플리케이션은 이 버퍼에서 데이터의 스트림을 읽습니다. 즉, TCP 연결의 양 끝은 각각 잣니의 송신버퍼와 수신 버퍼를 가지고 있습니다. 

## TCP 세그먼트 구조

### 순서번호와 확인 응답 번호

[그림 3.30]

TCP 세그먼트의 구성요소 중 순서번호와 확인 응답 번호가 중요합니다. TCP는 데이터가 구조화되어 있지 않고, 단지 순서대로 정렬된 바이트 스트림으로 봅니다. TCPㅢ 순서번호 사용은 이러한 관점을 반영하여, 순서번호를 일련의 전송된 세그먼트에 대해서가 아니라, 전송된 바이트의 스트림에 대해서입니다. 좀 더 쉽게 이해해보겠습니다.  

먼저 최대 세그먼트 크기는 Maximum egment size라고 하여 MSS라고 부릅니다. 이는 일반적으로 로컬 송신 호스트에 의해 전송될 수 있는 가장 큰 링크 계층 프레임의 길이(maximum transmission unit, MTU)에 의해서 일단 결정됩니다. 그리고 TCP 세그먼트와 TCP/IP 헤더 길이가 단일 링크 계층 프레임에 딱 맞도록 정해집니다. 정리하자면,MSS가 헤더를 포함한 TCP 세그먼트의 최대 크기가 아니라, 세그먼트에서의 애플리켕션 계층 데이터에 대한 최대 크기라는 점을 주의합니다. (MSS는 세그먼트의 데이터 필드의 크기를 제한합니다.) TCP가 웹 문서의 이미지와 같은 큰 파일을 전송할 때 일반적으로 MSS의 크기로 파일을 쪼갭니다.  

전송될 데이터가 500,000바이트 크기이고 MSS가 1000바이트일 때,  첫 번째 세그먼트는 순서번호 0, 두 번째 세그먼트는 순서번호가 1000입니다. 각 각의 순서번호는 적절한 TCP 세그먼트의 헤더 내부의 순서번호 필드에 삽입됩니다.  

다음은 확인응답번호입니다. 두 호스트는 서로 데이터를 보내는 동시에 받을 수 있음을 상기합니다. 호스트 A가 자신의 세그먼트에 삽입하는 확인응답 번호는 호스트 A가 호스트B로부터 기대하는 다음 바이트의 순서번호입니다. 예를 들어서 A가 0 ~536바이트까지의 데이터를 B로부터 받았으면, A는 세그먼트를 보낼 때 확인응답번호 필드에 537을 입력하고 송신합니다. 만약 A가 B로부터 0~100 세그먼트, 101~200 세그먼트, 201~300 세그먼트를 받고 싶은데 가운데 세그먼트가 누락된다면 A는 다음 세그먼트의 확인응답번호로 101을 적어서 보낼 것입니다. 이 때 세 번째 세그먼트가 두 번쨰 세그먼트보다 먼저 받아서 순서가 틀리게 되는데 이 때의 규칙은 개발자가 직접 구현하도록 되어 있습니다. 일반적으로는 먼저 받았던 세그먼트를 저장해두었다가 사용합니다. 

## 타임아웃 설정

타임아웃은 연결의 왕복시간보다 좀 더 커야합니다. 만약 그렇지 않으면 불필요한 재전송이 발생합니다. 하지만 너무 크면 세그먼트를 읽어버렸을 때, TCP는 세그먼트의 즉각적인 재전송을 하지 않습니다. 따라서 타임아웃은 EstimateRTT에 약간의 여유 값을 더한 값으로 설정하는 것이 바람직합니다.  

```c
EstimatedRTT =( 1 - a) * EstimatedRTT + a * SampleRTT // RTT 추정 값
DevRTT = (1 - b) * DevRTT + b * |SampleRTT - EstimatedRTT| //적당한 여유값
TimeoutInterval - EstimatedRTT + 4 * DevRTT
```

SampleRTT는 세그먼트가 송신된 시간과 그 세그먼트에 대한 긍정 응답이 도착한 시간까지의 시간 길이입니다. 모든 전송된 세그먼트에 대해서 SampleRTT를 측정하는 대신, 대부분의 TCP는 한 번에 하나의 SampleRTT만 측정합니다. SampleRTT는 혼잡과 종단 시스템에서의 부하 변화 때문에 세그먼트마다 다릅니다. 따라서 불규칙적인 값에 평균값을 채택해서 사용합니다.  

권장되는 a의 값은 0.125입니다. EstimatedRTT는 SampleRTT 값의 가중평균임에 주목합니다. 통계에서 이런 평균은 지수적 가중 이동 평균(exponential weighted moving average, EWMA)라고 부릅니다.  

DevRTT는 RTT의 변화율을 의미합니다. 이는 SampleRTT가 EstimatedRTT로부터 얼마나 많이 벗어나있는지에 대한 예측으로 정의합니다.  

초기 TimeoutInterval의 값으로 1초를 권고합니다. 그리고 타임아웃이 발생했을 때는 TimeoutInterval을 두 배로 일시적으로 변경합니다. 이를 통해서 타임아웃에 의한 후속 확인 응답이 너무 빠른 타임아웃으로 인해서 무시되는 것을 피합니다. 그리고 EstimatedRTT가 계산되면 다시 위 공식에 따라서 TimeoutInterval을 설정합니다.  


## TCP 송신자의 역할

1. TCP는 애플리케이션으로부터 데이터를 받고, 세그먼트로 이 데이터를 캡슐화하고, IP에게 세그먼트 넘긴다.
  - 각 세그먼트는 첫 번째 데이터 바이트의 바이트열 번호인 순서번호를 포함한다.
  - 타이머가 이미 다른 세그먼트에 대해서 실행 중이 아니면,  TCP는 이 세그먼트를 IP로 넘길 때 타이머를 시작한다.
  - 타이머에 대한 만료 주기는 TimeoutInterval이다.
2. 타임아웃
  - 타임아웃 이벤트에 대해 타임아웃을 일으킨 세그먼트를 재전송하여 응답한다.
  - 그리고 다시 타이머를 다시 시작한다.
3. 수신자로부터 수신 확인 응답 세그먼트 ACK 수신
  - 수신 확인응답이 확인되지 않은 가장 오래된 바이트의 순서번호를 SendBase로 저장한다. 
  - 따라서 SendBase - 1은 ㅅ신자에게 정확하고 차례대로 수신되었음을 알리는 마지막 바이트의 순서번호이다.
  - 위와 같은 로직은 누적 확인응답이라고 한다.
  - 다아직 확인 응답 안된 세그먼트들이 존재한다면 타이머를 다시 시작한다.

## 중복 ACK

A가 B에게 {순서번로 92, data 8byte}, {순서번로 100, data 20byte} , {순서번로 120, data 15byte}의 세그먼트를 보낸다고 가정하겠습니다. 세그먼트를 받는 호스트 B는 앞서 말했던 것처럼 '수신 확인응답이 확인되지 않은 가장 오래된 바이트의 순서번호를 SendBase로 저장'합니다. 따라서 두 번째 세그먼트가 유실되었다면 세 번째 세그먼트를 받고나서 두 번째 세그먼트가 유실된 것을 파악할 수 있고, B가 A에게 보내는 다음 세그먼트에 포함된 확인 응답 번호는 100이 됩니다. 만약 세 번째 세그먼트 이후 네 번째 세그먼트를 받았다고 해도 B가 A에게 보내는 세그먼트의 확인응답 번호는 여전히 100입니다. 따라서 A의 입장에서는 중복된 ACK을 받습니다.  

만약 호스트가 3개의 중복된 ACK을 수신하는 경우에는 세그먼트의 타이머가 만료되기 이전에 손실 세그먼트를 재전송하는 `빠른 재전송`을 합니다. 타임아웃이 발생할 때마다 TimeoutInterval을 2배로 늘리기 때문에 불필요한 기다림을 최소화 하기 위함입니다.  

## 흐름제어

TCP 연결의 각 종단은 수신 버퍼를 갖습니다. 만약 송신자가 점점 더 많은 데이터를 빠르게 전송하면 연결읜 수신 버퍼는 쉽게 오버플로될 수 있습니다. 이를 위해서 흐름제어 서비스를 제공합니다. 이는 흐름제어는 속도를 일치시키는 서비스입니다. 수신하는 애플리케이션이 읽는 속도와 송신자가 전송하는 속도를 갖게 합니다.  

데이터를 받는 호스트는 송신 호스트에게 모든 세그먼트의 윈도우 필드에 현재의 수신버퍼 여유값(전체 버퍼 크기 - (마지막으로 받은 바이트 - 마지막으로 읽은 바이트))을 명시합니다. 즉, 수신 호스트는 수신 버퍼의 크기, 마지막으로 읽고/받은 바이트를 기록합니다. 송신 호스트는 마지막으로 Sended/ACked 바이트를 유지합니다.  

만약 수신자가 송신자에게 여유공간이 0이라는 세그먼트를 보낸 뒤에 더 이상 보낼 세그먼트가 없다면, 송신자는 수신자의 버퍼에 여유공간이 생기는 것을 알 수 없습니다. 따라서 송신자가 확인한 수신자의 수신 버퍼 여유 공간이 0이라면, 1바이트 데이터로 세그먼트를 계속해서 전송합니다. 이 세그먼트들은 수신자에 의해서 긍정 확인 응답을 할 것이고 이는 결과적으로 버퍼가 비워지고 여유 공간은 0이 아닌 값이 되어 송신자가 모든 데이터를 송신할 수 있게 됩니다.  


## 혼잡제어

TCP 송신자는 IP 네트워크에서 혼잡 때문에 억제될 수 있습니다. 송싱자가 억제되는 것은 같지만 흐름제어와 분명히 다른 목적을 갖습니다. 

### 시나리오 : 무한 버퍼를 갖는 하나의 라우터

라우터도 수신 버퍼를 가집니다. 만악 송신자가 수신자에게 보내는 패킷이 중간에 거치는 링크의 최대 처리량보다 많은 전송률을 가지면 라우터의 버퍼에 데이터가 쌓입니다. 링크의 최대 처리량 가까이서 동작하는 것은 처리량 관점에서는 이상적이지만, 각각의 패킷은 무한대로 점차 지연되기 때문에 지연 관점에서는 적절하지 않습니다. 

### 시나리오 : 유한한 버퍼는 갖는 하나의 라우터

유한한 버퍼를 갖는 라우터에 의해서 오버프롤우 된 데이터는 라우터에서 버려집니다. 신뢰적인 데이터를 전송해야하기 때문에 버려진 데이터는 다시 전송됩니다. 먼저 송신자가 라우터의 수신 버퍼의 여유공간을 알 수 있다는 말도 안된느 가정을 하면, 원래 송신률과 재전송되는 패킷을 포함한 송신률은 같기 때문에 어떤 손실도 발생하지 않습니다. 이 때는 링크의 최대 용량 R의 절반으로 데이터가 송신됩니다. (보내고 받는 것을 동시에 하기 때문에 나누기 2)  다음으로 패킷이 확실하게 손실된 것을 아는 경우에만 송신자가 재전송하는 현실적인 경우를 보겠습니다. 이런 경우 패킷이 손실된 것을 알기 위해서 충분히 큰 타임아웃을 설정한다고 생각합니다. 이런 경우 재전송되는 패킷을 포함하는 송신률이 링크의 최대 용량 R 의 절반이 되어야 하기 때문에 애초에 보내는 송신률은 R/2보다 작은 값이 됩니다. 어쨌든 재전송하는 비용이 발생합니다. 마지막으로 타임아웃이 너무 빨라서 패킷이 손실되지 않았지만 재전송되는 경우입니다. 이 경우에는 수신자가 재전송된 패킷을 버립니다. 이 떄도 역시 불필요한 재전송이 발생합니다. 

### 네트워크 지원 혼잡제어

라우터가 각각의 출발지에게 처리하는 패킷의 패키 헤더 안에 어떻게 출발지의 전송률을 증가/감소시킬 것인지에 대한 정보를 넣어 보내는 것입니다. 

### TCP 혼잡제어

네트워크 지원 혼잡제어보다는 다음의 방법을 사용합니다. TCP송신자는 자신과 목적지 사이에 혼잡이 없음을 감지하면 송신자는 송신율을 높이고, 혼잡이 있으면 송신율을 줄입니다. 이때 세 가지 문제가 대두됩니다.  

1. 송신자는 자신의 연결에 트랙픽 전송률을 어떻게 제한하는가?
  - 혼잡 윈도우를 기록해서, 매 RTT의 시작 때 송신자는 혼잡 윈도우 만큼의 데이터를 전송한다. RTT가 끝나는 시점에 송신자의 송신율은 혼잡 왼도우 크기 / RTT Byte/second가 된다.
1. 송신자는 혼잡을 어떻게 감지하는가?
  - 타임아웃이나 3연속 중복 ACK 을 보고 혼잡이 일어남을 파악한다.
1. 혼잡을 감지하고 전송률을 변화시키기 위해서 어떤 알고리즘을 사용하는가?
  - 혼잡 왼도우의 크기를 결정하는 것이 중요한데, 확인 응답이 낮ㅇ느 속도로 오면 혼잡 윈도우는 낮은 속도로 증가하고, 확인은답이 빠른 속도로 오면 혼잡 윈도우는 높은 속도로 증가한다. 

이 문제를 다음과 같이 정리될 수 있습니다.

1. 손실된 세그먼트는 혼잡을 의미하며, 이에 따라 TCP 전송률은 한 세그먼트를 손실했을 때 줄어져야 한다.
1. 확인 응답된 세그먼트는 네트워크 송신자의 세그먼트를 수신자에게 전송된다는 것이고, 이에 따라 이전에 확인응답되지 않은 세그먼트에 대해  ACK가 도착하면, 송신자의 전송률은 증가할 수 있다.
1. 밴드폭 탐색. TCP 송신자는 혼잡이 발생하는 시점까지 전송률을ㅇ 증가시키고, 그 시점 이후부터는 줄여나가고, 다시 혼잡 시작이 발생하는지 보기 위한 탐색을 시작한다. 

### TCP 혼잡제어 알고리즘 

1. 슬로 스타트
  - TCP 연결이  시작될 떄 혼잡윈도우의 값은 일반적으로 1MSS로 초기화된다. 이에 따라 초기 전송률은 1MSS/RTT가 된다. 1MSS로 시작해서 한 세그먼트가 확인응답을 받을 때마다 1 MSS씩 증가한다. 
  - MSS가 증가할 때마다 지수적으로 증가하는 갯수만큼 세그먼트를 보낸다. 
  - 지수적으로 증가하는 MSS는 혼잡 윈도우의 절반(혼잡이 발생했을 때 시점의  1/2)를 ssthresh로 설정하고, 혼잡윈도우가 이 값에 도달하거나 지나칠 때 슬로 스타트를 종료한다.
  - 그리고 혼잡 회피 모드로 전환한다.
1. 혼잡 회피
  - 거의 혼잡이 발생할 수 있는 선까지 증가했기 때문에 보수적으로 증가한다.
  - 매 RTT 마다 하나의 MSS만큼 혼잡 윈도우를 증가시킨다.
    - 예를 들어 MSS가 1460 바이트이고 혼잡윈도우가 14600 바이트라면, 10개의 세그먼트가 한 RTT 내에 송신될 수 있다. 각 ACK은 1/10MSS만큼씩 혼잡 왼도우를 증가시키고 결과적으로는 RTT마다 MSS 씩 증가한다.
  - 이 과정은 타임아웃이 발생하면 혼잡 윈도우를 1로 바꾸면서 끝이난다.
1. 빠른 회복
  - ACK을 수신할 때마다  1MSS를 회복한다.
  - 혼잡이 발생했을 때의 혼잡 윈도우의 1/2까지 슬로스타트를 진행한다.
  - 이후에 혼잡회피와 같은 동작을 수행한다. 

[그림 3.52]
