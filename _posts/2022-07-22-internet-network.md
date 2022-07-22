---
layout: post
title: internet-network 의 동작
subtitle: internet-network 의 범용적 동작
categories: markdown
tags: [http]
---

> 핸드폰으로 메일을 보내거나 어떤 프로그램을 사용해서 다른 사용자에게 임의의 정보를 보낼 때 어떻게 동작하는 걸까? 

---

![img01](https://github.com/pcounter20/naEsa.github.io/blob/main/_posts/image/http_organize/img_01.png?raw=true)

---

위 그림에서 보면 요청에서 받기 위한 서버까지 수많은 노드를 통과해야 하는데, 그 과정에서 어떻게 올바른 목적지에 도착할 수 있을까?  이 약속을 규약한 것을 
IP(Internet Protocol) 이라 한다. 

택배는 받는 사람의 집 주소를 이용해 택배원 분이 해당 집 주소로 물품을 배달해 준다. 각 사용자의 컴퓨터 장소를 인터넷이 식별하기 위해 각 컴퓨터에 주소를 부여한다. 이러한 집 주소의 역할을 하는 주소를 IP 주소라 한다.

 인터넷에서 네트워크를 타고 전송되는 Hello, World 와 같은 데이터는 패킷이라고 한다.  해당 패킷은, 택배와 같이 수신자 주소(IP 주소)와 송신자 주소, Hello, World(보내고자 하는 데이터) 를 담고 있다.

즉, 요청측에서 응답측까지 어떠한 정보를 보내고자 할때, 패킷에 수송신 IP 주소와, 데이터 등을 담아 규약에 따라 원하는 목적지까지 도착하도록 한다.

---

![img02](https://github.com/pcounter20/naEsa.github.io/blob/main/_posts/image/http_organize/img_02.png?raw=true)

---

> 택배도 주소가 잘못되었거나 혹은 이사를 갔거나 택배사에 문제가 생겨 제대로 물품이 배송되지 못할때가 있다. 네트워크는?

