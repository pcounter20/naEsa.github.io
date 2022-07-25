---
layout: post
title: internet-network 의 동작
subtitle: internet-network 의 범용적 동작
categories: http
tags: [http]
---
#### 네트워크에서의 전송

> 핸드폰으로 메일을 보내거나 어떤 프로그램을 사용해서 다른 사용자에게 임의의 정보를 보낼 때 어떻게 동작하는 걸까? 

---

![img01](https://github.com/pcounter20/naEsa.github.io/blob/main/_posts/image/http_organize/img_01.png?raw=true)

---

#### 전송 규약

위 그림에서 보면 요청에서 받기 위한 서버까지 수많은 노드를 통과해야 하는데, 그 과정에서 어떻게 올바른 목적지에 도착할 수 있을까?  이 약속을 규약한 것을 IP(Internet Protocol) 이라 한다. 

택배는 받는 사람의 집 주소를 이용해 택배원 분이 해당 집 주소로 물품을 배달해 준다. 각 사용자의 컴퓨터 장소를 인터넷이 식별하기 위해 각 컴퓨터에 주소를 부여한다. 이러한 집 주소의 역할을 하는 주소를 IP 주소라 한다.

 인터넷에서 네트워크를 타고 전송되는 Hello, World 와 같은 데이터는 패킷이라고 한다.  해당 패킷은, 택배와 같이 수신자 주소(IP 주소)와 송신자 주소, Hello, World(보내고자 하는 데이터) 를 담고 있다.

즉, 요청측에서 응답측까지 어떠한 정보를 보내고자 할때, 패킷에 수송신 IP 주소와, 데이터 등을 담아 규약에 따라 원하는 목적지까지 도착하도록 한다.

---

![img02](https://github.com/pcounter20/naEsa.github.io/blob/main/_posts/image/http_organize/img_02.png?raw=true)

---

> 택배도 주소가 잘못되었거나 혹은 이사를 갔거나 택배사에 문제가 생겨 제대로 물품이 배송되지 못할때가 있다. 네트워크는?

IP protocol에는 몇가지의 한계점이 존재한다.

#### Ip protocol의 한계

* 비연결성  - 패킷을 받을 대상이 없거나 서비스가 불능 상태여도 패킷을 전송한다.

* 비신뢰성 - 중간에 패킷이 사라지거나, 손상, 탈취와 같은 문제가 발생하거나, 패킷의 순서가 이상하게 오거나 한다면?

* 같은 IP 주소를 사용하는 애플리케이션이 2개 이상이라면 어느 프로그램으로 전송하는가.

---

![img03](https://github.com/pcounter20/naEsa.github.io/blob/main/_posts/image/http_organize/img_03.png?raw=true)

---

#### OSI 7 모델

위의 문제점들을 선인들은 이미 적절한 방법으로 해결했다.  간략하게만 설명하자면, 우리 어플리케이션은 하드웨어와 소프트웨어간 또한 다른 하드웨어간의 통신을 OSI model을 통해 표준화되어 있다.
[osi 7 layer](https://ko.wikipedia.org/wiki/OSI_모형) 

패킷의 전송에는 여러 계층이 협업해 라우팅도 하고 통신 연결도 하는 등 여러 동작을 통해 패킷이 전송되는데, 이 계층 중 전송 계층에 대해서만 간략히 이해하고 넘어간다.

전송 계층에서는 발신부터 송신까지의 제어와 에러를 관리하는데, 해당 패킷의 전송이 가능한지, 혹은 실패했다면 다시 시도해서 보내는 신뢰성있는 통신을 보장한다. 이 계층에서 사용하는 통신 규약중에 익히 들어본 TCP, UDP 가 있다.

#### TCP 와 UDP

공통점 
* 포트번호를 활용해 같은 IP 내에서도 올바른 송신이 가능하게 한다. 
* 데이터 오류 검사를 위한 Check_Sum 을 제공한다.

차이점

* TCP : handshake 과정을 통해 신뢰성 있는 데이터 전송을 보장하며, 전송에 문제가 발생했을 경우 재송신 또한 지원한다. 일 대 일 통신을 제공한다. 단, 위와 같이 통신 연결을 확인하는 handshake 과정으로 인해 아래 작성할 UDP 보다 통신 속도가 비교적 느리다.

* UDP : handshake 과정이 존재하지 않으므로 비 연결형 서비스를 제공하며 일 대 일, 일 대 다, 다 대 다와 같은 여러 형태의 데이터 전송을 제공한다. TCP 와 비교하여 빠르지만, 문제 발생시 데이터의 재전송은 하지 않는다.

> 사용자가 만약 게임도 하고 유튜브도 시청하는 등 여러 동작을 각 다른 서버에 요청하고 받아온다면, 데이터 전송은 어떻게 이루어질까?

---

![img04](https://github.com/pcounter20/naEsa.github.io/blob/main/_posts/image/http_organize/img_04.png?raw=true)

---

웹 서비스 공부를 시작할 때 아파치 혹은 다른 서빙 웹 애플리케이션으로 로컬 서버를 띄우면 localhost 8080 과 같은 주소를 기억할 것이다. 이 때 8080 이 포트넘버이다. 즉, 클라이언트가 여러 서비스를 사용하며 각기 다른 서버에 요청해 응답 받을 때 각 서비스는 다른 포트넘버로 구분되어 있기 때문에 올바른 응답을 기대할 수 있다.

#### IP 주소와 DNS

> 우리는 웹사이트를 접속할 때 IP주소로 접속하는 경우는 거의 없다.

---
![img05](https://github.com/pcounter20/naEsa.github.io/blob/main/_posts/image/http_organize/img_05.png?raw=true)

---

IP 주소는 _XXX.200.200.xxx 와 같이 매우 이상한 숫자의 배열로 이루어져 있으며, 이 경우 접속하고자 하는 사이트를 일일히 기억해야만 한다. 현실적으로 불가능하며, 좋은 시스템이라 할 수 없다.

또한, 해당 서비스의 IP 주소는 영구하지 않아서 바뀔 수 있다. 친구의 전화번호가 영원 영구적이지 않은 것과 같다. 

도메인 네임 시스템(DNS)는 이러한 문제를 해결해 준다. DNS 서버는 각 웹사이트의 IP 주소와 우리가 식별하기 쉬운 _google.com 과 같은 문자열을 각각 가지고 있어, 요청자가 문자열로 요청을 하면 DNS 서버는 그에 매칭되는 IP 주소를 반환한다. 이런 서비스를 통해 원하는 애플리케이션으로 데이터를 전송 가능해진다.

**해당 포스팅은 아래의 강의를 정리했습니다**

[모든 개발자를 위한 Http 웹 기본 지식](https://www.inflearn.com/course/http-웹-네트워크)