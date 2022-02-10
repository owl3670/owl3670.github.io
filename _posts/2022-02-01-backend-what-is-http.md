---
layout: single
title: '[Backend] HTTP란 무엇인가'

categories:
  - Backend

toc: true
toc_sticky: true
toc_label: 'Index'
toc_icon: 'list'
---

HTTP(HyperText Transfer Protocol)란 HTML 문서와 같은 리소스들을 가져올 수 있도록 해주는 프로토콜로 웹에서 이루어지는 모든 데이터 교환의 기초이다. 최근에는 HTML 뿐만이 아닌 Plain Text, JSON, XML 등 다양한 형태의 정보를 주고 받을 수 있다.

### 동작 방식

HTTP 는 클라이언트-서버 프로토콜로 사용자가 요청(request)을 하면 서버에서 요청을 처리하고 응답(response)을 주는 형태이다.

#### 특징

- 간단함
  - HTTP는 사람이 읽을 수 있도록 간단하게 고안 되었다.
- 확장 가능
  - HTTP 헤더를 통해 클라이언트와 서버간의 새로운 기능을 추가 할 수 있다.
- 상태가 없음 (Stateless)
  - HTTP 는 상태를 저장하지 않으며 연속된 두 개의 요청 사이에 연결고리가 없다. 그러나 HTTP 쿠키를 통해 세션을 만들수는 있다.
- TCP/IP 연결
  - HTTP 는 메시지의 손실이 없는 연결을 요구하기에 TCP/IP 전송 위에서 동작한다.

#### HTTP Message

- Requests
  - Method : 클라이언트가 원하는 동작을 기재한다.
  - Path : 접근하려는 resource 의 경로를 기재한다.
  - Version : HTTP 의 버전을 기재한다.
  - Headers : 추가적인 정보를 기재한다.
- Responses
  - Version : HTTP 의 버전을 기재한다.
  - Status code : request 의 결과를 약속된 code로 기재한다.
  - Status message : code 에 대한 설명을 기재한다.
  - Headers : 추가적인 정보를 기재한다.
  - Body : 선택적인 정보로 불러와진 resource 등을 기재한다.

---

### 참고 자료

- <https://developer.mozilla.org/ko/docs/Web/HTTP/Overview>{: target="\_blank"}
