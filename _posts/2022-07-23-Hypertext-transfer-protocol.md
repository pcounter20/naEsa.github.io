---
layout: post
title: HyperText Transfer Protocol에 대해
subtitle: Http를 좀 더 자세히
categories: http
tags: [http]
---

#### 모든 전송은 HTTP

> HTML,TEXT 부터 Image 파일이나 영상,음성 등 인터넷을 타는 거의 모든 형태의 데이터를 전송 가능

http또한 일종의 프로토콜이며, 단체를 통해 표준화되어 전세계에서 사용되고 있다.  http/0.9 부터 http/3 까지 긴 시간에 거쳐 개선되어 왔으며, 현재는 **http/1.1** 와 http/2 가 가장 많이 사용되고 있다.

전송 계층에서 사용한다고 했던 protocol인 TCP, UDP 또한 http 을 기반으로 하고 있으며, **TCP : http/1.1, /2** , **UDP : http/3**
을 기반으로 한다.

#### HTTP의 특징

* 클라이언트 서버 구조
* 무상태 프로토콜과 비연결성
* HTTP 메시지
* 단순성과 확장 가능성

> 클라이언트 서버 구조

