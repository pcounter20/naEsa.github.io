---
layout: post
title: URI와 웹브라우저의 요청 흐름
subtitle: URI와 웹브라우저의 요청 흐름
categories: markdown
tags: [http]
---

#### URI

> URI, URL, URN? URL 은 들어본 것 같은데 나머진?

---
![img01](https://github.com/pcounter20/naEsa.github.io/blob/main/_posts/image/uri/img_01.png?raw=true)

---

위 그림과 같이 URI 는 URL과 URN을 포함해서 일컫는 말로 Uniform Resource Identifier 의 약자이다.  그 하위에 URL, URN이 존재한다.  URI 자체에 어떠한 형식이 있는것이 아니며, 넷상의 특정한 자원을 식별하기 위한 문자열을 뜻한다. **Resource Identifier** 자원 식별.

URN 은 자원의 이름을 정의한 것으로, 리소스의 위치(URL)는 변할 수 있지만 이름은 **변하지 않는 속성**을 가지고 있다. 보편적으로 이름만으로는 리소스를 찾는 방법이 보편화 되어 있지 않아서, URL을 많이 사용한다.

##### URL의 구성요소

---
![img02](https://github.com/pcounter20/naEsa.github.io/blob/main/_posts/image/uri/img_02.png?raw=true)

---
URL의 구성요소를 정리했다.

* 추가로 userInfo 항목이 있으며, url에 사용자 정보를 포함시켜 인증할 수 있으나, 생각해보면 인증정보를 url 에 담으면 안될거 같지 않은가. 거의 사용하지 않는다.

#### 웹브라우저의 요청 흐름

> 이전 포스트 [here](https://github.com/pcounter20/naEsa.github.io/blob/main/_posts/2022-07-22-internet-network.md) 에서 데이터 전송을 알아봤다. 좀 더 세세한 웹 브라우저의 요청 흐름은?

---
![img03](https://github.com/pcounter20/naEsa.github.io/blob/main/_posts/image/uri/img_03.png?raw=true)

---

1. 웹 브라우저는 위의 url 을 활용해 http 요청메시지를 생성한다.
2. socker 라이브러리를 사용 할 경우 TCP/IP 연결을 시도하고, 데이터를 전달한다.
3. TCP/IP 패킷을 생성하고, 생성한 HTTP 메시지를 포함시킨다.

![img04](https://github.com/pcounter20/naEsa.github.io/blob/main/_posts/image/uri/img_04.png?raw=true)

4. 다른 계층에서 일련의 과정을 통해 해석하고 응답메시지를 작성한다. (여기에는 렌더링 할 데이터(민트색)와 서버 동작을 나타내는 HTTP 상태 코드(녹색) 작성 타입(갈색) 등을 포함)

5. 웹 브라우저는 응답 패킷의 http 응답메시지를 해석해 민트 부분을 렌더링한다.


대개의 웹브라우저 요청 응답 흐름은 위와 같다.

**해당 포스팅은 아래의 강의를 정리했습니다**

[모든 개발자를 위한 Http 웹 기본 지식](https://www.inflearn.com/course/http-웹-네트워크)