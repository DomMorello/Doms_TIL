# DHCP (Dynamic Host Configuration Protocol)
- 호스트의 IP주소와 각종 TCP/IP 프로토콜의 기본 설정을 클라이언트에게 자동적으로 제공해주는 프로토콜

DHCP는 UDP 프로토콜을 기반으로 작동한다. UDP의 특징은 단말 간 연결을 수립하지 않는다. 그리고 신뢰성을 보장하지 않는다. 이 두가지 특성으로 인해 UDP는 TCP보다 성능이 좋다. 호스트가 시작됐을 때 DHCP 서버를 알지 못하기 때문에 서브넷 상의 모든 노드로 브로드캐스팅해야 한다. 성능이 좋아야 하고 연결 대상을 모르기 때문에 UDP를 사용한다.

### 원리
![image](https://user-images.githubusercontent.com/51396282/79306505-4a4dec80-7f30-11ea-984d-8514eb752112.png)<br>

##### DHCP Discover
- 메세지 방향: 단말 -> DHCP 서버
- 브로드캐스트 메세지 (Destination MAC = FF:FF:FF:FF:FF:FF)
- 의미: 단말이 같은 LAN(동일 Subnet)상에 브로드캐스팅해서 DHCP 서버가 있는지 크게 외치는 것과 같다.
- 주요 파라미터: Client MAC
##### DHCP Offer
- 메세지 방향: DHCP 서버 -> 단말
- 단말이 보낸 Discoer 메세지 내에 Broadcast Flag 값이 1 이면 브로드캐스트로 하고 0이면 유니캐스트로 한다.
- 의미: DHCP 서버가 "저 여기 있어요"라고 말하는 것과 같다. 이와 동시에 여러 네트워크 정보를 같이 보내준다.
- 주요 파라미터: Client MAC, IP (단말에 할당할 IP주소), Subnet Mask, Router(단말의 Default gateway IP 주소), DNS(DNS 서버 IP 주소), IP lease time(단말이 IP주소를 임대할 수 있는 시간), DHCP Server Identifier(DHCP 서버의 주소, 2개 이상의 DHCP 서버가 Offer를 보낼 수 있으므로 이 주소를 통해 구분한다)
##### DHCP Request
- 메세지 방향: 단말 -> DHCP 서버
- 브로드캐스트 메세지 (Destination MAC = FF:FF:FF:FF:FF:FF)
- 의미: 단말은 DHCP 서버의 존재를 알았고 서버가 보내준 네트워크 정보를 알았다. 이 메세지를 통해서 단말은 DHCP 서버 하나를 선택하고 해당 서버에게 단말이 사용할 네트워크 정보를 요청한다.
- 주요 파라미터: Client MAC, Requested IP address(이 IP주소를 사용하겠다. (DHCP Offer에서 보낸 IP주소)), DHCP Server Identifier(여러 개의 DHCP서버 중에서 Offer에서 보낸 DHCP 서버 주소를 하나 고름으로써 그 서버와 통신함)
##### DHCP Ack
- 메세지 방향: DHCP 서버 -> 단말
- 단말이 보낸 Request 메세지 내에 Broadcast Flag 값이 1 이면 브로드캐스트로 하고 0이면 유니캐스트로 한다.
- 의미: DHCP 서버가 단말에게 네트워크 정보를 전달해 주는 메세지. DHCP Offer의 네트워크 정보와 동일한 정보를 넘겨준다.






- [출처]
(https://terms.naver.com/entry.nhn?docId=2835899&cid=40942&categoryId=32851)
(https://websecurity.tistory.com/137)
