---
layout: single
title: '[Backend] 인터넷은 어떻게 동작하는가?'

categories:
  - Backend

toc: true
toc_sticky: true
toc_label: 'Index'
toc_icon: 'list'
---

![Image Not Found](/assets/images/backend_roadmap_2021_1.png)  
Backend [2021 로드맵](http://localhost:4000/backend/backend-roadmap-2021/) 중 인터넷의 동작원리를 공부해보고자 한다.

1969년 미국 국방부의 주도하에 만들어진 ARPAnet(Advanced Research Projects Agency)이 등장하며 인터넷의 시작을 알렸다.  
이후로 ARPAnet이 민간에 공개되었고, TCP/IP 가 등장하며 일반인들이 인터넷을 접할 수 있게 되었는데, 이제는 인터넷의 규모는 너무나도 커져 현대인들 중에 인터넷을 사용해보지 않은 사람을 찾기가 힘들게 되었다.  
이러한 인터넷을 공부한다는건 단순히 웹을 공부하는 것이 아닌 TCP/IP 기반의 전 세계적인 컴퓨터 네트워크를 이해하는 과정이라 생각해야 할 것이다.

### 본론

인터넷은 IP 및 TCP를 따르는 패킷 라우팅 네트워크를 사용하여 동작하는데,
한 쪽의 컴퓨터에서 다른쪽의 컴퓨터로 데이터를 전송하려 할 때 데이터는 패킷으로 작게 분할 되어 전송되게 되고 이러한 패킷은 TCP/IP 를 통해 목표한 곳에 도착하게된다.
먼저 인터넷을 사용하기 위해 인터넷에 연결된 컴퓨터들은 식별할 수 있는 고유한 주소를 가지고 있어야 하는데 이를 IP 주소라 한다.
한 쪽의 컴퓨터에서 다른쪽의 컴퓨터로 데이터를 전송한다면, 이러한 IP 주소를 통해 목표한 컴퓨터를 식별할 수 있게되고 서로간의 신뢰성 있는 데이터 송수신을 위해 TCP를 이용하게 된다.

네트워크 인프라까지 살펴보게 되면 컴퓨터는 라우터 혹은 모뎀을 통해 ISP(Internet Service Provider)에 연결되게 되고 ISP 는 Client들을 관리하게 되는데,
목표한 곳에 패킷을 전달하기 위해 ISP는 백본 또는 ISP가 관리하는 대역폭의 백본으로 패킷을 라우팅하고 패킷은 목표한 컴퓨터를 찾을 때까지 이동한다.

#### 인터넷 주소

인터넷에 연결된 컴퓨터는 고유한 주소값을 가져야 하는데, 이때의 주소값은 IP 주소이다. ISP 를 통하여 인터넷에 연결한다면 보통 전화 접속 세션 동안 임시 IP 주소가 할당되며, LAN 에서 인터넷에 연결한다면 컴퓨터에 고정 IP가 설정되어 있거나 DHCP(Dynamic Host Configuration Protocol) 서버에서 임시 IP 주소를 취득하여 사용하게 된다.

---

### 참고 자료

- <http://web.stanford.edu/class/msande91si/www-spr04/readings/week1/InternetWhitepaper.htm>{: target="\_blank"}
- <https://www.hp.com/us-en/shop/tech-takes/how-does-the-internet-work>{: target="\_blank"}
