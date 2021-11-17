# images/How does the Internet work?

인터넷은 컴퓨터들이 서로 통신 가능한 거대한 네트워크.

1960년대 미육군에서 기금한 연구 프로젝트에서 시작 > 1980년대 대학, 기업의 지원으로 공공의 기반으로 변화.

모든 컴퓨터를 연결하고 어떤 일이 있어도 연결 상태를 유지할 수 있는 방법을 찾는 방법이라는 점은 변하지 않음.

## 5분만에 배우는 인터넷 동작

![Untitled](images/How%20does%20the%20Internet%20work/Untitled.png)

배경지식

- 인터넷은 wire 이다.
- 서버는 인터넷에 **직접** 연결되는 컴퓨터이다.
- 웹 페이지는 서버 하드디스크에 있는 파일들이다.
- 모든 서버는 고유의 프로토콜 주소 혹은 IP 주소(컴퓨터들이 서로를 찾을 수 있는)를 가진다.

인터넷 동작

- client 컴퓨터는 서버가 아니다. → 인터넷에 직접 연결된 것이 아니기 때문.
- client 컴퓨터는 ISP(internet service provider)를 통해 간접적으로 인터넷에 연결된다.

![Untitled](images/How%20does%20the%20Internet%20work/Untitled%201.png)

- 이미지, 메일, 웹 페이지등 인터넷 자원들은 packet이라는 작은 단위로 전송받아 순서대로 재조합하여 사용한다.
- 인터넷에 직/간접적으로 연결된 모든 것(PC, server, cell phone, etc)은 IP 주소를 가진다.
- 인터넷에 연결된 두개, 혹은 그 이상의 장치들 사이에는 라우터가 있다.
- 패킷은 라우터를 지나갈 때마다 라우터의 IP address를 한 layer 씩 wrap 되고, 다시 되돌아 올때마다 unwrap된다.

## More detail

### Router

두 개의 컴퓨터가 통신할 때, 물리적 혹은 무선으로 연결되어 있어야 한다.

![Untitled](images/How%20does%20the%20Internet%20work/Untitled%202.png)

하지만 위와 같이 연결 된다면, 하단과 같이 10개의 컴퓨터만 연결해도 어마어마한 케이블이 소요된다.

![Untitled](images/How%20does%20the%20Internet%20work/Untitled%203.png)

이를 해결하기 위해 *라우터*가 등장한다. 라우터는 컴퓨터에서 보낸 메시지가 올바른 대상 컴퓨터에 도착하는지 확인하는 일만 한다.

![Untitled](images/How%20does%20the%20Internet%20work/Untitled%204.png)

이제 10대의 컴퓨터를 연결하는데 10개의 케이블과 라우터만 있으면 된다.

수십억대의 컴퓨터를 연결한다면 이 방식을 유지할 수 있을까? 단일 라우터는 그 정도로 확장할 수 없지만, 라우터 역시 컴퓨터이기 때문에, 라우터끼리 연결하는 방식으로 확장할 수 있다.

![Untitled](images/How%20does%20the%20Internet%20work/Untitled%205.png)

### Modem

![스크린샷 2021-11-16 오후 12.35.03.png](images/How%20does%20the%20Internet%20work/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-11-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.35.03.png)

이제는 먼 거리와 연결을 생각해보자. 아주 먼 거리를 어떻게 연결 할 수 있을까? 인터넷 구축 당시, 전화기 기반 시설이 잘 구축되어 있었다. 따라서 이 전화 시설과 네트워크를 연결하기 위해선, 모뎀이라는 장비가 필요했다. 모뎀은 네트워크 정보를 전화 시설에서 처리할 수 있도록 변환한다.

### ISP (Internet Service Provider)

내 인터넷 네트워크에서 다른 네트워크로 메시지를 보내고 싶다면? ISP 를 통해서 전달할 수 있다. ISP 는 특별한 라우터를 가지는데, 다른 ISP의 라우터에도 접근할 수 있다.

![스크린샷 2021-11-16 오후 1.15.26.png](images/How%20does%20the%20Internet%20work/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-11-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_1.15.26.png)

### Domain Name

인터넷에 연결된 모든 장비들은 IP(Internet Protocol) 주소를 가진다. 다른 네트워크로 메시지를 전송할 때도 이 IP가 사용되는데, 일반적으로는 네자리(IPv4, 최근 모자라서 IPv6)의 수이다. IP를 기억하기는 어렵기 때문에, 이를 쉽게 기억/표현할 수 있는 도메인 이름(Domain Name)을 사용한다.

[What is Domain Name?](https://www.notion.so/What-is-Domain-Name-f6860a44d5294f81b3f3ed4b1aac2513) 

### Web과 Internet

Web과 Internet을 혼동하기 쉽지만, 웹은 인터넷이라는 인프라 위에서 구현된 서비스의 일종이다. 인터넷에 연결된 '웹 서버'로서 웹 브라우저가 이해할 수 있는 서비스를 제공한다.

[웹의 동작 방식 - Web 개발 학습하기 | MDN](https://developer.mozilla.org/ko/docs/Learn/Getting_started_with_the_web/images/How_the_Web_works)

[웹페이지, 웹사이트, 웹서버 그리고 검색엔진의 차이는 무엇일까요? - Web 개발 학습하기 | MDN](https://developer.mozilla.org/ko/docs/Learn/Common_questions/Pages_sites_servers_and_search_engines)

[인터넷은 어떻게 동작하는가? - Web 개발 학습하기 | MDN](https://developer.mozilla.org/ko/docs/Learn/Common_questions/images/How_does_the_Internet_work)
