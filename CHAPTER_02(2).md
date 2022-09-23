## TCP/IP 4계층 모델
>인터넷 프로토콜 스위트는 인터넷에서 컴퓨터들이 서로 정보를 주고받는 데 쓰이는 프로토콜의 집합이다. 
이를 TCP/IP 4계층 모델 또는 OSI 7계층 모델로 설명한다.
TCP/IP 4계층 모델은 네트워크에서 사용되는 통신 프로토콜의 집합으로 계층들은 프로토콜의 네트워킹 범위에 따라 네 개의 추상화 계층으로 구성된다.

### 2.1 계층 구조
<img src="https://thebook.io/img/080326/080.jpg" width="400" >

#### TCP/IP 계층과 OSI 계층의 차이
OSI 계층은 애플리케이션 계층을 세 개로 쪼개고 링크 계층을 데이터 링크 계층, 물리 계층으로 나눠서 표현한다. 
또한 인터넷 계층을 네트워크 계층으로 부른다는 점이 다르다.
이 계층들은 특정 계층이 변경되었을 때 다른 계층이 영향을 받지 않도록 설계되어 있다.

#### TCIP/IP 4계층
<img src="https://thebook.io/img/080326/081.jpg" width="400" >

#### 애플리케이션 계층(application)
FTP, HTTP, SSH, SMTP, DNS 등 응용 프로그램이 사용되는 프로토콜 계층이며 웹 서비스, 이메일 등 서비스를 실질적으로 사람들에게 제공하는 층이다.

<small><span style="color:gray">❕**FTP**</span></small>
<small>장치와 장치 간의 파일을 전송하는 데 사용되는 표준 통신 프로토콜</small>
<small><span style="color:gray">❕**HTTP**</span></small>
<small>보안되지 않은 네트워크에서 네트워크 서비스를 안전하게 운영하기 위한 암호화 네트워크 프로토콜</small>
<small><span style="color:gray">❕**SSH**</span></small>
<small>World Wide Web을 위한 데이터 통신의 기초이자 웹 사이트를 이용하는 데 쓰는 프로토콜</small>
<small><span style="color:gray">❕**SMTP**</span></small>
<small>전자 메일 전송을 위한 인터넷 표준 통신 프로토콜</small>
<small><span style="color:gray">❕**DNS**</span></small>
<small>도메인 이름과 IP 주소를 매핑하는 서버</small>

#### 전송 계층(transport)
송신자와 수신자를 연결하는 통신 서비스를 제공하며 연결 지향 데이터 스트림 지원, 신뢰성, 흐름 제어를 제공하며, 애플리케이션과 인터넷 계층 사이의 데이터가 전달될 때의 중계 역할을 한다. 대표적으로 TCP, UDP

TCP는 패킷 사이의 순서를 보장하고 연결지향 프로토콜을 사용해서 연결을 하여 신뢰성을 구축해서 수신 여부를 확인하며 **가상회선 패킷 교환 방식**을 사용한다.

UDP는 순서를 보장하지 않고 수신 여부를 확인하지 않으며 단순히 데이터만 주는 **데이터그램 패킷 교환 방식**을 사용한다.

http://www.yes24.com/Product/Goods/108887922
